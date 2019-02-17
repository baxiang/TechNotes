####集合
集合框架主要java.util 包中
![image.png](https://upload-images.jianshu.io/upload_images/143845-6e329e256847d8cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**集合和数组**
(1)长度：数组的长度是不变的，集合的长度是可变的
(2)存储内容：数组存放的是基本数据类型和引用类型，集合只能存储引用类型。
(3)存储类型：数组存放的类型是相同，而集合可以实现不同类型。
##Collection接口 
Collection有两个重要的子接口，分别是List和Set。其中，List的特点是元素有序、元素可重复。Set的特点是元素无序，而且不可重复。List接口的主要实现类有ArrayList和LinkedList，Set接口的主要实现类有HashSet和TreeSet。
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
####List接口
**ArrayList**
底层数据结构是数组，查询快，增删慢，线程不安全，效率高。
```
ArrayList<String> list = new ArrayList<String>();
```
**LinkedList**
采用链表结构存储数据 这种结构的优点是操作列表数据快。当时随笔访问集合中的数据慢
|方法声明|	功能描述|
|---|----|
|addFirst()|	在集合的第0个位置添加元素|
|addLast()	|在集合的最后位置添加元素|
|getFirst()|	获取集合的第0个元素
|getLast()|	获取集合的最后一个元素
|removeFirst()	|删除集合的第0个元素
|removeLast()	|删除集合的最后一个元素
```
        LinkedList<String> list = new LinkedList<String>();
        list.addFirst("A");
        list.addFirst("B");
        list.addFirst("C");
        list.removeLast();
        list.addLast("F");
        System.out.println(list.toString());
#[C, B, F]
```
**Vector**
底层数据结构是数组，查询快，增删慢，线程安全，效率低。
```
public void addElement(E obj)：添加元素
public E elementAt(int index)：根据索引获取元素
public Enumeration elements()：获取所有的元素
```
##Set接口
Set集合是由一串无序的，不能重复的相同类型元素构成的集合。Set接口直接实现类是HashSet,HashSet是基于散列表数据结构实现的。
哈希表确定元素是否相同
1、 判断的是两个元素的哈希值是否相同。 如果相同，再判断两个对象的内容是否相同。
2、 判断哈希值相同，其实判断的是对象的HashCode方法。判断内容相同，用的是equals方法。
![image.png](https://upload-images.jianshu.io/upload_images/143845-79a08bfdc623d399.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####Map
Map集合存储元素是键值成对出现的，Map集合的键是唯一的，值是可重复的。
常用方法
```
put(K key, V value)	有添加和替换功能
putAll(Map m)	添加一个Map的元素
clear()	清空集合
remove(Object key)	根据键删除一个元素
containsKey()	判断集合是否包含指定的键
containsValue()	判断集合是否包含指定的值
isEmpty()	判断集合是否为空
get(Object key)	根据键获取值
keySet()	获取所有的键
values()	获取所有的值
entrySet()	获取所有的Entry
size()	获取集合元素的个数
```
##Iterator接口
Iterator主要用于迭代访问（即遍历）Collection中的元素，因此Iterator对象也被称为迭代器。
在调用Iterator的next()方法之前，迭代器的索引位于第一个元素之前，不指向任何元素，当第一次调用迭代器的next()方法后，迭代器的索引会向后移动一位，指向第一个元素并将该元素返回，当再次调用next()方法时，迭代器的索引会指向第二个元素并将该元素返回，依此类推，直到hasNext()方法返回false，表示到达了集合的末尾，终止对元素的遍历。
