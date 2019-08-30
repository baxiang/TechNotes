####浮动
布局的三种基本方式：
标准流 按照标签默认的特性摆放盒子即为标准流
浮动流 利用浮动摆放盒子即为浮动流
定位流 利用定位摆放盒子即为定位流
浮动最开始是做图文绕排的。
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
       .box{
            width: 300px;
            border: 1px solid #000;
           font-size: 12px;
        }
       img{
           width: 100px;
           height: 80px;
           float: left;
       }
    </style>
</head>
<body>
<div class="box">
    <img src="xuxin.png">2019年乒乓球日本公开赛16日全部结束，
    中国乒乓球队再一次展现王者风范，包揽了全部五项冠军！
    男单冠军：许昕；女单冠军：孙颖莎；女双冠军：刘诗雯/陈梦；男双冠军：樊振东/许昕；混双冠军：许昕/朱雨玲！再次祝贺中国乒乓球队！
</div>
</body>
</html>
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-1e5b7dadf83d0257.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
利用浮动实现布局,一个靠左一个靠右
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .box {
            width: 300px;
            height: 300px;
            border: 1px solid #000;
            font-size: 12px;
        }
        .left {
            float: left;
            width: 100px;
            height: 100px;
            background-color: red;

        }
        .right {
            width: 100px;
            height: 100px;
            float: right;
            background-color: green;
        }
    </style>
</head>
<body>
<div class="box">
   <div class="left"></div>
    <div class="right"></div>
</div>
</body>
</html>
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-258455f60d3c582b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
让元素直接排成一排
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .box {
            width: 300px;
            height: 300px;
            border: 1px solid #000;
            font-size: 12px;
        }
        .left {
            float: left;
            width: 100px;
            height: 100px;
            background-color: red;

        }
    </style>
</head>
<body>
<div class="box">
   <div class="left">1</div>
    <div class="left">2</div>
    <div class="left">3</div>
</div>
</body>
</html>
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-9f96b20fb8e96995.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####脱标
浮动的元素会“脱标”,不在占有标准流的位置
脱标的元素拥有行内块的表现
脱标表示脱离了标准流
标准流：块元素单独占一行，行内元素可以排一排的这种默认的盒子排列方式就是标准流 （按照默认的规则排列的）
1. 脱标的元素不占标准流的位置
浮动因为脱标的特性（脱离标准流了，不占位置，会盖住其他的标准流的盒子）， 所以，在使用上有一个口诀：要浮全浮（要浮动的话兄弟元素都浮动）
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .box {
            width: 300px;
            height: 300px;
            border: 1px solid #0094ff;
        }
        .box1 {
            width: 100px;
            height: 100px;
            background-color: red;
            float: left;
        }
        .box2 {
            width: 110px;
            height: 110px;
            background-color: green;
        }

    </style>
</head>
<body>
<div class="box">
    <div class="box1">1</div>
    <div class="box2">2</div>
</div>
</body>
</html>

```
![image.png](https://upload-images.jianshu.io/upload_images/143845-ca476a963101039c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不会继承父级块级的的宽度，内容有多个就撑多大 。
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .box {
            width: 300px;
            height: 300px;
            border: 1px solid #0094ff;
        }
        .box1 {
            height: 100px;
            background-color: red;
            float: left;
        }
    </style>
</head>
<body>
<div class="box">
    <div class="box1">浮动块</div>
</div>
</body>
</html>
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-d1ab2f3ff321b05d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 不论前身块级还是行内,可以直接写宽高 
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .box {
            width: 300px;
            height: 300px;
            border: 1px solid #0094ff;
        }
        .box1 {
            height: 100px;
            width: 100px;
            background-color: red;
            float: left;
        }
    </style>
</head>
<body>
<div class="box">
    <span class="box1">span浮动</span>
</div>
</body>
</html>
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-ba86a6e8b29f879d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4. margin:auto
对于脱标元素不起作用 无法居中
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .box {
            width: 300px;
            height: 300px;
            border: 1px solid #0094ff;
        }
        .box1 {
            height: 100px;
            width: 100px;
            background-color: red;
            float: left;
            margin: 0 auto;
        }
    </style>
</head>
<body>
<div class="box">
    <span class="box1">span浮动</span>
</div>
</body>
</html>

```

1. 浮动是在盒子内容区域浮动，不会超出padding区域（水平方向）
2. 浮动的盒子一排装不下的时候会掉下来（掉下来的位置会根据上一个浮动盒子的高度决定）
3. 右浮动会颠倒盒子顺序
4. 浮动的盒子压不住文字和图片
5. 尽量在标准流的盒子里面浮动
####闭合浮动
浮动带来的问题：浮动元素撑不开父级容器
解决办法（闭合浮动）：

1. 强行给父盒子添加高度 （不推荐，不利于后期维护）
2. 创建一个新的块级盒子放在浮动元素的最后面，给这个盒子添加一个css属性：clear:both;（不推荐，会产生一个多余的盒子）
3. 用伪元素闭合浮动 （推荐，书写一个公共类就可以使用）
4. 给父元素添加overflow:hidden; (在某些特定场景下使用不了)
####伪元素
就是在当前元素内容的前面或者后面追加一个盒子 这个盒子由css渲染
伪元素特性：

1、伪元素由css渲染  所以不会增加你的DOM（html结构）开销
2、伪元素默认是行内元素 可以进行转块等处理
3、伪元素不管有没有内容  content这个值一定不能少 即使没有 也要写个空
4、伪元素 官方推荐写::  但是为了兼容考虑 写成单冒号
5、因为伪元素是css渲染  所以JS获取不到
伪元素清除浮动完整代码：
```
.clearfix::after {
    content:'';
    display:block;
    clear:both;
    height:0;
    visibility:hidden;
}
.clearfix {
    *zoom:1;
}
或者
.clearfix:before,
.clearfix:after { 
    content:"";
    display:table;
}
.clearfix:after {
    clear:both;
}
/* 为了兼容IE6,7 */
.clearfix {
    *zoom:1;
}
```

