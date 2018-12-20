####可迭代对象
Iterable判断对象是否可迭代对象
```
from collections import Iterable

print(isinstance([1, 2, 3], Iterable))
print(isinstance({"name": "xiaohong"}, Iterable))
print(isinstance("test", Iterable))
```
####__iter__迭代器
一个具备了__iter__方法的对象，就是一个可迭代对象。
####迭代器对象


####生成器
生成器是一类特殊的迭代器
