## pymysql 
 pymysql 的GitHub地址:https://github.com/PyMySQL/PyMySQL
![image.png](https://upload-images.jianshu.io/upload_images/143845-db8c94c1e66ed0f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
常见数据类
|类型	|字节大小	|有符号范围(Signed)|	无符号范围(Unsigned)|
|--|---|---|---|
|TINYINT|	1|	-128 ~ 127	|0 ~ 255|
|SMALLINT	|2|	-32768 ~ 32767	|0 ~ 65535
|MEDIUMINT	|3	|-8388608 ~ 8388607	|0 ~ 16777215
|INT/INTEGER	|4	|-2147483648 ~2147483647|	0 ~ 4294967295
|BIGINT|	8	|-9223372036854775808 ~ 9223372036854775807|	0 ~ 18446744073709551615
字符串

|类型|	字节大小	|示例|
|--|---|--|
|CHAR	|0-255	|类型:char(3) 输入 'ab', 实际存储为'ab ', 输入'abcd' 实际存储为 'abc'|
|VARCHAR	|0-255	|类型:varchar(3) 输 'ab',实际存储为'ab', 输入'abcd',实际存储为'abc'|
|TEXT|	0-65535|	大文本|
日期时间类型

|类型|	字节大小|	示例|
|--|---|--|
|DATE	|4	|'2020-01-01'|
|TIME	|3	|'12:29:59'|
|DATETIME	|8	|'2020-01-01 12:29:59'|
|YEAR	|1	|'2017'|
|TIMESTAMP	|4	|'1970-01-01 00:00:01' UTC ~ '2038-01-01 00:00:01' UTC
安装方式
```
 python3 -m pip install PyMySQL
```
####Connection
创建对象：调用connect()方法用于建立与数据库的连接
```
conn=connect(参数列表)
```
参数host：连接的mysql主机，如果本机是'localhost'
参数port：连接的mysql主机的端口，默认是3306
参数database：数据库的名称
参数user：连接的用户名
参数password：连接的密码
参数charset：通信采用的编码方式，推荐使用utf8
对象的方法
close()关闭连接
commit()提交
cursor()返回Cursor对象，用于执行sql语句并获得结果

#####Cursor游标
用于执行sql语句，使用频度最高的语句为select、insert、update、delete
获取Cursor对象：调用Connection对象的cursor()方法
```
c=conn.cursor()
```
对象的方法
close()关闭
execute(operation [, parameters ])执行语句，返回受影响的行数，主要用于执行insert、update、delete语句，也可以执行create、alter、drop等语句
fetchone()执行查询语句时，获取查询结果集的第一个行数据，返回一个元组
fetchall()执行查询时，获取结果集的所有行，一行构成一个元组，再将这些元组装入一个元组返回
#### 连接数据库
```
import pymysql

conn = pymysql.connect(host='localhost', user='baxiang', password='123456', port=3306)
cursor = conn.cursor()
cursor.execute('SELECT VERSION()')
data = cursor.fetchone()
print(''.join(data))
cursor.execute('SHOW DATABASES')
data = cursor.fetchone()
print(''.join(data))
conn.close()
```
####创建数据库
```
import pymysql

conn = pymysql.connect(host='localhost', user='baxiang', password='123456', port=3306)
cursor = conn.cursor()
cursor.execute('CREATE DATABASE py_test DEFAULT CHARACTER  SET utf8')
cursor.execute('SHOW DATABASES')
dbList = cursor.fetchall()
for db in dbList:
    print('db name:'+''.join(db))
conn.close()
```
fetchall获取结果集的所有行，一行构成一个元组，再将这些元组装入一个元组返回

####创建表
```
import pymysql

conn = pymysql.connect(host='localhost', user='baxiang', password='123456', port=3306, db='py_test')
cursor = conn.cursor()
sql = '''
CREATE TABLE IF NOT EXISTS exam(
`id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
`name` varchar(255) NOT NULL,
`score` smallint(5) unsigned NOT NULL,
PRIMARY KEY (`id`)
)COMMENT='学生'
'''
cursor.execute(sql)
cursor.close()
conn.close()
```
#### 插入数据
```
import pymysql

conn = pymysql.connect(host='localhost', user='baxiang', password='123456', port=3306, db='py_test')
cursor = conn.cursor()
try:
    cursor.execute("INSERT INTO exam(id,name,score) VALUES (%s,%s,%s)", (2009100981, 'ba', 95))
    conn.commit()
except:
    conn.rollback()

cursor.close()
conn.close()
```
####查询数据
```
import pymysql

conn = pymysql.connect(host='localhost', user='baxiang', password='123456', port=3306, db='py_test')
cursor = conn.cursor()
try:
    cursor.execute("SELECT * FROM exam WHERE score >= 60")
    dataList = cursor.fetchall()
    for data in dataList:
        print(data)
except Exception as e:
    print(e)

cursor.close()
conn.close()
```
