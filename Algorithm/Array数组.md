数组（Array）是一种线性表数据结构。它用一组连续的内存空间，来存储一组具有相同类型的数据。
查找元素快：通过索引，可以快速访问指定位置的元素
####线性表&非线性表示
在非线性表中，数据之间并不是简单的前后关系。
####时间复杂度
数组支持随机访问，根据下标随机访问的时间复杂度为 O(1)
####插入操作
时间复杂度为 O(n),假设数组的长度为 n，现在，如果我们需要将一个数据插入到数组中的第 k 个位置。为了把第 k 个位置腾出来，给新来的数据，我们需要将第 k～n 这部分的元素都顺序地往后挪一位。
```
 public void add(int index, E e) {
        if (size == data.length)
            throw new IllegalArgumentException("array list is full");
        if (index < 0 || index > size) {
            throw new IllegalArgumentException("add error");
        }
        for (int i = size - 1; i >= index; i--) {
            data[i + 1] = data[i];
        }
        data[index] = e;
        size++;
    }
```
####删除操作Delete
时间复杂度为 O(n),
####动态数组
```
public void resize(int newCapcity){
        E[] newData =(E[])new Object[newCapcity];
        for(int i = 0;i<size;i++){
            newData[i] = data[i];
        }
        data = newData;
    }
```
