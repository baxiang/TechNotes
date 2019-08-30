##Servlet 过滤器概述
1. Servlet过滤器本身并不生成请求和响应对象，它只提供过滤作用。
2. Servlet过滤器能够在Servlet被调用之前检查Request对象，修改RequestHeader和Request内容；
3. 在、servlet被调用之后检查Response对象，修改Response Header和Response内容。Servlet过滤器负责过滤的web组件可以是Servelt、jsp、或html文件。
## Filter API
|方法|说明|
|---|---|
|public void init(FilterConfig filterConfig)|web 应用程序启动时，web 服务器将创建Filter 的实例对象，并调用其init方法，读取web.xml配置，完成对象的初始化功能，从而为后续的用户请求作好拦截的准备工作（filter对象只会创建一次，init方法也只会执行一次）。开发人员通过init方法的参数，可获得代表当前filter配置信息的FilterConfig对象。|
|public void doFilter (ServletRequest, ServletResponse, FilterChain)|用于完成实际的过滤操作，当客户请求访问与过滤器相关联的URL时，Servlet容器将先调用过滤器的这个方法，FilterChain参数用于访问后续过滤器|
|void  destroy()|过滤器在被取消前执行这个方法，释放过滤器申请的资源|
```
public class LoginFilter implements Filter {
    public void init(FilterConfig config) throws ServletException {
        String site = config.getInitParameter("Site");
        System.out.println(site);
    }
    public void destroy() {
    }

    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        System.out.println(req.getServerName()+req.getServerPort());
        chain.doFilter(req,resp);
    }

}

```
web.xml
```
 <filter>
        <filter-name>LoginFilter</filter-name>
        <filter-class>cn.java.servlet.LoginFilter</filter-class>
        <init-param>
            <param-name>Site</param-name>
            <param-value>www.google.com</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>LoginFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```
web.xml配置各节点说明
<filter>指定一个过滤器。
<filter-name>用于为过滤器指定一个名字，该元素的内容不能为空。
<filter-class>元素用于指定过滤器的完整的限定类名。
<init-param>元素用于为过滤器指定初始化参数，它的子元素<param-name>指定参数的名字，<param-value>指定参数的值。
在过滤器中，可以使用FilterConfig接口对象来访问初始化参数。
<filter-mapping>元素用于设置一个 Filter 所负责拦截的资源。一个Filter拦截的资源可通过两种方式来指定：Servlet 名称和资源访问的请求路径
<filter-name>子元素用于设置filter的注册名称。该值必须是在<filter>元素中声明过的过滤器的名字
<url-pattern>设置 filter 所拦截的请求路径(过滤器关联的URL样式)
<servlet-name>指定过滤器所拦截的Servlet名称。
<dispatcher>指定过滤器所拦截的资源被 Servlet 容器调用的方式，可以是REQUEST,INCLUDE,FORWARD和ERROR之一，默认REQUEST。用户可以设置多个<dispatcher>子元素用来指定 Filter 对资源的多种调用方式进行拦截。
<dispatcher>子元素可以设置的值及其意义
REQUEST：当用户直接访问页面时，Web容器将会调用过滤器。如果目标资源是通过RequestDispatcher的include()或forward()方法访问时，那么该过滤器就不会被调用。
INCLUDE：如果目标资源是通过RequestDispatcher的include()方法访问时，那么该过滤器将被调用。除此之外，该过滤器不会被调用。
FORWARD：如果目标资源是通过RequestDispatcher的forward()方法访问时，那么该过滤器将被调用，除此之外，该过滤器不会被调用。
ERROR：如果目标资源是通过声明式异常处理机制调用时，那么该过滤器将被调用。除此之外，过滤器不会被调用。
