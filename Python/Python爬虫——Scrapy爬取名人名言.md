[toscrape](http://quotes.toscrape.com/)是一个名人名言的网站
![image.png](https://upload-images.jianshu.io/upload_images/143845-b58c8f1fc814221b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一条名人名言的结构如下
```
<div class="quote" itemscope="" itemtype="http://schema.org/CreativeWork">
        <span class="text" itemprop="text">“I have not failed. I've just found 10,000 ways that won't work.”</span>
        <span>by <small class="author" itemprop="author">Thomas A. Edison</small>
        <a href="/author/Thomas-A-Edison">(about)</a>
        </span>
        <div class="tags">
            Tags:
            <meta class="keywords" itemprop="keywords" content="edison,failure,inspirational,paraphrased"> 
            
            <a class="tag" href="/tag/edison/page/1/">edison</a>
            
            <a class="tag" href="/tag/failure/page/1/">failure</a>
            
            <a class="tag" href="/tag/inspirational/page/1/">inspirational</a>
            
            <a class="tag" href="/tag/paraphrased/page/1/">paraphrased</a>
            
        </div>
    </div>
```
下一页
![image.png](https://upload-images.jianshu.io/upload_images/143845-72fa4d623e749da1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
<ul class="pager">
            
            
            <li class="next">
                <a href="/page/2/">Next <span aria-hidden="true">→</span></a>
            </li>
            
        </ul>
```
quotes.py 使用css 选择器实现
```
from tutorial.items import TutorialItem

class QuotesSpider(scrapy.Spider):
    name = 'quotes'
    allowed_domains = ['quotes.toscrape.com']
    start_urls = ['http://quotes.toscrape.com/']

    def parse(self, response):
        quotes = response.css('.quote')
        for quote in quotes:
            item = TutorialItem()
            item['text'] = quote.css('.text::text').extract_first()
            item['author'] = quote.css('.author::text').extract_first()
            item['tags'] = quote.css('.tags .tag::text').extract()
            yield item
        next = response.css('.pager .next a::attr("href")').extract_first()
        url = response.urljoin(next)
        yield scrapy.Request(url=url, callback=self.parse)
```
quotes.py 使用xpath 实现
```
class QuotesSpider(scrapy.Spider):
    name = 'quotes'
    allowed_domains = ['quotes.toscrape.com']
    start_urls = ['http://quotes.toscrape.com/']

    def parse(self, response):
        quotes = response.xpath('//div[@class="quote"]')
        for quote in quotes:
            item = TutorialItem()
            item['text'] = quote.xpath('span[@class="text"]/text()').extract_first()
            item['author'] = quote.xpath('span/small[@class="author"]/text()').extract_first()
            item['tags'] = quote.xpath('div[@class="tags"]/a[@class="tag"]/text()').extract()
            yield item
        next = response.xpath('//li[@class="next"]/a/@href').extract_first()
        url = response.urljoin(next)
        yield scrapy.Request(url=url, callback=self.parse)
```


items.py
```
class TutorialItem(scrapy.Item):

    text = scrapy.Field()
    author = scrapy.Field()
    tags = scrapy.Field()

```
