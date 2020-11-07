# Servlet

> `Servlet`是一种允许响应请求的Java类。虽然`Servlet`可以响应任何类型的请求，但它们通常被用来响应网络请求。一个`Servlet`必须部署在Java servlet容器中，它才能成为可用的。

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

### Javax.servlet.http

> ![image-20201024183118825](https://raw.githubusercontent.com/TestLove/Pictures/pictures/img/image-20201024183118825.png)

1. 不用覆盖service方法，而是覆盖doGet或者doPost方法。在少数情况，还会覆盖其他的5个方法。

2. 使用的是HttpServletRequest和HttpServletResponse对象。
3. **虽然response对象的getOutSream（）和getWriter（）方法都可以发送响应消息体，但是他们之间相互排斥，不可以同时使用，否则会发生异常。**

## 用Servlet下载二进制文件

你可以通过给`ServletContext`的getResourceAsStream() 方法传一个路径，获得一个你需要下载(被存在服务器的文件系统中)的文件的引用。这将返回一个`InputStream`对象可以用来读取文件内容。然后创建一个字节缓冲区，用于读取文件时从文件中获取数据块。最后真正的任务是读取文件内容并将它们复制到输出流中。这是通过使用一个while循环，这将不断读取`InputStream`直到文件结束。使用循环将数据块读入并写入输出流。在这之后，`ServletOutputStream` 对象flush方法被调用，来清除内容和释放资源。