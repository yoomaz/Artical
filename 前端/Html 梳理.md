# Html 学习笔记

1. 基本标签, 重点标签: ` <a>, <img> ` 
2. 几个mate标签的意思
3. 字符集

## html,css,js 的定位

* html 负责语义标记
* css 负责页面样式
* js 负责动态效果

## 字符集

utf-8包含了世界所有的文字，gbk只包含了中文，但是因为utf-8包含的内容多，标记每个字符的位数也多。一个汉字，utf-8用了3个字节，gbk只用了两个。
目前网易和腾讯都是用的 gbk

## SEO 优化的字段

```
    <meta name="Description" content="Hello World">
    <meta name="Keywords" content="Key1">
```

## Html 语法的特性

1. html 对换行不敏感，多个换行或者tab，最终都识别成一个空格
2. 标签分为 容器标签 和 文本标签，只有容器标签才可以包含其他标签，例如 p 标签如果包含了 h 标签，浏览器会自动给 p 标签加上结尾，把这两个标签分离开

## <img> 图片标签

1. alt 属性：在图片显示不出来的时候，会显示这个属性里面的值
2. title 属性：鼠标放在图片上的时候显示的值

## <a> 超链接标签

1. a 代表着 **anchor** 船抛锚的意思，就像是这个页面向另一个页面抛出了锚，跳转到另一个页面
2. href 读 何瑞福
3. title 悬停文本，和图片标签使用方式一样
4. target ：可以设置为 **_blank** ，表示在一个新的页面打开
5. <a> 是一个文本标签，但是可以被 <p> 标签包裹，因为 a 的语义小于 p，这点也是很奇怪

## div 和 span

1. span 是文本标签，只能包含文字，图片；例如 <a>, <img>


## 其他常用标签

1. 列表：

   ```html
   <ul> // 无序列表
   <ol> // 有序列表
   <dl> // 定义列表，一般用在网页尾部，购物指南，下面一列，配送方式，下面一些，例如京东最下方，纤细看CSS第一天笔记第9页
   	<dt>北京</dt>
   	<dd>国家首都，政治文化中心</dd>
       <dt>上海</dt>
   	<dd>魔都，有外滩、东方明珠塔、黄浦江</dd>
       <dt>广州</dt>
   	<dd>中国南大门，有珠江、小蛮腰</dd>
   </dl>
   ```

2. 表单

   ```
   <input type="">:
   text,password,raidobutton,checkbox,select->option,textarea
   // lable 标签主要是给 input 标签增加解释文字
   <input type="radio" name="sex" id="nv"  /> <label for="nv">女</label>
   ```

## 小技巧

1. 按住 **shift** 再按字母可以大写，比频繁切换大小写切换键 capslock 更快捷

## 面试题

Q：h1 标签有什么用？
A：R：在页面中增加主标题的语义
A：W：给文字加粗等等