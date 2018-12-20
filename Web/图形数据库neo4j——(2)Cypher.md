##基本语法
![image.png](https://upload-images.jianshu.io/upload_images/143845-c5398dc2311b7c9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[以上图片来源](http://codingisforeveryone.com.au/wp-content/uploads/2018/07/Neo4j-Cypher-Quick-Reference-v2018-PART-1.pdf)，非常感谢俞方桦博士提供的介绍Neo4j的资源

![image.png](https://upload-images.jianshu.io/upload_images/143845-f2fe1d6cc33aa58a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[以上图片来源](http://codingisforeveryone.com.au/wp-content/uploads/2018/07/Neo4j-Cypher-Quick-Reference-v2018-PART-1.pdf)，非常感谢俞方桦博士提供的介绍Neo4j的资源

![image.png](https://upload-images.jianshu.io/upload_images/143845-4b12eb450aa8af00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[以上图片来源](http://codingisforeveryone.com.au/wp-content/uploads/2018/07/Neo4j-Cypher-Quick-Reference-v2018-PART-1.pdf)，非常感谢俞方桦博士提供的介绍Neo4j的资源
![image.png](https://upload-images.jianshu.io/upload_images/143845-0bb42f4d17b3bee4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[以上图片来源](http://codingisforeveryone.com.au/wp-content/uploads/2018/07/Neo4j-Cypher-Quick-Reference-v2018-PART-1.pdf)，非常感谢俞方桦博士提供的介绍Neo4j的资源
![image.png](https://upload-images.jianshu.io/upload_images/143845-e12ebe06bb7741b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[以上图片来源](http://codingisforeveryone.com.au/wp-content/uploads/2018/07/Neo4j-Cypher-Quick-Reference-v2018-PART-1.pdf)，非常感谢俞方桦博士提供的介绍Neo4j的资源
##CREATE
```
CREATE (
   <node-name>:<label-name>
   { 	
      <Property1-name>:<Property1-Value>
      ........
      <Propertyn-name>:<Propertyn-Value>
   }
)
```
语法说明
|语法元素|说明|
|--|--|
| CREATE |创建节点命令|
| <node-name> |创建节点命令|
| <label-name> |创建节点命令|
| <Property1-name>...<Propertyn-name> |属性是键值对。 定义将分配给创建节点的属性的名称|
####创建单个节点
p是变量 Person是标签 {}里的是属性
```
CREATE(p:Person{name:"zhangsan",nation:"CHINA",age:22})
```
创建多个标签
```
CREATE(m:Movie:电影)
```
####创建多个节点
在每个节点之间使用逗号隔开
```
CREATE(ls:Person{name:"lisi",age:22,nation:"CHINA"}),(ww:Person{name:"wangwu",age:25,nation:"CHINA"})
```
创建节点关系
```
CREATE (p1:Profile1)-[r1:LIKES]->(p2:Profile2)
```
```
CREATE (a:Person{name:"zhaoliu"}),(b:Person{name:"cuihua"}),(a)-[:LIKES{id:1}]->(b)
```
只创建关系
```
MATCH(a:Person{name:"Anna"}),(b:Person{name:"Dani"}) CREATE (a)-[:KNOWS]->(b)
```
##MERGE
查找不存在则创建
```
MERGE(n:Person{name:"Anna"})RETURN n
```
##MATCH
![image.png](https://upload-images.jianshu.io/upload_images/143845-9543fd89293a5538.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```
MATCH 
(
   <node-name>:<label-name>
)
```
|语法元素	|描述|
|-|-|
|<node-name>|	这是我们要创建一个节点名称|
|<label-name>|	这是一个节点的标签名称|
为节点增加或者修改属性值
```
MATCH(a:Person{name:"Anna"}) SET a.age = 25 RETURN a
```
##RETURN
```
RETURN 
   <node-name>.<property1-name>,
   ........
   <node-name>.<propertyn-name>
```
|语法元素|描述|
|-|-|
|<node-name>	|它是我们将要创建的节点名称|
|<Property1-name>...<Propertyn-name>|	属性是键值对。 <Property-name>定义要分配给创建节点的属性的名称|
删除节点或者关系的属性
##DELETE删除节点和关系
```
MATCH(p:Person) WHERE p.name="zhangsan" DELETE p
```
删除关系
```
MATCH(p:Person)-[r:KNOWS]->() WHERE p.name="Anna" DELETE r
```
删除所有的节点和关系
```
MATCH(m:Movie)DETACH DELETE m
```

##REMOVE删除属性、标签 
```
MATCH(a:Person) REMOVE a.age RETURN a
```
删除标签
```
MATCH(m:Movie:电影) REMOVE m:电影 RETURN m
```
##INDEX索引
创建索引
```
CREATE INDEX ON:Custom(name)
```
 删除索引
```
DROP INDEX ON:Custom(name)
```
