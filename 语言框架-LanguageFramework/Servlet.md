# Servlet
## 1.什么是Servlet
它是sun公司开发动态web的一门技术

**本质上,实现了Servlet接口的Java程序,就是Servlet**

sun在这些API中提供一个接口:Servlet,如果要开发一个Servlet程序,只需要   
1. 编写一个类,实现Servlet接口
2. 再把开发好的Java类部署再web服务器中


## 2.主流的开发项目构建
1. 新建一个空的Maven项目
2. 删除``src``目录
3. 将子项目需要的所有依赖都导入在pom.xml中(也就是主工程)
4. 在项目目录中右键,添加新模块
5. 然后在新模块的``pom.xml``中配置parents,指向主工程的``pom.xml``

## 3.开发流程-helloworld
sun公司默认的有两个实现类:HttpServlet和
### 3.1.创建一个类继承HttpServlet
```JAVA
package top.gobanana;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doGet(req, resp);
        PrintWriter writer = resp.getWriter();// 响应流
        writer.println("hello world");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```
#### 3.1.1从源码分析HttpServlet继承关系
```
父      Servlet(接口类,其中有一方法名为service)
|         |
|    GenericServlet(抽象类,其中并没有实现父类中的service,这个方法还是空的)
|         |
|    HttpServlet(实现了service方法,判断请求类型,并执行对应的类方法)
|         |
V    自定义类,如果需要实现某种请求对应的动作,就重写HttpServlet中对应的方法
子
```
### 3.2.编写Servlet的映射(在web.xml中)
在web服务中注册刚才写的Servlet,然后给一个浏览器用来访问的路径,
```XML
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
<!--    注册Servlet-->
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>top.gobanana.HelloServlet</servlet-class>
    </servlet>
<!--    Servlet的请求路径-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
        <!-- 这里的"/hello就是路径名称" -->
        <!-- 如果tomcat服务器中给这个组件设置的地址为/s1,那么访问这个组件的绝对路径为"http://127.0.0.1:8080/s1/hello -->
        <!-- 是可以使用通配符匹配更多URL -->
    </servlet-mapping>
</web-app>
```
### 3.3.运行-配置tomcat环境,选中新写的组件,并设置跳转域名
## 4.Servlet原理
Servlet由Web服务器调用
## 5.web.xml中的url-mapping
1. 可以用通配符以匹配更多URL
2. URL的优先级
   1. 有固有路径的映射优先级最高
   2. 其次是通配符
## 6.重写doGet方法时，可用的方法
1. this.getInitParameter()获取``web.xml``中的初始化参数
2. this.getServletConfig()``web.xml``中的Servlet配置
3. this.getServletContext()返回Servlet上下文
4. getRequestDispatcher()进行请求转发
5. getResourceAsStream()获取服务器目录下的文件流
### ServletContext对象---Servlet上下文内容
web容器启动时,为每个web程序创建一个ServletContext对象,每一个此类型的对象就代表了一个web应用

PS:这里的web应用就是主项目中的子项目,所以同一个子项目中的所有的实现了servlet的类,都可以通过这唯一的ServletContext对象进行通信

### 6.1这样就实现了同一子项目中servlet**数据共享**(尽量用cookie和session)

在servlet1的doget方法中
```JAVA
// 获取SercletContext对象
ServletContext context = this.getServletContext()
String username = "test";
// 以键值对的形式存入变量username
context.setAttribute("username",username);
```

在servlet2的doget方法中
```JAVA
ServletContext context = this.getServletContext();
// 通过键取出的内容默认为对象,但是因为我们知道它被存入时是String所以强制转换为String
String username = (String) context.getAttribute("username");

```
PS:这里省略了在``web.xml``中的注册内容
### 6.2请求转发(浏览器地址栏中的URL是不变的，但是内容却是转发后的内容)
```JAVA
ServletContext context = this.getServletContext()
// 获取跳转到对应URL的请求转发对象
RequestDispatcher requestDispatcher = context.getRequestDispatcher("/exampleURL");
// 把doget方法中传入的两个参数再传过去,然后转发
requestDispatcher.forwark(req,resp);
```
### 6.3.重定向(让请求方直接发送新的请求到指定的URL)
### 6.4.读取配置文件(类加载或者反射来替代)

可以利用上下文对象读取项目中的配置文件(只要知道打包之后它所在的**相对路径**)

可能出现的问题:因为Maven的约定,不打包JAVA目录中的配置文件   
解决的办法:在配置文件``pom.xml``中添加打包规则即可

servlet应用
```JAVA
// 通过.properties文件在打包后的路径,获取其文件流
InputStream is = this.getSerlvetContext().getResourceAsStream("/examplePath1/examplePath2");
Properties prop = new Properties();
// 使用properties类对象加载前面取到的文件流
prop.load(is);
String user = prop.getProperty("username");
String pwd = prop.getProperty("password");
```
example.properties
```PROPERTIES
username=examplename
password=exampleword
```
## HttpServletResponse
web服务器每接收到客户端的http请求,会创建两个对象   
代表请求的--HttpServletRequest
代表相应的--httpServletResponse

相关方法的分类
1. 向浏览器发送数据
    ```JAVA
    ServletOutputStream getOutputStream() throws IOException;
    PrintWriter getWriter() throws IOException;
    ```
    ## 向浏览器上传图片
    1. 获取下载文件的路径
    2. 下载文件的文件名
    3. 设置头信息``Content-Disposition``
    4. 获取下载文件的输入流
    5. 创建缓冲区
    6. 获取OutputStream对象
    7. 将FileOutputStream流写入到buffer
2. 负责向浏览器发送响应头的方法
    ```JAVA
    void setCharacterEncoding(String var1);
    void setContentLength(int var1);
    void setDateHeader(String var1, long var2);
    void addDateHeader(String var1, long var2);
    void setHeader(String var1, String var2);
    void addHeader(String var1, String var2);
    void setIntHeader(String var1, int var2);
    void addIntHeader(String var1, int var2);
    void setStatus(int var1);
    ```

