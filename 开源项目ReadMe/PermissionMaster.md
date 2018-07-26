# PermissionMaster
一个 Android 权限请求框架，简化请求的逻辑，链式调用一行代码完成权限请求的操作。

### 添加仓库地址

由于仓库是我们自己搭建的，所以需要在 **项目(整个项目级别)**的 `build.gradle` 中加入仓库地址，共有两处，如下:

```
buildscript {
    repositories {
        jcenter()
        maven { url 'http://134.175.10.85:8081/repository/maven-public/' }
    }
    
    allprojects {
    repositories {
        jcenter()
        maven { url 'http://134.175.10.85:8081/repository/maven-public/' }
    }
}

}
```

### 添加依赖

在使用的 **moudle** 的 `build.gradle` 文件中添加如下部分：

```
dependencies {
    compile 'com.weipaitang:permissionmaster:1.0.4'
    
    // 如果 buildtools 的版本是比较新的，可以使用 implementation 的形式
    // implementation 'com.weipaitang:permissionmaster:1.0.4'
}
```

### 使用方法

#### 单个权限请求

传入 `PermissionListener` 回调处理请求的结果

+ 如果用户拒绝了，在回调结果里可以调用 `PermissionDeniedResponse.isPermanentlyDenied()` 确定是否是永久拒绝

```java
PermissionMaster.withActivity(MainActivity.this)  // 传入一个 Activity
                .withPermission(Manifest.permission.CAMERA) // 传入单个权限名称
                .withListener(new PermissionListener() { // 传入回调
                            @Override
                            public void onPermissionGranted(PermissionGrantedResponse 							  	response) {
                                // 处理权限申请成功
                                Log.i("MainActivity", "onPermissionGranted：" + response.getPermissionName());
                            }

                            @Override
                            public void onPermissionDenied(PermissionDeniedResponse 								response) {
                                // 处理权限申请拒绝
                                // 通过 response.isPermanentlyDenied() 可以判断是否永久拒绝
                                Log.i("MainActivity", "onPermissionDenied:" + response.isPermanentlyDenied());
                            }
                        })
                 .check();
```

#### 多个权限请求

一次多个权限请求，调用`withPermissions() ` 传多个权限的名字，然后加上 `MultiplePermissionsListener`  回调处理请求的结果，在回调结果里共有四个方法可以调用：

+ 是否所有权限都被授予
+ 有权限被拒绝
+ 获取被允许的权限列表
+ 获取被拒绝的权限列表

```java
PermissionMaster.withActivity(MainActivity.this)
                .withPermissions(Manifest.permission.CAMERA, Manifest.permission.READ_CONTACTS, Manifest.permission.RECORD_AUDIO)
                .withListener(new MultiplePermissionsListener() {
                            @Override
                            public void onPermissionsChecked(MultiplePermissionsReport report) {
                                // 是否所有权限都被授予
                                boolean allPermissionsGranted = report.areAllPermissionsGranted();
                                // 有权限被拒绝
                                boolean isAnyPermissionPermanentlyDenied = report.isAnyPermissionPermanentlyDenied();
                                // 获取被允许的权限列表
                                List<PermissionGrantedResponse> grantedPermissionResponses = report.getGrantedPermissionResponses();
                                // 获取被拒绝的权限列表
                                List<PermissionDeniedResponse> deniedPermissionResponses = report.getDeniedPermissionResponses();
                            }
                        })
                 .check();
```

#### 打开 app 系统权限界面

当用户点击永久拒绝后，将不会弹框提示用户去允许权限，这个时候我们需要引导用户去系统界面去手动允许相关权限 ：

```java
@param： Activity context: Activity
@param： int requestCode: 可以当前 Activity 里实现 onActivityResult 来取得用户操作结果
@param： OnClickListener onCancelClickListener: 当用户点击弹框中的取消，会回调这个 listener

public static void openSettingDialog(Activity context, int requestCode, OnClickListener onCancelClickListener)
```

当然也可以定制引导弹框的 title 和 content

```java
@param： String title: 弹框的名字，如果不传，默认为 “权限申请失败”
@param： String content: 弹框的内容，如果不传，默认为 “我们需要的一些权限被您拒绝或者系统发生错误申请失败，请您到设置页面手动授权，否则功能无法正常使用!”

public static void openSettingDialog(Activity context, int requestCode, String title, String content, OnClickListener onCancelClickListener)
```

### 后期维护

项目的地址在公司的git上： `https://git.weipaitang.com/app/PermissionMaster` ，可以down下来开一个新分支提代码，然后请求合并到 `master` 上，代码通过审核合并后，会通过增加一个版本号的形式发布更新到仓库