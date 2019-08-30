####概述
先进先出（FIFO—first in first out），只允许在表的前端（front）进行删除操作，在表的后端（rear）进行插入操作
######队尾
进行插入操作的端成为队尾
######对头
进行删除操作的端成为对头
######入队
在队列中插入一个队列元素称为入队
######出队
而从队列中删除一个队列元素称为出队
####单向队列（Queue)
只能在一端插入数据，另一端删除数据。
```
public ArrayQueue(int c) {
        capacity = c;
        queue = (Item[]) new Object[capacity];
        front =0;
        rear = -1;
        size = 0;
    }

    @Override
    public int getSize() {
        return size;
    }

    @Override
    public boolean isFull() {
        return size==capacity;
    }

    @Override
    public boolean isEmpty() {
        return size==0;
    }

    @Override
    public void enqueue(Item i) {
        if (isFull()){
            throw new IllegalArgumentException("queue is full");
        }
        queue[size++]=i;
    }

    @Override
    public Item dequeue() {
        if (isEmpty()){
            throw new IllegalArgumentException("queue is empty");
        }
        Item front = queue[0];
        for(int i =0;i<size-1;i++){
            queue[i]=queue[i+1];
        }
        size--;
        return front;
    }

    @Override
    public Item getFront() {
        if (isEmpty()){
            throw new IllegalArgumentException("queue is empty");
        }
        return queue[0];
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append("queue:[");
        for(int i=0;i<size;i++){
            sb.append(queue[i]);
            if (i!=size-1){
                sb.append(", ");
            }
        }
        sb.append("]");
        return sb.toString();
    }
```
####循环队列
```
public class ArrayQueue<Item> implements Queue<Item>{
    private int capacity;// 容量
    private Item[] queue;
    private int front;// 前端
    private int rear; // 后端
    private int size; //队列中实际元素总量

    public ArrayQueue(int c) {
        capacity = c;
        queue = (Item[]) new Object[capacity];
        front = 0;
        rear = -1;
        size = 0;
    }

    @Override
    public int getSize() {
        return size;
    }

    @Override
    public boolean isFull() {
        return size==capacity;
    }

    @Override
    public boolean isEmpty() {
        return size==0;
    }

    @Override
    public void enqueue(Item i) {
        if (isFull()){
            throw new IllegalArgumentException("queue is full");
        }
        if (rear==capacity-1){// 到达队尾
            rear = -1;
        }
        queue[++rear]=i;
        size++;
    }

    @Override
    public Item dequeue() {
        if (isEmpty()){
            throw new IllegalArgumentException("queue is empty");
        }
        Item i = queue[front];
        queue[front] = null;
        front++;
        if (front == capacity){
            front=0;
        }
        size--;
        return i;
    }

    @Override
    public Item peekFront() {
        if (isEmpty()){
            throw new IllegalArgumentException("queue is empty");
        }
        return queue[front];
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append("queue:[");
        if (front<=rear){
            for(int i=front;i<=rear;i++){
                sb.append(queue[i]);
                if (i!=rear){
                    sb.append(", ");
                }
            }
        }else{
            for(int i=front;i<=capacity-1;i++){
                sb.append(queue[i]);
                sb.append(", ");
            }
            for(int i=0;i<=rear;i++){
                sb.append(queue[i]);
                if (i!=rear){
                    sb.append(", ");
                }
            }
        }
        sb.append("]");
        return sb.toString();
    }
}
```
####双向队列（Deque）
每一端都可以进行插入数据和删除数据操作。
