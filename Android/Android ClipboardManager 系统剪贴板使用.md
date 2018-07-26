> 剪贴板是 Android 中的一个重要功能，本质上是系统的一个服务。

1. 获取剪贴板里的内容

    ```
    final ClipboardManager cm = (ClipboardManager) getSystemService(CLIPBOARD_SERVICE);
    ClipData data = cm.getPrimaryClip();
    //  ClipData 里保存了一个ArryList 的 Item 序列， 可以用 getItemCount() 来获取个数
    ClipData.Item item = data.getItemAt(0);
     String text = item.getText().toString();// 注意 item.getText 可能为空
    ```

2. 设置剪贴板内容变化的监听器

    直接给 `ClipboardManager` 加一个`ClipboardManager.OnPrimaryClipChangedListener` 监听器就可以

    ```
    final ClipboardManager cm = (ClipboardManager) getSystemService(CLIPBOARD_SERVICE);
            cm.addPrimaryClipChangedListener(new ClipboardManager.OnPrimaryClipChangedListener() {
                @Override
                public void onPrimaryClipChanged() {
                    ClipData data = cm.getPrimaryClip();
                    ClipData.Item item = data.getItemAt(0);
                    String text = item.getText().toString();
                    Log.i(TAG, text);
                }
            });
    ```
    
3. 往剪贴板里添加内容

```
    private void setClipDate() {
        final ClipboardManager cm = (ClipboardManager) getSystemService(CLIPBOARD_SERVICE);
        ClipData clipData = cm.getPrimaryClip();
        clipData.addItem(new ClipData.Item("hello"));
        cm.setPrimaryClip(clipData);
    }
```
这样添加的时候，在数据结构里是一个一个 Item 的形式，但是在获取的时候，例如粘贴，则会把所有 Item 拼接一起复制出来