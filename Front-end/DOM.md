####概念
**概念:** 文档对象模型（Document Object Model，简称DOM），是[W3C](https://baike.baidu.com/item/W3C)组织推荐的处理[可扩展标记语言](https://baike.baidu.com/item/%E5%8F%AF%E6%89%A9%E5%B1%95%E7%BD%AE%E6%A0%87%E8%AF%AD%E8%A8%80)的标准[编程接口](https://baike.baidu.com/item/%E7%BC%96%E7%A8%8B%E6%8E%A5%E5%8F%A3)。
 DOM的设计是以对象管理组织（OMG）的规约为基础的，因此可以用于任何编程语言。Dom技术使得用户页面可以动态地变化，如可以动态地显示或隐藏一个元素，改变它们的属性，增加一个元素等，Dom技术使得页面的交互性大大地增强。
**通俗理解:** 把页面上的内容转换成对象的形式,通过操作对象,达到操作页面上标签和标签属性的一组方法

####2. DOM 中常用的操作
- 获取元素
- 对元素进行操作(设置其属性或调用其方法)
- 动态创建元素
- 给元素注册事件

## 3. document对象

**概念: **document对象代表在浏览器中加载的页面


## 4.获取页面中的元素

 什么是元素?
> ​html中的标签在DOM中称为元素
> 为什么要获取页面上的元素呢?
> ​因为:我们想要操作页面上的元素,首先需要获取到对应的元素，然后才能进行后续操作

### 4.1 根据id获取元素

**语法:** document.getElementById('id名');

**作用:** 在整个文档中查找id名为传入的值的元素,如果没有返回null

```javascript
//html
<div id="box"></div>

//js
//在整个文档中查找id为box的元素
var div = document.getElementById('box');
console.log(div);  //返回的是对应的元素

```

### 4.2 根据标签名获取元素

**语法:** document.getElementsByTagName('标签名');

**作用:** 在整个文档中查找所有标签名为传入的值的元素,将这些所有符合条件元素的存放到一个伪数组中返回出来,如果没有就返回一个空的伪数组

```javascript
<div>div1</div>
<div>div2</div>
<div>div3</div>

<script>
var divs = document.getElementsByTagName("div");
for(var i =0;i<divs.length;i++){
    console.log(divs[i])
}
</script>
```

### 小结:
- 通过document这个对象调用获取元素的方法
- getElementById 返回的是对应的DOM元素, 如果没有返回null
- getElementsByTagName 返回的是存储DOM元素的伪数组,如果没有返回空的伪数组

####常用的非表单元素属性有哪些?

- href  超链接的url地址
- title  标签的标题属性
- id      标签的id属性
- src     引入外部文件的路径
- innerText   标签内的文本
- innerHTML   标签内的内容
