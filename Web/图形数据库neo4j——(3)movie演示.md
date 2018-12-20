##创建电影标签
![image.png](https://upload-images.jianshu.io/upload_images/143845-067a644292037404.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

电影属性包括电影名称，上映时间，宣传语。创建黑客帝国1-3部的节点

```
CREATE (TheMatrix:Movie {title:'The Matrix', released:1999, tagline:'Welcome to the Real World'})
CREATE (TheMatrixReloaded:Movie {title:'The Matrix Reloaded', released:2003, tagline:'Free your mind'})
CREATE (TheMatrixRevolutions:Movie {title:'The Matrix Revolutions', released:2003, tagline:'Everything that has a beginning has an end'})
```
##创建人物信息
![image.png](https://upload-images.jianshu.io/upload_images/143845-3e8b0efda5bbf63f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

创建的电影的演员，导演，制片人。属性包括姓名和出身年份。
```
CREATE (Keanu:Person {name:'Keanu Reeves', born:1964})
CREATE (Carrie:Person {name:'Carrie-Anne Moss', born:1967})
CREATE (Laurence:Person {name:'Laurence Fishburne', born:1961})
CREATE (Hugo:Person {name:'Hugo Weaving', born:1960})
CREATE (LillyW:Person {name:'Lilly Wachowski', born:1967})
CREATE (LanaW:Person {name:'Lana Wachowski', born:1965})
CREATE (JoelS:Person {name:'Joel Silver', born:1952})
```
##创建关系
演员的饰演关系ACTED_IN 其中包括角色名称属性,导演关系DIRECTED 制片关系PRODUCED 编剧 WROTE
```
CREATE
  (Keanu)-[:ACTED_IN {roles:['Neo']}]->(TheMatrix),
  (Carrie)-[:ACTED_IN {roles:['Trinity']}]->(TheMatrix),
  (Laurence)-[:ACTED_IN {roles:['Morpheus']}]->(TheMatrix),
  (Hugo)-[:ACTED_IN {roles:['Agent Smith']}]->(TheMatrix),
  (LillyW)-[:DIRECTED]->(TheMatrix),
  (LanaW)-[:DIRECTED]->(TheMatrix),
  (JoelS)-[:PRODUCED]->(TheMatrix)
```
##查询
查询汤姆汉克斯
```
MATCH (tom {name: "Tom Hanks"}) RETURN tom
```
查询电影名称云图
![image.png](https://upload-images.jianshu.io/upload_images/143845-bbd9d45326fbbb75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-32404cc2f5261e17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-73347c4e18957fa2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
CREATE (CloudAtlas:Movie {title:'Cloud Atlas', released:2012, tagline:'Everything is connected'})
CREATE (HalleB:Person {name:'Halle Berry', born:1966})
CREATE (JimB:Person {name:'Jim Broadbent', born:1949})
CREATE (TomT:Person {name:'Tom Tykwer', born:1965})
CREATE (DavidMitchell:Person {name:'David Mitchell', born:1969})
CREATE (StefanArndt:Person {name:'Stefan Arndt', born:1961})
CREATE
  (TomH)-[:ACTED_IN {roles:['Zachry', 'Dr. Henry Goose', 'Isaac Sachs', 'Dermot Hoggins']}]->(CloudAtlas),
  (Hugo)-[:ACTED_IN {roles:['Bill Smoke', 'Haskell Moore', 'Tadeusz Kesselring', 'Nurse Noakes', 'Boardman Mephi', 'Old Georgie']}]->(CloudAtlas),
  (HalleB)-[:ACTED_IN {roles:['Luisa Rey', 'Jocasta Ayrs', 'Ovid', 'Meronym']}]->(CloudAtlas),
  (JimB)-[:ACTED_IN {roles:['Vyvyan Ayrs', 'Captain Molyneux', 'Timothy Cavendish']}]->(CloudAtlas),
  (TomT)-[:DIRECTED]->(CloudAtlas),
  (LillyW)-[:DIRECTED]->(CloudAtlas),
  (LanaW)-[:DIRECTED]->(CloudAtlas),
  (DavidMitchell)-[:WROTE]->(CloudAtlas),
  (StefanArndt)-[:PRODUCED]->(CloudAtlas)
```
```
MATCH (cloudAtlas {title: "Cloud Atlas"}) RETURN cloudAtlas
```
查找演员,限制数量为10，返回演员的姓名
```
MATCH (people:Person) RETURN people.name LIMIT 10
```
查询90年代上映的电影
```
MATCH (nineties:Movie) WHERE nineties.released >= 1990 AND nineties.released < 2000 RETURN nineties.title;
```
查询汤姆汉克斯饰演的电影
```
MATCH (tom:Person {name: "Tom Hanks"})-[:ACTED_IN]->(tomHanksMovies) RETURN tom,tomHanksMovies
```
查询和汤姆汉克斯合作过的演员姓名
```
MATCH (tom:Person {name:"Tom Hanks"})-[:ACTED_IN]->(m)<-[:ACTED_IN]-(coActors) RETURN coActors.name
```
查询云图的所有关系 返回人物名称 和关系属性内容
```
MATCH (people:Person)-[relatedTo]-(:Movie {title: "Cloud Atlas"}) RETURN people.name, Type(relatedTo), relatedTo
```
查询凯文·贝肯1到3层树形关系
![image.png](https://upload-images.jianshu.io/upload_images/143845-29e4435c44d77999.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
MATCH (bacon:Person {name:"Kevin Bacon"})-[*1..3]-(hollywood)
RETURN DISTINCT hollywood

```
查询只是和汤姆汉克斯合作过的演员合作过电影的但是还没有和汤姆汉克斯合作过的演员名称
```
MATCH (tom:Person {name:"Tom Hanks"})-[:ACTED_IN]->(m)<-[:ACTED_IN]-(coActors),
      (coActors)-[:ACTED_IN]->(m2)<-[:ACTED_IN]-(cocoActors)
WHERE NOT (tom)-[:ACTED_IN]->()<-[:ACTED_IN]-(cocoActors) AND tom <> cocoActors
RETURN cocoActors.name AS Recommended, count(*) AS Strength ORDER BY Strength DESC
```

查询汤姆·汉克斯和汤姆·克鲁斯都合作过的演员和电影
```
MATCH (tom:Person {name:"Tom Hanks"})-[:ACTED_IN]->(m)<-[:ACTED_IN]-(coActors),
      (coActors)-[:ACTED_IN]->(m2)<-[:ACTED_IN]-(cruise:Person {name:"Tom Cruise"})
RETURN tom, m, coActors, m2, cruise
```
