##Urllib
Urllib 库，它是 Python 内置的 HTTP 请求库.不需要额外安装即可使用,在 Python中，有 Urllib 和 Urlib2 两个库可以用来实现Request的发送。而在 Python3 中，已经 没有Urllib2 ，统一为 Urllib
####urllib.request 请求
```
from urllib import request

response = request.urlopen('http://httpbin.org/ip')
headers = response.getheaders()
for header in headers:
    print(header)
print(response.status)
print(response.getheader("Server"))
print(response.getheader("Content-Type"))
print(response.read())
```
####Request类
```
 def __init__(self, url, data=None, headers={},
                 origin_req_host=None, unverifiable=False,
                 method=None):
        self.full_url = url
        self.headers = {}
        self.unredirected_hdrs = {}
        self._data = None
        self.data = data
        self._tunnel_host = None
        for key, value in headers.items():
            self.add_header(key, value)
        if origin_req_host is None:
            origin_req_host = request_host(self)
        self.origin_req_host = origin_req_host
        self.unverifiable = unverifiable
        if method:
            self.method = method
```
####请求头
``` python

import urllib.request as rq
headers = {'User-Agent':'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36'}

request = rq.Request(url='http://www.jianshu.com',headers = headers)
response = rq.urlopen(request)
print(response.getheader("Server"))
```
####请求参数
```
import urllib.request as rq
import urllib.parse as pr
data = bytes(pr.urlencode({"location": "北京", "ak": "VAuehGLIw7lW6ovwpnKboM3I","output": "json"}),encoding='utf-8')
request = rq.Request(url='http://api.map.baidu.com/telematics/v3/weather?',data = data,method='POST')
response = rq.urlopen(request)
print(response.read())
```
## ProxyHandler代理请求
```
from urllib import request

handle = request.ProxyHandler({'http':'106.57.23.28:34498'})
opener = request.build_opener(handle)
response = opener.open('http://httpbin.org/ip')
print(response.read())
```
## urllib.robotparser 爬虫协议解析
主要是用来识别网站的 robots.txt 文件，然后判断哪些网站可以爬，哪些网站不可以爬的。

##Requests
[Requests的Github地址](https://github.com/requests/requests)
![image.png](https://upload-images.jianshu.io/upload_images/143845-2d248a11dd097d8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

pip3 安装
```
pip3 installl request
```
## Response 
|属性|说明|
|-|-|
|status_code|状态码|
|text|内容|
|encoding||
|apparent_encoding||
|content||
request库开源的httpbin可以用于平常我们网络的知识的学习

![image.png](https://upload-images.jianshu.io/upload_images/143845-631c3b81c386019b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##Get请求
```
import requests

response = requests.get("https://www.baidu.com")
print(type(response))
print(response.status_code)
print(response.text.encode())
print(response.cookies)
```
 增加请求参数
```
import requests

data = {"location": "北京", "ak": "VAuehGLIw7lW6ovwpnKboM3I", "output": "json"}
response = requests.get("http://api.map.baidu.com/telematics/v3/weather",params=data)
print(response.status_code)
print(response.text)
```
####POST
```
import requests

formdata = {
    'i': '我是中国人',
    'doctype': 'json',
}
url = "http://fanyi.youdao.com/translate?smartresult=dict&smartresult=rule"
res = requests.post(url,data=formdata)
print(res.text)
```
#### 代理proxies
```
import requests

url = "http://httpbin.org/ip"
res = requests.get(url, proxies={"http": "112.192.248.34:26241"})
print(res.text)
```
####Cookie
http请求是无状态的。也就是说即使第一次和服务器连接后并且登录成功后，第二次请求服务器依然不能知道当前请求是哪个用户。cookie的出现就是为了解决这个问题，第一次登录后服务器返回一些数据（cookie）给浏览器，然后浏览器保存在本地，当该用户发送第二次请求的时候，就会自动的把上次请求存储的cookie数据自动的携带给服务器，服务器通过浏览器携带的数据就能判断当前用户是哪个了。

cookie的格式：
```
Set-Cookie: NAME=VALUE；Expires/Max-age=DATE；Path=PATH；Domain=DOMAIN_NAME ;SECURE
```
|参数|说明|
|--|--|
|NAME|cookie的名字|
|VALUE|cookie的值|
|Expires|cookie的过期时间|
|Path|cookie作用的路径|
|-Domain|-cookie作用的域名|
|-SECURE|-是否只在https协议下起作用|
```
import requests

url = "https://www.baidu.com/"
res = requests.get(url)
cookesJar = res.cookies
print(cookesJar)
print(requests.utils.dict_from_cookiejar(cookesJar))

```
## 爬取经典影片100排行榜
```
def get_one_page(url):
    try:
        headers = {"User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.87 Safari/537.36"}
        resp = requests.get(url, headers=headers)
        if resp.status_code == 200:
            return resp.text
        return  None
    except RequestException:
        return  None


def parse_one_page(html):
    pattern = re.compile('<dd>.*?board-index.*?>(\d+)</i>.*?data-src="(.*?)".*?name"><a'
                         + '.*?>(.*?)</a>.*?star">(.*?)</p>.*?releasetime">(.*?)</p>'
                         + '.*?integer">(.*?)</i>.*?fraction">(.*?)</i>.*?</dd>', re.S)
    items = re.findall(pattern,html)
    for item in items:
        yield {
            "index": item[0],
            "image": item[1],
            'title': item[2].strip(),
            'actor': item[3].strip()[3:],
            'time': item[4].strip()[5:],
            'score': item[5].strip() + item[6].strip()
        }
def write_to_json(content):
    with open('result.txt', "a", encoding="utf-8") as f:
        f.write(json.dumps(content, ensure_ascii=False)+"\n")
def main(offset):
   url = "http://maoyan.com/board/4?offset="+str(offset)
   html = get_one_page(url)
   for item in parse_one_page(html):
       write_to_json(item)



if __name__ == '__main__':
    for i in range(10):
        main(offset=i*10)
```
