##XPath

|表达式	|描述|
| :-: |  -| 
|nodename|选取此节点的所有子节点|
|/	|从根节点选取|
|//xxx|从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置|
|.	|选取当前节点|
|..	|选取当前节点的父节点|
|@xxx	|选取属性内容|
|/text()|选取文本内容|
|starts-with(@属性名称，属性字符相同部分)|以相同字符开始|
演示使用HTML内容
```
html = '''
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>测试-常规用法</title>
</head>
<body>
<div id="content">
    <ul id="test1">
        <li>这是第一条信息</li>
        <li>这是第二条信息</li>
        <li>这是第三条信息</li>
    </ul>
    <ul id="test2">
        <li>这是第4条信息</li>
        <li>这是第5条信息</li>
        <li>这是第6条信息</li>
    </ul>
    <div id="url">
        <a href="http://www.baidu.com">百度</a>
        <a href="http://www.163.com" title="Netease">网易</a>
    </div>
</div>
</body>
</html>
'''
```
##lxml库
```
from lxml import etree
selector = etree.HTML()
content = selector.xpath()

```
/代表根节点开始的逐层获取
```
from lxml import etree
selector = etree.HTML(html)
content = selector.xpath('/html/head/title/text()')
print(content)
```

获取所有的li标签
```
selector = etree.HTML(html)
content = selector.xpath('//li')
for c in content:
    print(c)
```
获取所有title属性的值
```
selector = etree.HTML(html)
content = selector.xpath('//@title')
for c in content:
    print(c)
```
[]获取列表的index
```
selector = etree.HTML(html)
content = selector.xpath('//ul[2]/li[1]/text()')
for c in content:
    print(c)
# 这是第4条信息
```
获取所有的li的文本内容，无论在什么位置
```
selector = etree.HTML(html)
content = selector.xpath('//li/text()')
for c in content:
    print(c)
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-7b241687e7949bad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

获取属性id=url的div下层的a标签所有href地址
```
selector = etree.HTML(html)
content = selector.xpath('//div[@id="url"]/a/@href')
for c in content:
    print(c)
```
获取属性class="test1"的ul下层的最后一个li标签的文本内容
```
selector = etree.HTML(html)
content = selector.xpath('//ul[@class="test1"]/li[last()]/text()')
print(content)
```
获取所有含有id属性且值为test1的ul下层li的文字内容
```
selector = etree.HTML(html)
content = selector.xpath('//ul[@id="test1"]/li/text()')
for c in content:
    print(c)
```
## 豆瓣读书250数据抓取
```
<table width="100%">
	<tbody><tr class="item">
		<td width="100" valign="top">
			<a class="nbg" href="https://book.douban.com/subject/25862578/" onclick="moreurl(this,{i:'1'})">
				<img src="https://img3.doubanio.com/view/subject/m/public/s27264181.jpg" width="90">
			</a>
		</td>
		<td valign="top">
			<div class="pl2">
				<a href="https://book.douban.com/subject/25862578/" onclick="&quot;moreurl(this,{i:'1'})&quot;" title="解忧杂货店">
					解忧杂货店
				</a>
				<br>
				<span style="font-size:12px;">ナミヤ雑貨店の奇蹟</span>
			</div>
			<p class="pl">[日] 东野圭吾 / 李盈春 / 南海出版公司 / 2014-5 / 39.50元</p>
			<div class="star clearfix">
				<span class="allstar45"></span>
				<span class="rating_nums">8.6</span>
				<span class="pl">(
					294790人评价
				)</span>
			</div>           
			<p class="quote" style="margin: 10px 0; color: #666">
				<span class="inq">一碗精心熬制的东野牌鸡汤，拒绝很难</span>
			</p>
		</td>
	</tr>
</tbody></table>
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-f492cc542d8a8fe9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```
import requests
from lxml import etree

headers={
    'Accept':'*/*',
    'Accept-Encoding':'gzip, deflate',
    'Accept-Language':'zh-CN,zh;q=0.8',
    'Connection':'keep-alive',
    'Origin':'https://book.douban.com',
    'Referer':'https://book.douban.com/',
    'User-Agent':'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.221 Safari/537.36 SE 2.X MetaSr 1.0'
}
for i in range(0, 10):
    url = 'https://book.douban.com/top250?start={}'.format(i * 25)
    html = requests.get(url, headers=headers).content
    sel = etree.HTML(html)
    tables = sel.xpath('//div[@class="indent"]/table')
    for table in tables:
        item = table.xpath('tr[@class="item"]/td[@valign="top"][2]')
        print(item[0].xpath('div[@class="pl2"]/a/@title')[0])
        print(item[0].xpath('p[@class="pl"]/text()')[0])
        quote = item[0].xpath('p[@class="quote"]/span/text()')
        if quote:
            print([0])
```

