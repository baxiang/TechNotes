##猫眼电影TOP100榜
####爬取内容名分析
![image.png](https://upload-images.jianshu.io/upload_images/143845-82e9904d40aba2d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-5b825b22e2f61a44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
图片的实际是data-src，而不是src需要实际看一下请求数据返回的response值
```
from Toscrape.items import MaoyanItem
import scrapy

class MaoyanMovieSpider(scrapy.Spider):
    name = 'maoyanMovie'
    allowed_domains = ['maoyan.com']
    start_urls = ['http://maoyan.com/board/4']

    def parse(self, response):
          itemList = response.xpath("//dl[@class='board-wrapper']/dd")
          for item in itemList:
              movie = MaoyanItem()
              movie['index'] = item.xpath(".//i[starts-with(@class,'board-index')]/text()").extract_first()
              movie['title'] = item.xpath(".//div[@class='movie-item-info']/p[@class='name']/a[1]/text()").extract_first()
              movie['pict'] = item.xpath(".//a[@class='image-link']/img[@class='board-img']/@data-src").extract_first()
              movie['star'] = item.xpath("normalize-space(.//div[@class='movie-item-info']/p[@class='star']/text())").extract_first()
              movie['time'] = item.xpath(".//div[@class='movie-item-info']/p[@class='releasetime']/text()").extract_first()
              scoreList = item.xpath(".//div[@class='movie-item-number score-num']/p[@class='score']")
              for scroe in scoreList:
                  integer = scroe.xpath("./i[@class='integer']/text()").extract_first()
                  fraction = scroe.xpath("./i[@class='fraction']/text()").extract_first()
                  movie['score'] = integer+fraction

              yield movie

          next_url= response.xpath("//ul[@class='list-pager']//a[contains(text(),'下一页')]/@href").extract_first()
          if next_url:
              yield scrapy.Request(url=response.urljoin(next_url), callback=self.parse)
```
创建item需要获取的内容选项、
```
class MaoyanItem(scrapy.Item):
    # define the fields for your item here like:
     index = scrapy.Field()
     title = scrapy.Field()
     pict = scrapy.Field()
     time = scrapy.Field()
     star = scrapy.Field()
     score = scrapy.Field()
```
创建main.py执行爬虫任务
```

from scrapy.cmdline import execute

# 执行命令启动爬虫
execute(["scrapy", "crawl", 'maoyanMovie'])
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-e809f5361907d2db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##豆瓣电影 Top 250
####爬取内容名分析
![image.png](https://upload-images.jianshu.io/upload_images/143845-05f63749014e9392.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
<li>
            <div class="item">
                <div class="pic">
                    <em class="">1</em>
                    <a href="https://movie.douban.com/subject/1292052/">
                        <img width="100" alt="肖申克的救赎" src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p480747492.webp" class="">
                    </a>
                </div>
                <div class="info">
                    <div class="hd">
                        <a href="https://movie.douban.com/subject/1292052/" class="">
                            <span class="title">肖申克的救赎</span>
                                    <span class="title">&nbsp;/&nbsp;The Shawshank Redemption</span>
                                <span class="other">&nbsp;/&nbsp;月黑高飞(港)  /  刺激1995(台)</span>
                        </a>
                            <span class="playable">[可播放]</span>
                    </div>
                    <div class="bd">
                        <p class="">
                            导演: 弗兰克·德拉邦特 Frank Darabont&nbsp;&nbsp;&nbsp;主演: 蒂姆·罗宾斯 Tim Robbins /...<br>
                            1994&nbsp;/&nbsp;美国&nbsp;/&nbsp;犯罪 剧情
                        </p> 
                        <div class="star">
                                <span class="rating5-t"></span>
                                <span class="rating_num" property="v:average">9.6</span>
                                <span property="v:best" content="10.0"></span>
                                <span>1153984人评价</span>
                        </div>
                            <p class="quote">
                                <span class="inq">希望让人自由。</span>
                            </p>
                    </div>
                </div>
            </div>
        </li>
```
####爬取内容字段
|内容|描述|
|---|---|
|index|电影排名|
|name|电影名称|
|director|电影导演|
|starring|电影主演|
|rating|电影评分|
|evaluate|电影评分|
|pict|电影剧照|
|year|电影上映时间|
|nation|电影所属国家|
|tags|电影类型|
items.py 增加需要爬取的内容
```
class MovieItem(scrapy.Item):
    # define the fields for your item here like:
     index = scrapy.Field()
     name = scrapy.Field()
     director =scrapy.Field()
     starring = scrapy.Field()
     rating= scrapy.Field()
     evaluate = scrapy.Field()
     pict = scrapy.Field()
     year = scrapy.Field()
     nation = scrapy.Field()
     tags = scrapy.Field()
```
```
import scrapy
import re
from Toscrape.items import MovieItem

class MovietopSpider(scrapy.Spider):
    name = 'movieTop'
    allowed_domains = ['movie.douban.com']
    start_urls = ['http://movie.douban.com/top250/']

    def parse(self, response):
        movieList = response.xpath("//ol[@class='grid_view']//li")
        for item in movieList:
            movie = MovieItem()
            movie['index'] = item.xpath(".//div[@class='pic']//em/text()").extract_first()
            movie['name'] = item.xpath(".//div[@class='hd']/a/span[@class='title'][1]/text()").extract_first()
            movie['rating'] = item.xpath(".//div[@class='star']/span[@class='rating_num']/text()").extract_first()
            movie['evaluate'] = item.xpath(".//p[@class='quote']/span[@class='inq']/text()").extract_first()
            movie['pict'] = item.xpath(".//div[@class='pic']/a[1]/img[1]/@src").extract_first()
            list = item.xpath(".//div[@class='bd']/p[1]/text()").extract()
            for content in list:
                location = re.search(r'导演:(.*?)\s{3}(.*)...', content)
                separator = re.search(r'(\d{4}.*\)?\s)/(.*?)/(.*)', content)
                if location:
                    movie['director'] = "".join(location.group(1).split())
                    movie['starring'] = "".join(location.group(2).split())[3:]
                elif separator:
                    movie['year'] = separator.group(1).strip()
                    movie['nation'] = separator.group(2).strip()
                    movie['tags'] = separator.group(3).strip()
                else:
                    movie['director'] = "".join(content.split())
            yield movie

        next_url = response.xpath('//span[@class="next"]/link/@href').extract_first()
        if next_url:
            yield scrapy.Request(url=response.urljoin(next_url), callback=self.parse)
```
main.py文件执行爬虫
```
from scrapy.cmdline import execute
# 执行命令启动爬虫
execute(["scrapy", "crawl", 'movieTop'])
```
![image.png](https://upload-images.jianshu.io/upload_images/143845-8b4b9fcc27b86a8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
