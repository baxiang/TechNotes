####HTML整体结构
```
 <html>
        <head>
            <title>标题</title>
        </head>
        <body>
           内容    
        </body>        
    </html>
```
####标签的关系
嵌套关系
```
  <html><head></head></html>
```
并列关系
```
 <head></head><body></body>
```
####列表
<ul>是无序列标签，英文单词为“unordered list”。
```
<ul>
     <li>列表一</li>
     <li>列表一</li>
     <li>列表一</li>
     <li>列表一</li>
 </ul>
```
Ol标签——代表HTML有序列表，是ordered lists的缩写
```
<ol>
     <li>列表一</li>
     <li>列表一</li>
     <li>列表一</li>
     <li>列表一</li>
 </ol>
```
自定义列表
```
<dl>
     <dt>列表标题</dt>
     <dd>列表标题的解释说明</dd>
     <dd>列表标题的解释说明</dd>
     <dd>列表标题的解释说明</dd>
     <dd>列表标题的解释说明</dd>
 </dl>
```
1. <ul></ul>或者<ol></ol>中只能嵌套<li></li>，直接在<ul></ul><ol></ol>标签中输入其他标签或者文字的做法是不被允许的。
 2. <li>与</li>之间相当于一个容器，可以容纳所有元素。
 3. <dl></dl>中只能嵌套<dt></dt>和<dd></dd>，直接在<dl></dl>标签中输入其他标签或者文字的做法是不被允许的。
 4. <dd></dd>之间相当于一个容器，可以容纳所有元素。<dt></dt>一样
####表格table
表格至少有三个基本的标签构成 table 代表一个表格 tr代表行 td代表单元格
tr必须嵌套在table标签中，td必须嵌套在tr或者th中
有几个单元格就代表有几列
```
<table>
    <tr>
        <td>单元格11</td>
        <td>单元格12</td>
        <td>单元格13</td>
    </tr>
    <tr>
        <td>单元格21</td>
        <td>单元格22</td>
        <td>单元格23</td>
    </tr>
    <tr>
        <td>单元格31</td>
        <td>单元格32</td>
        <td>单元格33</td>
    </tr>
</table>
```
表格属性

width  表格的宽度 
height 表格的高度 
border 表格的边框 
align  表格的对齐方式 
cellspacing 单元格与单元格的间距
cellpadding    单元格与单元格内容的间距
**表头**
```
<tr>
        <th>表头1</th>
        <th>表头2</th>
        <th>表头3</th>
    </tr>
```
####表单input
```
<input type="">
    type的取值：
    text 单行文本输入框
    password 密码框
    radio 单选框（在多个里面选择一个） 单选框要生效必须具备name属性 并且同一种类型的单选框的name取值必须一样
    checkbox 复选框（在多个里面选择某几个）
    button 普通按钮
    file 用户上传控件
    submit 提交按钮
```
