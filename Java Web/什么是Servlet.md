# 什么是Servlet?

Servlet（Servlet Applet），全称Java Servlet,是用Java编写的服务器端程序。

这些Servlet都要实现Servlet这个接口。

其主要功能在于交互式的浏览和修改数据，生成动态Web内容

Servlet运行于支持Java的应用服务器中。

HttpServlet 重写doGet 和 doPost 方法或者你也可以重写service方法完成对get和post请求响应。

### 通俗理解？

Servlet是一个运行了面向请求/ 响应服务器中的网络模块。

`请求`是客户的一个调用，可能是远程的。

`请求`包含了客户要发送给服务器的数据。

响应是服务器返回给客户的回答该请求的数据。

Servlet是一个JAVA对象，他以请求为输入，分析其数据，执行一些逻辑运算，并给客户发回一个响应。 

，Servlet作为驻留在[服务器端](https://www.baidu.com/s?wd=服务器端&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1YkmWbYuhf3m1nLmH0kuWbL0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EPj0kPWf3n1mk)HTTP明白的中间层，它们知道怎样在HTTP中通过RMI或IIOP在EJB和客户之间进行通信。

Java RMI 框架（远程方法调用）

只要一个类extends了java.rmi.Remote接口，即可成为存在于服务器端的远程对象，供客户端访问并提供一定的服务

### 入门理解

1. Servlet产生背景和主要作用:

   ​		动态网页可以实现客户端和服务器的交互，比如登录的功能，我们输入用户名和密码后提交到服务器，服务器会处理这些信息，如果正确的话则进入主页。若不正确，则给出提示信息。

   ​		Servlet是Java中用来处理BS架构下，客户端请求的响应处理

   ---

2. Servlet是什么:

   Servlet就是一个Java类

   Java包组成：javax.servlet和javax.servlet.http.

   ​	在javax.servlet包中定义了所有的Servlet类都必须实现或扩展的的通用接口和类.

   ​	在javax.servlet.http包中定义了采用HTTP通信协议的HttpServlet类。

   ---

3. Demo和时序图:

   login.html

   ```html
   <html>     
       <head>         
           <title>Login</title>     
       </head>     
       <body>        
           <form action="exam/login">         
               用户名：<input type="text" name="username"> <br>         
               密　码：<input type="password" name="password"> <br>         
               <input type="submit" value="登录">         
           </form>     
       </body> 
   </html> 
   ```

   web.xml

   ```xml
   <?xml version="1.0" encoding="ISO-8859-1"?> 
   <web-app xmlns="http://java.sun.com/xml/ns/javaee" 
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
   xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
   http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" version="2.5">  
    
        <servlet> 
           <servlet-name>MyServlet</servlet-name> 
           <servlet-class>loginservlet</servlet-class> 
        </servlet> 
        
        <servlet-mapping> 
           <servlet-name>MyServlet</servlet-name> 
           <url-pattern>/login</url-pattern> 
        </servlet-mapping>    
        
   </web-app> 
   ```

   loginServlet.java

   ```java
   import javax.servlet.http.*; 
   import javax.servlet.*; 
   import java.io.*; 
   public class loginservlet extends HttpServlet{ 
       public void doGet(HttpServletRequest req, 
                        HttpServletResponse resp) 
                 throws ServletException, 
                        IOException{ 
             String username = req.getParameter("username"); 
             String password = req.getParameter("password"); 
             System.out.println("username = " + username ); 
             System.out.println("password = " + password );          
             resp.setContentType("text/html"); 
             resp.getWriter().println("Login Success!"); 
     }              
   } 
   ```

   

![img](https://img-blog.csdn.net/20170806220053446?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzk2ODgzNTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

