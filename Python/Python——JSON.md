## json.loads()
json.loads 用于解码 JSON 数据,将Json格式字符串解码转换成Python对象
```
import json

arr = [1, 2, 3, 4]
print(json.loads(str(arr)))
dic = '{"name": "xiaoming", "age": 18}'
print(json.loads(dic))
#[1, 2, 3, 4]
#{'name': 'xiaoming', 'age': 18}
```
## json.dumps()
把一个Python对象编码转换成Json字符串
```
import json

arr = [1, 2, 3, 4]
print(json.dumps(arr))
dic = {"name": "xiaoming", "age": 18}
print(json.dumps(dic))
```
## json.dump()
将Python内置类型序列化为json对象后写入文件,ensure_ascii比较关键，True代表显示为编码形式，这个一般在中文里面特别不好用，所以建议关掉
```
import json

dic = {"name": "xiaohong", "age": 18}
json.dump(dic, open('json.txt', 'w'), ensure_ascii=False)
```
## json.load()
读取文件中json形式的字符串元素 转化成python类型
```
import json

dic = {"name": "xiaohong", "age": 18}
json.dump(dic, open('json.txt', 'w'), ensure_ascii=False)
content = json.load(open('json.txt'))
print(type(content), content)

```
