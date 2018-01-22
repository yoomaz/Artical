`in` 在python中的使用很常见，用处也很多，很强大，这里记录下几种常见的用法。

1. 在 for 循环中，获取列表或者元组的每一项：

   ```
   for item in list:
   ```

2. 判断左边的元素是否包含于列表，类似java中的List的contains方法

   ```
   if 1 in aa:
     print 'Very Good'
   else:
     print 'Not Bad'
   ```

   这里是判断 1 是否在 aa 内部

3. 可以用来判断字符串是否包含某一串,可以用来筛选文件使用

   ```
   if 'a' in 'qa'
     print 'ok'
   ```

   注意：如果是想比较两个字符串是否相等，还是需要用 `==` 或者 `!=`