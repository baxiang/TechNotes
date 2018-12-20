####字典创建
字典使用花括号 {}表示， 字典的元素采用键值对（key-value）的形式存储,字典的每个键值对用冒号 : 分割，每个键值对之间用逗号分割。
```
di ={'name':"bx",'age':26}
# {'name': 'bx', 'age': 26}
```
####dict函数
```
student = [('name', 'xiaoming'), ('age', 18)]
d = dict(student)
print(d)

detail = dict(name='xiaohong',age=20)
print(detail)
```
####keys
所有key的列表
```
aDict = {"a":1, "b":2, "c":3}
print(aDict.keys())
```
['a', 'b', 'c']
####values
```
aDict = {"a":1, "b":2, "c":3}
print(aDict.values())
[1, 2, 3]
```
####items
返回含所有（键，值）元祖的列表
```
student = {'name': 'xiaoming', 'age': 18}
print(student.items())
##dict_items([('name', 'xiaoming'), ('age', 18)])
```
####len
键值对的个数
```
aDict = {"a":1, "b":2, "c":3}
print(len(aDict))
3
```
####修改数据
```
student = {'name': 'xiaoming', 'age': 18}
print(student)
student['name'] = 'xiaowang'
student['age'] = 19
print(student)

```
####删除元素
一种是del另外一种是调用pop(key)
```
student = {'name': 'xiaoming', 'age': 18}
del student['name']
print(student)
student.pop('age')
print(student)
```
####判断元素是否存在
```
if "e" in dict:
    print("存在key等于e")
else:
    print("不存在key等于e")

if dict.get("b") != None:
    print("存在key等于b")
else:
    print("不存在key等于b")
```
####字典推导式
{key:value for循环 if判断} 
```
d = {'name': 'bx', 'age': 18}
di = {value: key for key, value in d.items()}
print(di)
##{'bx': 'name', 18: 'age'}
```
####clear
删除字典内所有的元素
```
student = {'name': 'xiaoming', 'age': 18}
student.clear()
print(student)
```
####get
dict.get(key.default=None)key代表要查找的键，如果没有找到就是用的设置default默认值
```
student = {'name': 'xiaoming', 'age': 18}
print(student.get('age'))
print(student.get('gender','none'))

```
