##正排索引
目录页对应的正排索引
文档Id到文档内容、单词的关联关系。
|ID|描述|
|---|-----|
|1|Java是世界排名第一的语言|
|2|Docker是流行的容器技术|
|3|PHP是世界上最好的语言|
##Inverted Index倒排索引
索引页对应的倒排索引
单词到索引的关联
倒排索引是搜索引擎的核心，主要包含两部分
单词词典（Term Dictionary）
记录所有文档的单词，一般比较大，记录单词到到倒排列表的关联信息
倒排列表（Posting List）
##Analyzer分词
全文搜索引擎会用某种算法对要建索引的文档进行分析， 从文档中提取出若干Token(词元)， 这些算法称为Tokenizer(分词器)；这些Token会被进一步处理， 比如转成小写等， 这些处理算法被称为Token Filter(词元处理器), 被处理后的结果被称为Term(词)， 文档中包含了几个这样的Term被称为Frequency(词频)。 引擎会建立Term和原文档的Inverted Index(倒排索引)， 这样就能根据Term很快到找到源文档了。 文本被Tokenizer处理前可能要做一些预处理， 比如去掉里面的HTML标记， 这些处理的算法被称为Character Filter(字符过滤器)， 这整个的分析算法被称为Analyzer(分析器)。

##Analyze API
```
POST _analyze
{
  "analyzer": "standard", 
  "text": "hello world"
}
```
分词结果
```
{
  "tokens": [
    {
      "token": "hello",
      "start_offset": 0,
      "end_offset": 5,
      "type": "<ALPHANUM>",
      "position": 0
    },
    {
      "token": "world",
      "start_offset": 6,
      "end_offset": 11,
      "type": "<ALPHANUM>",
      "position": 1
    }
  ]
}
````
指定索引的字段进行测试
```
POST /_analyze
{
  "tokenizer": "standard", 
  "filter": ["lowercase"], 
  "text": "Hello World"
}
```
##自带的分词器
####standard Analyzer
stop word
####Simple Analyzer
#### Whitespace Analyzer
####Stop Analyzer
stop word 指语气助词等修饰词性的词语，the,an,的，这，那
####keyword Anayyzer
####Pattern
####Language
##中文分词
```
POST /_analyze
{
  "text":"中华人民共和国国歌"
}
```
只会将中文语句独立分拆为一个个独立的字，这显然不符合我们的预期。这是因为Es默认的是英文分词器需要为其配置中文分词器
```
{
  "tokens": [
    {
      "token": "中",
      "start_offset": 0,
      "end_offset": 1,
      "type": "<IDEOGRAPHIC>",
      "position": 0
    },
    {
      "token": "华",
      "start_offset": 1,
      "end_offset": 2,
      "type": "<IDEOGRAPHIC>",
      "position": 1
    },
    {
      "token": "人",
      "start_offset": 2,
      "end_offset": 3,
      "type": "<IDEOGRAPHIC>",
      "position": 2
    },
    {
      "token": "民",
      "start_offset": 3,
      "end_offset": 4,
      "type": "<IDEOGRAPHIC>",
      "position": 3
    },
    {
      "token": "共",
      "start_offset": 4,
      "end_offset": 5,
      "type": "<IDEOGRAPHIC>",
      "position": 4
    },
    {
      "token": "和",
      "start_offset": 5,
      "end_offset": 6,
      "type": "<IDEOGRAPHIC>",
      "position": 5
    },
    {
      "token": "国",
      "start_offset": 6,
      "end_offset": 7,
      "type": "<IDEOGRAPHIC>",
      "position": 6
    },
    {
      "token": "国",
      "start_offset": 7,
      "end_offset": 8,
      "type": "<IDEOGRAPHIC>",
      "position": 7
    },
    {
      "token": "歌",
      "start_offset": 8,
      "end_offset": 9,
      "type": "<IDEOGRAPHIC>",
      "position": 8
    }
  ]
}
```
####IK
![image.png](https://upload-images.jianshu.io/upload_images/143845-732ccdef37f326d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
./elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.2.4/elasticsearch-analysis-ik-6.2.4.zip
-> Downloading https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.2.4/elasticsearch-analysis-ik-6.2.4.zip
[=================================================] 100%
-> Installed analysis-ik
$ ps -ef |grep elasticsearch
$ kill -9 1051 #杀死elasticsearch进程
$ elasticsearch
```
```
{
   "text":"今天北京天气","analyzer": "ik_max_word"
}
```
```
{
    "tokens": [
        {
            "token": "今天",
            "start_offset": 0,
            "end_offset": 2,
            "type": "CN_WORD",
            "position": 0
        },
        {
            "token": "北京",
            "start_offset": 2,
            "end_offset": 4,
            "type": "CN_WORD",
            "position": 1
        },
        {
            "token": "天气",
            "start_offset": 4,
            "end_offset": 6,
            "type": "CN_WORD",
            "position": 2
        }
    ]
}
```
IK Analysis 类型
IK Analysis主要有两种类型的分词形式，分别是 ik_max_word 和 ik_smart。
|分词形式|拆分粒度|
--|---|
ik_smart:|会做最粗粒度的拆分|
ik_max_word:|会将文本做最细粒度的拆分|
```
POST movie/_analyze
{
  "text":"中华人民共和国国歌", "analyzer": "ik_smart"
}
```
ik_max_word 返回的结果
```
{
  "tokens": [
    {
      "token": "中华人民共和国",
      "start_offset": 0,
      "end_offset": 7,
      "type": "CN_WORD",
      "position": 0
    },
    {
      "token": "国歌",
      "start_offset": 7,
      "end_offset": 9,
      "type": "CN_WORD",
      "position": 1
    }
  ]
}
```
```
POST movie/_analyze
{
  "text":"中华人民共和国国歌", "analyzer": "ik_max_word"
}
```
返回的结果
```
{
  "tokens": [
    {
      "token": "中华人民共和国",
      "start_offset": 0,
      "end_offset": 7,
      "type": "CN_WORD",
      "position": 0
    },
    {
      "token": "中华人民",
      "start_offset": 0,
      "end_offset": 4,
      "type": "CN_WORD",
      "position": 1
    },
    {
      "token": "中华",
      "start_offset": 0,
      "end_offset": 2,
      "type": "CN_WORD",
      "position": 2
    },
    {
      "token": "华人",
      "start_offset": 1,
      "end_offset": 3,
      "type": "CN_WORD",
      "position": 3
    },
    {
      "token": "人民共和国",
      "start_offset": 2,
      "end_offset": 7,
      "type": "CN_WORD",
      "position": 4
    },
    {
      "token": "人民",
      "start_offset": 2,
      "end_offset": 4,
      "type": "CN_WORD",
      "position": 5
    },
    {
      "token": "共和国",
      "start_offset": 4,
      "end_offset": 7,
      "type": "CN_WORD",
      "position": 6
    },
    {
      "token": "共和",
      "start_offset": 4,
      "end_offset": 6,
      "type": "CN_WORD",
      "position": 7
    },
    {
      "token": "国",
      "start_offset": 6,
      "end_offset": 7,
      "type": "CN_CHAR",
      "position": 8
    },
    {
      "token": "国歌",
      "start_offset": 7,
      "end_offset": 9,
      "type": "CN_WORD",
      "position": 9
    }
  ]
}
```
####jieba
##自然语言处理的分词系统
####hanlp
####THULAC
##自定义分词
CharacterFilter
Tokenizer
TokenFilter
##自定义分词的api
##分词的使用
1创建或者更新文档（index time)会对响应的文档进行分词处理。
索引时分词是通过配置index mapping中每个字段的analyzer属性实现的，不知道分词，默认standard
2 查询时（search Time）会对查询语句进行分词
查询是时候通过analyzer指定分词器
通过index mapping设置search_analyzer实现
明确字段是否需要分词，不需要分词的字段就将type设置为keyword,可以节省空间和提高写性能。







