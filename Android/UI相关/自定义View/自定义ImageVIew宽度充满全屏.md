### 遇到的问题

需要在显示一张图片，宽度充满全屏，高度不确定，使用了 ImageView 的集中 scaleType 没有合适的，常用的 fitXY 也不能用，最后通过自定义 ImageView 解决

### 自定义充满全屏宽度 ImageView

```
public class ResizableImageView extends AppCompatImageView {

    public ResizableImageView(Context context) {
        super(context);
    }

    public ResizableImageView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        Drawable drawable = getDrawable();
        if (drawable != null) {
            int width = MeasureSpec.getSize(widthMeasureSpec);
            //高度根据使得图片的宽度充满屏幕计算而得
            float scale = (float) width / (float) drawable.getIntrinsicWidth();
            int height = (int) Math.ceil(scale * (float) drawable.getIntrinsicHeight());
            setMeasuredDimension(width, height);
        } else {
            super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        }
    }
}
```

#### 总结

主要还是熟悉了几个 API：

ImageView 的获取图像 Drawable：

```
Drawable drawable = getDrawable();
```

Drawable 的获取图片高度和宽度：

```
drawable.getIntrinsicWidth()
getIntrinsicHeight()
```





