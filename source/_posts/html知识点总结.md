---
title: html知识点总结
date: 2020-02-03 19:41:25
tags: html
categories: 前端
---
上学期学习html都忘记的差不多了，简单的写一下博客！

    1 链接   <a href="http://www.runoob.com">这是一个链接</a>
    2 图像   <img src="/images/logo.png" width="258" height="39" />
    3 换行   <br>
    4 hr       元素可用于分隔内容
    5 注释    <!-- 这是一个注释 -->
    6 字体     <font size="5">这是5号字体文本</font>
    7 左右显示字体   <p>该段落文字从左到右显示。</p>  
                              <p><bdo dir="rtl">该段落文字从右到左显示。</bdo></p>
     
    8 段落里面引用  <q></q>
    9 删除字效果和插入字效果   <p>My favorite color is <del>blue</del> <ins>red</ins>!</p>
    10 HTML 文本格式化标签
    标签 	描述
    <b> 	定义粗体文本
    <em> 	定义着重文字
    <i> 	定义斜体字
    <small> 	定义小号字
    <strong> 	定义加重语气
    <sub> 	定义下标字
    <sup> 	定义上标字
    <ins> 	定义插入字
    <del> 	定义删除字
    HTML "计算机输出" 标签
    标签 	描述
    <code> 	定义计算机代码
    <kbd> 	定义键盘码
    <samp> 	定义计算机代码样本
    <var> 	定义变量
    <pre> 	定义预格式文本
    HTML 引文, 引用, 及标签定义
    标签 	描述
    <abbr> 	定义缩写
    <address> 	定义地址
    <bdo> 	定义文字方向
    <blockquote> 	定义长的引用
    <q> 	定义短的引用语
    <cite> 	定义引用、引证
    <dfn> 	定义一个定义项目。
    
    
    11
    <p>创建图片链接:
    <a href="//www.runoob.com/html/html-tutorial.html">
    <img  border="10" src="smiley.gif" alt="HTML 教程" width="32" height="32"></a></p>（其中border是调节图片边框的  例如0代表没有）
    
    
    12  <p>
    这是一个电子邮件链接：
    <a href="mailto:someone@example.com?Subject=Hello%20again" target="_top">
    发送邮件</a>
    </p>
    
    13   定义没有下划线的链接        <a href="//www.runoob.com/" style="text-decoration:none;">访问 runoob.com!</a>
    14 文本框 （定义在head之间）  <link rel="stylesheet" type="text/css" href="styles.css">
    15 定义背景颜色  全局 <body style="background-color:yellow;">
                               标题和段落  
    
    16 颜色和大小  <h1 style="font-family:verdana;">一个标题</h1>
                            <p style="font-family:arial;color:red;font-size:20px;">一个段落。</p>
    17 剧中  <h1 style="text-align:center;">居中对齐的标题</h1>
    18插入图像和动画   一个图像:
    <p> 
    <img src="smiley.gif" alt="Smiley face" width="32" height="32"></p>
    
    <p>
    一个动图:
    <img src="hackanm.gif" alt="Computer man" width="48" height="48"></p>
    19  不同位置的图片  <p>一个来自文件夹中的图像:</p>
    <img src="/images/chrome.gif" alt="Google Chrome" width="33" height="32">
    	<p>一个来自菜鸟教程的图像:</p>
    <img src="//www.runoob.com/images/logo.png" alt="runoob.com" width="336" height="69">
    20 表格  <h4>一行三列:</h4>
    <table border="1">
    <tr>
      <td>100</td>
      <td>200</td>
      <td>300</td>
    </tr>
    </table>
    21 colspan   rowspan    边距 cellpadding
    22表单  <form action="">
    First name: <input type="text" name="firstname"><br>
    Last name: <input type="text" name="lastname">
    </form>
    
    密码框为password
