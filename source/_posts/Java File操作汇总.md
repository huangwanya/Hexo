---
title: Java File操作汇总
date: 2020-03-29 13:28:43
tags: Java
categories: java基础
---
## 1、创建文件

🌂boolean java.io.File.createNewFile() throws IOException用来创建文件，如果文件存在，创建失败，返回false；

🌂🌂new File("a.txt");并不创建文件实体，只是创建一个指向“a.txt”的引用。

🌂🌂路径分隔符：File.separator

```java
package file.dol.sn;
 
import java.io.File;
 
public class FileDemo {
 
	public static void main(String[] args) {
		//new File()就相当于是C语言中定义一个指向“a.txt”的文件指针
		File f1 = new File("a.txt");
		try 
		{
			//创建文件:boolean java.io.File.createNewFile() throws IOException
			boolean b = f1.createNewFile();
			//第二次将会创建失败false，这里和流不一样
			System.out.println(b);
		} 
		catch (Exception e) 
		{
			// TODO: handle exception
		}
		//目录分隔符：File.separator，相当于“\\”跨平台。
		//File f2 = new File("E:\\tmp","b.txt");
		File f2 = new File("E:"+File.separator+"tmp","b.txt");
		System.out.println(f2);
	}
}
```
## 2、删除文件
🌂delete()：删除文件成功返回true，删除失败返回false（ boolean java.io.File.delete()  ）

🌂🌂deleteOnExit()：程序退出时，自动删除文件。一般用于对程序创建的临时文件进行操作，退出时删除。（ void java.io.File.deleteOnExit()  ）

```java
package file.dol.sn;
 
import java.io.File;
 
public class FileDemo {
 
	public static void main(String[] args) {
 
		File f1 = new File("a.txt");
		try 
		{
			boolean b = f1.createNewFile();
			//删除文件：boolean java.io.File.delete()
			f1.delete();
			//程序退出时，自动删除：void java.io.File.deleteOnExit()
			//f1.deleteOnExit();
		} 
		catch (Exception e) 
		{
			// TODO: handle exception
		}
	}
}
```
## 3、判断文件是否存在
exists()：判断文件是否存在（ boolean java.io.File.exists() ）

```java
package file.dol.sn;
 
import java.io.File;
 
public class FileDemo {
 
	public static void main(String[] args) {
 
		File f1 = new File("not.txt");
		//判断文件是否存在：boolean java.io.File.exists()
		boolean b = f1.exists();
		System.out.println(b);
	}
}
```
## 4、创建文件夹
🌂mkdir()：只能创建“一级目录”（boolean java.io.File.mkdir()）；

🌂🌂mkdirs()：可以创建多级目录（boolean java.io.File.mkdirs()）。

```java
package file.dol.sn;
 
import java.io.File;
 
public class FileDemo {
 
	public static void main(String[] args) {
 
		File dir1 = new File("abc");
		File dir2 = new File("a\\b\\c\\d");
		try 
		{
			//创建文件目录(即文件夹)：boolean java.io.File.mkdir()
			//该方法只能创建“一级”目录
			boolean b =  dir1.mkdir();
			System.out.println(b);
			//创建多级文件夹：boolean java.io.File.mkdirs()
			b =  dir2.mkdirs();
			System.out.println(b);
		} 
		catch (Exception e) 
		{
			System.out.println(e.toString());
		}
	}
}
```
## 5、文件类型判断
🌂exists()：判断文件是否存在，注意：一定要先判断这个；

🌂🌂isDirectory()：判断是否为文件夹；

🌂🌂🌂isFile()：判断是否为文件；

🌂🌂🌂🌂isHidden()：判断是否为隐藏文件；

🌂🌂🌂🌂🌂isAbsolute()：判断是否为绝对路径，这里不管文件是否存在都能判断。

```java
package file.dol.sn;
 
import java.io.File;
 
public class FileDemo {
 
	public static void main(String[] args) {
 
		File f = new File("C:\\abc.txt");
		try 
		{
			//判断文件是否存在
			if (f.exists())
			{
				//判断文件是否是文件夹
				if (f.isDirectory())
				{
					System.out.println("文件夹");
				}
				//判断文件是否是文件
				if (f.isFile())
				{
					System.out.println("文件");
				}
				//判断是否为隐藏文件
				if (f.isHidden())
				{
					System.out.println("隐藏文件");
				}
			}
			else 
			{
				System.out.println("文件不存在");
			}
			//判断是否为绝对路径，不管文件是否存在
			if (f.isAbsolute())
			{
				System.out.println("是绝对路径");
			}
		} 
		catch (Exception e) 
		{
			System.out.println(e.toString());
		}
	}
}
```
## 6、获取文件信息

🌂getName()：获取文件名；

🌂🌂getParent()：获取文件父目录；

🌂🌂🌂getPath()：获取文件路径；

🌂🌂🌂🌂getAbsolutePath()：获取文件绝对路径；

🌂🌂🌂🌂🌂lastModified()：获得文件最后一次被修改的时间；

🌂🌂🌂🌂🌂🌂length()：获取文件大小；

🌂🌂🌂🌂🌂🌂🌂renameTo()：文件剪切，将文件f1剪切然后粘贴到f2（相当于右键f1->剪切->粘贴->f2所在目录）

```java
package file.dol.sn;
 
import java.io.File;
 
public class FileDemo {
 
	public static void main(String[] args) {
 
		File f1 = new File("abc.txt");
		File f2 = new File("E:\\Java\\test.txt");
		try 
		{
			//获得文件名
			System.out.println(f1.getName());
			//获得绝对路径中的父目录，如：File("abc.txt")则该返回为null
			System.out.println(f1.getParent());
			//获得相对路径
			System.out.println(f1.getPath());
			//获得绝对路径
			System.out.println(f1.getAbsolutePath());
			//获得文件最后一次被修改的时间
			System.out.println(f1.lastModified());
			//获得文件大小
			System.out.println(f1.length());
			//文件剪切，将文件f1剪切然后粘贴到f2（相当于右键f1->剪切->粘贴->f2所在目录）
			f1.renameTo(f2);
		} 
		catch (Exception e) 
		{
			System.out.println(e.toString());
		}
	}
}
```
## 7、获取目录下文件名
🌂listRoots()：获取系统盘符；

🌂🌂list()：获取“X:\\”目录下的所有文件名，包括隐藏文件和文件夹（调用list()方法时，必须先封装一个目录，且必须存在的目录。）

🌂🌂🌂list(FilenameFilter filter)：列出文件名的时候，可以进行过滤操作（如：列出后缀名为.txt的文件）。

🌂🌂🌂🌂listFiles()：列出目录下文件名，不包括文件夹。

```java
package file.dol.sn;
 
import java.io.File;
import java.io.FilenameFilter;
 
public class FileDemo {
 
	public static void main(String[] args) {
		//1.获得系统有效盘符
		File[] files = File.listRoots();
		for (File f : files)
			System.out.println(f.toString());
		System.out.println("——————————————————————");
		//2.获得C:\\目录下的所有文件名，包括隐藏文件和文件夹
		//调用list()方法时，必须先封装一个目录，且必须存在的目录。
		File fnFile = new File("C:\\");
		String[] strings = fnFile.list();
		for (String s : strings)
			System.out.println(s);
		System.out.println("——————————————————————");
		//3.调用list()方法，列出后缀名为.txt的文件
		strings = fnFile.list(new FilenameFilter() {
			
			@Override
			public boolean accept(File dir, String name) {
				//找出后缀名为.txt的文件名
				return name.endsWith(".txt");
			}
		});
		for (String s : strings)
			System.out.println(s);
		System.out.println("——————————————————————");
		//4.获取C:\\目录下的文件夹，不包括文件夹
		files = fnFile.listFiles();
		for (File f : files)
			System.out.println(f.toString());
	}
}
```
## 8、递归打印所有文件名
**注意**：测试中，有些隐藏文件名不能访问还是其他的原因，当在打印根目录（如："C:\\"）下的所有文件名时，会有个叫“System Volume Information”的隐藏文件夹，访问失败，所以以下代码中，不访问隐藏文件。

```java
package file.dol.sn;
 
import java.io.File;
 
public class FileDemo {
 
	public static void main(String[] args) {
 
		File dirFile = new File("E:\\Dol");
		showDir(dirFile,0);
	}
	public static void showDir(File dir,int level)
	{
		System.out.println(printSpace(level)+"文件夹："+dir.getName());
		level += 4;
		File[] file = dir.listFiles();
		for (File f : file)
		{
			//递归进入所有非隐藏文件夹
			if (f.isDirectory() && !f.isHidden())
				showDir(f,level);
			//打印文件名
			else
				System.out.println(printSpace(level)+f.getName());
		}
	}
	//实现分层次打印，补充空格
	public static StringBuffer printSpace(int level)
	{
		StringBuffer space = new StringBuffer();
		for (int i = 0; i < level; ++i)
		{
			space.append("  ");
		}
		return space;
	}
}
```
## 9、递归删除整个文件夹

```java
package file.dol.sn;
 
import java.io.File;
 
public class FileDemo {
 
	public static void main(String[] args) {
 
		File dirFile = new File("E:\\Dol");
		deleteDir(dirFile,0);
	}
	public static void deleteDir(File dir,int level)
	{
		File[] file = dir.listFiles();
		//循环递归删除文件夹里面的所有内容
		for (File f : file)
		{
			//递归进入所有非隐藏文件夹
			if (f.isDirectory() && !f.isHidden())
				deleteDir(f,level);
			//删除文件
			else
			{
				f.delete();
				System.out.println(f.getName()+"——已删除");
			}
		}
		//删除该文件夹
		dir.delete();
		System.out.println(dir.getName()+"——已删除");
	}
}
```
## 10、Properties类

### 1）常用的基本操作，设置键值，获取值。

```java
package file.dol.sn;
 
import java.util.Properties;
import java.util.Set;
 
public class FileDemo {
 
	public static void main(String[] args) {
		//Properties是HashTable的子类，里面存放的都是键值对的字符串
		Properties prop = new Properties();
		//设置键值
		prop.setProperty("Dolphin", "海豚");
		prop.setProperty("Dol", "CSDN");
		//获取
		String value = prop.getProperty("Dolphin");
		System.out.println("@@@@value@@@@"+value);
		
		//返回一个集合
		Set<String> nameSet = prop.stringPropertyNames();
		for (String s : nameSet)
		{
			System.out.println(s+":"+prop.getProperty(s));
		}
	}
}
```
### 2）读取配置文件
读取配置文件，并对配置文件进行修改，修改后再保存。

```java
package file.dol.sn;
 
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Properties;
 
public class FileDemo {
 
	public static void main(String[] args) throws IOException{
 
		Properties prop = new Properties();
		//Properties加载的文件必须为键值对，#注释的不会被加载
		FileInputStream fis = new FileInputStream("pz.txt");
		prop.load(fis);
		//添加一个键值对
		prop.setProperty("Dol", "123");
		//对键值对进行修改
		prop.setProperty("Dol", "321");
		FileOutputStream fos = new FileOutputStream("pz.txt");
		//保存配置文件
		//void java.util.Properties.store(OutputStream arg0, String arg1) throws IOException
		//第二个参数为注释，可写可不写，写入时会自动添加#
		prop.store(fos, "comment");
		
		prop.list(System.out);
		fis.close();
		fos.close();
	}
}
```
## 11、SequenceInputStream类：连接多个流

说明：Enumeration（列举）
public interface Enumeration<E>实现 Enumeration 接口的对象，它生成一系列元素，一次生成一个。连续调用 nextElement 方法将返回一系列的连续元素。
例如，要输出 Vector<E> v 的所有元素，可使用以下方法：
  ```java
  for (Enumeration<E> e = v.elements(); e.hasMoreElements();)
    System.out.println(e.nextElement());
```
这些方法主要通过向量的元素、哈希表的键以及哈希表中的值进行枚举。枚举也用于将输入流指定到 SequenceInputStream 中。
**注**：此接口的功能与 Iterator 接口的功能是重复的。此外，Iterator 接口添加了一个可选的移除操作，并使用较短的方法名。新的实现应该优先考虑使用 Iterator 接口而不是 Enumeration 接口。
有两个方法：
🌂hasMoreElements() 测试此枚举是否包含更多的元素。
🌂🌂nextElement() 如果此枚举对象至少还有一个可提供的元素，则返回此枚举的下一个元素。

```java
package file.dol.sn;
 
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.SequenceInputStream;
import java.util.Enumeration;
import java.util.Vector;
 
public class FileDemo {
 
	public static void main(String[] args) throws IOException {
 
		//将多个流加入集合
		Vector<FileInputStream> vector = new Vector<FileInputStream>();
		vector.add(new FileInputStream("E:\\1.txt"));
		vector.add(new FileInputStream("E:\\2.txt"));
		vector.add(new FileInputStream("E:\\3.txt"));
 
		//java.util.Enumeration<FileInputStream>用法见说明
		Enumeration<FileInputStream> en = vector.elements();
		//连接多个流
		SequenceInputStream sis = new SequenceInputStream(en);
		//启动输出流
		FileOutputStream fos = new FileOutputStream("E:\\4.txt");
		//开始文件拷贝
		byte[] buf = new byte[1024];
		int len = 0;
		while ((len=sis.read(buf))!=-1)
			fos.write(buf,0,len);
		//关闭资源
		fos.close();
		sis.close();
	}
}
```
## 12、对象序列化实现Serializable接口

🌂添加序列号；

🌂🌂静态成员变量不可序列化；

🌂🌂🌂堆内存变量要想不被序列化，可以加transient关键字。

```java
package file.dol.sn;
 
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;
 
//序列化，必须实现Serializable接口，该接口不需要覆盖方法
class Person implements Serializable
{
	//记得添加序列化号
	public static final long serialVersionUID = 42L;
 
	//堆内存变量可序列化
	private String name;
	private int age;
	//如果不想将堆内存里面的变量序列化，如下声明就可以了
	//transient int age;
	//注意，静态成员变量不可序列化
	private static String sex = "male";
	public Person(String n, int a, String s)
	{
		name = n;
		age = a;
		sex = s;
	}
	//覆盖toString()方便println()打印
	public String toString()
	{
		return name+":"+age+":"+sex;
	}
}
public class FileDemo {
 
	public static void WriteOut() throws IOException
	{
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("obj.txt"));
		oos.writeObject(new Person("Dolphin", 20,"female"));
		oos.close();
	}
	public static void ReadIn() throws Exception
	{
		ObjectInputStream ois = new ObjectInputStream(new FileInputStream("obj.txt"));
		Person p = (Person)ois.readObject();
		ois.close();
		System.out.println(p);
	}
	//这里直接抛出一个较大的异常Exception（IOException的基类）
	public static void main(String[] args) throws Exception {
		//注意：以下函数需要分两次运行，每次只运行一个，方便观察运行效果
		//WriteOut();
		ReadIn();
	}
}
```
## 13、管道流

🌂平时两个流读写都是通过内存变量，进行数据操作的；

🌂🌂这里引入管道流，开启两个线程，一个读取流，一个写入流，直接将两个流对接上。

```java
package io.dol.sn;
 
import java.io.PipedInputStream;
import java.io.PipedOutputStream;
//实现Runnable方法，多线程
class Read implements Runnable
{
	private PipedInputStream in;
	public Read(PipedInputStream in) 
	{
		this.in = in;
	}
	public void run() 
	{
		try {
			byte[] buf = new byte[1024];
			int len = 0;
			//如果流中无数据，read()进入等待状态
			while ((len=in.read(buf))!=-1)
			{
				System.out.println(buf);
			}
			in.close();
		} catch (Exception e) {
			throw new RuntimeException("管道流读取失败");
		}
	}
}
 
class Write implements Runnable
{
	private PipedOutputStream out;
	public Write(PipedOutputStream in) 
	{
		this.out = out;
	}
	public void run() 
	{
		try {
			out.write("Piped lai la...".getBytes());
			out.close();
		} catch (Exception e) {
			throw new RuntimeException("管道流输出失败");
		}	
	}
}
 
public class PipedStreamDemo {
 
	public static void main(String[] args) {
		
		PipedInputStream in = new PipedInputStream();
		PipedOutputStream out = new PipedOutputStream();
		
		Read r = new Read(in);
		Write w = new Write(out);
		//开启两线程
		new Thread(r).start();
		new Thread(w).start();
	}
}
```
## 14、RandomAccessFile类

```java
package io.dol.sn;
 
import java.io.IOException;
import java.io.RandomAccessFile;
 
public class RafDemo {
 
	public static void Read() throws IOException
	{
		RandomAccessFile raf = new RandomAccessFile("raf.txt", "rw");
		byte[] buf= new byte[4];
		raf.read(buf);
		String name = new String(buf);
		int age;
		age = raf.readInt();
		
		raf.close();
		
		System.out.println("网名："+name);
		System.out.println("年龄："+age);
	}
	public static void Write() throws IOException
	{
		RandomAccessFile raf = new RandomAccessFile("raf.txt", "rw");
		raf.write("海豚".getBytes());
		raf.writeInt(20);
		raf.close();
	}
	public static void main(String[] args) throws IOException {
		
		Write();
		Read();
		//调整指针位置
		//raf.seek(pos);
		//跳过字节数
		//raf.skipBytes(n);
	}
}
```
## 15、DataStream类

```java
package io.dol.sn;
 
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
 
public class DataStreamDemo {
 
	public static void main(String[] args) throws IOException {
		//写数据
		DataOutputStream out = new DataOutputStream(new FileOutputStream("data.txt"));
		
		out.writeInt(123);
		out.writeDouble(56.88);
		out.writeBoolean(true);
		out.close();
		//读数据
		DataInputStream in = new DataInputStream(new FileInputStream("data.txt"));
		int nInt = in.readInt();
		double nDou = in.readDouble();
		boolean b = in.readBoolean();
		in.close();
		//显示
		System.out.println("nInt:"+nInt);
		System.out.println("nDou:"+nDou);
		System.out.println("b:"+b);
	}
}
```
## 16、ByteArrayStream类

```java
package io.dol.sn;
 
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
 
import javax.print.attribute.standard.Sides;
 
public class ByteArrayStreamDemo {
 
	public static void main(String[] args) {
 
		ByteArrayInputStream in = new ByteArrayInputStream("Dolphin".getBytes());
		ByteArrayOutputStream out = new ByteArrayOutputStream();
		
		int len = 0;
		// int java.io.ByteArrayInputStream.read() 从此输入流中读取下一个数据字节
		while ((len=in.read())!=-1)
		{
			out.write(len);
		}
		System.out.println(out.size());
	}
}

```