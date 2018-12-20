```
from pyquery import PyQuery as pq

html = '''
<div class='classContent'>
<li>conent1</li>
<li>conent2</li>
<li>conent3</li>
</div>
'''

content = pq(html)
result = content('li')
print(result)

```
##URL 初始化
```
content = pq(url='http://www.baidu.com',encoding="utf-8")
result = content('title')
print(result)
#  <title>百度一下，你就知道</title>
```
