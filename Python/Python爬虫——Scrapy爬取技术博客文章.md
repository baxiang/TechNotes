##创建工程
```
$scrapy startproject ArticleSpider
You can start your first spider with:
   
    scrapy genspider example example.com
```
##创建爬虫
通过scrapy genspide创建jobbole的爬虫
```
 $cd ArticleSpider
 $scrapy genspider jobbole blog.jobbole.com
```
##创建main.py
```
import sys
import os
from scrapy.cmdline import execute

sys.path.append(os.path.dirname(os.path.abspath(__file__)))
execute(['scrapy', 'crawl', 'jobbole'])
```
关闭settings.py文件的robot协议
```
ROBOTSTXT_OBEY = False
```
现在整个目录结构是
```
├── ArticleSpider
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-36.pyc
│   │   └── settings.cpython-36.pyc
│   ├── items.py
│   ├── middlewares.py
│   ├── pipelines.py
│   ├── settings.py
│   └── spiders
│       ├── __init__.py
│       ├── __pycache__
│       │   ├── __init__.cpython-36.pyc
│       │   └── jobbole.cpython-36.pyc
│       └── jobbole.py
├── main.py
└── scrapy.cfg
```
##xpath语法
|表达式|说明|
|---|---|
|article|选取所有的article元素|
|/article|选取根元素article|
|article|选取所有属于article的子元素的a元素|
|//div|选取所有的div子元素（无论出现在文档任何地方）|
|article//div|选取所有属于artical元素的后代div元素，不管它出现在artical之下的任何位置|
|//@class|选取所有名为class的属性|
```
import scrapy
from ArticleSpider.items import ArticlespiderItem
from scrapy.http import Request
import datetime

class JobboleSpider(scrapy.Spider):
    name = 'jobbole'
    allowed_domains = ['blog.jobbole.com']
    start_urls = ['http://blog.jobbole.com/all-posts/']

    def parse(self, response):
       itemList = response.xpath("//div[@id='archive']/div[starts-with(@class,'post')]")
       for item in itemList:
           article = ArticlespiderItem()
           article['url'] = item.xpath(".//a[@class='archive-title']/@href").extract_first()
           article['title'] = item.xpath(".//a[@class='archive-title']/text()").extract_first()
           article['desc'] = item.xpath(".//span[@class='excerpt']/p[1]/text()").extract_first()
           article['thumb'] = item.xpath(".//div[@class='post-thumb']//img/@src").extract_first()
           dateItems = item.xpath(".//p/text()").extract()
           if len(dateItems) >= 1:
               create_date = dateItems[1].strip().replace(" ·", "")
               try:
                  time = datetime.datetime.strptime(create_date,'%Y/%m/%d').date()
               except Exception as e:
                  time =datetime.datetime.now()
               article['date'] =time
           yield article

       next_url = response.xpath("//a[@class='next page-numbers']/@href").extract_first()
       if next_url:
          yield Request(url=next_url, callback=self.parse)

```
##创建Item
```
class ArticlespiderItem(scrapy.Item):
    # define the fields for your item here like:
    title = scrapy.Field()
    url = scrapy.Field()
    desc = scrapy.Field()
    date = scrapy.Field()
    thumb = scrapy.Field()
```
## 数据存储
```
import pymysql

class MysqlDBPipeline(object):
    def __init__(self):
        self.conn = pymysql.connect(host='localhost', user='baxiang', password='123456', port=3306,database='spider'
                                    )
        self.cursor = self.conn.cursor()
    def process_item(self, item, spider):
        sql ='INSERT INTO article(title,url,create_date,content,thumb) VALUES(%s,%s,%s,%s,%s)'
        self.cursor.execute(sql,(item['title'],item['url'],item['date'],item['desc'],item['thumb']))
        self.conn.commit()
        return item
    def spider_close(self):
        self.conn.close()
```
修改设置
```
ITEM_PIPELINES = {
   'ArticleSpider.pipelines.ArticlespiderPipeline': 300,
    'ArticleSpider.pipelines.MysqlDBPipeline': 299
}
```
##爬虫执行
```
import sys
import os
from scrapy.cmdline import execute

sys.path.append(os.path.dirname(os.path.abspath(__file__)))
execute(['scrapy', 'crawl', 'jobbole'])
```
