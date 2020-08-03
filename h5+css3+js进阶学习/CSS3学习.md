

# HTML5+CSS3+JS进阶学习

​	`html：结构，css：表现，javascript：行为`

## HTML5学习

### 学习重点

1. 不了解的知识点
2. 不了解的标签

###  < meta>标签

> keywords

表示关键词，可以指定多个

> description

指定网站的描述

### < iframe>标签`内联框架`

规定一个内联框架

一个内联框架被用来在当前 HTML 文档中嵌入另一个文档。

```html
<iframe src=""></iframe>
```

### 布局

```html
<!-- 网页头部-->
<header></header>
<!-- 网页的主体-->
<main>
    <!-- 左侧导航-->
	<nav></nav>
    <!-- 中间内容-->
    <article></article>
    <!-- 右侧边栏-->
    <aside></aside>
</main>
<!-- 网页的底部-->
<footer></footer>
```



## CSS3学习

### 学习重点

1. 学习未了解到的一些属性
2. 学习未经常使用的选择器（如：属性选择器）
3. 学习less和scss
4. 学习css3动画绘制

### 权重(优先级)

`内联样式1000>id选择器100>类和伪类10>元素选择器1>通配选择器>继承的样式`

### 单位

> em

相对于字体的大小来计算

1em = 1 font-size

> rem

相对于根元素字体大小来计算

### HSL值

`H：色相 0-360`

`S：饱和度 0% - 100%`

`L：亮度 0% - 100%`

```html
background-color:hsl(90,100%,50%);
```

### 文档流（normal flow）

网页是一个多层结构，最底下一层称为文档流，是网页的基础

我们创建的元素都是在文档流中进行排列的。

### 水平布局

一个元素在父元素中一定要满足以下等式

margin-left+border-left+padding-left+width+padding-right+border-right+margin-right

以上等式必须满足，如不成立，则为`过度约束`，等式自动调整

调整右边距

auto：浏览器自动补充设置。

如：`父元素width:100px,子元素margin-left:10px;margin-right:10px;width:auto;那么auto=80px`

所以常用这个特点让元素水平居中

### 垂直布局

子元素高度超过父元素，则溢出。

overflow，解决溢出

### 外边距重叠现象

- 相邻的垂直方向外边距会发生重叠现象
- 父子元素相邻外边距，子元素会传值给父元素（上外边距）
- 行内元素不支持宽度与高度
- 行内元素垂直方向的padding不会影响页面的布局
- 垂直方向的border不会影响页面布局
- 垂直方向margin不会影响布局

### 隐藏

```html
display:none//隐藏
visibility:visible//默认值
visibility:hidden//隐藏，原位置不动，占位置
```

### 重置样式

1.

```css
/* reset */
html,body,h1,h2,h3,h4,h5,h6,div,dl,dt,dd,ul,ol,li,p,blockquote,pre,hr,figure,table,caption,th,td,form,fieldset,legend,input,button,textarea,menu{margin:0;padding:0;}
header,footer,section,article,aside,nav,hgroup,address,figure,figcaption,menu,details{display:block;}
table{border-collapse:collapse;border-spacing:0;}
caption,th{text-align:left;font-weight:normal;}
html,body,fieldset,img,iframe,abbr{border:0;}
i,cite,em,var,address,dfn{font-style:normal;}
[hidefocus],summary{outline:0;}
li{list-style:none;}
h1,h2,h3,h4,h5,h6,small{font-size:100%;}
sup,sub{font-size:83%;}
pre,code,kbd,samp{font-family:inherit;}
q:before,q:after{content:none;}
textarea{overflow:auto;resize:none;}
label,summary{cursor:default;}
a,button{cursor:pointer;}
h1,h2,h3,h4,h5,h6,em,strong,b{font-weight:bold;}
del,ins,u,s,a,a:hover{text-decoration:none;}
body,textarea,input,button,select,keygen,legend{font:12px/1.14 arial,\5b8b\4f53;color:#333;outline:0;}
body{background:#fff;}
a,a:hover{color:#333;}
```

or 2.

```css
html,body,div,span,applet,object,,h1,h2,h3,h4,h5,h6,p,blockquote,pre,a,abbr,acronym,address,big,cite,code,del,dfn,em,img,ins,kbd,q,s,samp,small,strike,strong,sub,sup,tt,var,b,u,i,center,dl,dt,dd,ol,ul,li,fieldset,form,label,legend,table,caption,tbody,tfoot,thead,tr,th,td,article,aside,canvas,details,embed,figure,figcaption,footer,header,hgroup,menu,nav,output,ruby,section,summary,time,mark,audio,video {
	margin:0;
	padding:0;
	border:0;
	font-size:100%;
	font:inherit;
	font-weight:normal;
	vertical-align:baseline;
}
/* HTML5 display-role reset for older browsers */
article,aside,details,figcaption,figure,footer,header,hgroup,menu,nav,section {
	display:block;
}
ol,ul,li {
	list-style:none;
}
blockquote,q {
	quotes:none;
}
blockquote:before,blockquote:after,q:before,q:after {
	content:'';
	content:none;
}
table {
	border-collapse:collapse;
	border-spacing:0;
}
th,td {
	vertical-align:middle;
}
/* custom */
a {
	outline:none;
	color:#16418a;
	text-decoration:none;
	-webkit-backface-visibility:hidden;
}
a:focus {
	outline:none;
}
input:focus,select:focus,textarea:focus {
	outline:-webkit-focus-ring-color auto 0;
}

```

### 浮动

`脱离文档流`不占据文档流中的位置

浮动元素不从父元素中移除

浮动元素不会超过其他浮动元素

浮动元素不会盖住文字

`可以利用浮动设置文字环绕图片的效果`

脱离文档流后不在区分块与行内元素

### 属性：

#### 文本与文字

**vertical-align** 属性设置元素的垂直对齐方式。

​		middle  把此元素放置在父元素的中部。

**text-indent** 属性规定文本块中首行文本的缩进。

**text-decoration** 属性规定添加到文本的修饰。

| 值           | 描述                                            |
| :----------- | :---------------------------------------------- |
| none         | 默认。定义标准的文本。                          |
| underline    | 定义文本下的一条线。                            |
| overline     | 定义文本上的一条线。                            |
| line-through | 定义穿过文本下的一条线。                        |
| blink        | 定义闪烁的文本。                                |
| inherit      | 规定应该从父元素继承 text-decoration 属性的值。 |

##### 字体

```html
font-faminly:monospace;
```

- serif  衬线字体
- sans-serif 非衬线字体
- monospace 等宽字体

#### 渐进色

> 角度：

background-image: linear-gradient(-90deg, red, yellow);

> 节点：

background-image: linear-gradient(red, yellow, green);

> 彩虹色：

  /* 标准的语法 */ 

 background-image: linear-gradient(to right, red,orange,yellow,green,blue,indigo,violet);

> 透明

background-image: linear-gradient(to right,  rgba(255,0,0,0) ,  rgba(255,0,0,1));

> 径向渐变

background-image: radial-gradient(red, yellow, green);

 background-image: radial-gradient(red 5%, yellow 15%, green 60%);

> 形状与尺寸

**shape 参数定义了形状。它可以是值 circle 或 ellipse。其中，circle 表示圆形，ellipse 表示椭圆形。默认值是 ellipse。**

background-image: radial-gradient(circle, red, yellow, green);

size 参数定义了渐变的大小。它可以是以下四个值：

- **closest-side**
- **farthest-side**
- **closest-corner**
- **farthest-corner**

#### 背景

background-repeat 属性设置是否及如何重复背景图像。

默认地，背景图像在水平和垂直方向上重复。

| 值          | 描述                                                |
| :---------- | :-------------------------------------------------- |
| repeat      | 默认。背景图像将在垂直方向和水平方向重复。          |
| repeat-x    | 背景图像将在水平方向重复。                          |
| repeat-y    | 背景图像将在垂直方向重复。                          |
| `no-repeat` | `背景图像将仅显示一次。`                            |
| inherit     | 规定应该从父元素继承 background-repeat 属性的设置。 |

#### 列表

**list-style:none**：设置列表项目样式为不使用项目符号。

list-style用于设置列表项目相关内容，list-style的取值如下：

1、disc : 默认值，实心圆。

2、circle : 空心圆。

3、square : 实心方块。

4、decimal : 阿拉伯数字。

5、lower-roman : 小写罗马数字。

6、upper-roman : 大写罗马数字。

7、lower-alpha : 小写英文字母。

8、upper-alpha : 大写英文字母。

`9、none : 不使用项目符号。`



#### 弹性盒子

```css
		display: flex; //弹性盒子
		flex-wrap: wrap; //规定灵活的项目在必要的时候拆行或拆列。
		justify-content: space-between;	//项目位于各行之间留有空白的容器内。
```
##### 	子元素靠右显示

```html
margin-left: auto;
```

```html
   flex: 1;
   text-align: right;
```



#### 垂直居中

```css
		display: flex;
		justify-content: left;
		align-items: center;
```
父元素：

```html
display: flex;
```

子元素：

```html
align-self: center;
```



#### clear

clear 属性规定元素的哪一侧不允许其他浮动元素。

| 值      | 描述                                  |
| :------ | :------------------------------------ |
| left    | 在左侧不允许浮动元素。                |
| right   | 在右侧不允许浮动元素。                |
| both    | 在左右两侧均不允许浮动元素。          |
| none    | 默认值。允许浮动元素出现在两侧。      |
| inherit | 规定应该从父元素继承 clear 属性的值。 |

#### 解决父级元素塌陷问题

1. 增加父元素的高度

2. 增加一个空的div,清除浮动：clear : none

3. 在父级元素中增加 overflow ：hidden

   | 值      | 描述                                                     |
   | :------ | :------------------------------------------------------- |
   | visible | 默认值。内容不会被修剪，会呈现在元素框之外。             |
   | hidden  | 内容会被修剪，并且其余内容是不可见的。                   |
   | scroll  | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。 |
   | auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |
   | inherit | 规定应该从父元素继承 overflow 属性的值。                 |

父类添加一个伪类：after

```css
.div：after{

	content:'';

	display: block;

	clear：both;

}
```



#### 高度塌陷问题

子元素浮动，父元素高度塌陷。

##### BFC(Block Formatting Context)   块级格式化环境

CSS中的一个隐含属性，可以为一个元素开启BFC

开启BFC的元素不会被浮动元素覆盖

开启BFC的元素子元素与父元素外边距不会重叠

开启BFC的元素可以包含浮动的子元素

##### 通过特殊方式开启元素BFC：

​	设置元素的浮动(不推荐)

​	将元素设置为行内块元素(不推荐)

​	将元素的overflow设置为一个非visible的值

​	**常用：**overflow:hidden 开启其BFC

```html
.clearfix::before,
.clearfix::after{
	content:'';
	display:table;
	clear:both;
}
```



#### 定位

层级一样，由z-index决定。

##### 相对定位：

**relative** 相对于原来的位置进行偏移，原来位置保留

##### 绝对定位：

层级提升，元素层级越高越优先显示。

相对于父级元素定位，父级元素没有定位，相对于浏览器定位，`不在标准文档流中`，原来的位置不会被保留

##### 固定定位

```css
position:fixed;
```

#### 粘滞定位

```css
position：sticky
```

与相对定位的特点基本一致

不同点：`在元素到达某个位置时固定不动`

#### 背景透明

```css
opacity:0.5;
```

#### box-sizing

| 值          | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| content-box | 这是由 CSS2.1 规定的宽度高度行为。宽度和高度分别应用到元素的内容框。在宽度和高度之外绘制元素的内边距和边框。 |
| border-box  | 为元素设定的宽度和高度决定了元素的边框盒。就是说，为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制。通过从已设定的宽度和高度分别减去边框和内边距才能得到内容的宽度和高度。`可见高宽，不包括：margin\padding` |
| inherit     | 规定应从父元素继承 box-sizing 属性的值。                     |

#### outline

outline （轮廓）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

**注释：**`轮廓线不会占据空间`，也不一定是矩形。

outline 简写属性在一个声明中设置所有的轮廓属性。

可以按顺序设置如下属性：

- outline-color
- outline-style
- outline-width



### 选择器

```css

/* 下面兄弟选择器 */
.active + p{
background: #dddddd;
}
/* 下面所有兄弟选择器 */
.active ~ p{
background: #dddddd;
}
/* 结构伪类选择器 */
ul li:first-child{
    background-color: transparent;
}
ul li:last-child{
    background-color: transparent;
}
/* 选择父元素中第二个子元素 */
p:nth-child(2){
    background: #dddddd;
}
/* 选择父元素中第二个为P的元素 */
p:nth-of-type(1){
    background: #dddddd;
}

/* 属性选择器：
    属性名=属性值（正则）
    =绝对等于
    *=包含这个元素
    ^=开头
    $= 结尾*/
/* 存在id属性的元素 */
a[id]{
    background-color: rebeccapurple;
}
a[id=first]{
    background-color: rebeccapurple;
}
a[class*=first]{
    background-color: rebeccapurple;
}
a[href^=https]{
    background-color: rebeccapurple;
}
a[href$=jpg]{
    background-color: rebeccapurple;
}
```

### less and scss



### css3动画 