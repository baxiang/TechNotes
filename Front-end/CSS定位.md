定位的使用包含两个部分：
1. 定位的方式
2. 偏移值  left,right,top,bottom偏移值准确的理解是“距离什么位置有多大”  如 top:100px; 距离顶部为100像素 （向下走）。
####静态定位
所有的标准流都是静态定位
```
    position:static;
```
- 一般用于将某些已经定位的元素还原成标准流，用的很少
- 偏移值对于静态定位来说不起作用，我们以后说的元素定位不包括静态定位
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
            background-color: red;
            position: static;
            left: 500px;
            top: 500px;
        }

    </style>
</head>
<body>
<div class="box">
</div>
</body>
</html>
```

####相对定位
相对定位是相对自己的标准流的位置进行定位移动
```
    position:relative;
```
特性：
    1. 移动的出发点是自身标准流的位置
    2. 相对定位移动的元素不会对别的元素产生干扰，“没有脱标”，真正占得位置还是标准流的位置（肉体不在 灵魂永驻）
    3. 可以盖在标准流的上方
    4. 一般用于微调元素和配合绝对定位来实现效果
####绝对定位
```
    position:absolute;
```
特性：
    1.移动的出发点：
    	从绝对元素开始一直往上级找（直到找到最大的html标签），在这个过程中，只要有一个元素（A元素）是定位（相对，绝对，固定）的任何一个，这个绝对定位的元素就会参照这个A元素进行定位，并且不会在往上找了，如果一个都没有，最终会以html元素定位
    2.脱标
    	1.1. 脱标的元素不占标准流的位置
    	1.2. 不会继承父级的的宽度，内容有多个就撑多大 （不论块级还是行内）
    	1.3. 可以直接写宽高 （不论块级还是行内）
    	1.4. margin:auto对于脱标元素不起作用
巧妙运用：让一个定位盒子水平垂直居中
```
    left: 50%;
    top: 50%;
    margin-left:-自身宽度的一半;
    margin-top:-自身高度的一半;
```
使用方式：
    在工作中，绝对定位"大多"配合相对定位一起使用（父相子绝） 
    父相：在标准流上占有位置
    子绝：针对这个标准流在去移动   
注意：父绝子绝的情况也有，只是很少，不要完全形成思维定式。
####固定定位
```
    position: fixed;
```
特性：
1.脱标
    	1.1.脱标的元素不占标准流的位置
    	1.2.不会继承父级的的宽度，内容有多个就撑多大 （不论块级还是行内）
    	1.3.可以直接写宽高 （不论块级还是行内）
    	1.4.margin:auto对于脱标元素不起作用
    2.移动的出发点：浏览器窗口 （直接表现：滚动条对于固定元素没有作用）

四种定位总结

  |定位模式   |     	是否脱标占有位置  |	是否可以使用边偏移	|移动位置基准      |
|----|----|-----|---|
|  静态static|    	不脱标正常模式  	|不可以      |	正常模式   |     
|  相对定位relative	|不脱标占有位置|  	可以   |    	相对自身位置移动    
|  绝对定位absolute	|完全脱标，不占有位置	|可以      | 	相对于定位的父级移动位置|
|  固定定位fixed |完全脱标，不占有位置	|可以      | 	相对于浏览器移动位置  

####z-index  
控制“定位”元素的叠放层级

1. z-index只针对定位元素有效果
2. z-index值越大，层级越高
3. 如果父元素已经比较过层级了（父元素“都有”z-index的时候，并且值不为auto），那么子元素与子元素之间是不会再去比较的
