在 java 中，想声明一个全局变量可以用到 `static`，python 中则是使用 `global` 来达到全局变量的作用，下面是一段python脚本。

```
exitFlag

def change():
  global exitFlag
  exitFlag = 1
```
也就是说，声明一个全局变量，首先要在方法外的地方声明一个变量，然后在方法内使用它的时候，**先要对这个变量声明下 `global`** , 然后再进行修改，否则会人会你声明的是一个方法内的局部变量。