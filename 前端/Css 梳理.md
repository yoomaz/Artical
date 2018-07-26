# Css梳理

1. 选择器：
2. 常见样式：

## 选择器

+ 基本选择器：
  1. 标签选择器
  2. 类选择器
  3. id 选择器
+ 高级选择器
  1. 后台选择器：中间是空格
  2. 交集选择器：点号 
  3. 并集选择器：逗号



+ 标签选择器
+ id选择器
+ 类选择器： 一个 class 里可以有多个类
+ 后代选择器：空格代表后代
+ 交集选择器：中间没有空格
+ 并集选择器：中间是逗号

## 常见样式

没什么难的，就是要多看多记

1. color:red ; 字体颜色；c
2. font-size:40px ;字号大小；fos
3. background-color: blue; 背景颜色; bgc
4. font-weight: bold; 加粗; fwb,    不加粗： fwn，属性为normal
5. font-style: italic; 斜体;fsi, 也可以是:normal, fsn
6. text-decoration: underline; 下划线；tdu，也可以是：none， tdn

## 继承性和叠层性

**继承性**：自己有的属性，后台也会继承这些属性：这些关于文字样式的，都能够继承； 所有关于盒子的、定位的、布局的属性都不能继承。

**叠层性**：就是css处理冲突的能力，有多个css定义的时候，要进行权重计算，详细看CSS第2天笔记最后面

## 盒子模型

1. width，height

2. margin， padding

   如果写了四个值：`padding:30px 20px 40px 100px` , 顺序为上右下左
   如果写了三个值：上右下，左边和右边相同
   如果写了两个值：上右，缺少的地方和对边相同
   一些元素，默认带有padding，比如ul标签，我们为了做站的时候，便于控制，总是喜欢清除这个默认的padding：

3. border

   border属性能够被拆开，有两大种拆开的方式：
   1） 按3要素:border-width、border-style(solid dashed dotted)、border-color
   2） 按方向：border-top、border-right、border-bottom、border-left

## 标准文档流

### 块级标签和行内标签

Html 把标签分为**容器级**和**文本级**，CSS把标签分为块级标签和行内标签

1. 块级元素

   ● 霸占一行，不能与其他任何元素并列

   ● 能接受宽、高

   ● 如果不设置宽度，那么宽度将默认变为父亲的100%。

2. 行内元素

   ● 与其他行内元素并排

   ● 不能设置宽、高。默认的宽度，就是文字的宽度。

通过 `display:inline/block`可以两者相互转换



**脱离标准流有三种方式：**

1. 浮动
2. 绝对定位
3. 固定定位

## 浮动

特性：

1. 浮动的元素脱标
2. 浮动的元素互相贴靠
3. 浮动的元素有“字围”效果：不浮动的标签会贴着浮动的标签

一个span标签不需要转成块级元素，就能够设置宽度、高度了。所以能够证明一件事儿，就是所有标签已经不区分行内、块了。

也就是说，**一旦一个元素浮动了，那么，将能够并排了，并且能够设置宽高了。无论它原来是个div还是个span。** 



`float: left` 浮动标签



浮动有时候会对我们想要的效果有影响，例如：

```
<div>
	<ul>
		<li>HTML</li>
		<li>CSS</li>
		<li>JS</li>
		<li>HTML5</li>
		<li>设计模式</li>
	</ul>
</div>

<div>
	<ul>
		<li>学习方法</li>
		<li>英语水平</li>
		<li>面试技巧</li>
	</ul>
</div>
```

我们本以为这些li，会分为两排，但是，第二组中的第1个li，去贴靠第一组中的最后一个li了,最终显示一排。

这时候就要用到清除浮动了

## 清除浮动

1. 给父元素加上行高：只要浮动在一个有高度的盒子中，那么这个浮动就不会影响后面的浮动元素。所以就是清除浮动带来的影响了。但是一般我们不会给 div 设置高度，想要他的高度被内容撑起来。

2. 父元素加上：`clear:both` 标签，有副作用，margin 失效

3. 外/内墙法：加一个清除浮动的标签在中间，例如：

   ```
   cl{
   	clear: both;
   }
   .h16{
   	height: 16px;
   }
   ```

4. 偏方：`overflow:hidden` 

## Margin

### margin 塌陷问题

标准文档流中，竖直方向的margin不叠加，以较大的为准

如果不在标准流，比如盒子都浮动了，那么两个盒子之间是没有塌陷现象的

## 盒子居中

直接写成：margin : 0 auto

注意：

1. 使用margin:0 auto; 的盒子，必须有width，有明确的width
2. 只有标准流的盒子，才能使用margin:0 auto; 居中。

也就是说，当一个盒子浮动了、绝对定位了、固定定位了，都不能使用margin:0auto;

3. margin:0 auto;是在居中盒子，不是居中文本。文本的：居中，要使用 `text-align:center`

## 子标签的 margin 对父元素的影响

margin 适用于兄弟之间，如果用在父子之间不合适,如果父亲没有border，那么儿子的margin实际上踹的是“流”，踹的是这“行”。所以，父亲整体也掉下来了，设置margin-top，父元素也会往下有margin值，具体见CSS笔记第四章

## font 类型标签样式

主要有：

```
font-size

line-height

font-family
```

集中起来写就是：`font:12px/24px,"宋体"`  行高也可以使用百分比的形式：`font:12px/200%,"宋体"`



## a 标签样式

伪类:一个标签，不同的状态有不同的样式。

a:link/visited/hover/active,  必须要按照 love hate 爱恨准则的顺序写，但是一般 visited/hover 不写，就有下面的形式,写两种常用的：

```html
a {
  color:white;
}
a: hover {
  color:blue;
}
```

## background 标签样式

主要有：

```css
background-color: background-color: rgb(18,52,86); background-color:#ff0000;
background-image: background-image:url(images/wuyifan.jpg);
background-repeat:background-repeat:no-repeat/repeat-x/repeat-y
background-position:以屏幕左上角为起点进行偏移，可以是像素：12px 24px，也可以是单词：right bottom（图片放到右下角）;
background-attachment：背景是否固定。如果是 background-attachment:fixed，背景就会被固定住，不会被滚动条滚走
```

综合属性：

`background:red url(1.jpg) no-repeat 100px 100px fixed;`  等价于：

```css
background-color:red;
background-image:url(1.jpg);
background-repeat:no-repeat;
background-position:100px 100px;
background-attachment:fixed;
```

## 定位

有三种：相对定位，绝对定位，固定定位

### 相对定位

相对于目前的位置进行定位，不脱离标准流，只是影子出去了，真实位置不变

用途：

1. 微调元素
2. 做绝对定位的参考，子绝父相

```css
position: relative;
top: 10px; 相对原来位置的顶部10px，也就是向下移动10px
left: 40px;
```

## 绝对定位

脱离标准流，绝对定位之后，标签就不区分所谓的行内元素、块级元素了，不需要display:block;就可以设置宽、高了：

```css
span{
	position: absolute;
	top: 100px;
	left: 100px;
	width: 100px;
	height: 100px;
	background-color: pink;
}
```

参考点：

1. 绝对定位的参考点，如果用top描述，那么定位参考点就是页面的左上角,如果用bottom描述，那么就是浏览器首屏窗口尺寸，对应的页面的左下角
2. 一个绝对定位的元素，如果父辈元素中出现了也定位了的元素（一般设置为相对定位），那么将以父辈这个元素，为参考点。

绝对定位的盒子居中：

绝对定位之后，所有标准流的规则，都不适用了,所以margin:0 auto;失效, 如果想要居中，就是 `left:50%; margin-left: 负的宽度的一半`。

```
width: 600px;
height: 60px;
position: absolute;
left: 50%;
top: 0;
margin-left: -300px;   → 宽度的一半
```



## 固定定位

相对浏览器窗口定位。页面如何滚动，这个盒子显示的位置不变

`position:fixed`

## **z-index**

1. z-index值表示谁压着谁。数值大的压盖住数值小的。
2. 只有定位了的元素，才能有z-index值。也就是说，不管相对定位、绝对定位、固定定位，都可以使用z-index值。而浮动的东西不能用
3. z-index值没有单位，就是一个正整数。默认的z-index值是0。
4. 如果大家都没有z-index值，或者z-index值一样，那么谁写在HTML后面，谁在上面能压住别人。定位了的元素，永远能够压住没有定位的元素。
5. 从父现象：父亲怂了，儿子再牛逼也没用