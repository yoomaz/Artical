## 介绍
​	python中 getopt 模块，该模块是专门用来处理命令行参数的，例如下面这个命令就传入了 `-s, -d , -m`  参数：

```
python channel_builder.py -s /Users/graypn/ -d /Users/graypn/Documents -m 7
```

参数也分长格式和短格式

+ 短格式：`-s`
+ 长格式：`--source`

## 使用

1. 首先要导入包:

   ```python
   import sys
   import getopt
   ```

2. 下面是一段标准模板代码：

   ```python
   srcDir = None
   dstDir = None
   platform = "Phone"
   majorVersion = None
   subVersion = None
   channels = None
   try:    
     opts, args = getopt.getopt(sys.argv[1:], "hs:d:m:v:p:c:",                            
   ["help", "src=", "dst=", "major=", "version=", "platform=", "channels="])
   except getopt.GetoptError:    
     showUsage()
   for op, value in opts:    
     if op in ("-h", "--help"):       
        showUsage()    
     elif op in ("-s", "--src"):        
       srcDir = value    
     elif op in ("-d", "--dst"):       
       dstDir = value    
     elif op in ("-m", "--major"):        
       majorVersion = value    
     elif op in ("-v", "--version"):        
       subVersion = value    
     elif op in ("-p", "--platform"):        
       platform = value    
     elif op in ("-c", "--channels"):        
       channels = value
   if srcDir is None or dstDir is None or majorVersion is None or subVersion is None or channels is None:    
       showUsage()
   ```

   这个 `opts` 是一个字典类型，`getopt.getopt()` 函数第一个传入参，第二个传段参数名，第三个是长参数名，上面基本上是固定的格式，所以用到的时候直接复制就好。