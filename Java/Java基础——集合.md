####概述
集合是java中提供的一种容器，可以用来存储多个数据，集合框架主要java.util 包中，存储结构可以分为两大类，分别是单列集合java.util.Collection和双列集合java.util.Map。
Collection是单列集合类的根接口，用于存储一系列符合某种规则的元素，它有两个重要的子接口，分别是java.util.List和java.util.Set。其中，List的特点是元素有序、元素可重复。Set的特点是元素无序，而且不可重复。List接口的主要实现类有java.util.ArrayList和java.util.LinkedList，Set接口的主要实现类有java.util.HashSet和java.util.TreeSet。
![image.png](https://upload-images.jianshu.io/upload_images/143845-6e329e256847d8cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**集合和数组的区别**
(1)长度：数组的长度是不变的，集合的长度是可变的
(2)存储内容：数组存放的是基本数据类型和引用类型，集合只能存储引用类型。
(3)存储类型：数组存放的类型是相同，而集合可以实现不同类型。
##Collection接口方法
Collection是所有单列集合类的父接口，因此在Collection中定义了单列集合(List和Set)通用的方法，方法如下：
```
boolean add(E e)// 添加元素
boolean addAll(Collection c)//将指定 collection 中的所有元素都添加到此 collection 中
void clear()// 清空元素
boolean contains(E e)//如果 包含指定的元素，则返回 true
boolean isEmpty()//如果此 collection 不包含元素，则返回 true。
Iterator iterator()//返回在此 collection 的元素上进行迭代的迭代器
boolean remove(E e)// 移除元素
boolean removeAll(Collection c)//移除 collection 的元素
int size() // 返回此 collection 中的元素个数
Object[] toArray()//把集合中的元素，存储到数组中
```
####List接口
1. 它是一个元素存取有序的集合。例如，存元素的顺序是11、22、33。那么集合中，元素的存储就是按照11、22、33的顺序完成的）。
2. 它是一个带有索引的集合，通过索引就可以精确的操作集合中的元素（与数组的索引是一个道理）。
3. 集合中可以有重复的元素，通过元素的equals方法，来比较是否为重复的元素。

```
- public void add(int index, E element): 将指定的元素，添加到该集合中的指定位置上。
- public E get(int index):返回集合中指定位置的元素。
- public E remove(int index): 移除列表中指定位置的元素, 返回的是被移除的元素。
- public E set(int index, E element):用指定元素替换集合中指定位置的元素,返回值的更新前的元素。

```
**ArrayList**
底层数据结构是数组，查询快，增删慢，线程不安全，效率高。
```
ArrayList<String> list = new ArrayList<String>();
```
**LinkedList**
采用链表结构存储数据 这种结构的优点是操作列表数据快。当时随笔访问集合中的数据慢
```
- public void addFirst(E e):将指定元素插入此列表的开头。
- public void addLast(E e):将指定元素添加到此列表的结尾。
- public E getFirst():返回此列表的第一个元素。
- public E getLast():返回此列表的最后一个元素。
- public E removeFirst():移除并返回此列表的第一个元素。
- public E removeLast():移除并返回此列表的最后一个元素。
- public E pop():从此列表所表示的堆栈处弹出一个元素。
- public void push(E e):将元素推入此列表所表示的堆栈。
- public boolean isEmpty()：如果列表不包含元素，则返回true。

```
eg
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
####Set接口
Set集合是由一串无序的，不能重复的相同类型元素构成的集合。Set接口直接实现类是HashSet,HashSet是基于散列表数据结构实现的。
哈希表确定元素是否相同
1、 判断的是两个元素的哈希值是否相同。 如果相同，再判断两个对象的内容是否相同。
2、 判断哈希值相同，其实判断的是对象的HashCode方法。判断内容相同，用的是equals方法。
#####HashSet
```
 HashSet<String> set = new HashSet<>();
        set.add(new String("a"));
        set.add("b");
        set.add("c");
        set.add("a");
        //遍历
        for (String name : set) {
            System.out.println(name);
        }
```
####LinkedHashSet
HashSet保证元素唯一，可是元素存放进去是没有顺序的，在HashSet下面有一个子类java.util.LinkedHashSet，它是链表和哈希表组合的一个数据存储结构,实现了集合的顺序存储
```
    public static void main(String[] args) {

        LinkedHashSet<String> linkSet = new LinkedHashSet<>();
        for (int i = 0; i < 10; i++) {
            String s = getRandomString(10);
            System.out.printf("RandomString=%s\n", s);
            linkSet.add(s);
        }
        Iterator linkIterator = linkSet.iterator();
        while (linkIterator.hasNext()) {
            System.out.printf("LinkedHashSet=%s\n", linkIterator.next());
        }
    }

    // 生成指定长度的随机字符串
    public static String getRandomString(int length) {
        String str = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*";
        Random random = new Random();
        StringBuffer sb = new StringBuffer();
        int strLength = str.length();
        for (int i = 0; i < length; i++) {
            int number = random.nextInt(strLength);
            sb.append(str.charAt(number));
        }
        return sb.toString();
    }
```

##Iterator接口
Iterator主要用于迭代访问（即遍历）Collection中的元素，因此Iterator对象也被称为迭代器。
获取集合的迭代器
```
 Iterator iterator()
```
Iterator接口的常用方法如下：
```
 E next():返回迭代的下一个元素。
 boolean hasNext():如果仍有元素可以迭代，则返回 true。
```
在调用Iterator的next()方法之前，迭代器的索引位于第一个元素之前，不指向任何元素，当第一次调用迭代器的next()方法后，迭代器的索引会向后移动一位，指向第一个元素并将该元素返回，当再次调用next()方法时，迭代器的索引会指向第二个元素并将该元素返回，依此类推，直到hasNext()方法返回false，表示到达了集合的末尾，终止对元素的遍历。
```
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            list.add(i);
        }
        Iterator<Integer> iterator = list.iterator();
        while (iterator.hasNext()) {
            int i = iterator.next();
            System.out.println(i);
        }
    }
```
####for 遍历集合
它的内部原理其实是个Iterator迭代器，所以在遍历的过程中，不能对集合中的元素进行增删操作。for遍历格式如下：
```
    for(元素的数据类型  变量 : Collection集合or数组){ 
      	//写操作代码
    }
```
eg:
```
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        for (int i = 5; i > 0; i--) {
            list.add(i);
        }
       for (int i :list){
           System.out.println(i);
       }
    }
```
####Map
HashMap<K,V>：存储数据采用的哈希表结构，元素的存取顺序不能保证一致。由于要保证键的唯一、不重复，需要重写键的hashCode()方法、equals()方法。
LinkedHashMap<K,V>：HashMap下有个子类LinkedHashMap，存储数据采用的哈希表结构+链表结构。通过链表结构可以保证元素的存取顺序一致；通过哈希表结构可以保证的键的唯一、不重复，需要重写键的hashCode()方法、equals()方法。
Map集合存储元素是键值成对出现的，Map集合的键是唯一的，值是可重复的。常用方法：
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

size()	获取集合元素的个数
```
使用put方法时，若指定的键(key)在集合中没有，则没有这个键对应的值，返回null，并把指定的键值添加到集合中； 
若指定的键(key)在集合中存在，则返回值为集合中键对应的值（该值为替换前的值），并把指定键所对应的值，替换成指定的新值。
**Entry键值对对象**
我们已经知道，Map中存放的是两种对象，一种称为key(键)，一种称为value(值)，它们在在Map中是一一对应关系，这一对对象又称做Map中的一个Entry(项)。Entry将键值对的对应关系封装成了对象。即键值对对象，这样我们在遍历Map集合时，就可以从每一个键值对（Entry）对象中获取对应的键与对应的值。
 既然Entry表示了一对键和值，那么也同样提供了获取对应键和对应值得方法：
```
public K getKey()：获取Entry对象中的键。
public V getValue()：获取Entry对象中的值。
```
在Map集合中也提供了获取所有Entry对象的方法：
```
 public Set<Map.Entry<K,V>> entrySet(): 获取到Map集合中所有的键值对对象的集合(Set集合)。
```
 eg
 ``` java
public static void main(String[] arge){
        HashMap<String,String> map = new HashMap<String,String>();
        map.put("北京","北京");
        map.put("广东","广州");
        map.put("山东","济南");
        map.put("浙江","杭州");
        map.put("江苏","杭州");
        Set<Entry<String,String>> set = map.entrySet();
        for (Entry<String,String> entry: set){
            System.out.println(entry.getKey()+"："+entry.getValue());
        }
    }
```
存储自定义对象
 ```
  @Override
    public boolean equals(Object o) {
        if (this == o)
            return true;
        if (o == null || getClass() != o.getClass())
            return false;
        Student student = (Student) o;
        return age == student.age && Objects.equals(name, student.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
```
**LinkedHashMap**
我们知道HashMap保证成对元素唯一，并且查询速度很快，可是成对元素存放进去是没有顺序的，那么我们要保证有序，还要速度快怎么办呢？在HashMap下面有一个子类LinkedHashMap，它是链表和哈希表组合的一个数据存储结构。
```
    public static void main(String[] arg) {
        LinkedHashMap<String, String> map = new LinkedHashMap<String, String>();
        map.put("中国", "CHN");
        map.put("美国", "USA");
        map.put("俄罗斯", "RUS");
        map.put("法国", "FRA");
        map.put("英国", "UK");
        Set<Entry<String, String>> entrySet = map.entrySet();
        for (Entry<String, String> entry : entrySet) {
            System.out.println(entry.getKey() + "  " + entry.getValue());
        }
    }
```
