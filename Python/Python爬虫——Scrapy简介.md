##Scrapy模块
Scrapy Engine（引擎）：Scrapy框架的核心部分。负责在Spider和ItemPipeline、Downloader、Scheduler中间通信、传递数据等。
Spider（爬虫）：发送需要爬取的链接给引擎，最后引擎把其他模块请求回来的数据再发送给爬虫，爬虫就去解析想要的数据。这个部分是我们开发者自己写的，因为要爬取哪些链接，页面中的哪些数据是我们需要的，都是由程序员自己决定。
Scheduler（调度器）：负责接收引擎发送过来的请求，并按照一定的方式进行排列和整理，负责调度请求的顺序等。
Downloader（下载器）：负责接收引擎传过来的下载请求，然后去网络上下载对应的数据再交还给引擎。
Item Pipeline（管道）：负责将Spider（爬虫）传递过来的数据进行保存。具体保存在哪里，应该看开发者自己的需求。
Downloader Middlewares（下载中间件）：可以扩展下载器和引擎之间通信功能的中间件。
Spider Middlewares（Spider中间件）：可以扩展引擎和爬虫之间通信功能的中间件。
## 安装环境
####macOS 环境
需要安装c语言的编译环境
```
xcode-select --install
```
安装Scrapy
```
pip3 install Scrapy
```
## 创建项目
scrapy startproject xxx(项目名称)
```
scrapy startproject firstProject
New Scrapy project 'firstProject', using template directory '/usr/local/lib/python3.6/site-packages/scrapy/templates/project', created in:
    /Users/baxiang/Documents/Python/Scrapy/firstProject

You can start your first spider with:
    cd firstProject
    scrapy genspider example example.com
```
## 项目结构
```
.
├── firstProject
│   ├── __init__.py
│   ├── __pycache__
│   ├── items.py
│   ├── middlewares.py
│   ├── pipelines.py
│   ├── settings.py
│   └── spiders
│       ├── __init__.py
│       └── __pycache__
└── scrapy.cfg
```
items.py：用来存放爬虫爬取下来数据的模型。
middlewares.py：用来存放各种中间件的文件。
pipelines.py：用来将items的模型存储到本地磁盘中。
settings.py：爬虫的一些配置信息（比如请求头、多久发送一次请求、ip代理池等）。
scrapy.cfg：项目的配置文件
spiders包：所有的爬虫代码都存放到这个里面

####Items.py
定义需要抓取并需要后期处理的数据
####settings.py
文件设置scapy,其中两个地方是建议设置的。
ROBOTSTXT_OBEY设置为False。默认是True。即遵守机器协议，那么在爬虫的时候，scrapy首先去找robots.txt文件，如果没有找到。则直接停止爬取。
DEFAULT_REQUEST_HEADERS添加User-Agent。这个也是告诉服务器，我这个请求是一个正常的请求，不是一个爬虫。
#### pipeline.py
用于存放后期数据处理的功能。
## 常用命令
```
$ scrapy -h
Scrapy 1.5.0 - project: firstProject

Usage:
  scrapy <command> [options] [args]

Available commands:
  bench         Run quick benchmark test
  check         Check spider contracts
  crawl         Run a spider
  edit          Edit spider
  fetch         Fetch a URL using the Scrapy downloader
  genspider     Generate new spider using pre-defined templates
  list          List available spiders
  parse         Parse URL (using its spider) and print the results
  runspider     Run a self-contained spider (without creating a project)
  settings      Get settings values
  shell         Interactive scraping console
  startproject  Create new project
  version       Print Scrapy version
  view          Open URL in browser, as seen by Scrapy

Use "scrapy <command> -h" to see more info about a command

```

## 创建爬虫
#### 创建爬虫工程
```
scrapy startproject Toscrape
```
#### 创建爬虫文件
```
scrapy genspider news www.163.com
```
创建news.py文件内容
```
class NewsSpider(scrapy.Spider):
    name = 'news'
    allowed_domains = ['www.163.com']
    start_urls = ['http://www.163.com/']
    def parse(self, response):
        pass

```
name：这个爬虫的名字，名字必须是唯一的。
allow_domains：允许的域名。爬虫只会爬取这个域名下的网页，其他不是这个域名下的网页会被自动忽略。
start_urls：爬虫从这个变量中的url开始，第一次下载的数据将会从这些urls开始。
parse：引擎会把下载器下载回来的数据扔给爬虫解析，爬虫再把数据传给这个parse方法。这个是个固定的写法。这个方法的作用有两个，第一个是提取想要的数据。第二个是生成下一个请求的url。
