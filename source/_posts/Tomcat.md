---
title: Tomcat
date: 2020-08-08 20:18:16
tags: Tomcat
categories: java web
---
## **JavaWeb** **的概念** 

### a)什么是JavaWeb

JavaWeb 是指，所有通过 Java 语言编写可以通过浏览器访问的程序的总称，叫 JavaWeb。 

JavaWeb 是基于请求和响应来开发的。 

### b)什么是请求

请求是指客户端给服务器发送数据，叫请求 Request。 

### c)什么是响应

响应是指服务器给客户端回传数据，叫响应 Response。 

### d)请求和响应的关系 

请求和响应是成对出现的，有请求就有响应。

## **Web** **资源的分类** 

web 资源按实现的技术和呈现的效果的不同，又分为静态资源和动态资源两种。 

静态资源： html、css、js、txt、mp4 视频 , jpg 图片 

动态资源： jsp 页面、Servlet 程序 

## 常用的 **Web** **服务器** 

**Tomcat：**由 Apache 组织提供的一种 Web 服务器，提供对 jsp 和 Servlet 的支持。它是一种轻量级的 javaWeb 容器（服务 

器），也是当前应用最广的 JavaWeb 服务器（免费）。 

**Jboss：**是一个遵从 JavaEE 规范的、开放源代码的、纯 Java 的 EJB 服务器，它支持所有的 JavaEE 规范（免费）。 

**GlassFish：** 由 Oracle 公司开发的一款 JavaWeb 服务器，是一款强健的商业服务器，达到产品级质量（应用很少）。 

**Resin：**是 CAUCHO 公司的产品，是一个非常流行的服务器，对 servlet 和 JSP 提供了良好的支持， 

性能也比较优良，resin 自身采用 JAVA 语言开发（收费，应用比较多）。 

**WebLogic：**是 Oracle 公司的产品，是目前应用最广泛的 Web 服务器，支持 JavaEE 规范， 

而且不断的完善以适应新的开发要求，适合大型项目（收费，用的不多，适合大公司）。

## **Tomcat** **的使用**

**目录介绍** 

bin             专门用来存放 Tomcat 服务器的可执行程序 

conf           专门用来存放 Tocmat 服务器的配置文件 

lib              专门用来存放 Tomcat 服务器的 jar 包 

logs           专门用来存放 Tomcat 服务器运行时输出的日记信息 

temp         专门用来存放 Tomcdat 运行时产生的临时数据 

webapps  专门用来存放部署的 Web 工程。 

work         是 Tomcat 工作时的目录，用来存放 Tomcat 运行时 jsp 翻译为 Servlet 的源码，和 Session 钝化的目录。

### 如何启动 **Tomcat** **服务器** 

找到 Tomcat 目录下的 bin 目录下的 startup.bat 文件，双击，就可以启动 Tomcat 服务器。

### **Tomcat** **的停止** 

1、点击 tomcat 服务器窗口的 x 关闭按钮 

2、把 Tomcat 服务器窗口置为当前窗口，然后按快捷键 Ctrl+C 

3、找到 Tomcat 的 bin 目录下的 shutdown.bat 双击，就可以停止 Tomcat 服务器

### **如何修改** **Tomcat** **的端口号** 

找到 Tomcat 目录下的 conf 目录，找到 server.xml 配置文件（第69行）。

## web工程部署在**Tomcat** 

### 第一种部署方法：

只需要把 web 工程的目录拷贝到 Tomcat 的 webapps 目录下 

即可。 

#### **如何访问** **Tomcat** **下的** **web** **工程。** 

只需要在浏览器中输入访问地址格式如下： 

```
http://ip:port/工程名/目录下/文件名
```



### **第二种部署方法：** 

找到 Tomcat 下的 conf 目录\Catalina\localhost\ 下,创建如下的配置文件：

hw.xml 配置文件内容如下（hw自定义)：

```
<!-- Context 表示一个工程上下文 
path 表示工程的访问路径:/abc 
docBase 表示你的工程目录在哪里 --> 
<Context path="/hw" docBase="E:\book" />
```

```
访问这个工程的路径如下:
                   http://ip:port/hw/ 就表示访问 E:\book 目录
```

## idea运行web

本方法随着实践的增加，基本不会忘记，这里就没写上。