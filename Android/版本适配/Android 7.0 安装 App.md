## 前言

Andrid 7.0 继续提高了对用户隐私的保护和系统安全性，直接禁止掉了 `file://` 协议，我们通常使用下面的代码安装apk就会出现问题：

```java
    /**
     * 调用系统安装应用
     */
    public static boolean install(Context context, File file) {
        if (file == null || !file.exists() || !file.isFile()) {
            return false;
        }
        Intent intent = new Intent(Intent.ACTION_VIEW);
        intent.setDataAndType(Uri.fromFile(file), "application/vnd.android.package-archive");
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        context.startActivity(intent);
        return true;
    }
```

在 7.0 的设备上运行的时候会报 `FileUriExposedException` (文件Uri暴露)错误：

```
Caused by: android.os.FileUriExposedException
```

解决办法是通过定义 `FileProvider`，观察发现使用的是`content://` 协议

## 清单文件中定义 FileProvider

```xml
<provider
    android:name="android.support.v4.content.FileProvider"
    android:authorities="com.graypn.android_common.fileProvider"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/file_paths"/>
</provider>
```

注意下 `authorities `字段，同一个手机上，只能有一个名字为这个 `authorities` 的 `Provider` ，不能重复。

这里的 `meta-data` 设置了可访问的文件路径。

## 添加可访问的文件目录

在res目录下，增加xml文件夹，并新建一个名为 `file_paths.xml` 的文件。文件内容格式如下:

```xml
<paths>
    <external-path
        name="external_storage_root"
        path="."/>
</paths>
```

这里指定了根目录

## 与6.0及以下的系统兼容

以上这种通过 `FileProvider` 形式取得的 Uri 只能在 7.0 以上的设备上使用，在以下的系统回报这个错误：

```verilog
android.content.ActivityNotFoundException: No Activity found to handle Intent { act=android.intent.action.VIEW dat=content://com.graypn.android_common.fileProvider/external_storage_root/szssmartparty.apk typ=application/vnd.android.package-archive flg=0x1 }
```

 所以要有对不同的系统进行不同的处理，最终代码如下：

```java
    /**
     * 调用系统安装应用
     */
    public static boolean install(Context context, File file) {
        if (file == null || !file.exists() || !file.isFile()) {
            return false;
        }
        Intent intent = new Intent(Intent.ACTION_VIEW);
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        Uri apkUri;
        // Android 7.0 以上不支持 file://协议 需要通过 FileProvider 访问 sd卡 下面的文件，所以 Uri 需要通过 FileProvider 构造，协议为 content://
        if (Build.VERSION.SDK_INT >= 24) {
            // content:// 协议
            apkUri = FileProvider.getUriForFile(context, "com.graypn.android_common.fileProvider", file);
            //Granting Temporary Permissions to a URI
            intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
        } else {
            // file:// 协议
            apkUri = Uri.fromFile(file);
        }
        intent.setDataAndType(apkUri, "application/vnd.android.package-archive");
        context.startActivity(intent);
        return true;
    }
```