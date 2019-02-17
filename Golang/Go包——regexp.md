##正则表达式
####匹配普通文本字符
只包含普通的文本，代表去精确匹配这个文本
#### 匹配任意字符
.用来匹配一个任意字符
####使用字符集合
  如果是简单模式匹配，使用Match方法便可：
```
   ok, _ := regexp.Match(pat, []byte(searchIn))
```
 
  可以使用MatchString进行匹配：
```
   ok, _ := regexp.MatchString(pat, searchIn)
```
