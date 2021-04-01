# Servlet

Java Servelt 是运行在Web服务器或应用服务器上的程序，它是作为来自Web浏览器或其他HTTP客户端的请求和HTTP服务器上的数据库或应用程序之间的中间层。

通俗的讲，servlet是运行在web服务器如tomcat，jetty这样的应用服务器上的一段程序，他可以响应http协议的请求，并且实现用户自己的逻辑，最终将结果返回到用户的客户端（浏览器）

**Servlet生命周期**

从创建到销毁，主要包括以下过程：

- 调用init方法进行初始化操作
- 调用service方法处理请求，根据请求方法调用对应的doGet doPost doPut等方法
- 调用destory方法



































