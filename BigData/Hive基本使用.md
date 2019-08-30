Hive有三种复杂数据类型ARRAY、MAP 和 STRUCT。ARRAY和MAP与Java中的Array和Map类似，而STRUCT与C语言中的Struct类似，它封装了一个命名字段集合，复杂数据类型允许任意层次的嵌套。
创建数据表
```
create table test(
name string,
friends array<string>,
children map<string, int>,
address struct<street:string, city:string>
)
row format delimited fields terminated by ','
collection items terminated by '_'
map keys terminated by ':'
lines terminated by '\n';
```
查看数据结构
```
hive (default)> desc test;
OK
col_name	data_type	comment
name                	string              	                    
friends             	array<string>       	                    
children            	map<string,int>     	                    
address             	struct<street:string,city:string>	                    
Time taken: 0.053 seconds, Fetched: 4 row(s)

```
测试数据
```
liming,zhangsan_lisi,xiao ming:12_xiaoxiao ming:3,haidian_beijing
wangwu,zhaoliu_sunba_qianer,xiao wang:18_xiaoxiao wang:9,chao yang_beijing

```
加载测试数据
```
hive (default)> load data local inpath '/opt/module/data/people.txt' into table test;
Loading data to table default.test
Table default.test stats: [numFiles=1, numRows=0, totalSize=141, rawDataSize=0]
OK
Time taken: 0.354 seconds
```
查看数据内容
```
hive (default)> select *from test;
OK
test.name	test.friends	test.children	test.address
liming	["zhangsan","lisi"]	{"xiao ming":12,"xiaoxiao ming":3}	{"street":"haidian","city":"beijing"}
wangwu	["zhaoliu","sunba","qianer"]	{"xiao wang":18,"xiaoxiao wang":9}	{"street":"chao yang","city":"beijing"}
Time taken: 0.069 seconds, Fetched: 2 row(s)
```
##Mysql 查看hive表结构
```
mysql root@localhost:(none)> use hive;
You are now connected to database "hive" as user "root"
Time: 0.001s
mysql root@localhost:hive> select *from `TBLS`;
+--------+-------------+-------+------------------+-------+-----------+-------+------------+---------------+--------------------+--------------------+
| TBL_ID | CREATE_TIME | DB_ID | LAST_ACCESS_TIME | OWNER | RETENTION | SD_ID | TBL_NAME   | TBL_TYPE      | VIEW_EXPANDED_TEXT | VIEW_ORIGINAL_TEXT |
+--------+-------------+-------+------------------+-------+-----------+-------+------------+---------------+--------------------+--------------------+
| 1      | 1566392699  | 6     | 0                | root  | 0         | 1     | docs       | MANAGED_TABLE | <null>             | <null>             |
| 2      | 1566392821  | 6     | 0                | root  | 0         | 2     | word_count | MANAGED_TABLE | <null>             | <null>             |
+--------+-------------+-------+------------------+-------+-----------+-------+------------+---------------+--------------------+--------------------+

2 rows in set
Time: 0.012s
```
查看表的字段
```
mysql root@localhost:hive> select *from `COLUMNS_V2`
+-------+---------+-------------+-----------+-------------+
| CD_ID | COMMENT | COLUMN_NAME | TYPE_NAME | INTEGER_IDX |
+-------+---------+-------------+-----------+-------------+
| 1     | <null>  | line        | string    | 0           |
| 2     | <null>  | count       | bigint    | 1           |
| 2     | <null>  | word        | string    | 0           |
+-------+---------+-------------+-----------+-------------+

3 rows in set
Time: 0.012s
```
##加载数据到hive中
LOAD DATA LOCAL（本地文件需要添加LOCAL） INPATH '文件路径' OVERWRITE INTO TABLE 表名称;
