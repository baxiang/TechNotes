```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
       div{
           text-align: center;
       }
        span{
            text-align: center;
        }
    </style>
</head>
<body>
<div>div文字居中</div>
<span>span文字居中</span>
</bod
```
上述代码执行结果中span 标签是无法实现文字居中显示的.
![a](https://upload-images.jianshu.io/upload_images/143845-ba1c6e269a58ca7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####块级标签/元素
div，p, h1-h6 ol ul dl dt dd
h5新增的 header footer nav aside article section
display:block
特点：
1可设置宽高
2 独占一行
3 默认宽度是父级别标签的宽度
4主要用途为容器布局，可以嵌套其他块级元素或者行内标签。
5 p标签不要包裹其他块级元素

浏览器会增加
```
display:block
```
####行内标签/元素
span，a，stong,ins b, del,s,su,i,em
display:inline
1设置高宽没有效果 
2 可以允许其他行内元素排成一排
3内容有多大就显示多大，如果没有内容吗，那么就没有大小
4 一般用来包裹文字或者做一些小的装饰
5 不要行内标签不要嵌套块标签,但是a 标签不受此限制
####行内块标签/元素
img input
转换成行内块标签
```
display:inline-block
```
1行内块标签其实本质是一种特殊的行内标签，text-align和line-height都可以控制行内的块元素
2 允许其他的行内元素排成一排
3 设置宽高生效
