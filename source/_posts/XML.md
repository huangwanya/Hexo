---
title: XML
date: 2020-08-04 16:25:12
tags: XML
categories: java web
---
## XML简介

**什么是 xml？**

xml 是可扩展的标记性语言。

**xml 的作用？**

xml 的主要作用有： 

1、用来保存数据，而且这些数据具有自我描述性 

2、它还可以做为项目或者模块的配置文件 

3、还可以做为网络传输数据的格式（现在 JSON 为主）。

## xml 语法

   1.文档声明。 

2. 元素（标签） 

3. xml 属性 

4. xml 注释 

5. 文本区域（CDATA 区）

### 文档声明

**我们先创建一个简单 XML 文件，用来描述图书信息。** 

1.创建一个xml 文件

2.图书有id 属性 表示唯一 标识，书名，有作者，价格的信息

3.在浏览器中可以查看到文档

```
<?xml version="1.0" encoding="utf-8" ?>
<!-- xml声明 version是版本的意思   encoding是编码  -->
<books> <!-- 这是xml注释 -->
    <book id="SN123123413241"> <!-- book标签描述一本图书   id属性描述 的是图书 的编号  -->
        <name>九阳神功</name> <!-- name标签描述 的是图书 的信息 -->
        <author>小黄</author>		<!-- author单词是作者的意思 ，描述图书作者 -->
        <price>188888</price>		<!-- price单词是价格，描述的是图书 的价格 -->
    </book>
    <book id="SN12341235123">	<!-- book标签描述一本图书   id属性描述 的是图书 的编号  -->
        <name>降龙十八掌</name>	<!-- name标签描述 的是图书 的信息 -->
        <author>hw</author>	<!-- author单词是作者的意思 ，描述图书作者 -->
        <price>99999</price>	<!-- price单词是价格，描述的是图书 的价格 -->
    </book>
    <book id="SN19874665555" name="打狗棒法" author="huangwan" price="198989"/>
</books>
```

### **xml** **注释** 

**html 和 XML 注释 一样 : <!-- html 注释 -->**

### **元素（标签）** 

```
html 标签： 

格式：<标签名>封装的数据</标签名> 

单标签: <标签名 /> <br /> 换行 <hr />水平线 

双标签 <标签名>封装的数据</标签名> 

标签名大小写不敏感 

标签有属性，有基本属性和事件属性 

标签要闭合（不闭合 ，html 中不报错。但我们要养成良好的书写习惯。闭合） 
```

**1）什么是 xml 元素** 

*XML 元素*指的是从（且包括）开始标签直到（且包括）结束标签的部分。

元素可包含其他元素、文本或者两者的混合物。元素也可以拥有属性。

```
<bookstore>
<book category="CHILDREN">
  <title>Harry Potter</title> 
  <author>J K. Rowling</author> 
  <year>2005</year> 
  <price>29.99</price> 
</book>
<book category="WEB">
  <title>Learning XML</title> 
  <author>Erik T. Ray</author> 
  <year>2003</year> 
  <price>39.95</price> 
</book>
</bookstore> 

```

**2）XML命名规则**

XML 元素必须遵循以下命名规则： 

1.名称可以含字母、数字以及其他的字符

2.名称不能以数字或者标点符号开始 

3.名称不能以字符 “xml”（或者 XML、Xml）开始 （**它是可以的**）

4.名称不能包含空格

**3)xml 中的元素（标签）也 分成 单标签和双标签：** 

**单标签**

格式： <标签名 属性=”值” 属性=”值” ...... /> 

```
  <name>降龙十八掌</name>	<!-- name标签描述 的是图书 的信息 -->
```

**双标签**

格式：< 标签名 属性=”值” 属性=”值” ......>文本数据或子标签</标签名

```
    <book id="SN19874665555" name="打狗棒法" author="huangwan" price="198989"/>
```

### **xml** **属性** 

xml 的标签属性和 html 的标签属性是非常类似的，**属性可以提供元素的额外信息** 

在标签上可以书写属性： 

一个标签上可以书写多个属性。**每个属性的值必须使用 引号 引起来**。 

的规则和标签的书写规则一致

```
XML 属性
从 HTML，你会回忆起这个：<img src="computer.gif">。"src" 属性提供有关 <img> 元素的额外信息。

在 HTML 中（以及在 XML 中），属性提供有关元素的额外信息：

<img src="computer.gif">
<a href="demo.asp"> 
属性通常提供不属于数据组成部分的信息。在下面的例子中，文件类型与数据无关，但是对需要处理这个元素的软件来说却很重要：

<file type="gif">computer.gif</file>XML 属性必须加引号
属性值必须被引号包围，不过单引号和双引号均可使用。比如一个人的性别，person 标签可以这样写：

<person sex="female">或者这样也可以：

<person sex='female'>注释：如果属性值本身包含双引号，那么有必要使用单引号包围它，就像这个例子：

<gangster name='George "Shotgun" Ziegler'>或者可以使用实体引用：

<gangster name="George &quot;Shotgun&quot; Ziegler">XML 元素 vs. 属性
请看这些例子：

<person sex="female">
  <firstname>Anna</firstname>
  <lastname>Smith</lastname>
</person> 

<person>
  <sex>female</sex>
  <firstname>Anna</firstname>
  <lastname>Smith</lastname>
</person> 
在第一个例子中，sex 是一个属性。在第二个例子中，sex 则是一个子元素。两个例子均可提供相同的信息。

没有什么规矩可以告诉我们什么时候该使用属性，而什么时候该使用子元素。我的经验是在 HTML 中，属性用起来很便利，但是在 XML 中，您应该尽量避免使用属性。如果信息感觉起来很像数据，那么请使用子元素吧。
我最喜欢的方式
下面的三个 XML 文档包含完全相同的信息：

第一个例子中使用了 date 属性：

<note date="08/08/2008">
<to>George</to>
<from>John</from>
<heading>Reminder</heading>
<body>Don't forget the meeting!</body>
</note> 
第二个例子中使用了 date 元素：

<note>
<date>08/08/2008</date>
<to>George</to>
<from>John</from>
<heading>Reminder</heading>
<body>Don't forget the meeting!</body>
</note> 
第三个例子中使用了扩展的 date 元素（这是我的最爱）：

<note>
<date>
  <day>08</day>
  <month>08</month>
  <year>2008</year>
</date>
<to>George</to>
<from>John</from>
<heading>Reminder</heading>
<body>Don't forget the meeting!</body>
</note>
避免 XML 属性？
因使用属性而引起的一些问题：

属性无法包含多个值（子元素可以） 
属性无法描述树结构（子元素可以） 
属性不易扩展（为未来的变化） 
属性难以阅读和维护 
请尽量使用元素来描述数据。而仅仅使用属性来提供与数据无关的信息。

不要做这样的蠢事（这不是 XML 应该被使用的方式）：

<note day="08" month="08" year="2008"
to="George" from="John" heading="Reminder" 
body="Don't forget the meeting!">
</note>
针对元数据的 XML 属性
有时候会向元素分配 ID 引用。这些 ID 索引可用于标识 XML 元素，它起作用的方式与 HTML 中 ID 属性是一样的。这个例子向我们演示了这种情况：

<messages>
  <note id="501">
    <to>George</to>
    <from>John</from>
    <heading>Reminder</heading>
    <body>Don't forget the meeting!</body>
  </note>
  <note id="502">
    <to>John</to>
    <from>George</from>
    <heading>Re: Reminder</heading>
    <body>I will not</body>
  </note> 
</messages>
上面的 ID 仅仅是一个标识符，用于标识不同的便签。它并不是便签数据的组成部分。

在此我们极力向您传递的理念是：元数据（有关数据的数据）应当存储为属性，而数据本身应当存储为元素。

```

### 语法规则

1、所有 XML 元素都须有关闭标签

2、XML 标签对大小写敏感

3、XML 必须正确地嵌套

4、XML 文档必须有根元素

5、XML 的属性值须加引号

6、XML 中的特殊字符（如**>** )

### 文本区域（CDATA区）

CDATA 语法可以告诉 xml 解析器，我 CDATA 里的文本内容，只是纯文本，不需要 xml 语法解析 

CDATA 格式： 

```
<![CDATA[这里可以把你输入的字符原样显示，不会解析 xml ]]>
例如：
  <![CDATA[<<小黄>> ]]>
```

## **xml** 解析技术介绍

xml 可扩展的标记语言。 

不管是 html 文件还是 xml 文件它们都是标记型文档，都可以使用 w3c 组织制定的 dom 技术来解析。

document 对象表示的是整个文档（可以是 html 文档，也可以是 xml 文档）



**早期** **JDK** **为我们提供了两种** **xml** **解析技术** **DOM** **和** **Sax** **简介（**已经过时，但我们需要知道这两种技术）

dom 解析技术是 W3C 组织制定的，而所有的编程语言都对这个解析技术使用了自己语言的特点进行实现。 

Java 对 dom 技术解析标记也做了实现。 

sun 公司在 JDK5 版本对 dom 解析技术进行升级：SAX（ Simple API for XML ） 

SAX 解析，它跟 W3C 制定的解析不太一样。它是以类似事件机制通过回调告诉用户当前正在解析的内容。 

它是一行一行的读取 xml 文件进行解析的。不会创建大量的 dom 对象。 

所以它在解析 xml 的时候，在内存的使用上。和性能上。都优于 Dom 解析。 



第三方的解析： 

​                   jdom 在 dom 基础上进行了封装 、 

​                   **dom4j** 又对 jdom 进行了封装。 

​                   pull 主要用在 Android 手机开发，是在跟 sax 非常类似都是事件机制解析 xml 文件。 

​                   这个 Dom4j 它是第三方的解析技术。我们需要使用第三方给我们提供好的类库才可以解析 xml 文件。

## **dom4j** **解析技术**

由于 dom4j 它不是 sun 公司的技术，而属于第三方公司的技术，我们需要使用 dom4j 就需要到 dom4j 官网下载 dom4j 的 jar 包。

### **dom4j** **编程步骤：**

第一步： 先加载 xml 文件创建 Document 对象 

第二步：通过 Document 对象拿到根元素对象 

第三步：通过根元素.elelemts(标签名); 可以返回一个集合，这个集合里放着。所有你指定的标签名的元素对象 

第四步：找到你想要修改、删除的子元素，进行相应在的操作 

第五步，保存到硬盘上 

### **获取** **document** **对象** 

创建一个 lib 目录，并添加 dom4j 的 jar 包。并添加到类路径。

需要解析的 books.xml 文件内容 

```
<?xml version="1.0" encoding="utf-8" ?>
<!-- xml声明 version是版本的意思   encoding是编码  -->
<books> <!-- 这是xml注释 -->
    <book id="SN123123413241"> <!-- book标签描述一本图书   id属性描述 的是图书 的编号  -->
        <name>九阳神功</name> <!-- name标签描述 的是图书 的信息 -->
      <!--  <author>小黄</author>	-->	<!-- author单词是作者的意思 ，描述图书作者 -->
        <![CDATA[<<小黄>> ]]>
        <price>188888</price>		<!-- price单词是价格，描述的是图书 的价格 -->
    </book>
    <book id="SN12341235123">	<!-- book标签描述一本图书   id属性描述 的是图书 的编号  -->
        <name>降龙十八掌</name>	<!-- name标签描述 的是图书 的信息 -->
        <author>hw</author>	<!-- author单词是作者的意思 ，描述图书作者 -->
        <price>99999</price>	<!-- price单词是价格，描述的是图书 的价格 -->
    </book>
    <!--book id="SN19874665555" name="打狗棒法" author="huangwan" price="198989"/>-->
</books>
```

**解析获取** **Document** **对象的代码** 

**第一步，先创建** **SaxReader** **对象。这个对象，用于读取** **xml** **文件，并创建** **Document** 

```
package com.atguigu.pojo;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.io.SAXReader;
import org.junit.Test;

public class Dom4jTest {
    @Test
    public void test1() throws Exception {
        // 创建一个SaxReader输入流，去读取 xml配置文件，生成Document对象
        SAXReader saxReader = new SAXReader();

        Document document = saxReader.read("src/books.xml");

        System.out.println(document);
    }
}

```

### **遍历 标签 获取所有标签中的内容**

```
package com.atguigu.pojo;

import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;
import org.junit.Test;

import java.math.BigDecimal;
import java.util.List;

public class Dom4jTest2 {
    @Test
    public void test1() throws Exception {
        //1. 读取books.xml文件
        SAXReader saxReader = new SAXReader();
        //在Junit测试中，相对路径是从模块名开始算
        Document document = saxReader.read("src/books.xml");
        //2.通过document对象获取对象根元素
        Element rootElement = document.getRootElement();
        //System.out.println(rootElement);
        //2.通过document对象获取book标签对象
        //element()和lements()
        List<Element> books = rootElement.elements("book");
        //遍历，处理每个book标签转换成book类
        for (Element book : books) {
            //asXML()把标签对象转换为标签字符串
            // System.out.println(book.asXML());
            Element nameElement = book.element("name");  //单个name
            // System.out.println(nameElement.asXML());
            String nameTest = nameElement.getText();
            //getText()可以获取标签中文本的内容
            //  System.out.println(nameTest);
            //直接获取标签名的文本内容
            String priceTest = book.elementText("price");
           //   System.out.println(priceTest);
            String authorTest = book.elementText("author");
            String idValue = book.attributeValue("id");
            BigDecimal bigDecimal = new BigDecimal(priceTest);
            System.out.println(new Book(idValue, nameTest, new BigDecimal(priceTest), authorTest));
        }

    }
}
/*输出结果：
        Book{sn='SN123123413241', name='九阳神功', price=188888, author='小黄'}
        Book{sn='SN12341235123', name='降龙十八掌', price=99999, author='hw'}*/

```

