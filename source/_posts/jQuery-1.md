---
title: jQuery-1
date: 2020-07-19 21:11:54
tags: jQuery
categories: java web
---
## **jQuery** **介绍** 

**什么是 jQuery ?** 

jQuery，顾名思义，也就是 JavaScript 和查询（Query），它就是辅助 JavaScript 开发的 js 类库。 

**jQuery 核心思想！！！** 

它的核心思想是 write less,do more(写得更少,做得更多)，所以它实现了很多浏览器的兼容问题。 

**jQuery 流行程度** 

jQuery 现在已经成为最流行的 JavaScript 库，在世界前 10000 个访问最多的网站中，有超过 55%在使用 

jQuery。 

**jQuery 好处！！！** 

jQuery 是免费、开源的，jQuery 的语法设计可以使开发更加便捷，例如操作文档对象、选择 DOM 元素、 

制作动画效果、事件处理、使用 Ajax 

## **jQuery** **的初体验**

需求：使用 jQuery 给一个按钮绑定单击事件?

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
   <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
   <script type="text/javascript">
      // window.onload = function () {
      //     var btnObj = document.getElementById("btnId");
      //     // alert(btnObj);//[object HTMLButtonElement]   ====>>>  dom对象
      //     btnObj.onclick = function () {
      //        alert("js 原生的单击事件");
      //     }
      // }

      $(function () { // 表示页面加载完成 之后，相当 window.onload = function () {}
         var $btnObj = $("#btnId"); // 表示按id查询标签对象
         $btnObj.click(function () { // 绑定单击事件
            alert("jQuery 的单击事件");
         });
      });

   </script>



</head>
<body>
   <button id="btnId">SayHello</button>
</body>
</html>
```

**细节：**jQuery 一定要引入 jQuery 库，jQuery 中的$是一个函数 ，

**怎么为按钮添加点击响应函数的？** 

1、使用 jQuery 查询到标签对象 

2、使用标签对象.click( function(){} );

```
$(function () { // 表示页面加载完成 之后，相当 window.onload = function () {}
   var $btnObj = $("#btnId"); // 表示按id查询标签对象
   $btnObj.click(function () { // 绑定单击事件
      alert("jQuery 的单击事件");
   });
});
```

## **jQuery** **核心函数** 

$ 是 jQuery 的核心函数，能完成 jQuery 的很多功能。$()就是调用$这个函数

**1、传入参数为 [ 函数 ] 时：** 

表示页面加载完成之后。相当于 window.onload = function(){} 

**2、传入参数为 [ HTML 字符串 ] 时：** 

会对我们创建这个 html 标签对象 

**3、传入参数为 [ 选择器字符串 ] 时：** 

$(“#id 属性值”);                id 选择器，根据 id 查询标签对象 

$(“标签名”);                       标签名选择器，根据指定的标签名查询标签对象 

$(“.class 属性值”);           类型选择器，可以根据 class 属性查询标签对象 

**4、传入参数为 [ DOM 对象 ] 时：** 

会把这个 dom 对象转换为 jQuery 对象 

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
<script type="text/javascript">

   //核心函数的4个作用

    //传入参数为[函数]时：在文档加载完成后执行这个函数
    //传入参数为[HTML字符串]时：根据这个字符串创建元素节点对象
    //传入参数为[选择器字符串]时：根据这个字符串查找元素节点对象
    //传入参数为[DOM对象]时：将DOM对象包装为jQuery对象返回
    $(function () {
         //alert("页面加载完成之后，自动调用");

        $("    <div>" +
            "        <span>div-span1</span>" +
            "        <span>div-span2</span>" +
            "    </div>").appendTo("body");

         alert($("button").length);//3

        var btnObj = document.getElementById("btn01");
        // alert(btnObj);
        // alert( $(btnObj) );

        // alert( $("<h1></h1>") );
        alert($("button"));

    });

</script>
</head>
<body>
    <button id="btn01">按钮1</button>
    <button>按钮2</button>
    <button>按钮3</button>
</body>
</html>
```

## **jQuery** **对象和** **dom** **对象区分** 

### **什么是** **jQuery** **对象，什么是** **dom** **对象** 

#### **Dom** **对象** 

1.通过 getElementById()查询出来的标签对象是 Dom 对象 

2.通过 getElementsByName()查询出来的标签对象是 Dom 对象 

3.通过 getElementsByTagName()查询出来的标签对象是 Dom 对象 

4.通过 createElement() 方法创建的对象，是 Dom 对象 

DOM 对象 Alert 出来的效果是：*[object HTML* *标签名* *Element]* 

#### **jQuery** **对象** 

5.通过 JQuery 提供的 API 创建的对象，是 JQuery 对象 

6.通过 JQuery 包装的 Dom 对象，也是 JQuery 对象 

7.通过 JQuery 提供的 API 查询到的对象，是 JQuery 对象 

jQuery 对象 Alert 出来的效果是：[object Object]

### **jQuery** **对象的本质是什么？** 

jQuery 对象是 dom 对象的数组 + jQuery 提供的一系列功能函数。

### **jQuery** **对象和** **Dom** **对象使用区别** 

jQuery 对象不能使用 DOM 对象的属性和方法 

DOM 对象也不能使用 jQuery 对象的属性和方法 

### **Dom** **对象和** **jQuery** **对象互转** 

#### 1、dom对象转化为 **jQuery** 对象

1、先有 DOM 对象 

2、$( DOM 对象 ) 就可以转换成为 jQuery 对象 

#### 2、Query **对象转为** dom对象

1、先有 jQuery 对象 

2、jQuery 对象[下标]取出相应的 DOM 对象

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
<script type="text/javascript">

   $(function(){
      //testDiv.css("color","red")
      //testDiv.style.color = "blue";

      // var arr = [12,"abc",true];
      //
      // var $btns = $("button");
      //
      // for (var i = 0; i < $btns.length; i++){
      //     alert($btns[i]);
      // }

      // document.getElementById("testDiv").innerHTML = "这是dom对象的属性InnerHTML";
      // $("#testDiv").innerHTML = "这是dom对象的属性InnerHTML";

      // $("#testDiv").click(function () {
      //     alert("click()是jQuery对象的方法");
      // });

      // document.getElementById("testDiv").click(function () {
      //     alert("click()是jQuery对象的方法");
      // });

      // alert( $(document.getElementById("testDiv"))[0] );

      alert( $("button:first") );
   });

</script>
</head>
<body>
   <div id="testDiv">Atguigu is Very Good!</div>
   
   <button id="dom2dom">使用DOM对象调用DOM方法</button>
   <button id="dom2jQuery">使用DOM对象调用jQuery方法</button>
   <button id="jQuery2jQuery">使用jQuery对象调用jQuery方法</button>
   <button id="jQuery2dom">使用jQuery对象调用DOM方法</button>



</body>
</html>
```

## **jQuery** 选择器

### 基本选择器

\#ID选择器：根据 id 查找标签对象 

.class 选择器：根据 class 查找标签对象 

element 选择器：根据标签名查找标签对象 

\* 选择器：表示任意的，所有的元素 

selector1，selector2 组合选择器：合并选择器 1，选择器 2 的结果并返回 

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
   <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
      <title>Untitled Document</title>
      <style type="text/css">
         div, span, p {
             width: 140px;
             height: 140px;
             margin: 5px;
             background: #aaa;
             border: #000 1px solid;
             float: left;
             font-size: 17px;
             font-family: Verdana;
         }
         
         div.mini {
             width: 55px;
             height: 55px;
             background-color: #aaa;
             font-size: 12px;
         }
         
         div.hide {
             display: none;
         }
      </style>
      <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
      <script type="text/javascript">
         
            $(function () {
               //1.选择 id 为 one 的元素 "background-color","#bbffaa"
               $("#btn1").click(function () {
                  // css() 方法 可以设置和获取样式
                  $("#one").css("background-color","#bbffaa");
               });


               //2.选择 class 为 mini 的所有元素
               $("#btn2").click(function () {
                  $(".mini").css("background-color","#bbffaa");
               });

               //3.选择 元素名是 div 的所有元素
               $("#btn3").click(function () {
                  $("div").css("background-color","#bbffaa");
               });

               //4.选择所有的元素
               $("#btn4").click(function () {
                  $("*").css("background-color","#bbffaa");
               });

               //5.选择所有的 span 元素和id为two的元素
               $("#btn5").click(function () {
                  $("span,#two").css("background-color","#bbffaa");
               });

            });

      </script>
   </head>
   <body>
<!--   <div>
      <h1>基本选择器</h1>
   </div>  -->   
      <input type="button" value="选择 id 为 one 的元素" id="btn1" />
      <input type="button" value="选择 class 为 mini 的所有元素" id="btn2" />
      <input type="button" value="选择 元素名是 div 的所有元素" id="btn3" />
      <input type="button" value="选择 所有的元素" id="btn4" />
      <input type="button" value="选择 所有的 span 元素和id为two的元素" id="btn5" />
      
      <br>
      <div class="one" id="one">
         id 为 one,class 为 one 的div
         <div class="mini">class为mini</div>
      </div>
      <div class="one" id="two" title="test">
         id为two,class为one,title为test的div
         <div class="mini" title="other">class为mini,title为other</div>
         <div class="mini" title="test">class为mini,title为test</div>
      </div>
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini"></div>
      </div>
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini" title="tesst">class为mini,title为tesst</div>
      </div>
      <div style="display:none;" class="none">style的display为"none"的div</div>
      <div class="hide">class为"hide"的div</div>
      <div>
         包含input的type为"hidden"的div<input type="hidden" size="8">
      </div>
      <span class="one" id="span">^^span元素^^</span>
   </body>
</html>
```

### **层级选择器**

ancestor descendant   后代选择器 ：在给定的祖先元素下匹配所有的后代元素 

parent > child 子元素选择器：在给定的父元素下匹配所有的子元素 

prev + next 相邻元素选择器：匹配所有紧接在 prev 元素后的 next 元素 

prev ~ sibings 之后的兄弟元素选择器：匹配 prev 元素之后的所有 siblings 元素

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
   <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
      <title>Untitled Document</title>
      <style type="text/css">
         div, span, p {
             width: 140px;
             height: 140px;
             margin: 5px;
             background: #aaa;
             border: #000 1px solid;
             float: left;
             font-size: 17px;
             font-family: Verdana;
         }
         
         div.mini {
             width: 55px;
             height: 55px;
             background-color: #aaa;
             font-size: 12px;
         }
         
         div.hide {
             display: none;
         }        
      </style>
      <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
      <script type="text/javascript">
         $(document).ready(function(){
            //1.选择 body 内的所有 div 元素
            $("#btn1").click(function(){
               $("body div").css("background", "#bbffaa");
            });

            //2.在 body 内, 选择div子元素  
            $("#btn2").click(function(){
               $("body > div").css("background", "#bbffaa");
            });

            //3.选择 id 为 one 的下一个 div 元素 
            $("#btn3").click(function(){
               $("#one+div").css("background", "#bbffaa");
            });

            //4.选择 id 为 two 的元素后面的所有 div 兄弟元素
            $("#btn4").click(function(){
               $("#two~div").css("background", "#bbffaa");
            });
         });
      </script>
   </head>
   <body> 
   
<!--   <div>
      <h1>层级选择器:根据元素的层级关系选择元素</h1>
      ancestor descendant  ：
      parent > child           ：
      prev + next          ：
      prev ~ siblings       ：
   </div>  -->
      <input type="button" value="选择 body 内的所有 div 元素" id="btn1" />
      <input type="button" value="在 body 内, 选择div子元素" id="btn2" />
      <input type="button" value="选择 id 为 one 的下一个 div 元素" id="btn3" />
      <input type="button" value="选择 id 为 two 的元素后面的所有 div 兄弟元素" id="btn4" />
      <br><br>
      <div class="one" id="one">
         id 为 one,class 为 one 的div
         <div class="mini">class为mini</div>
      </div>
      <div class="one" id="two" title="test">
         id为two,class为one,title为test的div
         <div class="mini" title="other">class为mini,title为other</div>
         <div class="mini" title="test">class为mini,title为test</div>
      </div>
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini"></div>
      </div>
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini" title="tesst">class为mini,title为tesst</div>
      </div>
      <div style="display:none;" class="none">style的display为"none"的div</div>
      <div class="hide">class为"hide"的div</div>
      <div>
         包含input的type为"hidden"的div<input type="hidden" size="8">
      </div>
      <span id="span">^^span元素^^</span>
   </body>
</html>
```

### 过滤选择器

#### **基本过滤器：**

**:first** 获取第一个元素 

**:last** 获取最后个元素 

**:not(selector)** 去除所有与给定选择器匹配的元素 

**:even** 匹配所有索引值为偶数的元素，从 0 开始计数 

**:odd** 匹配所有索引值为奇数的元素，从 0 开始计数 

**:eq(index)** 匹配一个给定索引值的元素 

**:gt(index)** 匹配所有大于给定索引值的元素 

**:lt(index)** 匹配所有小于给定索引值的元素 

**:header** 匹配如 h1, h2, h3 之类的标题元素 

**:animated** 匹配所有正在执行动画效果的元素 

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
   <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
      <title>Untitled Document</title>
      <style type="text/css">
         div, span, p {
             width: 140px;
             height: 140px;
             margin: 5px;
             background: #aaa;
             border: #000 1px solid;
             float: left;
             font-size: 17px;
             font-family: Verdana;
         }
         
         div.mini {
             width: 55px;
             height: 55px;
             background-color: #aaa;
             font-size: 12px;
         }
         
         div.hide {
             display: none;
         }        
      </style>
      <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
      <script type="text/javascript">
         $(document).ready(function(){
            function anmateIt(){
               $("#mover").slideToggle("slow", anmateIt);
            }
            anmateIt();
         });
         
         $(document).ready(function(){
            //1.选择第一个 div 元素  
            $("#btn1").click(function(){
               $("div:first").css("background", "#bbffaa");
            });

            //2.选择最后一个 div 元素
            $("#btn2").click(function(){
               $("div:last").css("background", "#bbffaa");
            });

            //3.选择class不为 one 的所有 div 元素
            $("#btn3").click(function(){
               $("div:not(.one)").css("background", "#bbffaa");
            });

            //4.选择索引值为偶数的 div 元素
            $("#btn4").click(function(){
               $("div:even").css("background", "#bbffaa");
            });

            //5.选择索引值为奇数的 div 元素
            $("#btn5").click(function(){
               $("div:odd").css("background", "#bbffaa");
            });

            //6.选择索引值为大于 3 的 div 元素
            $("#btn6").click(function(){
               $("div:gt(3)").css("background", "#bbffaa");
            });

            //7.选择索引值为等于 3 的 div 元素
            $("#btn7").click(function(){
               $("div:eq(3)").css("background", "#bbffaa");
            });

            //8.选择索引值为小于 3 的 div 元素
            $("#btn8").click(function(){
               $("div:lt(3)").css("background", "#bbffaa");
            });

            //9.选择所有的标题元素
            $("#btn9").click(function(){
               $(":header").css("background", "#bbffaa");
            });

            //10.选择当前正在执行动画的所有元素
            $("#btn10").click(function(){
               $(":animated").css("background", "#bbffaa");
            });
            //11.选择没有执行动画的最后一个div
            $("#btn11").click(function(){
               $("div:not(:animated):last").css("background", "#bbffaa");
            });
         });
      </script>
   </head>
   <body>
      <input type="button" value="选择第一个 div 元素" id="btn1" />
      <input type="button" value="选择最后一个 div 元素" id="btn2" />
      <input type="button" value="选择class不为 one 的所有 div 元素" id="btn3" />
      <input type="button" value="选择索引值为偶数的 div 元素" id="btn4" />
      <input type="button" value="选择索引值为奇数的 div 元素" id="btn5" />
      <input type="button" value="选择索引值为大于 3 的 div 元素" id="btn6" />
      <input type="button" value="选择索引值为等于 3 的 div 元素" id="btn7" />
      <input type="button" value="选择索引值为小于 3 的 div 元素" id="btn8" />
      <input type="button" value="选择所有的标题元素" id="btn9" />
      <input type="button" value="选择当前正在执行动画的所有元素" id="btn10" />    
      <input type="button" value="选择没有执行动画的最后一个div" id="btn11" />


      <h3>基本选择器.</h3>
      <br><br>
      <div class="one" id="one">
         id 为 one,class 为 one 的div
         <div class="mini">class为mini</div>
      </div>
      <div class="one" id="two" title="test">
         id为two,class为one,title为test的div
         <div class="mini" title="other">class为mini,title为other</div>
         <div class="mini" title="test">class为mini,title为test</div>
      </div>
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini"></div>
      </div>
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini" title="tesst">class为mini,title为tesst</div>
      </div>
      <div style="display:none;" class="none">style的display为"none"的div</div>
      <div class="hide">class为"hide"的div</div>
      <div>
         包含input的type为"hidden"的div<input type="hidden" size="8">
      </div>
      <div id="mover">正在执行动画的div元素.</div>
   </body>
</html>
```

#### **内容过滤器：**

**:contains(text)** 匹配包含给定文本的元素 

**:empty** 匹配所有不包含子元素或者文本的空元素 

**:parent** 匹配含有子元素或者文本的元素 

**:has(selector)** 匹配含有选择器所匹配的元素的元素

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
   <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
      <title>Untitled Document</title>
      <style type="text/css">
         div, span, p {
             width: 140px;
             height: 140px;
             margin: 5px;
             background: #aaa;
             border: #000 1px solid;
             float: left;
             font-size: 17px;
             font-family: Verdana;
         }
         
         div.mini {
             width: 55px;
             height: 55px;
             background-color: #aaa;
             font-size: 12px;
         }
         
         div.hide {
             display: none;
         }        
      </style>
      <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
      <script type="text/javascript">
         $(document).ready(function(){
            function anmateIt(){
               $("#mover").slideToggle("slow", anmateIt);
            }
   
            anmateIt();             
         });
         
         /** 
         :contains(text)   
         :empty             
         :has(selector)     
         :parent          
         */
         $(document).ready(function(){
            //1.选择 含有文本 'di' 的 div 元素
            $("#btn1").click(function(){
               $("div:contains('di')").css("background", "#bbffaa");
            });

            //2.选择不包含子元素(或者文本元素) 的 div 空元素
            $("#btn2").click(function(){
               $("div:empty").css("background", "#bbffaa");
            });

            //3.选择含有 class 为 mini 元素的 div 元素
            $("#btn3").click(function(){
               $("div:has(.mini)").css("background", "#bbffaa");
            });

            //4.选择含有子元素(或者文本元素)的div元素
            $("#btn4").click(function(){
               $("div:parent").css("background", "#bbffaa");
            });
         });
      </script>
   </head>
   <body>    
      <input type="button" value="选择 含有文本 'di' 的 div 元素" id="btn1" />
      <input type="button" value="选择不包含子元素(或者文本元素) 的 div 空元素" id="btn2" />
      <input type="button" value="选择含有 class 为 mini 元素的 div 元素" id="btn3" />
      <input type="button" value="选择含有子元素(或者文本元素)的div元素" id="btn4" />
      
      <br><br>
      <div class="one" id="one">
         id 为 one,class 为 one 的div
         <div class="mini">class为mini</div>
      </div>
      <div class="one" id="two" title="test">
         id为two,class为one,title为test的div
         <div class="mini" title="other">class为mini,title为other</div>
         <div class="mini" title="test">class为mini,title为test</div>
      </div>
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini"></div>
      </div>
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini" title="tesst">class为mini,title为tesst</div>
      </div>
      <div style="display:none;" class="none">style的display为"none"的div</div>
      <div class="hide">class为"hide"的div</div>
      <div>
         包含input的type为"hidden"的div<input type="hidden" size="8">
      </div>
      <div id="mover">正在执行动画的div元素.</div>
   </body>
</html>
```

#### **属性过滤器：**

**[attribute]** 匹配包含给定属性的元素。

**[attribute=value]** 匹配给定的属性是某个特定值的元素

**[attribute!=value]** 匹配所有不含有指定的属性，或者属性不等于特定值的元素。 

**[attribute^=value]** 匹配给定的属性是以某些值开始的元素 

**[attribute$=value]** 匹配给定的属性是以某些值结尾的元素 

**[attribute*=value]** 匹配给定的属性是以包含某些值的元素 

**[attrSel1][attrSel2][attrSelN]** 复合属性选择器，需要同时满足多个条件时使用。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Untitled Document</title>
<style type="text/css">
div,span,p {
   width: 140px;
   height: 140px;
   margin: 5px;
   background: #aaa;
   border: #000 1px solid;
   float: left;
   font-size: 17px;
   font-family: Verdana;
}

div.mini {
   width: 55px;
   height: 55px;
   background-color: #aaa;
   font-size: 12px;
}

div.hide {
   display: none;
}
</style>
<script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
<script type="text/javascript">
   /**
[attribute]          
[attribute=value]     
[attribute!=value]         
[attribute^=value]        
[attribute$=value]        
[attribute*=value]        
[attrSel1][attrSel2][attrSelN]  
   
   
   */
   $(function() {
      //1.选取含有 属性title 的div元素
      $("#btn1").click(function() {
         $("div[title]").css("background", "#bbffaa");
      });
      //2.选取 属性title值等于'test'的div元素
      $("#btn2").click(function() {
         $("div[title='test']").css("background", "#bbffaa");
      });
      //3.选取 属性title值不等于'test'的div元素(*没有属性title的也将被选中)
      $("#btn3").click(function() {
         $("div[title!='test']").css("background", "#bbffaa");
      });
      //4.选取 属性title值 以'te'开始 的div元素
      $("#btn4").click(function() {
         $("div[title^='te']").css("background", "#bbffaa");
      });
      //5.选取 属性title值 以'est'结束 的div元素
      $("#btn5").click(function() {
         $("div[title$='est']").css("background", "#bbffaa");
      });
      //6.选取 属性title值 含有'es'的div元素
      $("#btn6").click(function() {
         $("div[title*='es']").css("background", "#bbffaa");
      });
      
      //7.首先选取有属性id的div元素，然后在结果中 选取属性title值 含有'es'的 div 元素
      $("#btn7").click(function() {
         $("div[id][title*='es']").css("background", "#bbffaa");
      });
      //8.选取 含有 title 属性值, 且title 属性值不等于 test 的 div 元素
      $("#btn8").click(function() {
         $("div[title][title!='test']").css("background", "#bbffaa");
      });
   });
</script>
</head>
<body>
   <input type="button" value="选取含有 属性title 的div元素." id="btn1" style="display: none;"/>
   <input type="button" value="选取 属性title值等于'test'的div元素." id="btn2" />
   <input type="button"
      value="选取 属性title值不等于'test'的div元素(没有属性title的也将被选中)." id="btn3" />
   <input type="button" value="选取 属性title值 以'te'开始 的div元素." id="btn4" />
   <input type="button" value="选取 属性title值 以'est'结束 的div元素." id="btn5" />
   <input type="button" value="选取 属性title值 含有'es'的div元素." id="btn6" />
   <input type="button"
      value="组合属性选择器,首先选取有属性id的div元素，然后在结果中 选取属性title值 含有'es'的 div 元素."
      id="btn7" />
   <input type="button"
      value="选取 含有 title 属性值, 且title 属性值不等于 test 的 div 元素." id="btn8" />

   <br>
   <br>
   <div class="one" id="one">
      id 为 one,class 为 one 的div
      <div class="mini">class为mini</div>
   </div>
   <div class="one" id="two" title="test">
      id为two,class为one,title为test的div
      <div class="mini" title="other">class为mini,title为other</div>
      <div class="mini" title="test">class为mini,title为test</div>
   </div>
   <div class="one">
      <div class="mini">class为mini</div>
      <div class="mini">class为mini</div>
      <div class="mini">class为mini</div>
      <div class="mini"></div>
   </div>
   <div class="one">
      <div class="mini">class为mini</div>
      <div class="mini">class为mini</div>
      <div class="mini">class为mini</div>
      <div class="mini" title="tesst">class为mini,title为tesst</div>
   </div>
   <div style="display: none;" class="none">style的display为"none"的div</div>
   <div class="hide">class为"hide"的div</div>
   <div>
      包含input的type为"hidden"的div<input type="hidden" value="123456789"
         size="8">
   </div>
   <div id="mover">正在执行动画的div元素.</div>
</body>
</html>
```

#### 表单过滤器:

**:input** 匹配所有 input, textarea, select 和 button 元素 

**:text** 匹配所有 文本输入框 

**:password** 匹配所有的密码输入框 

**:radio**匹配所有的单选框 

**:checkbox** 匹配所有的复选框 

**:submit** 匹配所有提交按钮 

**:image** 匹配所有 img 标签 

**:reset** 匹配所有重置按钮 

**:button** 匹配所有 input type=button <button>按钮 

**:file** 匹配所有 input type=file 文件上传 

**:hidden** 匹配所有不可见元素 display:none 或 input type=hidden

#### **表单对象属性过滤器：**

**:enabled** 匹配所有可用元素 

**:disabled** 匹配所有不可用元素 

**:checked** 匹配所有选中的单选，复选，和下拉列表中选中的 option 标签对象

**:selected** 匹配所有选中的option

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
   <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
      <title>Untitled Document</title>
      <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
      <script type="text/javascript">
         $(function(){
            
            
      /**
      :input        
      :text     
      :password  
      :radio        
      :checkbox  
      :submit    
      :image        
      :reset        
      :button    
      :file     
      :hidden    
      
      表单对象的属性
      :enabled      
      :disabled     
      :checked      
      :selected     
      */

               
            //1.对表单内 可用input 赋值操作
            $("#btn1").click(function(){
               // val()可以操作表单项的value属性值
               // 它可以设置和获取
               $(":text:enabled").val("我是万能的程序员");
            });
            //2.对表单内 不可用input 赋值操作
            $("#btn2").click(function(){
               $(":text:disabled").val("管你可用不可用，反正我是万能的程序员");
            });
            //3.获取多选框选中的个数  使用size()方法获取选取到的元素集合的元素个数
            $("#btn3").click(function(){
               alert( $(":checkbox:checked").length );
            });
            //4.获取多选框，每个选中的value值
            $("#btn4").click(function(){
               // 获取全部选中的复选框标签对象
               var $checkboies = $(":checkbox:checked");
               // 老式遍历
               // for (var i = 0; i < $checkboies.length; i++){
               //     alert( $checkboies[i].value );
               // }

               // each方法是jQuery对象提供用来遍历元素的方法
               // 在遍历的function函数中，有一个this对象，这个this对象，就是当前遍历到的dom对象
               $checkboies.each(function () {
                  alert( this.value );
               });

            });
            //5.获取下拉框选中的内容  
            $("#btn5").click(function(){
               // 获取选中的option标签对象
               var $options = $("select option:selected");
               // 遍历，获取option标签中的文本内容
               $options.each(function () {
                  // 在each遍历的function函数中，有一个this对象。这个this对象是当前正在遍历到的dom对象
                  alert(this.innerHTML);
               });
            });
         }) 
      </script>
   </head>
   <body>
      <h3>表单对象属性过滤选择器</h3>
       <button id="btn1">对表单内 可用input 赋值操作.</button>
       <button id="btn2">对表单内 不可用input 赋值操作.</button><br /><br />
       <button id="btn3">获取多选框选中的个数.</button>
       <button id="btn4">获取多选框选中的内容.</button><br /><br />
         <button id="btn5">获取下拉框选中的内容.</button><br /><br />
       
      <form id="form1" action="#">         
         可用元素: <input name="add" value="可用文本框1"/><br>
         不可用元素: <input name="email" disabled="disabled" value="不可用文本框"/><br>
         可用元素: <input name="che" value="可用文本框2"/><br>
         不可用元素: <input name="name" disabled="disabled" value="不可用文本框"/><br>
         <br>
         
         多选框: <br>
         <input type="checkbox" name="newsletter" checked="checked" value="test1" />test1
         <input type="checkbox" name="newsletter" value="test2" />test2
         <input type="checkbox" name="newsletter" value="test3" />test3
         <input type="checkbox" name="newsletter" checked="checked" value="test4" />test4
         <input type="checkbox" name="newsletter" value="test5" />test5
         
         <br><br>
         下拉列表1: <br>
         <select name="test" multiple="multiple" style="height: 100px" id="sele1">
            <option>浙江</option>
            <option selected="selected">辽宁</option>
            <option>北京</option>
            <option selected="selected">天津</option>
            <option>广州</option>
            <option>湖北</option>
         </select>
         
         <br><br>
         下拉列表2: <br>
         <select name="test2">
            <option>浙江</option>
            <option>辽宁</option>
            <option selected="selected">北京</option>
            <option>天津</option>
            <option>广州</option>
            <option>湖北</option>
         </select>
      </form>       
   </body>
</html>
```

## **jQuery** **元素筛选**

```
q()              获取给定索引的元素                                     功能跟 :eq() 一样
first()           获取第一个元素                                         功能跟 :first 一样
last()            获取最后一个元素                                       功能跟 :last 一样
filter(exp)       留下匹配的元素
is(exp)           判断是否匹配给定的选择器，只要有一个匹配就返回，true
has(exp)          返回包含有匹配选择器的元素的元素                       功能跟 :has 一样
not(exp)          删除匹配选择器的元素                                   功能跟 :not 一样
children(exp)     返回匹配给定选择器的子元素                             功能跟 parent>child 一样
find(exp)         返回匹配给定选择器的后代元素                           功能跟 ancestor descendant 一样
next()            返回当前元素的下一个兄弟元素                           功能跟 prev + next 功能一样
nextAll()         返回当前元素后面所有的兄弟元素                          功能跟 prev ~ siblings 功能一样
nextUntil()       返回当前元素到指定匹配的元素为止的后面元素
parent()          返回父元素
prev(exp)         返回当前元素的上一个兄弟元素
prevAll()         返回当前元素前面所有的兄弟元素
prevUnit(exp)     返回当前元素到指定匹配的元素为止的前面元素
siblings(exp)     返回所有兄弟元素
add()             把add匹配的选择器的元素添加到当前jquery对象中
```

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
   <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
      <title>DOM查询</title>
      <style type="text/css">
         div, span, p {
             width: 140px;
             height: 140px;
             margin: 5px;
             background: #aaa;
             border: #000 1px solid;
             float: left;
             font-size: 17px;
             font-family: Verdana;
         }
         
         div.mini {
             width: 55px;
             height: 55px;
             background-color: #aaa;
             font-size: 12px;
         }
         
         div.hide {
             display: none;
         }        
      </style>
      <script type="text/javascript" src="../script/jquery-1.7.2.js"></script>
      <script type="text/javascript">
         $(document).ready(function(){
            function anmateIt(){
               $("#mover").slideToggle("slow", anmateIt);
            }
            anmateIt();
            
   /**
               
   过滤
   eq(index|-index)         
   first()                
   last()                    
   hasClass(class)          
   filter(expr|obj|ele|fn)    
   is(expr|obj|ele|fn)1.6*    
   has(expr|ele)           
   not(expr|ele|fn)         
   slice(start,[end])           
   
   查找
   children([expr])         
   closest(expr,[con]|obj|ele)1.6*   
   find(expr|obj|ele)              
   next([expr])               
   nextall([expr])             
   nextUntil([exp|ele][,fil])1.6*     
   parent([expr])                 
   parents([expr])             
   parentsUntil([exp|ele][,fil])1.6*  
   prev([expr])               
   prevall([expr])             
   prevUntil([exp|ele][,fil])1.6*     
   siblings([expr])            
   
   串联
   add(expr|ele|html|obj[,con])   
                     
   
   */
            
            //(1)eq()  选择索引值为等于 3 的 div 元素
            $("#btn1").click(function(){
               $("div").eq(3).css("background-color","#bfa");
            });
            //(2)first()选择第一个 div 元素
             $("#btn2").click(function(){
                //first()   选取第一个元素
               $("div").first().css("background-color","#bfa");
            });
            //(3)last()选择最后一个 div 元素
            $("#btn3").click(function(){
               //last()  选取最后一个元素
               $("div").last().css("background-color","#bfa");
            });
            //(4)filter()在div中选择索引为偶数的
            $("#btn4").click(function(){
               //filter()  过滤   传入的是选择器字符串
               $("div").filter(":even").css("background-color","#bbffaa");
            });
             //(5)is()判断#one是否为:empty或:parent
            //is用来检测jq对象是否符合指定的选择器
            $("#btn5").click(function(){
               alert( $("#one").is(":empty") );
            });
            
            //(6)has()选择div中包含.mini的
            $("#btn6").click(function(){
               //has(selector)  选择器字符串    是否包含selector
               $("div").has(".mini").css("background-color","#bfa");
            });
            //(7)not()选择div中class不为one的
            $("#btn7").click(function(){
               //not(selector)  选择不是selector的元素
               $("div").not('.one').css("background-color","#bfa");
            });
            //(8)children()在body中选择所有class为one的div子元素
            $("#btn8").click(function(){
               //children()  选出所有的子元素
               $("body").children("div.one").css("background-color","#bfa");
            });
            
            
            //(9)find()在body中选择所有class为mini的div元素
            $("#btn9").click(function(){
               //find()  选出所有的后代元素
               $("body").find("div.mini").css("background-color","#bfa");
            });
            //(10)next() #one的下一个div
            $("#btn10").click(function(){
               //next()  选择下一个兄弟元素
               $("#one").next("div").css("background-color","#bfa");
            });
            //(11)nextAll() #one后面所有的span元素
            $("#btn11").click(function(){
               //nextAll()   选出后面所有的元素
               $("#one").nextAll("span").css("background-color","#bfa");
            });
            //(12)nextUntil() #one和span之间的元素
            $("#btn12").click(function(){
               //
               $("#one").nextUntil("span").css("background-color","#bfa")
            });
            //(13)parent() .mini的父元素
            $("#btn13").click(function(){
               $(".mini").parent().css("background-color","#bfa");
            });
            //(14)prev() #two的上一个div
            $("#btn14").click(function(){
               //prev()  
               $("#two").prev("div").css("background-color","#bfa")
            });
            //(15)prevAll() span前面所有的div
            $("#btn15").click(function(){
               //prevAll()   选出前面所有的元素
               $("span").prevAll("div").css("background-color","#bfa")
            });
            //(16)prevUntil() span向前直到#one的元素
            $("#btn16").click(function(){
               //prevUntil(exp)   找到之前所有的兄弟元素直到找到exp停止
               $("span").prevUntil("#one").css("background-color","#bfa")
            });
            //(17)siblings() #two的所有兄弟元素
            $("#btn17").click(function(){
               //siblings()    找到所有的兄弟元素，包括前面的和后面的
               $("#two").siblings().css("background-color","#bfa")
            });
            
            
            //(18)add()选择所有的 span 元素和id为two的元素
            $("#btn18").click(function(){
   
               //   $("span,#two,.mini,#one")
               $("span").add("#two").add("#one").css("background-color","#bfa");
               
            });
            


         });
         
         
      </script>
   </head>
   <body>    
      <input type="button" value="eq()选择索引值为等于 3 的 div 元素" id="btn1" />
      <input type="button" value="first()选择第一个 div 元素" id="btn2" />
      <input type="button" value="last()选择最后一个 div 元素" id="btn3" />
      <input type="button" value="filter()在div中选择索引为偶数的" id="btn4" />
      <input type="button" value="is()判断#one是否为:empty或:parent" id="btn5" />
      <input type="button" value="has()选择div中包含.mini的" id="btn6" />
      <input type="button" value="not()选择div中class不为one的" id="btn7" />
      <input type="button" value="children()在body中选择所有class为one的div子元素" id="btn8" />
      <input type="button" value="find()在body中选择所有class为mini的div后代元素" id="btn9" />
      <input type="button" value="next()#one的下一个div" id="btn10" />
      <input type="button" value="nextAll()#one后面所有的span元素" id="btn11" />
      <input type="button" value="nextUntil()#one和span之间的元素" id="btn12" />
      <input type="button" value="parent().mini的父元素" id="btn13" />
      <input type="button" value="prev()#two的上一个div" id="btn14" />
      <input type="button" value="prevAll()span前面所有的div" id="btn15" />
      <input type="button" value="prevUntil()span向前直到#one的元素" id="btn16" />
      <input type="button" value="siblings()#two的所有兄弟元素" id="btn17" />
      <input type="button" value="add()选择所有的 span 元素和id为two的元素" id="btn18" />

      
      <h3>基本选择器.</h3>
      <br /><br />
      文本框<input type="text" name="account" disabled="disabled" />
      <br><br>
      <div class="one" id="one">
         id 为 one,class 为 one 的div
         <div class="mini">class为mini</div>
      </div>
      <div class="one" id="two" title="test">
         id为two,class为one,title为test的div
         <div class="mini" title="other"><b>class为mini,title为other</b></div>
         <div class="mini" title="test">class为mini,title为test</div>
      </div>
      
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini"></div>
      </div>
      <div class="one">
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini">class为mini</div>
         <div class="mini" title="tesst">class为mini,title为tesst</div>
      </div>
      <div style="display:none;" class="none">style的display为"none"的div</div>
      <div class="hide">class为"hide"的div</div>
      <span id="span1">^^span元素 111^^</span>
      <div>
         包含input的type为"hidden"的div<input type="hidden" size="8">
      </div>
      <span id="span2">^^span元素 222^^</span>
      <div id="mover">正在执行动画的div元素.</div>
   </body>
</html>
```