####bean注解
spring提供了多个注解声明bean为Spring管理的Bean
@Controller 声明此类事一个MVC类，通常与@RequestMapping一起使用
@Service 声明此类是一个业务处理类，通常与@Transactional一起使用

####@responseBody
@responseBody作用是将controller的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML.
[http://localhost:8080/responsebody](http://localhost:8080/responseBody)
```
    @ResponseBody
    @RequestMapping("/responseBody")
    public String responseBody(){
        return "hello springmvc";
    }
```
####RequestParam
把请求中的指定名称的参数传递给控制器中形参赋值
属性：
1. value：请求参数中的名称
2. required：请求参数中是否必须提供此参数，默认值是true，必须提供
[http://localhost:8080/param?id=111](http://localhost:8080/param?id=111)
```
    @ResponseBody
    @RequestMapping("/param")
    public String requestParam(@RequestParam(value = "id",required = false) String userId){
        return "hello"+userId;
    }
```
####RequestBody
获取请求参数提的内容，由于get方法没有body 所有get不可以使用，
属性 required：是否必须有请求体，默认值是true
```
    @ResponseBody
    @RequestMapping(value = "/requestBody",method = RequestMethod.POST)
    public String requestBody(@RequestBody String body){
        return body;
    }
```
curl 测试请求Post
```
curl -X POST http://localhost:8080/requestBody \
  -H 'Content-Type: application/json' \
  -d '{"uid":12}'
```
####PathVariable
拥有绑定url中的占位符的。/delete/{id}，{id}就是占位符
 属性 value：指定url中的占位符名称
[http://localhost:8080/user/1100](http://localhost:8080/user/1100)
```
    @ResponseBody
    @RequestMapping(value = "/user/{id}")
    public String request(@PathVariable("id") String id){
        return "uid:"+id;
    }
```
#####RequestHeader
获取指定请求头的值
 属性 value：请求头的名称
```
    @ResponseBody
    @RequestMapping(value = "/lang")
    public String requestHead(@RequestHeader("accept-language") String lang){
        return lang;
    }
```
```
curl -X GET http://localhost:8080/lang \
  -H 'Accept-Language: zh-CN' \
```
####CookieValue
获取指定cookie的名称的值
属性 value：cookie的名称
[http://localhost:8080/cookie](http://localhost:8080/cookie)
```
    @ResponseBody
    @RequestMapping(value = "/cookie")
    public String requestCookie(@CookieValue("JSESSIONID") String cookie){
        return cookie;
    }
```
####ModelAttribute
1. 出现在方法上：表示当前方法会在控制器方法执行前线执行。
2. 出现在参数上：获取指定的数据给参数赋值。
应用场景：
1当提交表单数据不是完整的实体数据时，保证没有提交的字段使用数据库原来的数据。
2修饰的方法没有返回值
####SessionAttributes
多次执行控制器方法间的参数共享
属性1. value：指定存入属性的名称
