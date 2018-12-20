##Servlet简介
 Servlet(Server Applet)是Java Servlet的简称，称为小服务程序或服务连接器，用Java编写的服务器端程序， 主要功能在于交互式地浏览和修改数据，生成动态Web内容。
 ######HttpServletRequest
浏览器对服务器的一次访问称之为一次请求，请求用HttpServletRequest对象 来表示
######HttpServletResponse
服务器给浏览器的一次反馈称之为一次响应，响应用HttpServletResponse对 象来表示
 ######ServletContext
 ######ServletConfig
在web.xml文件中给某一个Servlet配置一些配置信息，当服务器被启 动的时候，这些配置信息就会被封装到某一个ServletConfig对象中去。因此 ServletConfig表示的是某一个Servlet的配置文件。
 ######转发与重定向的区别
1.实现转发调用的是HttpServletRequest对象中的方法，实现重定向调用的 是HttpServletResponse对象中的方法
2.转发时浏览器中的url地址栏不会发生改变，重定向时浏览器中的url地址会 发生改变
3.转发时浏览器只请求一次服务器，重定向时浏览器请求两次服务器
## 生命周期
Servlet对象生命周期依赖于容器, 因为Servlet规范已经约定了Servlet对象必须放在实现Servlet规范的容器中运行, 换区话说Servlet对象从出生到死亡都由容器调度。所以容器负责Servlet的创建、初始化、运行、释放.生命周期方法
1.init(ServletConfig); 负责初始化Servlet.
2.service(ServletRequest, ServletResponse); 负责处理请求.
3destory(); 负责释放Servlet.
```
public class LifecycleServlet extends HttpServlet {
    public LifecycleServlet() {
        // TODO Auto-generated constructor stub
        System.out.println("Servlet构造方法");
    }

    @Override
    public void init(ServletConfig config) throws ServletException {
        super.init(config);
        System.out.println("Servlet init");
    }

    @Override
    protected void service(HttpServletRequest arg0, HttpServletResponse arg1) throws ServletException, IOException {
        System.out.println("Servlet service");
    }

    @Override
    public void destroy() {
        // TODO Auto-generated method stub
        super.destroy();
        System.out.println("Servlet destroy");
    }
}
```
配置文件
```<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>Lifecycle</servlet-name>
        <servlet-class>cn.java.servlet.LifecycleServlet</servlet-class>
    </servlet>
    <servlet-mapping>

        <servlet-name>Lifecycle</servlet-name>
        <url-pattern>/lifecycle</url-pattern>
    </servlet-mapping>
</web-app>
```
第一次访问是依次调用了构造方法、init方法、service方法,之后只会调用service方法。当停止tomcat的时候会调用destroy。

1. Servlet的生命周期是先由容器创建Servlet对象, 所以会调用其构造方法.然后再调用init方法进行初始化, 最后调用service方法处理请求.由次可以见Servlet只在第一次访问时候创建.创建完后容器就调用了servlet的init方法.
2. Servlet对象在整个容器中只有一份,属于单例.因为多次的相同url的请求过来, 都没有再调用构造方法, 显然没有在创建新的Sevlet对象.
3. Servlet必须提供无参数构造器.由于Sevlet对象不是我们手动创建的, 而是交给容器负责。在这里我们使用的是tomcat容器.而tomcat容器是使用反射技术Class clazz = Class.forName();和clazz.newInstance()创建Servlet对象.所以我们必须提供无参构造器.当然Java中如果你不提供构造器, 默认就会存在一个无参构造器。
4. 要正确的让Servlet得到释放, 必须得停止tomcat容器.如果是程序异常中断, Servlet并不能得到释放.
