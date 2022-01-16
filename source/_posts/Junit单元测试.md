---
title: Junit单元测试
date: 2020-05-02 15:44:16
tags: Java
categories: java单元测试
---
## 测试分类：

1.黑盒测试：不需要写代码，给出输入值，看程序能否输出期望值。

2.白盒测试：需要写代码，关注程序具体执行流程。

## Junit使用：白盒测试

废话不多说，直接附上代码：

```Java
package itcast;
/**
 * 计算器类
 */
public class Calculator{
    //加法
    public int add(int a,int b){
       // int i=3/0;
        return a+b;
    }

    //减法
    public int sub(int a,int b){
        return a-b;
    }
}
```



当然我们也可以使用创建对象来看，但是就不知道是否是执行正确的，并且一个主方法只可以运行一个，就很麻烦，因此我们就用单元测试。

```Java
package itcast;

import java.util.Calendar;

public class CalculatorTest{
    public static void main (String[] args) {
        //创建对象
        Calculator C=new Calculator();
        //调用方法
       // int result=C.add(1,2);
        int result=C.sub(1,1);
        System.out.println(result);
    }
}

```



**测试代码：**

```Java
package test;

import itcast.Calculator;
import org.junit.After;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;

public class CalculatorTest{
    /**
     * 初始化方法；
     * 用于资源申请，所有测试方法在执行之前都会执行该方法
     */
    @Before
    public void init(){
        System.out.println("执行init....");
    }

    /**
     * 释放资源；
     * 在所有方法执行完成后，都会自动执行该方法
     */
    @After
    public void close(){
        System.out.println("执行close....");
    }
    /**
     * 测试add方法
     */
    @Test
    public void testAdd(){
      //  System.out.println("我被执行了");
        //1.创建计算器对象
        Calculator c= new Calculator();
        //2.调用方法
        int result=c.add(1,2);
        System.out.println(result);
        //3.断言 我断言这个结果是3
        Assert.assertEquals(3,result);

    }
    @Test
    public  void testSub(){
        Calculator c= new Calculator();
        //2.调用方法
        int result=c.sub(1,2);
        System.out.println(result);
        System.out.println("执行testSub");
        //3.断言 我断言这个结果是-1
        Assert.assertEquals(-1,result);
    }
}

```



**说明：**单元测试中主要就是给方法加@Test，然后倒入依赖环境，其次代码中用到了断言；



## 判定结果（断言）：

**红色：**失败

**绿色：**成功

**代码：**

```
 Assert.assertEquals(期望结果,运算结果);
```



## 补充：

@Before：修饰的方法会在测试方法之前被自动执行；

 @After：修饰的方法会在测试方法之后被自动执行；

注意：无论测试方法是否会出异常，这两个方法都会被执行。