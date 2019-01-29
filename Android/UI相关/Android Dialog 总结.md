# Android Dialog 总结



### 常见问题

1. Dialog 中包含 EditText，弹出时候不显示键盘，弹框消失的时候，键盘没有消失

   默认弹出带有 EditText 的 DIalog 是不会自动弹出键盘的，所以我们需要手动弹出键盘，但是弹出的时候，EditText并没有获取焦点，所以 Dialog 消失的时候，键盘没有跟着消失，解决方案如下：

   1. 先弹出 Dialog
   2. 延迟 200 毫秒让 EditText 获取焦点，同时弹出键盘，键盘的焦点在 EditText 上

   ```
   private void showKeyboard(final Context context, final EditText editText) {
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




#### 