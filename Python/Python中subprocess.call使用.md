### 作用

​	这个方法的作用是执行一些命令行的命令，例如 `sh xxx.sh` , `java -jar xxx.jar` 等，会开启一个子进程去执行，并且**等待子进程结束**才继续执行其他的，使用起来非常方便，就是需要注意一些小细节

### 使用

首先是代码：

```
retcode = subprocess.call(["java", '-jar', jarPath, apkFile, channel, version, targetDstDir])
```
里面放了一个元组，注意：**元组里的元素不能有空格，每个命令或者标识单独一个字符串**，例如上面的命令，写成下面的语句就是错误的：
```
retcode = subprocess.call([ 'java -jar', jarPath, apkFile, channel, version, targetDstDir])
```