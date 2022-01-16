---
title: Java 流(Stream)、文件(File)和IO
date: 2020-03-29 10:36:58
tags: Java
categories: java基础
---
## Java 流(Stream)、文件(File)和IO
Java.io 包几乎包含了所有操作输入、输出需要的类。所有这些流类代表了输入源和输出目标。
Java.io 包中的流支持很多种格式，比如：基本类型、对象、本地化字符集等等。
一个流可以理解为一个数据的序列。输入流表示从一个源读取数据，输出流表示向一个目标写数据。
Java 为 I/O 提供了强大的而灵活的支持，使其更广泛地应用到文件传输和网络编程中。
但本节讲述最基本的和流与 I/O 相关的功能。我们将通过一个个例子来学习这些功能。
### 读取控制台输入
Java 的控制台输入由 System.in 完成。
为了获得一个绑定到控制台的字符流，你可以把 System.in 包装在一个 BufferedReader 对象中来创建一个字符流。
下面是创建 BufferedReader 的基本语法：
```java
BufferedReader br = new BufferedReader(new 
                      InputStreamReader(System.in));
```
BufferedReader 对象创建后，我们便可以使用 read() 方法从控制台读取一个字符，或者用 readLine() 方法读取一个字符串。
### 从控制台读取多字符输入

从 BufferedReader 对象读取一个字符要使用 read() 方法，它的语法如下：
```java
int read( ) throws IOExceptionBufferedReader br = new BufferedReader(new 
                      InputStreamReader(System.in));
```
每次调用 read() 方法，它从输入流读取一个字符并把该字符作为整数值返回。 当流结束的时候返回 -1。该方法抛出 IOException。
下面的程序示范了用 read() 方法从控制台不断读取字符直到用户输入 "q"。
BRRead.java 文件代码：
//使用 BufferedReader 在控制台读取字符
 
```java
import java.io.*;
 
public class BRRead {
    public static void main(String args[]) throws IOException {
        char c;
        // 使用 System.in 创建 BufferedReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        System.out.println("输入字符, 按下 'q' 键退出。");
        // 读取字符
        do {
            c = (char) br.read();
            System.out.println(c);
        } while (c != 'q');
    }
}BufferedReader br = new BufferedReader(new 
                      InputStreamReader(System.in));
```

### 从控制台读取字符串
从标准输入读取一个字符串需要使用 BufferedReader 的 readLine() 方法。
它的一般格式是：
String readLine( ) throws IOException
下面的程序读取和显示字符行直到你输入了单词"end"。
//使用 BufferedReader 在控制台读取字符
```java
import java.io.*;
 
public class BRRead {
    public static void main(String args[]) throws IOException {
        char c;
        // 使用 System.in 创建 BufferedReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        System.out.println("输入字符, 按下 'q' 键退出。");
        // 读取字符
        do {
            c = (char) br.read();
            System.out.println(c);
        } while (c != 'q');
    }
}
```

JDK 5 后的版本我们也可以使用 Java Scanner 类来获取控制台的输入。
### 控制台输出
下面的例子用 write() 把字符 "A" 和紧跟着的换行符输出到屏幕：
```java
import java.io.*;
 
//演示 System.out.write().
public class WriteDemo {
    public static void main(String args[]) {
        int b;
        b = 'A';
        System.out.write(b);
        System.out.write('\n');
    }
}
```

注意：write() 方法不经常使用，因为 print() 和 println() 方法用起来更为方便。

### 读写文件
```java
package file;
//文件名 :fileStreamTest2.java
import java.io.*;

public class fileStreamTest2 {
    public static void main(String[] args) throws IOException {
       // OutputStream f=new FileOutputStream("files.txt");
       File f = new File("files.txt");
       FileOutputStream fop = new FileOutputStream(f);
        // 构建FileOutputStream对象,文件不存在会自动新建

        OutputStreamWriter writer = new OutputStreamWriter(fop, "UTF-8");
        // 构建OutputStreamWriter对象,参数可以指定编码,默认为操作系统默认编码,windows上是gbk

        writer.append("中文输入");
        // 写入到缓冲区

        writer.append("\r\n");
        // 换行

        writer.append("English");
        // 刷新缓存冲,写入到文件,如果下面已经没有写入的内容了,直接close也会写入

        writer.close();
        // 关闭写入流,同时会把缓冲区内容写入文件,所以上面的注释掉

        fop.close();
        // 关闭输出流,释放系统资源

        FileInputStream fip = new FileInputStream(f);
        // 构建FileInputStream对象

        InputStreamReader reader = new InputStreamReader(fip, "UTF-8");
        // 构建InputStreamReader对象,编码与写入相同

        StringBuffer sb = new StringBuffer();
        while (reader.ready()) {
            sb.append((char) reader.read());
            // 转成char加到StringBuffer对象中
        }
        System.out.println(sb.toString());
        reader.close();
        // 关闭读取流

        fip.close();
        // 关闭输入流,释放系统资源
        File files;
        System.out.println("目录f的路径是：" + f.getPath());
        System.out.println("目录f的绝对路径是：" + f.getAbsolutePath());
        System.out.println("目录f的绝对路径F是：" + f.getAbsoluteFile());
        System.out.println("文件f的名字是：" + f.getName());
        System.out.println("文件f能读吗：" + f.canRead());
        System.out.println("文件f能写吗：" + f.canWrite());
        System.out.println("文件f规范路径字符串：" + f.getCanonicalPath());
        System.out.println("文件f规范路径File对象：" + f.getCanonicalFile());
        System.out.println("文件f的父路径字符串：" + f.getParent());
        System.out.println("文件f的父路径File对象：" + f.getParentFile());

}
}
```