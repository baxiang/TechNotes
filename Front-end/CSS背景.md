####背景颜色
background-color
```
 background-color: red;
background-color: rgb(88,88,88);
```
####背景图片
```
  background-image: url("show.jpeg");
```
####背景平铺
图片不平铺
```
 background-repeat: no-repeat;
```
####背景是否滚动
可选值 scroll fixed
```
 background-attachment: fixed;
```

背景颜色
background-color: red; 

颜色可以设置成十六进制 或者 rgb 或者 rgba 
1.5.2. 背景图片
```
background-image: url(图片路径);
```
1.5.3. 背景平铺
```
background-repeat: 背景平铺;

1.repeat 平铺  默认的
2.no-repeat 不平铺
3.repeat-x 水平平铺
4.repeat-y 垂直平铺
1.5.4. 背景是否滚动
```
background-attachment: 背景是否滚动;
```
1.scroll  默认值 图片跟随盒子一起滚动
2.fixed      图片不跟随盒子一起滚动
```
1.5.5. 背景位置
```
background-position: 背景位置;
```
1.方位名词 right top left center bottom 书写的顺序不论
2.像素 如果是像素 那么默认在以“当前盒子”的左上角为0 0原点 构建一个坐标系 第一值是X轴的位置 第二个值是Y轴的位置 交互的点就是图片开始显示的起始位置
    多说一嘴：
    1、程序里面的坐标系X轴水平向右  Y轴垂直向下
    2、注意顺序
3.百分比  百分比参照的自身盒子的宽高： 最终的位置是当前盒子自身的宽高的百分比 - 图片自身的宽高的百分比
4.还可以混写 混写是需要考虑顺序
####背景的简写
background:背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;
```
如：background: #fff url() no-repeat scroll center center;
```
####img和背景图片的区别：
img不需要专门写宽高就能够显示在页面上
而背景图片默认是撑不开容器的 需要专门写宽高
一般产品插入图都推荐使用img  而一些小的icon 或者很少更新的图片 再或者超大的图片推荐使用背景图
而且背景图可以让内部的文字盖在上面，但是img不行（除非后期用定位）
