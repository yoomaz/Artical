使用Gradle的插件com.getkeepsafe.dexcount

如果使用Android Studio的话可以使用这个插件，插件的地址[在这里]
https://github.com/KeepSafe/dexcount-gradle-plugin

在project 的 build.gradle 的加入下面代码：
```java
dependencies { 
      classpath 'com.getkeepsafe.dexcount:dexcount-gradle-plugin:0.6.1' 
}
```

在 app 的 build.gradle 的加入下面代码：
```java
apply plugin: 'com.getkeepsafe.dexcount'
```
然后点击运行，在编译的过程中，就会在 `/app/build/outputs/dexcount/debug.txt `里显示每个包方法数，其中还有炫酷的html网页展现形式