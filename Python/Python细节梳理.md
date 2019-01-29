# Python细节梳理

## 前言

因为最近在写一个 android 构建脚本，需要使用python，就借这个机会学一下这个语言吧，这里记录一下使用过程中遇到的一些不大不小的问题，会不断更新，同时方便以后再遇到相同问题方便查找。

## Tip

1. 升级Python

   我的操作系统是 OSX ，内置了2.7的版本，想升级到3.x的版本，下面是具体的方法。

   从官网下载最新的安装包: `https://www.python.org/`

   双击安装，装完以后，会在用户目录下的 `.bash_profile` 里自动写入 `bin` 目录方便命令行调用，这个时候要注意，在终端里输入 `python` 还是显示的2.7版本，要输入python3才会显示你安装的版本。

   系统的python安装目录：
   `/System/Library/Frameworks/Python.framework/2.7`
   自己下载安装的目录：
   `/Library/Frameworks/Python.framework/Versions/3.5`

2. Python 学习网站

   `http://www.runoob.com/python/python-tutorial.html`
   非常推荐，里面还有js，html5等教程，排版我也很喜欢，简洁大方，入门学习必备

3. Python错误： SyntaxError: Non-ASCII 

   在python文件里写中文会出现这个错误，是由于编码导致的，在文件的最开头，加入以下代码转为 `utf-8`就好

   ```python
   #!/usr/bin/python
   # -*-coding:utf-8-*-
   ```

4. range 和 xrange 函数

   其实本质的区别就是 `range` 生成一个数组，`xrange` 生成一个生成器，供 `list` 使用
   常见用法：
   `range(5, 10)` ：为一个[5，10）的列表
   `range(5, 10, 2)`: 为一个[5，10）的列表，增量为 2

   `list(xrange(1,5))` ：生成一个列表
   ` list(xrange(0,6,2))` ：生成递增为 2 的列表

   更详细可以参考这篇文章：
   `http://blog.csdn.net/karldoenitz/article/details/23476801`

5. 脚本的入口函数

   ```
   if __name__ == "__main__":  
     main()
   ```
   当调用 python xxx.py 时候，会首先执行这一段代码，执行main( )方法, main( )是自己写的方法

6. 异常处理

   固定语法：

   ```
   try:
     XXX
   except IOError(...):
     XXX
   ```
   感觉和 java 很像，就是 catch 变成 except 了，这个 Error 也是有继承关系的


## 模块

#### 文件处理

1. 判断文件是否存在

   ```python
   import os
   
   os.path.exists(file_path)
   ```

2. 连接路径和文件

   ```python
   import os
   
   os.path.join(path, file_name)
   ```

3. 拷贝文件，使用 shutil

   ```python
   import shutil
   
   shutil.copy(origin_file, to_file)
   ```

4. 打开文件，读取文件内容

   ```python
   INFO_FILE_NAME = 'pic_info.txt'
   
   with open(INFO_FILE_NAME) as info:
       info_content = info.readlines()
   ```

   注意，这里 `info_content` 是一个 List 列表，里面包含每一行的字符串

5. 获取当前文件所在路径

   ```python
   import os
   os.path.abspath(os.path.dirname(__file__))
   ```
	


### 字符串

1. 判断字符串是否包含某些字符

   ```python
   str = 'hello'
   result = str.find('h')
   ```

2. 替换字符

   ```
   str = 'hello'
   result = str.replace('h','H')
   ```

3. 截取

   ```
   str = 'key=value'
   array_result = str.split('=')
   
   print(array_result[0]) // key
   print(array_result[1]) // value
   ```

