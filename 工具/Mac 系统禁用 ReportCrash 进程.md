晚上在 terminal 里敲代码，发现系统异常的卡顿。在 Activity Monitor 里发现 ReportCrash 进程占用了大量的 CPU，不断发送崩溃报告说明不断有进程在崩溃,
把这个 ReportCrash 禁用掉就好了

禁用命令：
```
launchctl unload -w /System/Library/LaunchAgents/com.apple.ReportCrash.plist
sudo launchctl unload -w /System/Library/LaunchDaemons/com.apple.ReportCrash.Root.plist
```

重启方法：
```
launchctl load -w /System/Library/LaunchAgents/com.apple.ReportCrash.plist
sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.ReportCrash.Root.plist
```