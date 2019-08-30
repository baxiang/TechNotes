网页的布局本质就是把网页的元素（图片，文字都）都放入盒子里面，然后按照自己的需要摆放盒子的过程就是网页布局。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
       div{
           width: 100px;
           height: 100px;
           background-color: cadetblue;
           padding: 20px;
           border: red 10px solid;
       }
    </style>
</head>
<body>
<div>盒子模型展示</div>
</body>
</html>
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-83967988433a6f72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####边框border
 边框有三部分组成：边框大小  边框样式（dashed 虚线 dotted 点线 double 双实线） 边框颜色;
```
 border: 10px solid red;
```
如果想要单独某一条边框的写法top bottom left right
```
border-方位名词:边框大小 边框样式 边框颜色;
border-top:10px solid gray;
```
边框是一个复合属性，每一个部分都可以有单独的属性去控制
```
边框大小；border-width
边框样式：border-style
边框颜色：border-color
```
细线表格 
border-collapse:collapse;
设置圆角
borde-radius:值; 一个值控制的上左 上右 下右 下左;
```
borde-radius:50%
```
####内边距padding
padding的取值可以是1-4个
一个值：控制整个上下左右
两个值：第一个控制上下  第二个控制左右
三个值：第一个控制的上 第二个控制的左右 第三个控制的下
四个值：上右下左
单个方向的取值 padding-方位名称
```
padding-top
padding-bottom
padding-left
padding-right
```
需要注意的是行内元素里面不要写上下padding，左右可以
####外边距margin
1.margin的取值方式和padding一样
2.margin的大小只会移动盒子的位置，并不会对盒子的大小造成影响（特殊情况例外）
3.行内元素也不要给上下的margin
4 auto 实现块级元素水平居中显示
```
margin: 0 auto;
```
eg:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
       .box{
           width: 100px;
           height: 100px;
           background-color: red;
           margin: 0 auto;
       }
    </style>
</head>
<body>
<div class="box">盒子水平居中</div>
</body>
</html>
```
使用auto前提条件：必须是块级元素 同时必须有固定的width。
text-align和margin区别
两者都可以是实现水平居中
text-align是控制盒子内部的文字或者内部的行内块 
margin:0 auto的不同 前者控制的是盒子本身
实现清除内外边距
```
* { 
    padding: 0;
    margin: 0; 
}
```
####相邻块元素垂直外边距的合并
当上下相邻的两个块元素相遇时，如果上面的元素有下外边距margin-bottom，
下面的元素有上外边距margin-top，则他们之间的垂直间距不是margin-bottom与margin-top之和
而是两者中的较大者。这种现象被称为相邻块元素垂直外边距的合并。
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
       .box1{
            width: 100px;
            height: 100px;
            background-color: red;
           margin-bottom: 50px;

        }
       .box2{
           width: 100px;
           height: 100px;
           background-color: green;
           margin-top: 50px;
       }
    </style>
</head>
<body>
<div class="box1">盒子1</div>
<div class="box2">盒子2</div>
</body>
</html>

```
![image.png](https://upload-images.jianshu.io/upload_images/143845-502d931146a2bc4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####嵌套块元素垂直外边距的合并
对于两个嵌套关系的块元素，如果父元素没有上内边距及边框，则父元素的上外边距会与子元素的上外边距发生合并，合并后的外边距为两者中的较大者，即使父元素的上外边距为0，也会发生合并。
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
       .box1{
            width: 100px;
            height: 100px;
            background-color: red;
        }
       .box2{
           width: 100px;
           height: 50px;
           background-color: green;
           margin-top: 50px;
       }
    </style>
</head>
<body>
<div class="box1">
    <div class="box2"></div>
</div>
</body>
</html>
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-43591aa6e1a1631a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

解决方案：
1不要给子元素margin-top 给父元素添加padding-top
2给父容器添加上边框或者上padding
3 给父元素添加overflow:hidden。

在实际工作中，我们很难直接得到盒子的内容的大小，所以我们会直接将整个盒子量出来，在后续如果需要添加padding的情况下 一定要减掉 padding。
如果这个块级盒子没有width属性（从父级继承宽度）的时候，添加padding和border不会撑大盒子（盒子内容部分会自动压缩）

