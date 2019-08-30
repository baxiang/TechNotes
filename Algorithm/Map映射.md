存储(键，值)数据对的数据结构(key,value)
|dict|key|value|
|----|----|----|
|词典|单词|释义|
|名册|身份证号|人|
|车辆管理|车牌号|车|
|数据库|id|信息|
|词频统计|单词|词频|

```
public boolean contains(K key){
        return getNode(key)!=null;
    }

    public V get(K key){
        Node node = getNode(key);
        return node==null?null:node.value;
    }

    public void add(K key,V value){
        Node node = getNode(key);
        if (node ==null){
            dummyHead.next = new Node(key,value,dummyHead.next);
            size++;
        }else{
            node.value = value;
        }
    }
    public V remove(K key){
        Node prev = dummyHead;
        while (prev.next!= null){
            if (prev.next.key.equals(key)){
                break;
            }
            prev = prev.next;
        }
        if (prev.next!=null){
            Node delNode = prev.next;
            prev.next = delNode.next;
            delNode.next = null;
            return delNode.value;
        }
        return null;
    }
```
Hash Function
Hash Collisions
拉链发 创建链表
