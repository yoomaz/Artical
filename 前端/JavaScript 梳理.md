# JavaScript 梳理 

## 函数

## 创建方式

1. 定义函数

   ```
   function fun(){
       alert("我是自定义函数")
   }
   fun();  // 函数不调用，自己不执行
   ```

2. 声明函数

   ```
   var fun1 = function(){
       alert("直接量声明")
   }
   fun1();  也需要调用
   ```

3. 使用 Function 关键字

   ```
   var fun2 = new Function("var a = 10; var b = 20; alert(a+b)");

   fun2();
   ```