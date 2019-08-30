####概念
链表通过指针将一组零散的内存块串联在一起。其中，我们把内存块称为链表的“结点”。为了将所有的结点串起来，每个链表的结点除了存储数据之外，还需要记录链上的下一个结点的地址，通常记录下个结点地址的指针叫作后继指针。或者需要记录链行的上一个节点的地址，称为前继指针。
数据存储在节点Node中
```
 class Node{
        public E e;
        public Node next;
}
```
**头结点**
第一个结点叫作头结点，用来记录链表的基地址。有了它，我们就可以遍历得到整条链表。
**尾节点**
最后一个结点叫作尾结点,尾结点的指针不是指向下一个结点，而是指向一个空地址 NULL，表示这是链表上最后一个结点。
优点
链表结构可以充分利用计算机内存空间，实现灵活的内存动态
缺点：
1链表要想随机访问第 k 个元素，就没有数组那么高效了。因为链表中的数据并非连续存储的，所以无法像数组那样，根据首地址和下标，通过寻址公式就能直接计算出对应的内存地址，而是需要根据指针一个结点一个结点地依次遍历，直到找到相应的结点。
2链表由于增加了结点的指针域，空间开销比较大
####dummyHead
为链表设置虚拟节点
```
private Node dumyHead;
    private int size;

    
    public LinkedList(){
        dumyHead = new Node(null);
        size = 0;
    }
    
   public void add(int index, E e){
        if (index<0 ||index>size){
            return;
        }
            Node prev = dumyHead;
            for(int i =0;i<index;i++){
                prev = prev.next;
            }
            Node node = new Node(e);
            node.next = prev.next;
            prev.next = node;
            //prev.next = new node(e.pre.next)
            size++;
   }
```
####获取元素
```
public E Get(int index){
        if(index<0||index>=size){
            throw  new IllegalArgumentException("GET faile");
        }
        Node  curr = dumyHead.next;
        for (int i =0;i<index;i++){
            curr = curr.next;
        }
        return curr.e;
   }
```
####update
```
public void set(int index,E e){
        if(index<0||index>=size){
            throw  new IllegalArgumentException("GET faile");
        }
        Node  curr = dumyHead.next;
        for (int i =0;i<index;i++){
            curr = curr.next;
        }
        curr.e = e;
    }
```
####contain
```
 public boolean contain(E e){
        Node curr = dumyHead.next;
        while (curr!=null){
              if (curr.e.equals(e)){
                  return true;
              }
              curr = curr.next;
        }
        return false;
    }
```
####delete
将该节点的上一个节点的next指向该节点的下一个节点。
```
 public E remove(int index){
        if(index<0||index>=size){
            throw  new IllegalArgumentException("GET faile");
        }
        Node  prev = dumyHead;
        for (int i =0;i<index;i++){
            prev = prev.next;
        }
        Node delNode = prev.next;
        prev.next = delNode.next;
        delNode.next = null;
        size--;
        return delNode.e;
    }

```
####单链表（Single-Linked List）
一个单链表的节点(Node)分为两个部分，第一个部分(data)保存或者显示关于节点的信息，另一个部分存储下一个节点的地址。最后一个节点存储地址的部分指向空值。
####循环链表
单链表的尾结点指针指向空地址，循环链表的尾结点指针是指向链表的头结点。
####双向链表Doubly Linked List
双向链表支持两个方向，每个结点不止有一个后继指针 next 指向后面的结点，还有一个前驱指针 prev 指向前面的结点。
####链表常见错误
```
p->next = x;  // 将 p 的 next 指针指向 x 结点；
x->next = p->next;  // 将 x 的结点的 next 指针指向 b 结点；
```
p->next 指针在完成第一步操作之后，已经不再指向结点 b 了，而是指向结点 x。第 2 行代码相当于将 x 赋值给 x->next，自己指向自己。因此，整个链表也就断成了两半.
#####时间复杂度
space O(n)
prepend O(1)
append O(1)
lookup O(n)
insert  O(1)
delete O(1)

