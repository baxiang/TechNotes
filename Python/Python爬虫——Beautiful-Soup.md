## Beautiful Soup
Beautiful Soup是Python处理HTML或XML的解析库，使用Beautiful Soup需要安装Beautiful Soup库和lxml的库
 Beautiful Soup[官方下载地址](https://www.crummy.com/software/BeautifulSoup/#Download)
![image.png](https://upload-images.jianshu.io/upload_images/143845-121f97bd3a618535.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 Beautiful Soup的安装方式
```
pip install beautifulsoup4
```
```
from bs4 import BeautifulSoup
soup = BeautifulSoup('<p>HelloPython</p>','lxml')
print(soup.p.string)
# HelloPython
```
####获取属性
```
from bs4 import BeautifulSoup
html = '''
<html>
<head><title>BeautifulSoup Demo</title></head>
<body>
<p class="titleClass" name="titleName">titleContent</p>
</body>
</html>
'''

soup = BeautifulSoup(html,'lxml')
print(soup.p.attrs)
print(soup.p.attrs['name'])
```
#### 获取内容
string获取节点的文本内容
```
from bs4 import BeautifulSoup
html = '''
<html>
<head><title>BeautifulSoup Demo</title></head>
<body>
<p class="titleClass" name="titleName">titleContent</p>
</body>
</html>
'''

soup = BeautifulSoup(html,'lxml')
print(soup.p.string)
print(soup.head.string)
```
## find_all
通过节点查找内容
```
from bs4 import BeautifulSoup
html = '''
<html>
<head><title>BeautifulSoup Demo</title></head>
<body>
<div class='classContent1'>
content0
</div>
<div class='classContent2'>
<li>conent1</li>
<li>conent2</li>
<li>conent3</li>
</div>
</body>
</html>
'''

soup = BeautifulSoup(html,'lxml')
result = soup.find_all('div')
print(result)
```
通过属性查找
```
from bs4 import BeautifulSoup
html = '''
<div class='classContent'>
<li>conent1</li>
<li>conent2</li>
<li>conent3</li>
</div>
'''

soup = BeautifulSoup(html,'lxml')
result = soup.find_all(attrs={'class':'classContent'})
print(result)

```
查找节点内容
```
from bs4 import BeautifulSoup
import re
html = '''
<div class='classContent'>
<li>conent1</li>
<li>conent2</li>
<li>conent3</li>
</div>
'''

soup = BeautifulSoup(html,'lxml')
result = soup.find_all(text=re.compile('conent'))
print(result)
# ['conent1', 'conent2', 'conent3']
```
##select 选择器
```
from bs4 import BeautifulSoup
import re
html = '''
<div class='classContent'>
<li>conent1</li>
<li>conent2</li>
<li>conent3</li>
</div>
'''

soup = BeautifulSoup(html,'lxml')
result = soup.select('div li')
print(result)

```
获取豆瓣读书
```
from bs4 import BeautifulSoup
import requests
url = 'https://book.douban.com/top250?icn=index-book250-all'
urls = ['https://book.douban.com/top250?start={}'.format(str(n)) for n in range(0,250,25)]

def get_book(url):
    wb_data = requests.get(url)
    soup = BeautifulSoup(wb_data.text,'lxml')
    titles = soup.select('div.pl2 > a')
    imgs = soup.select('a.nbg > img')
    cates = soup.select('p.quote > span')
    for title,img,cate in zip(titles,imgs,cates):
        data = {
            'title':title.get_text(),
            'img':img.get('src'),
            'cate':cate.get_text()
        }
        print(data)

for url_urls in urls:
    get_book(url_urls)
```
