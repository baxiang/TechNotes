
## 安装redis-py
GitHub地址https://github.com/andymccurdy/redis-py

```
 pip3 install redis
```
##连接redis
　redis-py提供两个类Redis和StrictRedis用于实现Redis的命令，StrictRedis用于实现大部分官方的命令，并使用官方的语法和命令，Redis是StrictRedis的子类
```
import redis

r = redis.StrictRedis(host='localhost', port=6379, db=0)
r.set('foo', 'bar')
c = r.ge
```
## 连接池
redis-py使用connection pool来管理对一个redis server的所有连接，避免每次建立、释放连接的开销。默认，每个Redis实例都会维护一个自己的连接池。可以直接建立一个连接池，然后作为参数Redis，这样就可以实现多个Redis实例共享一个连接池。
```
import redis

pool = redis.ConnectionPool(host='localhost', port=6379, decode_responses=True)
clientOne = redis.Redis(connection_pool=pool)
clientTwo = redis.Redis(connection_pool=pool)

clientOne.set('name1', 'zhangsan')
print(clientOne.get('name1'))
clientTwo.set('name2', 'lisi')
print(clientTwo.get('name2'))
print(clientOne.client_list())
print(clientTwo.client_list())
# 可以看出两个连接的id是一致的，说明是一个客户端连接
```
##管道
redis-py默认在执行每次请求都会创建（连接池申请连接）和断开（归还连接池）一次连接操作，如果想要在一次请求中指定多个命令，则可以使用pipline实现一次请求指定多个命令，并且默认情况下一次pipline 是原子性操作。
```
import redis

pool = redis.ConnectionPool(host='localhost', port=6379)
r = redis.Redis(connection_pool=pool)

pipe = r.pipeline()

r.set('name', 'zhangsan')
r.set('name', 'lisi')
r.set('name', 'wangwu')
pipe.execute()
```
## 常用操作
####设置
在Redis中设置值，默认，不存在则创建，存在则修改。 ex表示过期时间（秒），px表示过期时间（毫秒）， nx表示如果设置为True，则只有name不存在时，当前set操作才执行，xx表示如果设置为True，则只有name存在时，set操作才执行。
```
 def set(self, name, value, ex=None, px=None, nx=False, xx=False):
```
设置过期时间为1秒
```
r = redis.StrictRedis(host='localhost', port=6379, decode_responses=True)
r.set("name","zhangsan",ex=1)
print(r.get("name"))
time.sleep(1)
print(r.get("name"))
```
设置不存在采取修改
```
import redis

r = redis.StrictRedis(host='localhost', port=6379, decode_responses=True)
r.delete("name")# 表示删除key1
r.set("name","bx",nx=True)# 表示不存在才设置
print(r.get("name"))

```

