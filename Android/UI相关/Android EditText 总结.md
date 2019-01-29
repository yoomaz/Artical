# Android EditText 总结

###EditText 改变光标颜色

光标其实是一个 Drawable，EditText 里有个属性去修改这个 Drawable

主要是修改 EditText 的 `android:textCursorDrawable="@drawable/edit_cursor_color"` 属性

edit_cursor_color.xml 的内容：

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle" >

    <size android:width="2dp" />

    <solid android:color="@color/color_abab3a" />

</shape>
```

注意这个 <size> 标签，用来定义宽度



### EditText 和软键盘常见用法

#### 前因

产品有这样一个需求，一个输入框，刚开始进入页面的时候不获取焦点，第一次点击输入框光标到最后，后面再点击定位到具体文字，键盘收起来输入框失去焦点。

#### 分析与实现

其实即使把 `EditText` 的使用方式组合起来，再监听一下软键盘的抬起，其中的一些小细节还是需要考虑的。

+ 功能一：`EditText` 刚进入见面的时候不获取焦点，不自动弹出键盘

  解决：在父布局 加上下面两个属性，获取焦点即可：

  ```
  android:focusable="true"
  android:focusableInTouchMode="true"
  ```

+ 功能二：第一次点击后光标到最后，再点击定位到具体文字

  这个稍微麻烦一点，首先关闭 `EditText` 的焦点：

  ```
  android:focusable="false"
  ```

  然后监听 `EditText`的点击事件：

  ```java
  etLeaveWords.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  if (!etLeaveWords.hasFocus()) {
                      etLeaveWords.setFocusable(true);
                      etLeaveWords.setFocusableInTouchMode(true);
                      etLeaveWords.requestFocus();
                      KeyboardUtil.softShow(mContext, etLeaveWords);
                      String message = etLeaveWords.getText().toString();
                      if (!TextUtils.isEmpty(message)) {
                          etLeaveWords.setSelection(message.length());
                      }
                  }
              }
          });
  ```

  注意这几部是一个整体，因为 requestFocus() 后用户需要再点击一次才可以显示键盘，这里我们直接强行显示出来：

  ```java
  etLeaveWords.setFocusable(true);
  etLeaveWords.setFocusableInTouchMode(true);
  etLeaveWords.requestFocus();
  KeyboardUtil.softShow(mContext, etLeaveWords); // 显示键盘
  ```

  至于定位，直接调用 `setSelection()` 方法

  至于软件盘消失，`EditText` 失去焦点，用的网上的一个方案：

  ```
  etLeaveWords.getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
              @Override
              public void onGlobalLayout() {
                  Rect r = new Rect();
                  etLeaveWords.getWindowVisibleDisplayFrame(r);
  
                  int heightDiff = etLeaveWords.getRootView().getHeight() - (r.bottom - r.top);
                  if (heightDiff < 200) {
                  	// 处理键盘隐藏
                      etLeaveWords.setFocusable(false);
                  }
              }
          });
  ```



### EditText 在 Dialog 中显示，弹出Dialog的同时，弹出键盘

#### 前因

1. Dialog 中有一个 EditText，在弹出 Dialog 的时候默认不弹出键盘

2. 如果强行把键盘弹出来，关闭 Dialog 后，键盘不消失

#### 原因

强行弹出的键盘焦点不在 EditText 上，我们在弹出前需要让 EditText 获取焦点

#### 解决

弹出 Dialog 后，调用下面的方法：

```
    private static void showKeyboard(final Context context, final EditText editText) {
        if (editText == null || context == null) {
            return;
        }
        editText.postDelayed(new Runnable() {
            @Override
            public void run() {
                //设置可获得焦点
                editText.setFocusable(true);
                editText.setFocusableInTouchMode(true);
                //请求获得焦点
                editText.requestFocus();
                //调用系统输入法
                InputMethodManager inputManager = (InputMethodManager) context.getSystemService(Context.INPUT_METHOD_SERVICE);
                if (inputManager != null) {
                    inputManager.showSoftInput(editText, 0);
                }
            }
        }, 200);
    }
```







