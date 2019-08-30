####border-radius
**border-radius**是向元素添加圆角边框。
**使用方法：**

**border-radius:10px;** /* 所有角都使用半径为10px的圆角 */ 

[![image](https://upload-images.jianshu.io/upload_images/143845-3f6ae1d7e47d414c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://img.mukewang.com/52e216d2000195ef01110111.jpg) 

**border-radius: 5px 4px 3px 2px;** /* 四个半径值分别是左上角、右上角、右下角和左下角，顺时针 */ 

[![image](https://upload-images.jianshu.io/upload_images/143845-b0707bef1d6e3a53.jpg?imageMogr2/auto-orient/strip)](http://img.mukewang.com/52e216f9000131a201110111.jpg)
实心圆：
方法：把宽度（width）与高度(height)值设置为一致（也就是正方形），并且四个圆角值都设置为它们值的一半。如下代码：
```
div{
    height:100px;/*与width设置一致*/
    width:100px;
    background:#9da;
    border-radius:50px;/*四个圆角值都设置为宽度或高度值的一半*/
    }
```
设置左半圆
```
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>border-radius</title>
<style type="text/css">
div.semi-circle{ 
    height:100px;
    width:50px;
    background:#9da;
    border-radius:50px 0 0 50px;
    }
   
</style>
</head>
<body>
<div class="semi-circle">
</div>
```
#### box-shadow
box-shadow是向盒子添加阴影。支持添加一个或者多个
```
box-shadow: X轴偏移量 Y轴偏移量 [阴影模糊半径] [阴影扩展半径] [阴影颜色] [投影方式];
```
参数介绍：

[![image](https://upload-images.jianshu.io/upload_images/143845-c12c3345b7500e95.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://img.mukewang.com/54292d620001ffb107080250.jpg) 
注意：inset 可以写在参数的第一个或最后一个，其它位置是无效的。
