---
title: JavaWeb
date: 2021-05-16 18:58:37
tags: javaweb
categories: java web
---
**前言：**看了B站小狂神的javaweb写的笔记，虽然这种大众化的笔记csdn.....等等水文多得是，但是大致看了一眼，都是cv大法，并没有什么参考价值，里面很多东西都是缺头少尾的，当然我也可能是没有发现好的文章，看了十几篇关于狂神的javaweb感觉都是一篇文章的复制品，而且里面记录的真的是缺头少尾，还是自己整理一份属于自己的javaweb笔记比较好，那么下面一起来看看我整理的这篇web笔记吧！

## 1、服务器

web开发，换而言之就是少不了服务器那么我们就从最基本的开始讲解吧，首先我们需要配置一个服务器，对于我来说首选Tomcat。这个参考我的另外一篇文章即可，[javaweb&Tomcat简介](http://www.huangwan.run/2020/08/08/Tomcat/)。

## 2、Http

### 2.1、什么是HTTP

(超文本传输协议）是一个简单的请求-响应协议，它通常运行在TCP之上。

🌂文本：html，字符串，…
🌂超文本：图片，音乐，视频，定位，地图.……
🌂端口:80
**Https:安全的**

### 2.2、两个时代

**http1.0：**客户端可以与web服务器连接后，只能获得一个web资源，断开连接
**http2.0：**客户端可以与web服务器连接后，可以获得多个web资源。

### 2.3、Http请求

🌂客户端–发请求（Request）–服务器
**百度：**

```
Request URL:https://www.baidu.com/   请求地址
Request Method:GET    get方法/post方法
Status Code:200 OK    状态码：200
Remote（远程） Address:14.215.177.39:443

Accept:text/html  
Accept-Encoding:gzip, deflate, br
Accept-Language:zh-CN,zh;q=0.9    语言
Cache-Control:max-age=0
Connection:keep-alive
```

#### 1、请求行

🌂请求行中的请求方式：GET
🌂请求方式：Get,Post,HEAD,DELETE,PUT,TRACT.…
     get：请求能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效
     post:请求能够携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效。

#### 2、消息头

```
Accept：告诉浏览器，它所支持的数据类型
Accept-Encoding：支持哪种编码格式  GBK   UTF-8   GB2312  ISO8859-1
Accept-Language：告诉浏览器，它的语言环境
Cache-Control：缓存控制
Connection：告诉浏览器，请求完成是断开还是保持连接
HOST：主机..../.
```

### 2.4、Http响应

🌂服务器–响应…….客户端
**百度：**

```
Cache-Control:private    缓存控制
Connection:Keep-Alive    连接
Content-Encoding:gzip    编码
Content-Type:text/html   类型 
```

#### 1、响应体

```
Accept：告诉浏览器，它所支持的数据类型
Accept-Encoding：支持哪种编码格式  GBK   UTF-8   GB2312  ISO8859-1
Accept-Language：告诉浏览器，它的语言环境
Cache-Control：缓存控制
Connection：告诉浏览器，请求完成是断开还是保持连接
HOST：主机..../.
Refresh：告诉客户端，多久刷新一次；
Location：让网页重新定位；
```



#### 2、响应状态码

```
200：请求响应成功200
3xx:请求重定向·重定向：你重新到我给你新位置去；
4xx:找不到资源404·资源不存在；
5xx:服务器代码错误 500 502:网关错误
```

**常见面试题：**
当你的浏览器中地址栏输入地址并回车的一瞬间到页面能够展示回来，经历了什么？

## 3、Maven

**我为什么要学习这个技术？**

🌂在Javaweb开发中，需要使用大量的jar包，我们手动去导入；
🌂如何能够让一个东西自动帮我导入和配置这个jar包。

### 3.1 Maven项目架构管理工具

**Maven的核心思想：约定大于配置**

有约束，不要去违反。
Maven会规定好你该如何去编写我们Java代码，必须要按照这个规范来；

### 3.2下载安装Maven

下载地址：[官网](https://maven.apache.org/)

![](https://img-blog.csdnimg.cn/20210509145137881.png)

### 3.3配置环境变量

在我们的系统环境变量中配置如下配置：

- M2_HOME maven目录下的bin目录
- MAVEN_HOME maven的目录
- 在系统的path中配置%MAVEN_HOME%\bin

配置完成之后win+r输入cmd进入命令窗口测试安装是否完成

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210509145821257.png)

### 3.4阿里云镜像

- 镜像：mirrors
- 作用：加速我们的下载
- 国内建议使用阿里云的镜像

需要打开安装maven目录下面的conf文件夹下面的settings.xml修改下载镜像，并且在maven安装目录下建立一个本地仓库：repository

```xml
  <mirror>
     <id>nexus-aliyun</id>
     <mirrorOf>*</mirrorOf>
     <name>Nexus aliyun</name>
     <url>http://maven.aliyun.com/nexus/content/groups/public</url>
 </mirror>
```

配置本地仓库地址：

```xml
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
<localRepository>C:\Maven\apache-maven-3.8.1\repository</localRepository>
```

### 3.4在IntelliJ IDEA的配置

此处想着小白也会吧，就懒得放截图了.............

## 4、Servlet

### 4.1、Servlet简介

Servlet就是sun公司开发动态web的一门技术。
Sun在这些API中提供一个接口叫做：Servlet，如果你想开发一个Servlet程序，只需要完成两个小步骤：
        🌂编写一个类，实现Servlet接口
        🌂把开发好的Java类部署到web服务器中。
**把实现了Servlet接口的Java程序叫做，Servlet**

### 4.2、HelloServlet

Serlvet接口Sun公司有两个默认的实现类：HttpServlet，GenericServlet
       

### 4.3、编写一个Servlet程序

编写一个普通类

实现Servlet接口，这里我们直接继承HttpServlet

```
public class HelloServlet extends HttpServlet {
    
    //由于get或者post只是请求实现的不同的方式，可以相互调用，业务逻辑都一样；
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //ServletOutputStream outputStream = resp.getOutputStream();
        PrintWriter writer = resp.getWriter(); //响应流
        writer.print("Hello,Serlvet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

**编写Servlet的映射**

为什么需要映射：我们写的是JAVA程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要再web服务中注册我们写的Servlet，还需给他一个浏览器能够访问的路径；

```xml
<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <!--注册Servlet-->
  <servlet>
    <servlet-name>hello</servlet-name>
    <servlet-class>com.huang.servlet.HelloServlet</servlet-class>
  </servlet>
  <!--Servlet的请求路径-->
  <servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>/hello</url-pattern>
  </servlet-mapping>

</web-app>
```

然后就是配置一下Tomcat发布项目，就可以访问啦！

### 4.4、Servlet原理

Servlet是由Web服务器调用，web服务器在收到浏览器请求之后，会：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210509152042950.png)

4.4、Mapping问题

1.一个Servlet可以指定一个映射路径

```
<servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
```



```
<servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello/*</url-pattern>
    </servlet-mapping>
```


3.默认请求路径

```
  <!--默认请求路径-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>
```


4.指定一些后缀或者前缀等等….

```
<!--可以自定义后缀实现请求映射
    注意点，*前面不能加项目映射的路径
    hello/sajdlkajda.qinjiang
    -->
<servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>*.huangwan</url-pattern>
</servlet-mapping>
```


优先级问题 指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求；

```
<!--404-->
<servlet>
    <servlet-name>error</servlet-name>
    <servlet-class>com.kuang.servlet.ErrorServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>error</servlet-name>
    <url-pattern>/*</url-pattern>
</servlet-mapping>

```

### 4.5、ServletContext

web容器在启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用；

#### 1、共享数据

我在这个Servlet中保存的数据，可以在另外一个servlet中拿到；

```servlet
package com.huang.servlet;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //this.getInitParameter()   初始化参数
        //this.getServletConfig()   Servlet配置
        //this.getServletContext()  Servlet上下文
        ServletContext context = this.getServletContext();

        String username = "黄皖"; //数据
        context.setAttribute("username",username); //将一个数据保存在了ServletContext中，名字为：username 值 username

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}

```

```servlet
package com.huang.servlet;

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
        String username = (String) context.getAttribute("username");

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

配置web.xml

```xml
<servlet>
    <servlet-name>ServletContext</servlet-name>
    <servlet-class>com.huang.servlet.GetServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>ServletContext</servlet-name>
    <url-pattern>/slc</url-pattern>
  </servlet-mapping>
```

#### 2、获取初始化参数

web.xml中（配置和注册都写进去啦）

     <context-param>
        <param-name>url</param-name>
        <param-value>https://huangwan.run</param-value>
      </context-param>
    
      <servlet>
        <servlet-name>demo02</servlet-name>
        <servlet-class>com.huang.servlet.TestDemo02</servlet-class>
      </servlet>
      <servlet-mapping>
        <servlet-name>demo02</servlet-name>
        <url-pattern>/02</url-pattern>
      </servlet-mapping>


​    

获取

	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		ServletContext context = this.getServletContext();
		String url = context.getInitParameter("url");
		resp.getWriter().print(url);
	}

#### 3、请求转发

```servle
package com.huang.servlet;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

//3、请求转发
public class TestDemo03 extends HelloServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        System.out.println("进入了TestDemo02");
        RequestDispatcher requestDispatcher = context.getRequestDispatcher("/02");//转发的请求路径
        requestDispatcher.forward(req,resp);//调用forward实现请求转发
        //合并写  context.getRequestDispatcher("/gp").forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

web.xml

```xml
<servlet>
    <servlet-name>demo03</servlet-name>
    <servlet-class>com.huang.servlet.TestDemo03</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>demo03</servlet-name>
    <url-pattern>/03</url-pattern>
  </servlet-mapping>

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021050917324319.png)

#### 4、读取资源文件

项目结构构造图如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210509174350831.png)

```servlet
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        InputStream is = this.getServletContext().getResourceAsStream("/WEB-INF/classes/com/huang/servlet/hw.properties");

        Properties prop = new Properties();
        prop.load(is);
        String user = prop.getProperty("username");
        String pwd = prop.getProperty("password");

        resp.getWriter().print(user+":"+pwd);
    }
```

### 4.6、HttpServletResponse

```
web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest
对象，代表响应的一个HttpServletResponse；
```

如果要获取客户端请求过来的参数：找HttpServletRequest
如果要给客户端响应一些信息：找HttpServletResponse

#### 1、简单分类

负责向浏览器发送数据的方法

```
 servletOutputstream getOutputstream() throws IOException;
    Printwriter getwriter() throws IOException;
```

负责向浏览器发送响应头的方法

```
void setCharacterEncoding(String var1)；
void setContentLength(int var1)；
void setContentLengthLong(long var1);
void setContentType(String var1)；
void setDateHeader(String varl,long var2)
void addDateHeader(String var1,long var2)
void setHeader(String var1,String var2);
void addHeader(String var1,String var2)；
void setIntHeader(String var1,int var2);
void addIntHeader(String varl,int var2);
```

#### 2、下载文件

1.向浏览器输出消息（一直在讲，就不说了）
2.下载文件
   🌂要获取下载文件的路径
   🌂下载的文件名是啥？
   🌂设置想办法让浏览器能够支持下载我们需要的东西
   🌂获取下载文件的输入流
   🌂创建缓冲区
   🌂获取OutputStream对象
   🌂将FileOutputStream流写入到bufer缓冲区
   🌂使用OutputStream将缓冲区中的数据输出到客户端！

代码如下：

```
package com.huang.servlet;

import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.URLEncoder;
import java.util.Properties;

public class TestDemo05 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1. 要获取下载文件的路径
        String realPath = "C:\\JavaDemo\\java-web\\javaweb-servlet\\src\\main\\resources\\hw.png";
        System.out.println("下载文件的路径："+realPath);
        // 2. 下载的文件名是啥？
        String fileName = realPath.substring(realPath.lastIndexOf("\\") + 1);
        // 3. 设置想办法让浏览器能够支持(Content-Disposition)下载我们需要的东西,中文文件名URLEncoder.encode编码，否则有可能乱码
        resp.setHeader("Content-Disposition","attachment;filename="+ URLEncoder.encode(fileName,"UTF-8"));
        // 4. 获取下载文件的输入流
        FileInputStream in = new FileInputStream(realPath);
        // 5. 创建缓冲区
        int len = 0;
        byte[] buffer = new byte[1024];
        // 6. 获取OutputStream对象
        ServletOutputStream out = resp.getOutputStream();
        // 7. 将FileOutputStream流写入到buffer缓冲区,使用OutputStream将缓冲区中的数据输出到客户端！
        while ((len=in.read(buffer))>0){
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

**注意：**记得在resources下面放资源以及在web.xml中配置

#### 3、验证码功能

验证怎么来的?

- 前端实现

- 后端实现，需要用到Java的图片类，生产一个图片

  ```
  package com.huang.servlet;
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
  
          //如何让浏览器3秒自动刷新一次;
          resp.setHeader("refresh","3");
  
          //在内存中创建一个图片
          BufferedImage image = new BufferedImage(80,20,BufferedImage.TYPE_INT_RGB);
          //得到图片
          Graphics2D g = (Graphics2D) image.getGraphics(); //笔
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
          ImageIO.write(image,"jpg", resp.getOutputStream());
  
      }
  
      //生成随机数
      private String makeNum(){
          Random random = new Random();
          String num = random.nextInt(9999999) + "";
          StringBuffer sb = new StringBuffer();
          for (int i = 0; i < 7-num.length() ; i++) {
              sb.append("0");
          }
          num = sb.toString() + num;
          return num;
      }
  
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          doGet(req, resp);
      }
  }
  
  
  ```

  ```
   <servlet>
      <servlet-name>ImageServlet</servlet-name>
      <servlet-class>com.huang.servlet.ImageServlet</servlet-class>
    </servlet>
    <servlet-mapping>
      <servlet-name>ImageServlet</servlet-name>
      <url-pattern>/img</url-pattern>
    </servlet-mapping>
  ```

  

#### 4、实现重定向

下面随便写一个小测试看看重定向的规律这里需要配置web.xml，并且需要一个已经配好了的地址，例如**/img**

```
package com.huang.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class Resp extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp. sendRedirect("/img");//重定向
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

下面写一个用重定向实现登陆的小例子吧，嘻嘻，就是如此的皮，来打我呀，有没有被可爱到呀，嘻嘻嘻.........

**index.jsp**

```jsp
<%@ page contentType="text/html; charset=UTF-8" language="java" %>
<html>
<body>
<h2>Hel1o World!</h2>

《%--这里超交的路径,需要寻找到项目的路径--%>
<%--${pageContext. request, contextPath}代表当前的项目--%>
<form action="${pageContext. request.contextPath}/login" method="get">
    用户名: <input type="text" name="username"> <br>
    密码: <input type="password" name="password"> <br>
    <input type="submit">
</form>

</body>
</html>

```


**RequestTest.java**

```
package com.huang.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class RequestTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //处理方求
        String username = req.getParameter("username");
        String password = req.getParameter( "password");

        System.out.println(username+":"+password);

        resp.sendRedirect("/success.jsp");
    }


    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```


**重定向页面success.jsp**

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Apple
  Date: 2021/5/9
  Time: 20:12
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html; charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>success</h1>
<h2>这就是一个大大的成功！</h2>
<h2>注意看地址栏是否变化了！！</h2>
</body>
</html>

```

**web.xml配置**

```xml
  <servlet>
    <servlet-name>requset</servlet-name>
    <servlet-class>com.huang.servlet.RequestTest</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>requset</servlet-name>
    <url-pattern>/login</url-pattern>
  </servlet-mapping>
```

**注意：**这里有个小坑，新建的jsp文件一定是在webapp下面的，而不是在WEB-INF下面，这个一定要注意，不然就会报404错误找不到资源，资源未公开。

**面试题：请你聊聊重定向和转发的区别？**

🌂相同点

- 页面都会实现跳转

🌂不同点

- 请求转发的时候，url不会产生变化
- 重定向时候，url地址栏会发生变化；

### 4.7、HttpServletRequest

HttpServletRequest代表客户端的请求,用户通过Http协议访问服务器, HTTP请求中的所有信息会被封装到HttpServletRequest,通过这个HttpServletRequest的方法,获得客户端的所有信息;

```
package com.huang.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Arrays;

public class HttpServletRequest extends HttpServlet {
    @Override
    protected void doGet(javax.servlet.http.HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobbys = req.getParameterValues("hobbys");
        System.out.println("==========");
        //后台接收中文乱码问题
        System. out.println(username);
        System. out.p
        
        rintln(password);
        System. out.println(Arrays.toString(hobbys));
        System. out.println("============");
        System. out.println(req.getContextPath());
        //通过请求转发
        //这里的/代表当前的web应用
        req.getRequestDispatcher("/success.jsp").forward(req,resp);
    }

    @Override
    protected void doPost(javax.servlet.http.HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

## 5、Cookie、Session

#### 5.1、会话

**会话：**用户打开一个浏览器，点击了很多超链接，访问多个web资源，关闭浏览器，这个过程可以称之为会话；

**有状态会话：**一个同学来过教室，下次再来教室，我们会知道这个同学，曾经来过，称之为有状态会话；

**客户端 服务端**

服务端给客户端一个 信件，客户端下次访问服务端带上信件就可以了； cookie
服务器登记你来过了，下次你来的时候我来匹配你； seesion

#### 5.2、保存会话的两种技术

**cookie：**客户端技术 （响应，请求）
**session：**服务器技术，利用这个技术，可以保存用户的会话信息？ 我们可以把信息或者数据放在Session中！
                  常见常见：网站登录之后，你下次不用再登录了，第二次访问直接就上去了！

#### 5.3、Cookie

![](https://img-blog.csdnimg.cn/20210511122646700.png)

1.从请求中拿到cookie信息
2.服务器响应给客户端cookie

```
Cookie[] cookies = req.getCookies(); //获得Cookie
cookie.getName(); //获得cookie中的key
cookie.getValue(); //获得cookie中的vlaue
new Cookie("lastLoginTime", System.currentTimeMillis()+""); //新建一个cookie
cookie.setMaxAge(24*60*60); //设置cookie的有效期
resp.addCookie(cookie); //响应给客户端一个cookie
```

**cookie：**一般会保存在本地的 用户目录下 appdata；

一个网站cookie是否存在上限？

🌂一个Cookie只能保存一个信息；
🌂一个web站点可以给浏览器发送多个cookie，最多存放20个cookie；
🌂Cookie大小有限制4kb；
🌂300个cookie浏览器上限

**删除Cookie**

🌂不设置有效期，关闭浏览器，自动失效；
🌂设置有效期时间为 0 ；
**编码解码：**

```
URLEncoder.encode("黄皖","utf-8")
URLDecoder.decode(cookie.getValue(),"UTF-8")
```



#### 5.4、Session（重点）

**什么是Session：**

服务器会给每一个用户（浏览器）创建一个Seesion对象；
一个Seesion独占一个浏览器，只要浏览器没有关闭，这个Session就存在；
用户登录之后，整个网站它都可以访问！–> 保存用户的信息；保存购物车的信息……
…
…
…

**Session和cookie的区别：**

Cookie是把用户的数据写给用户的浏览器，浏览器保存 （可以保存多个）
Session把用户的数据写到用户独占Session中，服务器端保存 （保存重要的信息，减少服务器资源的浪费）
Session对象由服务创建；

**使用场景：**

保存一个登录用户的信息；
购物车信息；
在整个网站中经常会使用的数据，我们将它保存在Session中；

![](https://img-blog.csdnimg.cn/2021051113451353.png)

**使用Session：**

新建两个包，分别为session和pojo，以及在web.xml中注册和配置session过期时间。

**session.java**

```
package com.huang.session;

import com.huang.pojo.Person;

import javax.servlet.ServletException;
import javax.servlet.http.*;
import java.io.IOException;

public class Session extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //解决乱码问题
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session = req.getSession();
        //给Session中存东西
        session.setAttribute("name",new Person("黄皖",1));
        //获取Session的ID
        String sessionId = session.getId();

        //判断Session是不是新创建
        if (session.isNew()){
            resp.getWriter().write("session创建成功,ID:"+sessionId);
        }else {
            resp.getWriter().write("session以及在服务器中存在了,ID:"+sessionId);
        }

        //Session创建的时候做了什么事情；
//        Cookie cookie = new Cookie("JSESSIONID",sessionId);
//        resp.addCookie(cookie);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

**session2.java**

```
package com.huang.session;

import com.huang.pojo.Person;

import javax.servlet.ServletException;
import javax.servlet.http.*;
import java.io.IOException;

public class Session extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //解决乱码问题
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session = req.getSession();
        //给Session中存东西
        session.setAttribute("name",new Person("黄皖",1));
        //获取Session的ID
        String sessionId = session.getId();

        //判断Session是不是新创建
        if (session.isNew()){
            resp.getWriter().write("session创建成功,ID:"+sessionId);
        }else {
            resp.getWriter().write("session以及在服务器中存在了,ID:"+sessionId);
        }

        //Session创建的时候做了什么事情；
//        Cookie cookie = new Cookie("JSESSIONID",sessionId);
//        resp.addCookie(cookie);

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

**person.java**

```Servlet
package com.huang.pojo;

public class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```

**web.xml**

```
  <servlet>
    <servlet-name>Session</servlet-name>
    <servlet-class>com.huang.session.Session</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>Session</servlet-name>
    <url-pattern>/s1</url-pattern>
  </servlet-mapping>

  <servlet>
    <servlet-name>Session2</servlet-name>
    <servlet-class>com.huang.session.Session2</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>Session2</servlet-name>
    <url-pattern>/s2</url-pattern>
  </servlet-mapping>
```

##### 手动配置过期时间

手动配置session过期时间，也就是只要打开这个就自动过期，再打开以前的网页又会重新生成一个session，下面看看看简单的生成代码吧，这个web.xml的注册在上面，放在一起了。

```
package com.huang.session;

import com.huang.pojo.Person;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class Session2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //得到Session
        HttpSession session = req.getSession();

        Person person = (Person) session.getAttribute("name");

        System.out.println(person.toString());

        HttpSession Session = req.getSession();
        session.removeAttribute("name");
//手动注销Session
        session.invalidate();

    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

##### 自动配置过期时间

```
<!--设置Session默认的失效时间-->
<session-config>
    <!--15分钟后Session自动失效，以分钟为单位-->
    <session-timeout>15</session-timeout>
</session-confi
```

## 6、JSP

#### 6.1、什么是JSP

Java Server Pages ： Java服务器端页面，也和Servlet一样，用于动态Web技术！

**最大的特点：**

写JSP就像在写HTML
**区别：**
 🌂HTML只给用户提供静态的数据
 🌂JSP页面中可以嵌入JAVA代码，为用户提供动态数据；

#### 6.2、JSP原理

**思路：**JSP到底怎么执行的！

1.代码层面没有任何问题

2.服务器内部工作

​     🌂tomcat中有一个work目录；

​     🌂IDEA中使用Tomcat的会在IDEA的tomcat中生产一个work目录
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021051114105383.png)

浏览器向服务器发送请求，不管访问什么资源，其实都是在访问Servlet！

JSP最终也会被转换成为一个Java类！

JSP 本质上就是一个Servlet

```
//初始化
  public void _jspInit() {
      
  }
//销毁
  public void _jspDestroy() {
  }
//JSPService
  public void _jspService(.HttpServletRequest request,HttpServletResponse response)
```

##### 1.判断请求

##### 2.内置一些对象

```
final javax.servlet.jsp.PageContext pageContext;  //页面上下文
javax.servlet.http.HttpSession session = null;    //session
final javax.servlet.ServletContext application;   //applicationContext
final javax.servlet.ServletConfig config;         //config
javax.servlet.jsp.JspWriter out = null;           //out
final java.lang.Object page = this;               //page：当前
HttpServletRequest request                        //请求
HttpServletResponse response                      //响应
```

##### 3.输出页面前增加的代码

```
response.setContentType("text/html");       //设置响应的页面类型
pageContext = _jspxFactory.getPageContext(this, request, response,
       null, true, 8192, true);
_jspx_page_context = pageContext;
application = pageContext.getServletContext();
config = pageContext.getServletConfig();
session = pageContext.getSession();
out = pageContext.getOut();
_jspx_out = out;
```

##### 4.直接使用

以上的这些个对象我们可以在JSP页面中直接使用！
其实就是jsp>java>class(Servlet)

在JSP页面中:只要是 JAVA代码就会原封不动的输出；

如果是HTML代码，就会被转换为：

```java
out.write("<html>\r\n");
```

这样的格式，输出到前端！

#### 6.3、JSP基础语法

任何语言都有自己的语法，JAVA中有,。 JSP 作为java技术的一种应用，它拥有一些自己扩充的语法（了解，知道即可！），Java所有语法都支持！

**JSP表达式**

```
  <%--JSP表达式
  作用：用来将程序的输出，输出到客户端
  <%= 变量或者表达式%>
  --%>
  <%= new java.util.Date()%>
```

**jsp脚本片段**

  

```
<%--jsp脚本片段--%>
  <%
    int sum = 0;
    for (int i = 1; i <=100 ; i++) {
      sum+=i;
    }
    out.println("<h1>Sum="+sum+"</h1>");
  %>
```

**脚本片段的再实现**

```
  <%
    int x = 10;
    out.println(x);
  %>
  <p>这是一个JSP文档</p>
  <%
    int y = 2;
    out.println(y);
  %>

  <hr>


  <%--在代码嵌入HTML元素--%>
  <%
    for (int i = 0; i < 5; i++) {
  %>
    <h1>Hello,World  <%=i%> </h1>
  <%
    }
  %>
```

**JSP声明**

```
<%!
    static {
      System.out.println("Loading Servlet!");
    }

    private int globalVar = 0;
    
    public void kuang(){
      System.out.println("进入了方法Kuang！");
    }

  %>
```


JSP声明：会被编译到JSP生成Java的类中！其他的，就会被生成到_jspService方法中！

在JSP，嵌入Java代码即可！

```
<%%>
<%=%>
<%!%>
<%--注释--%>
```


JSP的注释，不会在客户端显示，HTML就会！

#### 6.4、JSP指令

```
<%@page args.... %>
<%@include file=""%>

<%--@include会将两个页面合二为一--%>

<%@include file="common/header.jsp"%>

<h1>网页主体</h1>

<%@include file="common/footer.jsp"%>

<hr>

<%--jSP标签
    jsp:include：拼接页面，本质还是三个
    --%>
<jsp:include page="/common/header.jsp"/>

<h1>网页主体</h1>

<jsp:include page="/common/footer.jsp"/>
```

#### 6.5、9大内置对象

🌂PageContext 存东西
🌂Request 存东西
🌂Response
🌂Session 存东西
🌂Application 【SerlvetContext】 存东西
🌂config 【SerlvetConfig】
🌂out
🌂page ，不用了解
🌂exception

```
pageContext.setAttribute("name1","黄皖1号"); //保存的数据只在一个页面中有效
request.setAttribute("name2","黄皖2号"); //保存的数据只在一次请求中有效，请求转发会携带这个数据
session.setAttribute("name3","黄皖3号"); //保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器
application.setAttribute("name4","黄皖4号");  //保存的数据只在服务器中有效，从打开服务器到关闭服务器
```

**request：**客户端向服务器发送请求，产生的数据，用户看完就没用了，比如：新闻，用户看完没用的！

**session：**客户端向服务器发送请求，产生的数据，用户用完一会还有用，比如：购物车；

**application：**客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户还可能使用，比如：聊天数据；

#### 6.6、JSP标签、JSTL标签、EL表达式

```
<!-- JSTL表达式的依赖 -->
<dependency>
    <groupId>javax.servlet.jsp.jstl</groupId>
    <artifactId>jstl-api</artifactId>
    <version>1.2</version>
</dependency>
<!-- standard标签库 -->
<dependency>
    <groupId>taglibs</groupId>
    <artifactId>standard</artifactId>
    <version>1.1.2</version>
</dependency>
```

**EL表达式： ${ }**

🌂获取数据
🌂执行运算
🌂获取web开发的常用对象
**JSP标签**

```
<%--
  Created by IntelliJ IDEA.
  User: Apple
  Date: 2021/5/11
  Time: 19:25
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<h1>页面1</h1>
<jsp:forward page="/Jsptag2.jsp">
    <jsp:param name="name" value="huangwan"/>
    <jsp:param name="age" value="23"/>
</jsp:forward>

</body>
</html>

```

```
<%--
  Created by IntelliJ IDEA.
  User: Apple
  Date: 2021/5/11
  Time: 19:27
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>页面2</h1>
名字：<%=request.getParameter("name")%>
年龄：<%=request.getParameter("age")%>
</body>
</html>

```

**JSTL表达式**

JSTL标签库的使用就是为了弥补HTML标签的不足；它自定义许多标签，可以供我们使用，标签的功能和Java代码一样！

格式化标签

SQL标签

XML 标签

核心标签 （掌握部分）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210511184901453.png)

JSTL标签库使用步骤

引入对应的 taglib
使用其中的方法
在Tomcat 也需要引入 jstl的包，否则会报错：JSTL解析错误

**c：if**

```
<head>
    <title>Title</title>
</head>

<body>

<h4>if测试</h4>

<hr>

<form action="coreif.jsp" method="get">
    <%--
    EL表达式获取表单中的数据
    ${param.参数名}
    --%>
    <input type="text" name="username" value="${param.username}">
    <input type="submit" value="登录">
</form>


<%--判断如果提交的用户名是管理员，则登录成功--%>
<c:if test="${param.username=='admin'}" var="isAdmin">
    <c:out value="管理员欢迎您！"/>
</c:if>

<%--自闭合标签--%>
<c:out value="${isAdmin}"/>

</body>
```

**c:choose c:when**

```
<body>

<%--定义一个变量score，值为85--%>
<c:set var="score" value="55"/>

<c:choose>
    <c:when test="${score>=90}">
        你的成绩为优秀
    </c:when>
    <c:when test="${score>=80}">
        你的成绩为一般
    </c:when>
    <c:when test="${score>=70}">
        你的成绩为良好
    </c:when>
    <c:when test="${score<=60}">
        你的成绩为不及格
    </c:when>
</c:choose>

</body>
```


**c:forEach**

```
<%

    ArrayList<String> people = new ArrayList<>();
    people.add(0,"张三");
    people.add(1,"李四");
    people.add(2,"王五");
    people.add(3,"赵六");
    people.add(4,"田六");
    request.setAttribute("list",people);

%>

<%--
var , 每一次遍历出来的变量
items, 要遍历的对象
begin,   哪里开始
end,     到哪里
step,   步长
--%>
<c:forEach var="people" items="${list}">
    <c:out value="${people}"/> <br>
</c:forEach>

<hr>

<c:forEach var="people" items="${list}" begin="1" end="3" step="1" >
    <c:out value="${people}"/> <br>
</c:forEach>
```

## 7、JavaBean

实体类

JavaBean有特定的写法：

- 必须要有一个无参构造
- 属性必须私有化
- 必须有对应的get/set方法；

一般用来和数据库的字段做映射 ORM；

ORM ：对象关系映射

- 表—>类
- 字段–>属性
- 行记录---->对象

**people表**

**people表**

| id   | name | age  | address |
| ---- | ---- | ---- | ------- |
| 1    | 黄皖 | 3    | 地球    |
| 2    | 小黄 | 18   | 中国    |
| 3    | 小皖 | 100  | 湖北    |

**People.java**

```java
package com.huang.pojo;

public class People {
    private int id;
    private String name;
    private int age;
    private  String address;

    public People() {
    }

    public People(int id, String name, int age, String address) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.address = address;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}

```

**Javabean.jsp**

```
<%--
  Created by IntelliJ IDEA.
  User: Apple
  Date: 2021/5/13
  Time: 18:22
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<jsp:useBean id="people" class="com.huang.pojo.People" scope="page"></jsp:useBean>
<jsp:setProperty name="people" property="id" value="1"></jsp:setProperty>
<jsp:setProperty name="people" property="name" value="黄皖"></jsp:setProperty>
<jsp:setProperty name="people" property="age" value="23"></jsp:setProperty>
<jsp:setProperty name="people" property="address" value="麻城"></jsp:setProperty>

id：<jsp:getProperty name="people" property="id"/>
姓名：<jsp:getProperty name="people" property="name"/>
年龄：<jsp:getProperty name="people" property="age"/>
地址：<jsp:getProperty name="people" property="address"/>
</body>
</html>

```

注：这里直接访问Javabean.jsp即可

## 8、MVC三层架构

**什么是MVC：** Model view Controller 模型、视图、控制器

早些年用户直接访问控制层，控制层就可以直接操作数据库；

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210513185720722.png)

```
servlet--CRUD-->数据库
弊端：程序十分臃肿，不利于维护  
servlet的代码中：处理请求、响应、视图跳转、处理JDBC、处理业务代码、处理逻辑代码

架构：没有什么是加一层解决不了的！
程序猿调用
|
JDBC
|
Mysql Oracle SqlServer ....
```

8.1、MVC三层架构

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210513185928150.png)

7.1、MVC三层架构

**Model**

   🌂业务处理 ：业务逻辑（Service）
   🌂数据持久层：CRUD （Dao）
**View**

   🌂展示数据
   🌂提供链接发起Servlet请求 （a，form，img…）
**Controller （Servlet）**

   🌂接收用户的请求 ：（req：请求参数、Session信息….）

   🌂交给业务层处理对应的代码

   🌂控制视图的跳转

```
登录--->接收用户的登录请求--->处理用户的请求（获取用户登录的参数，username，password）---->交给业务层处理登录业务（判断用户名密码是否正确：事务）--->Dao层查询用户名和密码是否正确-->数据库
```



## 9、Filter （重点）

Filter：过滤器 ，用来过滤网站的数据；

   🌂处理中文乱码
   🌂登录验证….
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210513193839463.png)

​        例如我们新建一个ShowServlet.class文件并且在web.xml中注册，在浏览器端输出一句话，此时就会发现中文会乱码，虽然我们可以加一行代码来解决这个问题，但是如果网页足够多的时候不可能每一个都进行重复的工作哟，那么可以用一个简单的过滤器来实现这个问题，不仅更加安全而且还不用在每个servlet里面进行重复的工作。

**ShowServlet**

```
public class ShowServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       //resp.setCharacterEncoding("GBK");
        resp.getWriter().write("你好呀，小黄");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

**web.xml**

```
<servlet>
  <servlet-name>filter</servlet-name>
  <servlet-class>com.huang.servlet.ShowServlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>filter</servlet-name>
  <url-pattern>/show</url-pattern>
</servlet-mapping>

<servlet-mapping>
  <servlet-name>filter</servlet-name>
  <url-pattern>/filter/show</url-pattern>
</servlet-mapping>
```

现在我们就用filter来实现这些要求，新建CharacterEncodingFilter，来实现这些要求。

**CharacterEncodingFilter**

```
package com.huang.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

//字符编码过滤器 <!--只要是 /servlet的任何请求，会经过这个过滤器-->
@WebFilter("/servlet/*")
public class CharacterEncodingFilter implements Filter {
    /**
     * 初始化：web服务器启动，就以及初始化了，随时等待过滤对象出现！
     *
     * @param filterConfig
     * @throws ServletException
     */
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("CharacterEncodingFilter初始化");
    }

    //Chain : 链

    /**
     * 1. 过滤中的所有代码，在过滤特定请求的时候都会执行
     * 2. 必须要让过滤器继续同行
     * chain.doFilter(request,response);
     */
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding("utf-16");
        response.setCharacterEncoding("utf-16");
        response.setContentType("text/html;charset = utf-16");

        System.out.println("CharacterEncodingFilter执行前...");
        chain.doFilter(request, response);// 让我们的请求继续走，如果不写，我们的程序到这就拦截停止了
        System.out.println("CharacterEncodingFilter执行后...");
    }

    //销毁：web服务器关闭的时候，过滤会销毁
    @Override
    public void destroy() {
        System.out.println("CharacterEncodingFilter销毁");
    }
}

```

**web.xml**

```
  <servlet>
    <servlet-name>filter</servlet-name>
    <servlet-class>com.huang.servlet.ShowServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>filter</servlet-name>
    <url-pattern>/show</url-pattern>
  </servlet-mapping>

  <servlet-mapping>
    <servlet-name>filter</servlet-name>
    <url-pattern>/filter/show</url-pattern>
  </servlet-mapping>

<filter>
  <filter-name>CharacterEncodingFilter</filter-name>
  <filter-class>com.huang.filter.CharacterEncodingFilter</filter-class>
</filter>
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/filter/show</url-pattern>
  </filter-mapping>
```

## 10、监听器

实现一个监听器的接口；（有N种）

1. 编写一个监听器

   实现监听器的接口…

   **OnlineCountListener.java**

   ```
   package com.huang.servlet;
   
   import javax.servlet.ServletContext;
   import javax.servlet.annotation.WebListener;
   import javax.servlet.http.HttpSession;
   import javax.servlet.http.HttpSessionEvent;
   import javax.servlet.http.HttpSessionListener;
   //统计网站在线人数： 统计session
   @WebListener
   public class OnlineCountListener implements HttpSessionListener {
       /**
        * 创建session监听:看你的一举一动
        * 一旦创建session就会触发一次这个事件
        *
        * @param se
        */
       @Override
       public void sessionCreated(HttpSessionEvent se) {
           HttpSession session = se.getSession();
           System.out.println(session.getId());
           ServletContext context = session.getServletContext();
           Integer onlineCount = (Integer) context.getAttribute("onlineCount");
   
           if (onlineCount == null) {
               onlineCount = new Integer(1);
           } else {
               int count = onlineCount.intValue();
               onlineCount = new Integer(count + 1);
           }
           context.setAttribute("onlineCount", onlineCount);
   
       }
   
       /**
        * 销毁session监听
        * 一旦销毁session就会触发一次这个事件
        *
        * @param se
        */
       @Override
       public void sessionDestroyed(HttpSessionEvent se) {
           HttpSession session = se.getSession();
           ServletContext context = session.getServletContext();
   
           Integer onlineCount = (Integer) context.getAttribute("onlineCount");
   
           if (onlineCount == null) {
               onlineCount = new Integer(0);
           } else if (onlineCount >= 1) {
               int count = onlineCount.intValue();
               onlineCount = new Integer(count - 1);
           }
           context.setAttribute("onlineCount", onlineCount);
       }
   
   }
   ```

   **web.xml**

   ```
     <listener>
       <listener-class>com.huang.servlet.OnlineCountListener</listener-class>
     </listener>
   ```

   **index.jsp**

   ```
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <body>
   <h2>当前有<spqn style="color: aquamarine"><%=this.getServletConfig().getServletContext().getAttribute("onlineCount")%></spqn>人在线</h2>
   
   </body>
   </html>
   ```

   ## 13、过滤器、监听器常见应用

   **监听器：GUI编程中经常使用；**

```
package com.huang.servlet;

import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;

public class TestPanel {
    public static void main(String[] args) {
        Frame frame = new Frame("中秋节快乐");  //新建一个窗体
        Panel panel = new Panel(null); //面板
        frame.setLayout(null); //设置窗体的布局

        frame.setBounds(300,300,500,500);
        frame.setBackground(new Color(0,0,255)); //设置背景颜色

        panel.setBounds(50,50,300,300);
        panel.setBackground(new Color(0,255,0)); //设置背景颜色

        frame.add(panel);

        frame.setVisible(true);

        //监听事件，监听关闭事件
        frame.addWindowListener(new WindowListener() {
            @Override
            public void windowOpened(WindowEvent e) {
                System.out.println("打开");
            }

            @Override
            public void windowClosing(WindowEvent e) {
                System.out.println("关闭ing");
                System.exit(0);
            }

            @Override
            public void windowClosed(WindowEvent e) {
                System.out.println("关闭ed");
            }

            @Override
            public void windowIconified(WindowEvent e) {

            }

            @Override
            public void windowDeiconified(WindowEvent e) {

            }

            @Override
            public void windowActivated(WindowEvent e) {
                System.out.println("激活");
            }

            @Override
            public void windowDeactivated(WindowEvent e) {
                System.out.println("未激活");

            }
        });

    }
}
```

## 11、做个登陆判断的小Demo

用户登录之后才能进入主页！用户注销后就不能进入主页了！

1. 用户登录之后，向Sesison中放入用户的数据
2. 进入主页的时候要判断用户是否已经登录；要求：在过滤器中实现！

先看看项目结构吧：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210516183713145.png)

**登录页面：Login.jsp**

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Apple
  Date: 2021/5/16
  Time: 15:42
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>登陆</h1>
<form action="/servlet/login" method="post">
    <input type="text" name="username" >
    <input type="submit">
</form>
</body>
</html>
```

**处理登陆的LoginServlet**

```java
package com.huang.servlet;

import com.huang.util.Constant;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取前端参数的请求
        String username = req.getParameter("username");
        if (username.equals("admin")){
            req.getSession().setAttribute(Constant.USER_SESSION,req.getSession().getId());
            resp.sendRedirect("/sys/success.jsp");
        }else {
            resp.sendRedirect("/error.jsp");

        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

**登陆成功的success.jsp**

```
<%--
  Created by IntelliJ IDEA.
  User: Apple
  Date: 2021/5/16
  Time: 15:41
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%--<%
    Object user_session = request.getSession().getAttribute("USER_SESSION");
    if (user_session==null){
        response.sendRedirect("/Login.jsp");
    }
%>--%>
<h1>这是一个主页</h1>
<p><a href="/servlet/logout">注销</a></p>
</body>
</html>
```

**处理判断是否是登陆状态的LoginoutServlet**

```java
package com.huang.servlet;

import com.huang.util.Constant;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class LoginoutServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Object user_session = req.getSession().getAttribute(Constant.USER_SESSION);
        if (user_session!=null){
            req.getSession().removeAttribute(Constant.USER_SESSION);
            resp.sendRedirect("/Login.jsp");
        }else {
            resp.sendRedirect("/Login.jsp");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

**错误页面error.jsp**

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Apple
  Date: 2021/5/16
  Time: 16:08
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>这是一个错误页面</h1>
<p><a href="/Login.jsp">返回登录页面</a></p>
</body>
</html>
```

Constant.java

```java
package com.huang.util;

public class Constant {
    public final static String USER_SESSION="USER_SESSION";
}
```

**web.xml**

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>

  <servlet>
    <servlet-name>LoginServlet</servlet-name>
    <servlet-class>com.huang.servlet.LoginServlet</servlet-class>
  </servlet>
<servlet-mapping>
  <servlet-name>LoginServlet</servlet-name>
  <url-pattern>/servlet/login</url-pattern>
</servlet-mapping>


  <servlet>
    <servlet-name>LoginoutServlet</servlet-name>
    <servlet-class>com.huang.servlet.LoginoutServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>LoginoutServlet</servlet-name>
    <url-pattern>/servlet/logout</url-pattern>
  </servlet-mapping>


  <filter>
    <filter-name>SysFilter</filter-name>
    <filter-class>com.huang.filter.SysFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>SysFilter</filter-name>
    <url-pattern>/sys/*</url-pattern>
  </filter-mapping>

</web-app>
```

