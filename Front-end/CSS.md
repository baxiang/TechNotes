####标签选择器
会将页面上所有符合的标签都选择上，但是不能实现差异化选择
```
    标签名 {属性1: 值1;属性2: 值2;}
```

####类选择器
声明自定义类名 
```
.自定义类名 {
属性1:值1;
属性2:值2;
} 
```
给对应的元素添加class类名属性 `class="自定义的类名" `
```
<style>
    .box {
        font-size:25px;
    }
</style>
<div class="box">内容</div>
</body>

```
####多标签选择器
一个元素可以拥有多个类名 类名和类名之间用空格隔开 多类名选择器可以让我们解决更复杂的一些需求
```
<style>
    .red {
        color: red;
    }
    .green{
        color: green;
    }
    .ft10 {
        font-size: 10px;
    }
    .ft20 {
        font-size: 20px;
    }
</style>

<div class="red ft10">内容1</div>
<div class="green ft20">内容2</div>
```
#### id选择器
id选择器的使用方式和类选择器基本一致,id选择器一般配合后期的JS使用较多，效果和类选择器一样，只不过是类选择器可以被多个元素调用，但是id选择器只能被一个元素调用 在同一个页面中吗，不能出现两个id值相同的元素
声明id 
```css
#自定义id名字 {
属性1:值1;
属性2:值2;}
```
调用id 给对应的元素添加属性 id="自定义id"
```
<style>
    #box{
        font-size: 20px;
    }
</style>

<div id="box">id选择器</div>
```
####通配符选择器
选中任何元素，后期用于页面初始化。
```css
* {
    属性1: 值1;
    属性2：值2;
}
```
####伪类选择器
伪类选择器可以理解为选择的元素的一种状态，并不是如之前直接选中元素就完事了
```
a:link   没有被访问的时候的状态
a:visited  访问之后的状态    
a:hover        鼠标移动上去之后的状态
a:active    鼠标按下的状态
```
伪类选择器在实际工作中，不会写这么多，意义不大，推荐简写的方式完成
```
a {}
a:hover {}
```
文字的对齐 缩进 下划线
水平对齐
```
text-align:值;  
取值：left right center
```
该属性控制的是标签  “内部的文字”  水平居中
首行缩进
```
text-indent:值;
取值可以是像素，也可以是em  推荐写法：text-indent:2em;
```
字体下划线和删除线
```
text-decoration:值;
取值：underline 下划线  line-through 删除线 none 去掉多余的样式
```
#### 行高
行高控制的是文字与文字之间的上下距离 （行距）
```
line-height:值;
值的取值是像素
```
小技巧：如果将标签的高度和行高设置成一样，那么这个标签里面的文字可以在这个标签里面垂直居中
两者结合使用可以让单行文字在标签内部水平垂直居中
####样式表位置
######内嵌式样式表
内嵌式样式表是在html里面嵌套一个style标签，将css语句都写在style标签里面
```
<style>
    css语句
</style>
```
######外链式样式表
单独创建一个后缀名为.css的文件，在html文件里面通过link标签引入css文件
```
<link rel="stylesheet" href="css文件的地址" />
```
######行内式样式表
将样式直接写在标签本身上，以属性的形式存在
```
<div style="color:green; font-size:20px;"></div>
```
三种样式表总结

| 样式表       | 优点                     | 缺点                     | 使用情况       | 控制范围           |
| ------------ | ------------------------ | ------------------------ | -------------- | ------------------ |
| 行内式样式表 | 书写方便，权重高         | 没有实现样式和结构相分离 | 较少           | 控制一个标签（少） |
| 内嵌式样式表 | 部分结构和样式相分离     | 没有彻底分离             | 较多           | 控制一个页面（中） |
| 外链式样式表 | 完全实现结构和样式相分离 | 需要引入                 | 最多，强烈推荐 | 控制整个站点（多） |
####标签的三种显示模式
######块级元素
可设置宽和高
独占一行
默认宽度是父级标签的宽度
p这种段落标签不要嵌套块级元素
代表标签：div,p,ol,li,ul,dt,dd,dl,header,footer,aside,nav,article,section
display:block;

 ######行内元素
span a strong ins b del s u i em 
		
		特点：
		1、宽高对于行内元素没有作用
		2、可以允许其他行内元素排成一排
		3、内容有多大就撑多大 如果没有内容，那么没有大小
		4、一般用来包裹文字或者做一些小的装饰(行内标签不要嵌套块级标签 a除外)

#####行内块元素
行内块标签其实本质上是一种特殊的行内标签 （text-align和line-height都可以控制行内块元素）
允许其他的行内元素排一排
可以设置宽高
代表标签：input,img
display:inline-block
####block，inline和inlinke-block细节对比
######display:block
block元素会独占一行，多个block元素会各自新起一行。默认情况下，block元素宽度自动填满其父元素宽度。
block元素可以设置width,height属性。块级元素即使设置了宽度,仍然是独占一行。
block元素可以设置margin和padding属性。
######display:inline
inline元素不会独占一行，多个相邻的行内元素会排列在同一行里，直到一行排列不下，才会新换一行，其宽度随元素的内容而变化。
inline元素设置width,height属性无效。
inline元素的margin和padding属性，水平方向的padding-left, padding-right, margin-left, margin-right都产生边距效果；但竖直方向的padding-top, padding-bottom, margin-top, margin-bottom不会产生边距效果。
######display:inline-block
简单来说就是将对象呈现为inline对象，但是对象的内容作为block对象呈现。之后的内联对象会被排列在同一行内。比如我们可以给一个link（a元素）inline-block属性值，使其既具有block的宽度高度特性又具有inline的同行特性。

```
style type="text/css">
    .box {
        text-align: center;
    }
    a {
        width: 100px;
        height: 50px;
        background-color: red;
        color: #fff;
        display: inline-block;
        text-align: center;
        line-height: 50px;
        text-decoration: none;
    }

</style>
</head>
<body>
<div class="box">
    <a href="#">导航</a>
    <a href="#">导航</a>
    <a href="#">导航</a>
    <a href="#">导航</a>
    <a href="#">导航</a>
</div>
```
	

