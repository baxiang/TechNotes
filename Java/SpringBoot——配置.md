SpringBoot使用一个全局的配置文件，配置文件名是固定的；
•application.properties
•application.yml
修改服务器端口
```
server:
  port: 8081
```
####YMAL语法
k:(空格)v：表示一对键值对（空格必须有）；
以空格的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的
```
    server:
        port: 8081
        path: /hello
```
属性和值也是大小写敏感；
2、值的写法

字面量：普通的值（数字，字符串，布尔）
	k: v：字面直接来写；
		字符串默认不用加上单引号或者双引号；
		\"\"：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思
```
name:   "zhangsan \n lisi"：输出；zhangsan 换行  lisi
```
''：单引号；会转义特殊字符，特殊字符最终只是一个普通的字符串数据
```
name:   ‘zhangsan \n lisi’：输出；zhangsan \n  lisi
```
对象、Map（属性和值）（键值对）：
k: v：在下一行来写对象的属性和值的关系；注意缩进
对象还是k: v的方式
```
    friends:
    		lastName: zhangsan
    		age: 20
```
行内写法：
```
    friends: {lastName: zhangsan,age: 18}
```
#### 2、@Value获取值和@ConfigurationProperties获取值比较

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |

配置文件yml还是properties他们都能获取到值；
如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value；
如果说，我们专门编写了一个javaBean来和配置文件进行映射，我们就直接使用@ConfigurationProperties；
@PropertySource：加载指定的配置文件；






