### 简介

使用到 github 上排名第一的 Banner 库，地址是：https://github.com/youth5201314/banner，目前最新版本是 1.4.9

### 使用方法

1. 添加依赖

   ```groovy
   compile 'com.youth.banner:banner:1.4.9'
   ```

2. 引入布局

   ```xml
   <com.youth.banner.Banner
           android:id="@+id/banners"
           android:layout_width="match_parent"
           android:layout_height="200dp"
           android:background="@mipmap/banner"
           app:image_scale_type="fit_xy"
           app:indicator_drawable_selected="@drawable/bg_banner_light"
           app:indicator_drawable_unselected="@drawable/bg_banner_default"
           app:indicator_height="3dp"
           app:indicator_width="3dp"
           app:scroll_time="500"/>
   ```

   这里的 `indicator_drawable_selected` 是一个 drawable xml资源，可以写成自己想要的形式和大小，例如选中状态的红色圆角：

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <shape xmlns:android="http://schemas.android.com/apk/res/android">

       <corners android:radius="4dp"/>
       <solid android:color="@color/red"/>

   </shape>
   ```

   还有 `scroll_time` 是指打开后第一次滑动的开始时间，`delay_time` 是滑动的时间间隔

   更加详细的自定义字段见 github 上项目里说明

3. 图片的加载框架可以自己定义，继承 ImageLoader 实现它的 displayImage 方法就可以了，注意这里传入的上下文是 ApplicationContext ，防止内存泄漏：

   ```java
   public class GlideImageLoader extends ImageLoader {
       @Override
       public void displayImage(Context context, Object path, ImageView imageView) {
           //具体方法内容自己去选择，次方法是为了减少banner过多的依赖第三方包，所以将这个权限开放给使用者去选择
           Glide.with(context.getApplicationContext())
                   .load(path)
                   .crossFade()
                   .into(imageView);
       }
   }
   ```

4. 具体使用, 传入图片列表和图片加载器，图片列表是一个 List 集合，可以为图片URL，也可以是本地的R资源 id

   ```java
   viewHolder.banners.setImages(picList)
                           .setImageLoader(new GlideImageLoader())
                           .start();
   ```

### 混淆

```java
# glide 的混淆代码
-keep public class * implements com.bumptech.glide.module.GlideModule
-keep public enum com.bumptech.glide.load.resource.bitmap.ImageHeaderParser$** {
  **[] $VALUES;
  public *;
}
# banner 的混淆代码
-keep class com.youth.banner.** {
    *;
 }
```

