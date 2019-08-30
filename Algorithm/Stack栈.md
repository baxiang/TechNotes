####栈概述
又称堆栈，它是运算受限的线性表，其限制是仅允许在一端进行插入和删除操作。按照先进后出（First In Last Out )的原则存储数据 
####栈顶top
允许进行插入和删除操作的一段被称为栈顶,栈的入口、出口的都是栈的顶端位置。
####栈底bottom
无法进行数据操作的一端被称为栈底
####压栈PUSH
就是存元素。即，把元素存储到栈的顶端位置，栈中已有元素依次向栈底方向移动一个位置。
####弹栈POP
就是取元素。即，把栈的顶端位置元素取出，栈中已有元素依次向栈顶方向移动一个位置。
```
public interface Stack<Item> {
    int getSize();
    boolean isEmpty();
    void push(Item e);
    Item pop();
}

```
基本操作
```
 @Override
    public int size() {
        return top;
    }

    @Override
    public boolean isEmpty() {
        return top==0;
    }

    @Override
    /**
     * 压栈
     */
    public void push(Item e) {
        if (size()== stack.length){
            expandCapacity();
        }
        stack[top]=e;
        top++;
    }

    /**
     * 栈扩容
     */
    private void expandCapacity(){
        stack = Arrays.copyOf(stack,stack.length*2);
    }

    @Override
    /**
     * 出栈 返回当前栈顶元素
     */
    public Item pop() {
        if (isEmpty())
            throw  new EmptyStackException();
        top--;
        Item item = stack[top];
        return item;
    }

    @Override
    public Item peek() {
        if (isEmpty())
            throw  new EmptyStackException();
        return stack[top-1];
    }
```
通过链表实现栈
```
public class LinkedStack <Item> implements Iterator<Item> {

    private class Node{
        Item item;
        Node next;

        public Node(Item item, Node next) {
            this.item = item;
            this.next = next;
        }
    }
    private Node first;//栈顶元素
    private int size;//栈元素总数
    private Node currNode;

    public void push(Item item){
        first = new Node(item,first);
        currNode = first;
        size++;
    }

    public Item pop(){
        Node currFirst = first;
        first = currFirst.next;
        currNode = first;
        size--;
        return currFirst.item;
    }

    public boolean isEmpty(){
        return size==0;
    }

    public int getSize() {
        return size;
    }

    @Override
    public boolean hasNext() {
        return currNode!=null;
    }

    @Override
    public Item next() {
        Item item = currNode.item;
        currNode = currNode.next;
        return item;
    }
    
    @Override
    public void remove() {
        throw new UnsupportedOperationException();
    }
}

```
