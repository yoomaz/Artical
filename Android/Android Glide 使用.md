### 加载 GIF 图片到 ImageView 中
通常 Android 的 ImageView 不能加载 Gif 图片，如不做任何处理，那么加载到 ImageView 中的Gif 只显示第一帧。 
Glide 加载 Gif 图片就很方便：
```
Glide.with(this).load(R.drawable.loading).into(imageView); 
或者
Glide.with(this).load("图片地址：url").asGif().into(iv);
```
这里如果使用了.asGif()方法的话，传入的图片必须是 gif 图，其他图会报错。当然不使用.asGif()方法同样也可以加载gif图。但是用上面这种，有时候加载 gif 图很慢或者出不来，就需要加载缓存策略
```
Glide.with(this).load(url).asGif().diskCacheStrategy(DiskCacheStrategy.SOURCE).into(imageView);
```
加入了缓存策略，缓存策略有四种如下:
```
/** Caches with both {@link #SOURCE} and {@link #RESULT}. */
ALL(true, true),
/** Saves no data to cache. */
NONE(false, false),
/** Saves just the original data to cache. */
SOURCE(true, false),
/** Saves the media item after all transformations to cache. */
RESULT(false, true);
```

上面这种形式，会重复不断的播放，如果只想播放一次就停在最后一帧，可以这样做：
```
Glide.with(this).load("url").diskCacheStrategy(DiskCacheStrategy.SOURCE).into(new GlideDrawableImageViewTarget(iv, 1));
```

#### 图形变换

1. 圆形：https://github.com/wasabeef/glide-transformations
2. 圆角：https://github.com/vinc3m1/RoundedImageView