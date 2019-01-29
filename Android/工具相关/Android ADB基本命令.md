一、什么是ADB
ADB全称Android Debug Bridge, 是android sdk里的一个工具, 用这个工具可以直接操作管理android模拟器或者真实的andriod设备(手机)。同时，ADB是一个客户端-服务器端程序，其中客户端是你用来操作的电脑，服务器端是android设备。

二、ADB的常用命令
1.查看设备

adb devices [-l]

这个命令是查看当前连接的设备, 如果加上 -l 参数则会显示具体的手机型号

2.启动服务

adb start-service

启动adb服务

3.关闭服务

adb kill-service

关闭adb服务，一般如果有其他程序占用了adb的连接，可以通过关掉再重启服务的方式

4.从手机中取数据

adb pull <远程路径> <本地路径>

5.像手机中发送数据

adb push <本地路径> <远程路径>

6.进入和退出手机的Shell

adb shell   //进入shell

exit //退出shell

7.安装软件

adb install [-r] [-s]

这个命令将指定的apk文件安装到设备上.

－r 强制安装（在某些情况下可以已安装应用程序或者正在运行，可加上此参数强制安装）

－s 将apk文件安装在SD-Card

8.卸载软件

adb uninstall [-k] <软件名>

如果加 -k 参数,为卸载软件但是保留配置和缓存文件.

9.重启设备

adb reboot [bootloader|recovery]

adbreboot-bootloader

直接重启设备回到使用界面adb reboot即可

重启设备到bootloader引导模式：adb reboot-bootloader或adb reboot bootloade

重启到recovery刷机模式：adb reboot recovery

10. logcat

使用管道过滤信息 ：adb logcat | grep Google

不区分大小写：adb logcat | grep -i Google

