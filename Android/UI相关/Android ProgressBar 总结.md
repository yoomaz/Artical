# Android ProgressBar 总结

### 实例1：实现圆角颜色渐变水平进度 ProgressBar

1. 首先在布局中定义一个 ProgressBar

   ```xml
   <ProgressBar
       android:id="@+id/pb_horizontal"
       style="@android:style/Widget.ProgressBar.Horizontal"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:indeterminate="false"
       android:max="100"
       android:progress="80"
       android:progressDrawable="@drawable/progressbar_reduce_price" />
   ```

   style：属性决定这个是一个水平方向的进度条

   progressDrawable：我们通过配置这个属性，来定义进度条的样式

2. 在 drawable 下创建背景文件 `progressbar_reduce_price`

   ```
   <?xml version="1.0" encoding="utf-8"?>
   <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
   
       <item android:id="@android:id/background">
           <shape>
               <corners android:radius="8dp" />
               <solid android:color="@color/color_FBE8E6" />
           </shape>
       </item>
   
       <item android:id="@android:id/progress">
           <scale
               android:drawable="@drawable/progressbar_reduce_price_scale"
               android:scaleWidth="100%" />
       </item>
   
   </layer-list>
   ```

   一个 layer-list 配置两个属性，一个背景，一个进度条

   因为是圆角的，进度条需要用到 scale 属性，如果不加，直接用 shape 标签，进度条会充满整个进度，没有进度的效果

3. 创建 progressbar_reduce_price_scale 文件，配置进度条背景, 这里简单一个渐变的背景

   ```
   <shape xmlns:android="http://schemas.android.com/apk/res/android">
   
       <corners android:radius="8dp" />
   
       <gradient
           android:endColor="@color/color_ff8a65"
           android:startColor="@color/color_d32f2f" />
   
   </shape>
   ```

   如果不用圆角，直接修改 corners 属性即可