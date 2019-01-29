# Android TextView-Spannable

先把 String 包装成 SpannableString

```
String str ="你想来些水果还是饮料";
SpannableString spanStr = new SpannableString(str);
```

一些特殊 Span：

1. 下划线：`UnderlineSpan`

   ```
   //设置下划线文字
   spanStr.setSpan(new UnderlineSpan(), str.indexOf("水"), str.indexOf("果")+1, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
   ```

2. 文字颜色：`ForegroundColorSpan`

   ```
   //设置文字的前景色
   spanStr.setSpan(new ForegroundColorSpan(Color.RED), str.indexOf("水"), str.indexOf("果")+1, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
   ```

3. 文字大小：`AbsoluteSizeSpan`

   ```
   spanStr.setSpan(new AbsoluteSizeSpan(50), str.indexOf("水"), str.indexOf("果")+1, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
   ```

4. 文字超链接：`ClickableSpan`

   ```
   spanStr.setSpan(new ClickableSpan() {
   	@Override
   	public void onClick(View widget) {
   		startActivity(new Intent(MainActivity.this, vegetable.class));
   	}
   },str.indexOf("水"), str.indexOf("果")+1, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
   ```

   注意，TextView 要调用方法 `tv.setMovementMethod(LinkMovementMethod.getInstance())`才能生效

