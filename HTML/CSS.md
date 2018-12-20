##样式
1内部样式表
控制一个页面,部分结构和样式相分离，没有彻底分离
```
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        div{
            color: red;
        }
    </style>
</head>
<body>
   <div>内嵌样式表</div>
</body>
```
2 行类样式表/内联样式表
书写方便，权重高，没有实现样式和结构相互分离，主要用于控制一个标签
```
 <div style="color: red">行类样式表</div>
  <div >行类样式表</div>
  <div>行类样式表</div>
```
3外部样式表
 <link rel="stylesheet">
```
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="CSS/style.css">
</head>
<body>
   <div>外类样式表</div>
   <div>外类样式表</div>
   <div>外类样式表</div>
</body>
</html>
```
##块级元素
常用的块元素<h1>~<h6><p><div><ul><ol><li>
##行内元素
<a><strong><span><b><em><u><s><del><i>其中<span>标签是最典型的行内标签。
特点：
1和相邻的行内元素在一行上
2 高、宽无效 但水平方向的padding和margin可以设置、垂直方向的无效。
3默认宽度就是它本身内容的宽度
4行内元素只能容纳文本或则其他行内元素（a元素）
5只有文字才能组成段落，因此p里面的不能放块级元素，同理还有这些标签。<h1>~<h6><dt>他们都是文字块标签，里面不能放其他块级别元素。
6链接里面不能再放链接
##行内块元素（inline-block）
<img><input><td>
高度行高外边距以及内边距可以控制
##标签显示模式转换display
块转行内:display:inline
行内转块：display:block
块、行内元素转换为行内块display:inline-block
```
<head>
    <meta charset="UTF-8">
    <title>Title</title>
   <style>
       div{
           width: 100px;
           height: 100px;
           background-color: pink;
           display:inline;/*块转行内*/
       }
       span{
           width: 100px;
           height: 100px;
           background-color: hotpink;
           display:block;
       }
       a{
           width: 50px;
           height: 20px;
           background-color: deeppink;
           display: inline-block;/*函内块模式*/
       }
   </style>
</head>
<body>
    <div>123</div>
    <div>456</div>
    <div>789</div>
    <span>abc</span>
    <span>efg</span>
    <span>hij</span>
 <a href="#">123</a><a href="#">123</a>
```
##CSS复合选择器
####交集选择器
由两个选择器构成，其中第一个为标签选择器，第二个为class选择器，两个选择器之间不能有空格，如h3.special
```
<head>
    <meta charset="UTF-8">
    <title>Title</title>
   <style>
       .red{
           color: red;
       }
       p.red{
           font-size: 30px;
       }
       div.red{
           font-size: 15px;
       }
   </style>
</head>
<body>
    <div class="red">熊大</div>
    <p class="red">小强</p>
</body>
```
##并集选择器
```
<head>
    <meta charset="UTF-8">
    <title>Title</title>
   <style>
      div,
      p,
      .daye,
      span
      {
          color: red;
          font-size: 18px;
      }
   </style>
</head>
<body>
    <div >刘德华</div>
    <p >张学友</p>
    <span>郭富城</span>
    <h1>凤姐</h1>
    <h1 class="daye">凤大爷</h1>
</body>
```
##后代选择器
后代选择器又称为包含选择器，用来选择元素或元素组的后代，其写法就是把外层标签写在前面，内层标签写在后面，中间用空格分隔。当标签发生嵌套的时，内层标签纪就成为外层标签的后代。
## 子元素选择器
子元素选择器只能选择最为某元素子元素的元素，其写法就是把父级标签写在前面，子级标签写在后面，中间跟一个>进行连接，注意符号左右两侧各保留一个空格。
## 属性选择器
##伪类选择器
##伪元素选择器
::first-letter
::first-line
::selection
E::before 
E::after
