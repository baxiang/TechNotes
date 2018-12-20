Mapping是ES中的一个很重要的内容，它类似于传统关系型数据中表结构定义，用于定义一个索引（index）的某个类型（type）的数据的结构.在ES中，我们无需手动创建type（相当于table）和mapping(相关与schema)。在默认配置下，ES可以根据插入的数据自动地创建type及其mapping。
mapping中主要包括字段名、字段数据类型和字段索引类型这3个方面的定义。
####定义index下字段名(Field Name)
与传统数据库字段名作用一样，就是给字段起个唯一的名字，好让系统和用户能识别。
Mapping中的字段类型一旦设定后，禁止直接修改，原因是Lucene实现的倒排索引生成后是不允许修改的，如果需要修改的，只能重新建立新的索引，然后做reindex操作。
```
PUT test_index
{
  "mappings": {
    "doc":{
      "dynamic": false,
      "properties": {
        "title":{
          "type": "text"
        },
        "name":{
          "type": "keyword"
        },
        "age":{
          "type": "integer"
        }
      }
    }
  }
}
```
获取当前mapping
```
GET test_index/_mapping
```
增加字段
通过dynamic参数来控制字段的新增
true(默认)允许自动新增字段,false不允许自动新增字段，但是文档可以直接写入，但是无法对字段进行查询等操作。strict 文档不能写入数据
```
PUT test_index/doc/1
{
  "title":"test-title",
  "gender":"male"
}
POST test_index/_search
{
  "query": {
    "match": {
      "gender": "male"
    }
  }
}
```
##数据类型
定义该字段保存的数据的类型，不符合数据类型定义的数据不能保存到ES中。下表列出的是ES中所支持的数据类型。（大类是对所有类型的一种归类，小类是实际使用的类型。）
|大类|	包含的小类|
|---|---|
|String|	string|
|Whole number|	byte, short, integer, long|
|Floating point|	float, double|
|Boolean	|boolean|
|Date	|date|
复杂数据类型
数组类型，对象类型，嵌套类型
地理位置数据类型
geo_point
专用类型
记录ip地址ip
实现自动补全completion
记录分词数token_count
记录字符串hash值murmur3
多字段特性multi-fileds
允许对一个字段采用不同的配置，姓名的实现拼音搜索，只需要增加姓名一个子字段为pinyin即可。

####字段类型
索引是ES中的核心，ES之所以能够实现实时搜索，完全归功于Lucene这个优秀的Java开源索引。在传统数据库中，如果字段上建立索引，我们仍然能够以它作为查询条件进行查询，只不过查询速度慢点。而在ES中，字段如果不建立索引，则就不能以这个字段作为查询条件来搜索。也就是说，不建立索引的字段仅仅能起到数据载体的作用。string类型的数据肯定是日常使用得最多的数据类型，下面介绍mapping中string类型字段可以配置的索引类型。
|索引类型|解释|
|----|----|
|analyzed|	首先分析这个字符串，然后再建立索引。换言之，以全文形式索引此字段。|
|not_analyzed	|索引这个字段，使之可以被搜索，但是索引内容和指定值一样。不分析此字段。|
|no	|不索引这个字段。这个字段不能被搜索到。|
如果索引类型设置为analyzed，在表示ES会先对这个字段进行分析（一般来说，就是自然语言中的分词），ES内置了不少分析器（analyser），如果觉得它们对中文的支持不好，也可以使用第三方分析器。由于笔者在实际项目中仅仅将ES用作普通的数据查询引擎，所以并没有研究过这些分析器。如果将ES当做真正的搜索引擎，那么挑选正确的分析器是至关重要的。
copy_to将改字段复制到目标字段，实现类似_all的作用，不会出现在_source中，只用来搜索。
```
PUT test_index
{
  "mappings": {
    "test_type":{
      "properties": {
        "first_name":{
          "type": "text",
          "copy_to": "full_name"
        },
        "last_name":{
          "type": "text",
          "copy_to": "full_name"
        },
        "full_name":{
          "type": "text"
        }
      }
    }
  }
}
```
插入数据和查询数据
```
PUT test_index/test_type/1
{
  "first_name":"tom",
  "last_name":"hanks"
}


POST test_index/_search
{
  "query": {
    "match": {
      "full_name": {
        "query": "tom hanks",
        "operator": "and"
      }
    }
  }
}
```
index控制当前字段是否索引，默认为true,记录索引，false表示不可以搜索。
```
PUT test_index
{
  "mappings": {
    "test_type":{
      "properties": {
        "name":{
          "type": "text",
          "index": false
        }
      }
    }
  }
}
```
查询的时候会报错
```
PUT test_index/test_type/1
{
  "name":"tom"
}
POST test_index/_search
{
  "query": {
    "match": {
      "name": "tom"
    }
  }
}
```
会报错，告知当前字段没有创建索引
```
"caused_by": {
            "type": "illegal_argument_exception",
            "reason": "Cannot search on field [name] since it is not indexed."
          }
```
index_options用于控制倒排索记录的内容
|配置|记录描述|
|--|---|
|docs|只记录doc id|
|freqs|记录doc id和term frequencies|
|positions|记录doc id,term frequencies,term positon|
|offsets|记录doc id,term frequencies,term positon,character offsets|
text类型默认配置为position，其他默认为docs，记录越多，占用空间越大。
null_value 当字段遇到null值处理策略，默认为null,空值，此事es会忽律改值，可以通过设置字段的默认值。
####Dynamic Mapping
es可以自动识别文档字段类型，从而降低用户使用成本,es是依靠JSON文档的字段类型来实现自动识别字段类型。
|JSON类型|ES类型|
---|----
null|忽略|
boolean|boolean|
浮点类型|float|
整数|long
object|object
date|2018-10-10
array|有第一个null值的类型决定
string|匹配为日期则设为date类型（默认开启)，设为text类型，并附带keyword的子字段

####Dynamic Templates
允许根据es自动识别的数据类型，字段名来动态设定字段类型。
match_mapping_type 匹配es自动识别的字段类型。
match,unmatch 匹配
####自定义Mapping的建议
1写入一条文档到es的临时索引中，获取es自动生成的mapping
2 通过_mapping获取配置，修改步骤1得到的mapping,自定义相关配置
3 使用步骤2的mapping的创建实际所需索引
####索引模板
索引模板，英文为Index Template,主要用于在新建索引的时自动应用的先设定的配置
```
{
  "blog": {
    "mappings": {
      "paper": {
        "properties": {
          "date": {
            "type": "date"
          },
          "pID": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "publish": {
            "type": "boolean"
          },
          "uID": {
            "type": "long"
          }
        }
      }
    }
  }
}
```
ElasticSearch 5.0以后，string类型有重大变更，移除了string类型，string字段被拆分成两种新的数据类型: text用于全文搜索的,而keyword用于关键词搜索。
ElasticSearch字符串会默认生成两次索引，一次是自己的text索引，会被分词放入倒排索引中，一个是总长度是256个字符的keyword索引，直接将最长为256的字符串放入倒排索引。将会自动创建下面的动态映射(dynamic mappings):
```
{
    "class_code": {
        "type": "text",
        "fields": {
            "keyword": {
                "type": "keyword",
                "ignore_above": 256
            }
        }
    }
}
```
这就是造成部分字段还会自动生成一个与之对应的“filed名称.(点号)keyword查询
|类型|区别|
|----|----|
|Text|会分词，然后进行索引支持模糊、精确查询不支持聚合|
|keyword|不进行分词，直接索引 支持模糊、精确查询 支持聚合|


