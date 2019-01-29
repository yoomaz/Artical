# Android Bitmap 和 BitmapFactory

https://www.cnblogs.com/Free-Thinker/p/6394797.html

## Bitmap

### 相关方法

1. 通过Bitmap的静态方法static Bitmap createBitmap()系
2. 

##BitmapFactory

### 相关方法

1. 通过BitmapFactory工厂类的static Bitmap decodeXxx()系

   + **decodeResource**(Resource res, int id)

     根据给定的资源文件Id解析成位图

     ```
     Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.mipmap.banner_default_temp1);
     ```

   + **decodeFile**(String pathName)

     对应的文件解析成的位图对象

   + **decodeStream**(InputStream in)

     把输入流解析成位图

   + **decodeByteArray**(byte[] data, int offset, int length)

     从指定字节数组的offset位置开始，将长度为length的数据解析成位图

   + **decodeFileDescriptor**(FileDescriptor fd)

     从FileDescriptor中解析成的位图对象

   通过源码发现：最终都用调用相关的 native 方法去解码：

   ```
       private static native Bitmap nativeDecodeStream(InputStream is, byte[] storage,
               Rect padding, Options opts);
       private static native Bitmap nativeDecodeFileDescriptor(FileDescriptor fd,
               Rect padding, Options opts);
       private static native Bitmap nativeDecodeAsset(long nativeAsset, Rect padding, Options opts);
       private static native Bitmap nativeDecodeByteArray(byte[] data, int offset,
               int length, Options opts);
   ```

   decodeFile 会把 file 转化为 FileInputStream ，然后调用 decodeStream

   decodeResource 会把 resId 通过 Resource 的 openRawResource 转成 InputStream ，然后调用 decodeStream

   在 decodeStream 里，如果 Stream 是 asset 文件夹里的，调用 nativeDecodeAsset，否则调用 nativeDecodeStream