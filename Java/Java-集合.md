## 集合与数组
(1)数组的长度是不变的，集合的长度是可变的
(2)数组存放的是基本数据类型和引用类型，集合只能存储引用类型。
(3)数组存放的类型是相同，而集合可以实现不同类型。
##接口 Collection
集合框架主要java.util 包中
```
boolean add(Object o)// 添加元素
boolean addAll(Collection c)//将指定 collection 中的所有元素都添加到此 collection 中
void clear()// 清空元素
boolean contains(Object o)//如果 包含指定的元素，则返回 true
boolean isEmpty()//如果此 collection 不包含元素，则返回 true。
Iterator iterator()//返回在此 collection 的元素上进行迭代的迭代器
boolean remove(Object o)// 移除元素
boolean removeAll(Collection c)//移除 collection 的元素
int size() // 返回此 collection 中的元素数
Object[] toArray()//返回包含此 collection 中所有元素的数组
```
## List接口的实现类
###常用方法

### ArrayList
快速随机访问速度快，缺点是插入删除速度比较慢，不是线程安全的
```
ArrayList<String> list = new ArrayList<String>();
```
### LinkedList
采用链表结构存储数据 这种结构的优点是操作列表数据快。当时随笔访问集合中的数据慢
```
LinkedList< String> linkedList=new LinkedList<String>();
```
##Map
Map集合存储元素是键值成对出现的，Map集合的键是唯一的，值是可重复的。
## 常用方法
```
Object put(Object key, Object value)： 加入元素
Object remove(Object key)： 删除与Key相关的元素
void putAll(Map t)：  将来自特定映像的所有元素添加给该映像
void clear()：从映像中删除所有映射
boolean containsKey(Object key)：判断集合是否包含指定的键
boolean containsValue(Object value):判断集合是否包含指定的值
boolean isEmpty()：判断集合是否为空
```
