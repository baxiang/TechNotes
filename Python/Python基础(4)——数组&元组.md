##数组
####数组定义
以中括号([])表示，每个元素以逗号隔开，里面可以存放相同的数据类型也可以存放不同的数据类型。
```
list = [1,2,3,4,5]
list = [1,False,"string"]
```
####元素的迭代
```
list =['a','b','c','d']
for i in list:
    print(i)
```
####元素索引
len表示当前数组的长度，索引是从0开始的。负数表示倒着索引，起始位置是-1.
```
list =['1','2','3','4']
print(len(list))
print(list[1])
print(list[-1])
```
#### 数组切片
```python
list = ["1","2","3","4","5"]
print(list[2:4])
print(list[:3])
print(list[3:])
#['3', '4']
#['1', '2', '3']
#['4', '5']
```
#### 增加元素
append列表添加元素
```
list = [1,2,3]
print(list)
list.append(4)
print(list)
```
insert(index, obj)插入元素，index为要插入的索引位置，obj为插入的值
``` python
alist = [2,3,4]
alist.insert(0,1)
print(alist)
# [1, 2, 3, 4]
```
extend(seq)在列表末尾追加的是数组元素，append只能是单个元素
```
li = [1,2,4,5]
li.extend([0,8])
print(li)
# [1, 2, 4, 5, 0, 8]
```
####删除元素
pop根据下标进行删除
```
alist = [1,2,3,4]
alist.pop(2)
print(alist)
[1, 2, 4]
```
remove：根据元素的值进行删除
```
alist = [1,2,3,4]
alist.remove(3)
print(alist)
```
#### 判断元素是否在数组中
in（存在）,如果存在那么结果为true，否则为false
not in（不存在），如果不存在那么结果为true，否则false
```
alist = [1,2,3,4]
if 3 in alist:
    print("存在数据3")
```
####clear() 
清空数组
```
li = [1,2,3,4,5]
li.clear()
print(li)
# []
```
####sort()
sort() 对列表中元素进行排序
```
li = [2,1,6,4,5,3]
li.sort()
print(li)
#[1, 2, 3, 4, 5, 6]
```
####reverse()
reverse() 倒序列表中元素
```
li = [1,2,3,4,5]
li.reverse()
print(li)
# [5, 4, 3, 2, 1]
```
#### 列表推导式
[表达式 for 变量 in 列表]    或者  [表达式 for 变量 in 列表 if 条件]
计算10以内偶数的平方
```
result = []
for i in range(1, 10):
    if i % 2 == 0:
        result.append(i * i)
print(result)
```
使用列表推导式
```
r = [i * i for i in range(1, 10) if i % 2 == 0]
print(r)

```
生成随机数列，并且查找随机数列中的偶数
```
from random import randint

result = [randint(0, 20) for _ in range(10)]
print(result)
r = [x for x in result if x % 2 == 0]
print(r)
```
##元组
元组(tuple)使用小括号表示，tuple一旦初始化就不能修改，当定义tuple的时候，tuple的元素必须被确定。
```
aTuple = (1,2,3)
# 元组数据无法修改
aTuple[2] = 4
# TypeError: 'tuple' object does not support item assignment
```
####一个元素的tuple表示
()表示tuple,但是也可以被定义成数学符号小括号，为了避免产生歧义，只有1个元素的元组必须元素末尾增加一个逗号(,)
```
tu = (1)
print(type(tu))
tu =(1,)
print(type(tu))
# <class 'int'>
# <class 'tuple'>
```
####元组和数组的类型互转
```
tu =(1,2,4,5,6)
print(list(tu))#元组转为数组
li =[1,2,3,4,5]
print(tuple(li))#数组转为元组
# [1, 2, 4, 5, 6]
# (1, 2, 3, 4, 5)
```
