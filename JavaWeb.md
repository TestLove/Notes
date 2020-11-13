# Servlet

> `Servlet`是一种允许响应请求的Java类。虽然`Servlet`可以响应任何类型的请求，但它们通常被用来响应网络请求。一个`Servlet`必须部署在Java servlet容器中，它才能成为可用的。
>
> ![image](https://img-blog.csdnimg.cn/2018120522281643.gif)

- 客户端发送请求至服务器
- 服务器启动并调用Servlet，Servlet根据客户端请求生成响应内容并将其传给服务器
- 服务器将响应返回客户端

## 构建Servlet

1. 所有的`Servlet`必须继承`javax.servlet.GenericServlet`或`javax.servlet.http. HttpServlet`。

2. 重写了doGet()和doPos()方法。这些方法定义在`HttpServlet`类中。每当GET或POST请求到来时，它被映射到其各自对应的方法（例如，如果您发送一个 HTTP GET请求，然后servlet的doGet()方法被调用。

3. 还有一些你可以重写的其他方法，被用来控制应用程序，比如getServletInfo()。

4. `HttpServletRequest`和`HttpServletResponse`是doxxx()方的默认参数类型。

## Servlet生命周期方法

> init( ),service( ),destroy( )是Servlet生命周期的方法。代表了Servlet从“出生”到“工作”再到“死亡 ”的过程

1. **init( )**,当Servlet第一次被请求时，Servlet容器就会开始调用这个方法来初始化一个Servlet对象出来，但是这个方法在后续请求中不会在被Servlet容器调用，就像人只能“出生”一次一样。我们可以利用init（ ）方法来执行相应的初始化工作。调用这个方法时，Servlet容器会传入一个ServletConfig对象进来从而对Servlet对象进行初始化。

2. **service( )**方法，每当请求Servlet时，Servlet容器就会调用这个方法。就像人一样，需要不停的接受老板的指令并且“工作”。第一次请求时，Servlet容器会先调用init( )方法初始化一个Servlet对象出来，然后会调用它的service( )方法进行工作，但在后续的请求中，Servlet容器只会调用service方法了。

3. **destory**,当要销毁Servlet时，Servlet容器就会调用这个方法，就如人一样，到时期了就得死亡。在卸载应用程序或者关闭Servlet容器时，就会发生这种情况，一般在这个方法中会写一些清除代码

## Servlet 接口详解

### ServletRequset接口

> Servlet容器对于接受到的每一个Http请求，<u>都会创建一个ServletRequest对象，并把这个对象传递给Servlet的Sevice( )方法</u>
>
> ```java
> public interface ServletRequest {
>     int getContentLength();//返回请求主体的字节数
>     String getContentType();//返回主体的MIME类型
>     String getParameter(String var1);//返回请求参数的值
> }
> ```

getParameter是在ServletRequest中最常用的方法，可用于获取查询字符串的值。

### ServletResponse接口

> 表示一个Servlet响应，在调用Servlet的Service( )方法前，<u>Servlet容器会先创建一个ServletResponse对象，并把它作为第二个参数传给Service( )方法</u>
>
> 
>
> ```java
> public interface ServletResponse {
> String getCharacterEncoding();
> String getContentType();
> ServletOutputStream getOutputStream() throws IOException;
> PrintWriter getWriter() throws IOException;
> void setCharacterEncoding(String var1);
> void setContentLength(int var1);
> void setContentType(String var1);
> void setBufferSize(int var1);
> int getBufferSize();
> void flushBuffer() throws IOException;
> void resetBuffer();
> boolean isCommitted(); 
> void reset();
> void setLocale(Locale var1);
> Locale getLocale();
> ```
> }

在发送任何HTML之前，应该先调用setContentType（）方法，设置响应的内容类型，并将“text/html”作为一个参数传入，这是在告诉浏览器响应的内容类型为HTML，需要以HTML的方法解释响应内容而不是普通的文本，或者也可以加上“charset=UTF-8”改变响应的编码方式以防止发生中文乱码现象。

### ServletConfig接口

> 当Servlet容器初始化Servlet时，Servlet容器会给Servlet的init( )方式传入一个ServletConfig对象。

### ServletContext对象

>  ServletContext对象表示Servlet应用程序。每个Web应用程序都只有一个ServletContext对象
>
> 有了ServletContext对象，就可以共享从应用程序中的所有资料处访问到的信息，并且可以动态注册Web对象。前者将对象保存在ServletContext中的一个内部Map中
>
> ServletContext就是一个“域对象”，它存在于整个应用中，并在在整个应用中有且仅有1份，它表示了当前整个应用的“状态”，你也可以理解为某个时刻的ServletContext代表了这个应用在某个时刻的“一张快照”，这张“快照”里面包含了有关应用的许多信息，应用的所有组件都可以从ServletContext获取当前应用的状态信息

### ServletContextListener

> ServletContextListener是一个接口，我们随便写一个类，只要这个类实现了ServletContextListener接口，那么这个类就实现了【监听ServletContext】的功能
>
> ```java
> package javax.servlet;
> import java.util.EventListener;
> public interface ServletContextListener extends EventListener {
>     void contextInitialized(ServletContextEvent var1);
>     void contextDestroyed(ServletContextEvent var1);
> }
> ```
>
> 

### Javax.servlet.http

> ![image-20201024183118825](https://raw.githubusercontent.com/TestLove/Pictures/pictures/img/image-20201024183118825.png)

1. 不用覆盖service方法，而是覆盖doGet或者doPost方法。在少数情况，还会覆盖其他的5个方法。
2. 使用的是HttpServletRequest和HttpServletResponse对象。
3. **虽然response对象的getOutSream（）和getWriter（）方法都可以发送响应消息体，但是他们之间相互排斥，不可以同时使用，否则会发生异常。**



## 用Servlet下载二进制文件

你可以通过给`ServletContext`的getResourceAsStream() 方法传一个路径，获得一个你需要下载(被存在服务器的文件系统中)的文件的引用。这将返回一个`InputStream`对象可以用来读取文件内容。然后创建一个字节缓冲区，用于读取文件时从文件中获取数据块。最后真正的任务是读取文件内容并将它们复制到输出流中。这是通过使用一个while循环，这将不断读取`InputStream`直到文件结束。使用循环将数据块读入并写入输出流。在这之后，`ServletOutputStream` 对象flush方法被调用，来清除内容和释放资源。

## Servlet、Servlet容器、Tomcat

### Servlet容器:

> Servlet容器也叫做Servlet引擎，是Web服务器或应用程序服务器的一部分，用于在发送的请求和响应之上提供网络服务，解码基于 MIME的请求，格式化基于MIME的响应。==Servlet没有main方法，不能独立运行，它必须被部署到Servlet容器中，由容器来实例化和调用 Servlet的方法（如doGet()和doPost()），Servlet容器在Servlet的生命周期内包容和管理Servlet。==在JSP技术 推出后，管理和运行Servlet/JSP的容器也称为Web容器。

### Tomcat

> Tomcat是一个免费的开放源代码的Servlet容器。
>
> Tomcat服务器接受客户请求并做出响应的过程如下：
>
> 1）客户端（通常都是浏览器）访问Web服务器，发送HTTP请求。
> 2）Web服务器接收到请求后，传递给Servlet容器。
> 3）Servlet容器加载Servlet，产生Servlet实例后，向其传递表示请求和响应的对象。
> 4）Servlet实例使用请求对象得到客户端的请求信息，然后进行相应的处理。
> 5）Servlet实例将处理结果通过响应对象发送回客户端，容器负责确保响应正确送出，同时将控制返回给Web服务器。

![image-20201113182316535](https://raw.githubusercontent.com/TestLove/Pictures/main/img/7tDpNOXqMjRYJH8.png)

- Server 元素表示整个 Catalina servlet 容器。
- Service元素表示一个或多个连接器组件的组合，这些组件共享一个用于处理传入请求的引擎组件。Server 中可以有多个 Service。
- Executor表示可以在Tomcat中的组件之间共享的线程池。
- Connector代表连接组件。Tomcat 支持三种协议：HTTP/1.1、HTTP/2.0、AJP。
- Context元素表示一个Web应用程序，它在特定的虚拟主机中运行。每个Web应用程序都基于Web应用程序存档（WAR）文件，或者包含相应的解包内容的相应目录，如Servlet规范述。
- Engine元素表示与特定的Catalina服务相关联的整个请求处理机器。它接收并处理来自一个或多个连接器的所有请求，并将完成的响应返回给连接器，以便最终传输回客户端。
- Host元素表示一个虚拟主机，它是一个服务器的网络名称（如“www.testlove.cn”）与运行Tomcat的特定服务器的关联。