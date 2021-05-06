### Tomcat

​	

bin目录下开始项目启动

```
--webapps：tomcat服务器的web目录
		-ROOT
		-kuangstudy：网站的目录名
			-WEB-INF
				-classes：java程序
				-lib：web应用所依赖的jar包
				-web.xml：网络配置文件
			-index.html  默认的首页
			-static
				-css
					-style.css
				-js
				-img
			-......
```



```
http://localhost:8080/project

project是项目名，与ROOT同级的目录
```



### MAVEN

设置文件：conf\settings.xml

<localRepository>.... </localRepository> 设置本地仓库



资源导出问题解决：

```

  <build>
    .......
      <resources>
        <resource>
            <directory>src/main/resources</directory>
            <excludes>
                <exclude>**/*.properties</exclude>
                <exclude>**/*.xml</exclude>
             </excludes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
    ......
</build>
```



### Servlet

iml文件

```
<?xml version="1.0" encoding="UTF-8"?>
<module type="JAVA_MODULE" version="4" />
```



开发Servlet程序：（实际上就是实现了Servlet接口的Java程序）

​		1.编写一个类，实现Servlet接口

​		2.把开发好的java类部署到web服务器中



第一个程序：

​		1.构建一个普通的maven项目，删掉里面的src目录，在这个项目里面建立module；这个空的工程就是maven主工程

​		2.关于maven父子工程的理解

​		父项目会有

```html
<modules>
    <module>servlet-01</module> //子项目名字
</modules>
```

​		子项目会有

```
		<parent>
				<artifactId> .. </artifactId>
				<groupId>.. </groupId>
				<version> ..</version>
		</parent>
```



​		3.Maven环境优化

​			1.修改web.xml为最新

​			2.将maven的结构搭建完整



​		4.编写servlet程序

​			1.编写一个普通类

​			2.实现servlet接口，这里我们直接继承HttpServlet



Servlet依赖

```
<!--        Servlet依赖-->
<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>

```



jsp-api

```
<!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>javax.servlet.jsp-api</artifactId>
    <version>2.3.3</version>
    <scope>provided</scope>
</dependency>
```

jstl依赖

```jsp
<dependency>
      <groupId>org.glassfish.web</groupId>
      <artifactId>jstl-impl</artifactId>
      <version>1.2</version>
      <exclusions>
        <exclusion>
          <groupId>javax.servlet</groupId>
          <artifactId>servlet-api</artifactId>
        </exclusion>
        <exclusion>
          <groupId>javax.servlet.jsp</groupId>
          <artifactId>jsp-api</artifactId>
        </exclusion>
        <exclusion>
          <groupId>javax.servlet.jsp.jstl</groupId>
          <artifactId>jstl-api</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
```



standard 标签库

```html
  <dependency>
      <groupId>taglibs</groupId>
      <artifactId>standard</artifactId>
      <version>1.1.2</version>
    </dependency>
```



连接数据库：

```html
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.47</version>
</dependency>
```





web.xml配置

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0"
         metadata-complete="true">


  
</web-app>
```





web设置初始参数

```html
<context-param>
<param-name>url</param-name>    
  <param-value>jdbc:mysql://localhost:3306/mybatis</param-value>
</context-param>
```



对应jar包：

C:\Environment\apache-maven-3.6.3\maven-repo\javax\servlet\servlet-api\2.5



```html
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
        <html>
        <head>
        <title></title>
        </head>
        <body>
        </body>
        </html>

```





### 第一个servlet程序





```java
package com.acow.servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

public class Test extends HttpServlet {

    //由于get或者host只是请求实现的不同的方式，可以相互调用，业务逻辑都一样；

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer=resp.getWriter();   //响应流
        writer.print("Hello,Servlet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req,resp)
    }
}
```



接下来：编写servlet的映射

​		why:我们写的是Java程序，但是需要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要在web服务中注册我们写的servlet，还需要给他一个浏览器能访问的路径



```
  <servlet>
      <servlet-name>servlet1</servlet-name>
       <servlet-class>net.test.TestServlet</servlet-class>
   </servlet>
 
   <servlet-mapping>
      <servlet-name>servlet1</servlet-name>
       <url-pattern>*.do</url-pattern>
   </servlet-mapping>

```



接下来：配置Tomcat





注：一个Servlet可以指定一个 多个 或通用的映射路径



通用：

```html
  <servlet-mapping>
      <servlet-name>hello</servlet-name>
       <url-pattern>/hello/*</url-pattern>
   </servlet-mapping>

```



可以自定义后缀

```
 <servlet-mapping>
      <servlet-name>hello</servlet-name>
       <url-pattern>*.do</url-pattern>
   </servlet-mapping>
```



防止中文出现乱码

```java
resp.setHeader("Content-Type","text/html;charset=UTF-8");
```

### ServletContext

(每一个web工程中只有一个ServletContext对象)

web容器在启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用：

​	



​	共享数据：

```java
package com.acow.servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class HelloServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //this.getInitParameter(); 初始化参数
        //this.getServletConfig(); Servlet配置
        //this.getServletContext(); Servlet上下文
        ServletContext context=this.getServletContext();
        
        String username="acow"; //数据
        context.setAttribute("username",username); //将一个数据保存在了ServletContext中，名字为：username,值username
        
        
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```







```java
package com.acow.servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class GetServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();

        String username =(String)context.getAttribute("username");

        resp.setContentType("text/html");
        resp.setCharacterEncoding("utf-8");
        resp.getWriter().print("名字"+username);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```





```java
package com.acow.servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ServletDemo03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context=this.getServletContext();
        
        String url = context.getInitParameter("url");
        resp.getWriter().print(url);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```





```java
package com.acow.servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ServletDemo04 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context=this.getServletContext();

        context.getRequestDispatcher("/gp").forward(req,resp);
        //转发请求的路径，调用forward实现请求转发

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```





### 读取资源文件

​	Properties

在java目录下新建properties

在resources目录下新建properties



发现：都被打包到了同一个路径下：classes,我们俗称这个路径为classpath:

思路：需要一个文件流

```java
package com.acow.servlet;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class ServletDemo05 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        InputStream is= this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");

        Properties prop=new Properties();
        prop.load(is);

        String user=prop.getProperty("username");
        String pwd=prop.getProperty("password");

        resp.getWriter().print(user+":"+pwd);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```





### HttpServletResponse

负责向浏览器发送数据的方法

```java
ServletOutPutStream getOutputStream() throws IOException;
PrintWriter getWriter() throws IOException;
```



常见应用：

1.向浏览器输出消息

2.下载文件



```java
com.acow.servlet;

import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.FileInputStream;
import java.io.IOException;
import java.net.URLEncoder;

public class FileServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.要获取下载文件的路径
        String realPath="C:\\Users\\jinyu\\IdeaProjects\\com.acow.practice\\response\\target\\classes\\我1.png";
        System.out.println("下载文件的路径："+realPath);
        //2.下载的文件明是啥？
        String filename=realPath.substring(realPath.lastIndexOf("\\")+1);
        //3.设置想办法让浏览器能够支持（Content-Disposition）下载我们需要的东西,中文文件名URLEncoder.encode编码，否则会乱码
        resp.setHeader("Content-Disposition","attachment;filename="+ URLEncoder.encode(filename,"UTF-8"));
        //4.获取下载文件的输入流
        FileInputStream in = new FileInputStream(realPath);
        //5.创建缓冲区
        int len=0;
        byte[] buffer = new byte[1024];
        //6.获取OutputStream对象
        ServletOutputStream out=resp.getOutputStream();
        //7.将FileOutputStream流写入到buffer缓冲区,使用OutputStream将缓冲区中的数据输出到客户端
        while((len=in.read(buffer))>0){
            out.write(buffer,0,len);
        }

        in.close();
        out.close();

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



### Response验证码实现

```java
package com.acow.servlet;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;

public class ImageServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //让浏览器3秒自动刷新一次
        resp.setHeader("refresh","3");

        //在内存中创建一个图片
        BufferedImage image=new BufferedImage(80,20,BufferedImage.TYPE_INT_RGB);
        //得到图片
        Graphics2D g=(Graphics2D)image.getGraphics();//笔
        //设置图片的背景颜色
        g.setColor(Color.white);
        g.fillRect(0,0,80,20);
        //给图片写数据
        g.setColor(Color.BLUE);
        g.setFont(new Font(null,Font.BOLD,20));
        g.drawString(makeNum(),0,20);

        //告诉浏览器，这个请求用图片的方式打开
        resp.setContentType("image/jpeg");
        //网站存在缓存，不让浏览器缓存
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");

        //把图片写给浏览器
        ImageIO.write(image,"jpg",resp.getOutputStream());

    }

    //生成随机数
    private String makeNum(){
        Random random=new Random();
        String num = random.nextInt(99999999) + "";
        StringBuffer sb=new StringBuffer();
        for (int i = 0; i < 7-num.length(); i++) {
            sb.append("0");
        }
        num=sb.toString()+num;
        return num;

    }


    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



### 重定向

​		重定向和转发的区别？

相同点：

 		1.页面都会实现跳转

不同点：

​		1.请求转发的时候，url不会产生变化

​		2.重定向，url会变

```java
package com.acow.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class RedirectServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.sendRedirect("/r/img");//重定向
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```





```html
<html>
<body>
<h2>Hello World!</h2>

<%--//pageContext.request.contextPath代表当前的项目--%>
<form action="${pageContext.request.contextPath}/login" method="get">
        用户名：<input type="text" name="username"><br>
        密码：<input type="password" name="password"><br>
        <input type="submit">
<form>
</body>
</html>
```



```java
package com.acow.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class RequestTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //处理请求
        String username=req.getParameter("username");
        String password=req.getParameter("password");

        System.out.println(username+":"+password);

        resp.sendRedirect("/r/success.jsp");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```



### HttpServletRequest



```java
package com.acow.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Arrays;

public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");

        String username=req.getParameter("username");
        String password=req.getParameter("password");
        String[] hobbies=req.getParameterValues("hobbies");
        System.out.println("=====================");
        System.out.println(username);
        System.out.println(password);
        System.out.println(Arrays.toString(hobbies));
        System.out.println("=====================");

        //通过请求转发
        req.getRequestDispatcher("/success.jsp").forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```





### Cookie

```java
package com.acow.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Date;

public class CookieDemo01 extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决中文乱码
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");

        PrintWriter out=resp.getWriter();

        //Cookie，服务器从客户端获取；
        Cookie[] cookies=req.getCookies();//这里返回数组，说明Cookie可能存在多个

        //判断Cookie是否存在
        if(cookies!=null){
            //如果存在怎么办
            out.write("你上一次访问的时间是：");

            for (int i = 0; i < cookies.length; i++) {
                Cookie cookie=cookies[i];
                //获取cookie的名字
                if(cookie.getName().equals("lastLoginTime")){
                    //获取cookie中的值
                    long lastLoginTime=Long.parseLong(cookie.getValue());
                    Date date=new Date(lastLoginTime);
                    out.write(date.toLocaleString());
                }
            }

        }else{
            out.write("这是您第一次访问本站");
        }

        //服务给客户端响应一个cookie;
        Cookie cookie=new Cookie("lastLoginTime",System.currentTimeMillis()+"");

        //cookie有效期为1天
        cookie.setMaxAge(24*60*60);

        resp.addCookie(cookie);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}



```

1.从请求中拿到cookie的信息

2.服务器响应给客户端cookie

```
Cookie[] cookies=req.getCookies(); //获得cookie
cookie.getName(); //获得cookie中的key 
cookie.getValue(); //获得cookie中的value
new Cookie("lastLoginTime",System.currentTimemillis()+""); //新建一个cookie
cookie.setMaxAge(24*60*60); //设置cookie的有效期
resp.addCookie(cookie); //响应给客户端一个cookie
```

cookie:一般会保存在本地的 用户目录下 appdata;







### Session



```java
package com.acow.servlet;

import com.acow.pojo.Person;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class SessionDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码的问题
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session =req.getSession();

        //给Session中存东西
        session.setAttribute("name",new Person("jjj",20));

        //获取Session中的ID
        String sessionId=session.getId();

        //判断Session是不是新创建
        if(session.isNew()){
            resp.getWriter().write("session创建成功，ID"+sessionId);
        }else{
            resp.getWriter().write("session以及在服务器中存在了，ID"+sessionId);
        }


    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       doGet(req, resp);
    }
}

```



```java
package com.acow.servlet;

import com.acow.pojo.Person;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class SessionDemo02 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码的问题
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session=req.getSession();

        Person person=(Person)session.getAttribute("name");

        System.out.println(person.toString());

    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }
}
```







```java
package com.acow.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class SessionDemo03 extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session=req.getSession();
        session.removeAttribute("name");
        session.invalidate();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```





设置session默认的失效时间

```html
<session-config>
	<!--15分钟后session自动失效-->
	<session-timeout>15</session-timeout>
</session-config>
```







Session和cookie的区别:

​		1.cookie是把用户的数据写给用户的浏览器，浏览器保存（可以保存多个）

​		2.Session把用户的数据写到用户独占的Session中，服务器端保存（保存重要的信息，减少服务器资源的浪费）

  	3.Session对象由服务器创建



使用场景：

​		1.保存一个登录用户的信息

​		2.购物车信息

​		3.在整个网站中，经常会使用的数据

### JSP



常用标识

```jsp
<%= %>
<%  %>
<%! %>

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page import="java.util.*" %>

<%--定制错误页面--%>
<%@ page errorPage="error/500.jsp" %>

<%@include file="common/header.jsp"%>
```



```html
<error-page>
  <error-code>404</error-code>
  <location>/error/404.jsp</location>
</error-page>
```



JSP：内置对象

```jsp
<html>
<body>
<h2>Hello World!</h2>
<%@ page import="java.util.*" %>

<%
    pageContext.setAttribute("name1","A");//保存的数据只在一个页面中有效
    request.setAttribute("name2","B");//保存的数据只在一次请求中有效，请求转发会携带这个数据
    session.setAttribute("name3","C");//保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器
    application.setAttribute("name4","D");//保存的数据在服务器有效，从打开服务器到关闭服务器
%>

<%
    String name1=(String)pageContext.findAttribute("name1");
    String name2=(String)pageContext.findAttribute("name2");
    String name3=(String)pageContext.findAttribute("name3");
    String name4=(String)pageContext.findAttribute("name4");
%>

<h1>取出的值为：</h1>
<h3>${name1}</h3>
<h3>${name2}</h3>
<h3>${name3}</h3>
<h3>${name4}</h3>


</body>
</html>
```



页面转换

```jsp
<%
		pageContent.forward("/index.jsp");
		//request.getRequestDispatcher("index.jsp").forward(request,response);
%>
```



request:客户端向服务器发送请求，产生的数据，用户看完就没用了

session:客户端向服务器发送请求，产生的数据，用户用完一会还有用，如：购物车

application:客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户还可能使用，如：聊天数据





```jsp
<jsp:forward page="/success.jsp">
    <jsp:param name="name" value="acow"/>
    <jsp:param name="age" value="12"/>
</jsp:forward>
```



### JSTL标签

jstl表达式的依赖：

```html
<dependency>
  <groupId>javax.servlet.jsp.jstl</groupId>
  <artifactId>jstl-api</artifactId>
  <version>1.2</version>
</dependency>
```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false"%>
```





jstl标签库的使用步骤：

​		1.引入对应的taglib

​		2.使用其中的方法

​		3.在Tomcat也需要引入jstl的包,否则会报错





```jsp
<form action="coreif.jsp" method="get">
    <%--
    EL表达式获取表单中的数据
    --%>
    <input type="text" name="username" value="${param.username}">
    <input type="submit" value="登录">
</form>
```





管理员判断：

```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false"%>
<html>
<head>
    <title>Title</title>
</head>
<body>

<h4>if测试</h4>

<hr>

<form action="coreif.jsp" method="get">
    <%--
    EL表达式获取表单中的数据
    --%>
    <input type="text" name="username" value="${param.username}">
    <input type="submit" value="登录">
</form>

<c:if test="${param.username=='admin'}" var="isAdmin">
    <c:out value="管理员欢迎您！"/>
</c:if>

<c:out value="${isAdmin}"/>


</body>
</html>
```





foreach

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page import="java.util.ArrayList" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%
    ArrayList<String> people=new ArrayList<>();
    people.add(1,"A");
    people.add(2,"B");
    people.add(3,"C");
    people.add(4,"D");
    request.setAttribute("list",people);
%>

<%--
    var,每一次遍历出来的变量
    items,要遍历的对象
--%>    
    
<c:forEach var="people" items="${list}">
    <c:out value="${people}"/> <br>
</c:forEach>


</body>
</html>
```



javabean

```jsp
<jsp:useBean id="people" class="com.acow.test.People" scope="page"/>

<jsp:setProperty name="people" property="address" value="nanjing"></jsp:setProperty>
<jsp:setProperty name="people" property="id" value="1"></jsp:setProperty>
<jsp:setProperty name="people" property="age" value="3"></jsp:setProperty>
<jsp:setProperty name="people" property="name" value="acow"></jsp:setProperty>
```





### MVC三层架构

![image-20210306155725895](C:\Users\jinyu\AppData\Roaming\Typora\typora-user-images\image-20210306155725895.png)



Model：

​		1.业务处理：业务逻辑（Service）

​		2.数据持久层：CRUD (Dao)

View:

​		1.展示数据

​		2.提供链接发起的Servlet请求

Controller(Servlet)

​		1.接受用户的请求：（req:请求参数、Session信息...）

​		2.交给业务层处理对应的代码

​		3.控制试图的跳转





### 过滤器

```java
package com.acow.filter;

import javax.servlet.*;
import java.io.IOException;

public class CharacterEncodingFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("CharacterEncodingFilter初始化");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        servletRequest.setCharacterEncoding("utf-8");
        servletResponse.setCharacterEncoding("utf-8");
        servletResponse.setContentType("text/html;charset=UTF-8");

        System.out.println("CharacterEncodingFilter执行前......");
        filterChain.doFilter(servletRequest,servletResponse);//让我们的请求继续走，如果不写，程序到这里就被拦截停止！
        System.out.println("CharacterEncodingFilter执行后......");

    }

    @Override
    public void destroy() {
        System.out.println("CharacterEncodingFilter销毁");
    }
}
```





```html
<filter>
  <filter-name>CharacterEncodingFilter</filter-name>
  <filter-class>com.acow.filter.CharacterEncodingFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>CharacterEncodingFilter</filter-name>
  <url-pattern>/servlet/*</url-pattern>
</filter-mapping>
```

### 监听器

```java
package com.acow.listener;

import javax.servlet.Servlet;
import javax.servlet.ServletContext;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class OnlineCountListener implements HttpSessionListener
{

    //一旦创建Session就会触发一次这个事件
    @Override
    public void sessionCreated(HttpSessionEvent se) {
        ServletContext ctx=se.getSession().getServletContext();

        Integer onlineCount=(Integer) ctx.getAttribute("OnlineCount");

        if(onlineCount==null){
            onlineCount=1;
        }else{
            onlineCount=onlineCount+1;
        }

        ctx.setAttribute("OnlineCount",onlineCount);

    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        ServletContext ctx=se.getSession().getServletContext();

        Integer onlineCount=(Integer) ctx.getAttribute("OnlineCount");

        if(onlineCount==null){
            onlineCount=0;
        }else{
            onlineCount=onlineCount-1;
        }

        ctx.setAttribute("OnlineCount",onlineCount);

    }
}
```



测试表单：

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

    <h1>当前有 <span><%=this.getServletConfig().getServletContext().getAttribute("onlineCount")%></span>
人在线

</body>
</html>
```



注册监听器：

```html
<listener>
  <listener-class>com.acow.listener.OnlineCountListener</listener-class>
</listener>
```