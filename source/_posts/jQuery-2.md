---
title: jQuery-2
date: 2020-07-30 16:18:57
tags: jQuery
categories: java web
---
## jQuery 的属性操作

html()                   它可以设置和获取起始标签和结束标签中的内容。                 跟 dom 属性 innerHTML 一样。 

text()                     它可以设置和获取起始标签和结束标签中的文本。                 跟 dom 属性 innerText 一样。 

val()                       它可以设置和获取表单项的 value 属性值。                              跟 dom 属性 value 一样 



**val 方法同时设置多个表单项的选中状态：**

```
<!DOCTYPE html>
<html lang="zh_CN">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" src="script/jquery-1.7.2.js"></script>
    <script type="text/javascript">

        $(function () {
/*
            // 批量操作单选
            $(":radio").val(["radio2"]);
            // 批量操作筛选框的选中状态
            $(":checkbox").val(["checkbox3","checkbox2"]);
            // 批量操作多选的下拉框选中状态
            $("#multiple").val(["mul2","mul3","mul4"]);
            // 操作单选的下拉框选中状态
            $("#single").val(["sin2"]);
*/
            $("#multiple,#single,:radio,:checkbox").val(["radio2","checkbox1","checkbox3","mul1","mul4","sin3"]);
        });

    </script>
</head>
<body>
<body>
    单选：
    <input name="radio" type="radio" value="radio1" />radio1
    <input name="radio" type="radio" value="radio2" />radio2
    <br/>
    多选：
    <input name="checkbox" type="checkbox" value="checkbox1" />checkbox1
    <input name="checkbox" type="checkbox" value="checkbox2" />checkbox2
    <input name="checkbox" type="checkbox" value="checkbox3" />checkbox3
    <br/>

    下拉多选 ：
    <select id="multiple" multiple="multiple" size="4">
        <option value="mul1">mul1</option>
        <option value="mul2">mul2</option>
        <option value="mul3">mul3</option>
        <option value="mul4">mul4</option>
    </select>
    <br/>

    下拉单选 ：
    <select id="single">
        <option value="sin1">sin1</option>
        <option value="sin2">sin2</option>
        <option value="sin3">sin3</option>
    </select>
</body>
</body>
</html>
```

**细节：**注意 单选、多选、、下拉多选、下拉单选设置单选还是多选以及是否是下拉细节。



对于jQuery 的属性操作，例如获取文本值以及设置最开始的值都跟dom大同小异

```
<!DOCTYPE html>
<html lang="zh_CN">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" src="script/jquery-1.7.2.js"></script>
    <script type="text/javascript">

        $(function () {
            // 不传参数，是获取，传递参数是设置
            // alert( $("div").html() );// 获取
            // $("div").html("<h1>我是div中的标题 1</h1>");// 设置

            // 不传参数，是获取，传递参数是设置
            // alert( $("div").text() ); // 获取
            // $("div").text("<h1>我是div中的标题 1</h1>"); // 设置

            // 不传参数，是获取，传递参数是设置
            $("button").click(function () {
                alert($("#username").val());//获取
                $("#username").val("超级程序猿");// 设置
            });


        });

    </script>
</head>
<body>
    <div>我是div标签 <span>我是div中的span</span></div>
    <input type="text" name="username" id="username" />
    <button>操作输入框</button>
</body>
</html>
```

## jQuery练习一

**全选，全不选，反选**

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="../../script/jquery-1.7.2.js"></script>
<script type="text/javascript">
	
	$(function(){
		// 给全选绑定单击事件
		$("#checkedAllBtn").click(function () {
			$(":checkbox").prop("checked",true);
		});

		// 给全不选绑定单击事件
		$("#checkedNoBtn").click(function () {
			$(":checkbox").prop("checked",false);
		});

		// 反选单击事件
		$("#checkedRevBtn").click(function () {
			// 查询全部的球类的复选框
			$(":checkbox[name='items']").each(function () {
				// 在each遍历的function函数中，有一个this对象。这个this对象是当前正在遍历到的dom对象
				this.checked = !this.checked;
			});

			// 要检查 是否满选
			// 获取全部的球类个数
			var allCount = $(":checkbox[name='items']").length;
			// 再获取选中的球类个数
			var checkedCount = $(":checkbox[name='items']:checked").length;

			// if (allCount == checkedCount) {
			// 	$("#checkedAllBox").prop("checked",true);
			// } else {
			// 	$("#checkedAllBox").prop("checked",false);
			// }

			$("#checkedAllBox").prop("checked",allCount == checkedCount);

		});

		// 【提交】按钮单击事件
		$("#sendBtn").click(function () {
			// 获取选中的球类的复选框
			$(":checkbox[name='items']:checked").each(function () {
				alert(this.value);
			});
		});

		// 给【全选/全不选】绑定单击事件
		$("#checkedAllBox").click(function () {

			// 在事件的function函数中，有一个this对象，这个this对象是当前正在响应事件的dom对象
			// alert(this.checked);

			$(":checkbox[name='items']").prop("checked",this.checked);
		});

		// 给全部球类绑定单击事件
		$(":checkbox[name='items']").click(function () {
			// 要检查 是否满选
			// 获取全部的球类个数
			var allCount = $(":checkbox[name='items']").length;
			// 再获取选中的球类个数
			var checkedCount = $(":checkbox[name='items']:checked").length;

			$("#checkedAllBox").prop("checked",allCount == checkedCount);
		});

	});
	
</script>
</head>
<body>

	<form method="post" action="">
	
		你爱好的运动是？<input type="checkbox" id="checkedAllBox" />全选/全不选 
		
		<br />
		<input type="checkbox" name="items" value="足球" />足球
		<input type="checkbox" name="items" value="篮球" />篮球
		<input type="checkbox" name="items" value="羽毛球" />羽毛球
		<input type="checkbox" name="items" value="乒乓球" />乒乓球
		<br />
		<input type="button" id="checkedAllBtn" value="全　选" />
		<input type="button" id="checkedNoBtn" value="全不选" />
		<input type="button" id="checkedRevBtn" value="反　选" />
		<input type="button" id="sendBtn" value="提　交" />
	</form>

</body>
</html>
```

## **DOM** **的增删改**

```
<!DOCTYPE html>
<html lang="zh_CN">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" src="script/jquery-1.7.2.js"></script>
    <script type="text/javascript">

        /*
        内部插入：
        appendTo() a.appendTo(b) 把 a 插入到 b 子元素末尾，成为最后一个子元素
        prependTo() a.prependTo(b) 把 a 插到 b 所有子元素前面，成为第一个子元素
        
        外部插入：
        insertAfter() a.insertAfter(b) 得到 ba
        insertBefore() a.insertBefore(b) 得到 ab 
        
        替换: 
        replaceWith() a.replaceWith(b) 用 b 替换掉 a 
        replaceAll() a.replaceAll(b) 用 a 替换掉所有 b 
        
        删除：
        remove() a.remove(); 删除 a 标签 
        empty() a.empty(); 清空 a 标签里的内容
        */

        $(function () {
            //attr
            // alert( $(":checkbox:first").attr("name") ); // 获取
            // $(":checkbox:first").attr("name","abc") ; // 设置

            // $(":checkbox").prop("checked",false );// 官方觉得返回undefined是一个错误

            // $(":checkbox:first").attr("abc","abcValue");
            // alert( $(":checkbox:first").attr("abc") );

             $("<h1>标题</h1>").prependTo( $("div") );
            // $("<h1>标题</h1>").insertAfter("div");

            // $("<h1>标题</h1>").insertBefore( $("div") );

            // $("<h1>标题</h1>").replaceWith("div");

            // $("div").replaceWith( $("<h1>标题</h1>") );

            // $("<h1>标题</h1>").replaceAll( "div" );


            // $("div").empty();

        });


    </script>
</head>
<body>
<body>
    <br/>
    多选：
    <input name="checkbox" type="checkbox" checked="checked" value="checkbox1" />checkbox1
    <input name="checkbox" type="checkbox" value="checkbox2" />checkbox2
    <br/><br/>
    <div>1234</div>
</body>
</body>
</html>
```

## **jQuery** **练习二** 

**从左到右，从右到左练习**

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
	<style type="text/css">
		select {
			width: 100px;
			height: 140px;
		}
		
		div {
			width: 130px;
			float: left;
			text-align: center;
		}
	</style>
	<script type="text/javascript" src="script/jquery-1.7.2.js"></script>
	<script type="text/javascript">
		// 页面加载完成
		$(function () {
			// 第一个按钮 【选中添加到右边】
			$("button:eq(0)").click(function () {
				$("select:eq(0) option:selected").appendTo($("select:eq(1)"));
			});
			// 第二个按钮 【全部添加到右边】
			$("button:eq(1)").click(function () {
				$("select:eq(0) option").appendTo($("select:eq(1)"));
			});

			// 第三个按钮 【选中删除到左边】
			$("button:eq(2)").click(function () {
				$("select:eq(1) option:selected").appendTo($("select:eq(0)"));
			});

			// 第四个按钮 【全部删除到左边】
			$("button:eq(3)").click(function () {
				$("select:eq(1) option").appendTo($("select:eq(0)"));
			});
		});
	</script>
</head>
<body>

	<div id="left">
		<select multiple="multiple" name="sel01">
			<option value="opt01">选项1</option>
			<option value="opt02">选项2</option>
			<option value="opt03">选项3</option>
			<option value="opt04">选项4</option>
			<option value="opt05">选项5</option>
			<option value="opt06">选项6</option>
			<option value="opt07">选项7</option>
			<option value="opt08">选项8</option>
		</select>
		
		<button>选中添加到右边</button>
		<button>全部添加到右边</button>
	</div>
	<div id="rigth">
		<select multiple="multiple" name="sel02">
		</select>
		<button>选中删除到左边</button>
		<button>全部删除到左边</button>
	</div>

</body>
</html>
```

**动态添加、删除表格记录**

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Untitled Document</title>
<link rel="stylesheet" type="text/css" href="styleB/css.css" />
<script type="text/javascript" src="../../script/jquery-1.7.2.js"></script>
<script type="text/javascript">
	
	$(function(){
		
		//删除用户的方法
		function delA(){
			//获取要删除员工的名字
			var name = $(this).parents("tr").find("td:eq(0)").text();
			//弹出一个确认框
			var flag = confirm("确认删除"+name+"吗？");
			if(flag){
				//删除当前a所在的tr
				$(this).parents("tr").remove();
			}
			//取消默认行为
			return false;
		}
		
		//删除用户
		//$("a").click(delA);
		$("a").live("click" , delA);
		
		//添加员工
		$("#addEmpButton").click(function(){
			//获取用户填写的内容
			var name = $("#empName").val();
			var email = $("#email").val();
			var salary = $("#salary").val();
			
			//创建tr
			/*
				<tr>
					<td>Tom</td>
					<td>tom@tom.com</td>
					<td>5000</td>
					<td><a href="#">Delete</a></td>
				</tr>
			*/
			$("<tr></tr>").append("<td>"+name+"</td>")
						  .append("<td>"+email+"</td>")
						  .append("<td>"+salary+"</td>")
						  .append("<td><a href='#'>Delete</a></td>")
						  .appendTo("#employeeTable");
			
		});
	});
	
	
</script>
</head>
<body>

	<table id="employeeTable">
		<tr>
			<th>Name</th>
			<th>Email</th>
			<th>Salary</th>
			<th>&nbsp;</th>
		</tr>
		<tr>
			<td>Tom</td>
			<td>tom@tom.com</td>
			<td>5000</td>
			<td><a href="#">Delete</a></td>
		</tr>
		<tr>
			<td>Jerry</td>
			<td>jerry@sohu.com</td>
			<td>8000</td>
			<td><a href="#">Delete</a></td>
		</tr>
		<tr>
			<td>Bob</td>
			<td>bob@tom.com</td>
			<td>10000</td>
			<td><a href="#">Delete</a></td>
		</tr>
	</table>

	<div id="formDiv">
	
		<h4>添加新员工</h4>

		<table>
			<tr>
				<td class="word">name: </td>
				<td class="inp">
					<input type="text" name="empName" id="empName" />
				</td>
			</tr>
			<tr>
				<td class="word">email: </td>
				<td class="inp">
					<input type="text" name="email" id="email" />
				</td>
			</tr>
			<tr>
				<td class="word">salary: </td>
				<td class="inp">
					<input type="text" name="salary" id="salary" />
				</td>
			</tr>
			<tr>
				<td colspan="2" align="center">
					<button id="addEmpButton" value="abc">
						Submit
					</button>
				</td>
			</tr>
		</table>

	</div>

</body>
</html>

```

## CSS 样式操作

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<style type="text/css">
	/*
	
	addClass() 添加样式 
	removeClass() 删除样式 
	toggleClass() 有就删除，没有就添加样式。 
	offset() 获取和设置元素的坐标
	
	*/
	
	div{
		width:100px;
		height:260px;
	}
	
	div.whiteborder{
		border: 2px white solid;
	}
	
	div.redDiv{
		background-color: red;
	}
	
	div.blueBorder{
		border: 5px blue solid;
	}
	
</style>

<script type="text/javascript" src="script/jquery-1.7.2.js"></script>
<script type="text/javascript">
	

	$(function(){
		
		var $divEle = $('div:first');
		
		$('#btn01').click(function(){
			//addClass() - 向被选元素添加一个或多个类
			$divEle.addClass('redDiv blueBorder');
		});
		
		$('#btn02').click(function(){
			//removeClass() - 从被选元素删除一个或多个类 
			$divEle.removeClass();
		});
	
		
		$('#btn03').click(function(){
			//toggleClass() - 对被选元素进行添加/删除类的切换操作 
			$divEle.toggleClass('redDiv')
		});
		
		
		$('#btn04').click(function(){
			//offset() - 返回第一个匹配元素相对于文档的位置。
			var pos = $divEle.offset();
			console.log(pos);

			$divEle.offset({
				top:100,
				left:50
			});

		});
		
	
		
	})
</script>
</head>
<body>
	<table align="center">
		<tr>
			<td>
				<div class="border">
				</div>
			</td>
			
			<td>
				<div class="btn">
					<input type="button" value="addClass()" id="btn01"/>
					<input type="button" value="removeClass()" id="btn02"/>
					<input type="button" value="toggleClass()" id="btn03"/>
					<input type="button" value="offset()" id="btn04"/>
				</div>
			</td>
		</tr>
	</table>
	
	
	
	<br /> <br />
	
	
	<br /> <br />
	
	
	
</body>
</html>
```

## jQuery 动画

**基本动画** 

show()                        将隐藏的元素显示 

hide()                          将可见的元素隐藏。 

toggle()                       可见就隐藏，不可见就显示。 

以上动画方法都可以添加参数。 

1、第一个参数是动画 执行的时长，以毫秒为单位 

2、第二个参数是动画的回调函数 (动画完成后自动调用的函数) 

**淡入淡出动画** 

fadeIn()                          淡入（慢慢可见） 

fadeOut()                      淡出（慢慢消失） 

fadeTo()                        在指定时长内慢慢的将透明度修改到指定的值。0 透明，1 完成可见，0.5 半透明 

fadeToggle()                 淡入/淡出 切换 

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<link href="css/style.css" type="text/css" rel="stylesheet" />
		<script type="text/javascript" src="script/jquery-1.7.2.js"></script>
	
<script type="text/javascript">
	/* 	
		基本
		show([speed,[easing],[fn]]) 
		hide([speed,[easing],[fn]]) 
		toggle([speed],[easing],[fn]) 
		滑动
		slideDown([spe],[eas],[fn]) 
		slideUp([speed,[easing],[fn]]) 
		slideToggle([speed],[easing],[fn]) 
		淡入淡出
		fadeIn([speed],[eas],[fn]) 
		fadeOut([speed],[eas],[fn]) 
		fadeTo([[spe],opa,[eas],[fn]]) 
		fadeToggle([speed,[eas],[fn]])
		*/
		$(function(){
			//显示   show()
			$("#btn1").click(function(){
				$("#div1").show(2000,function () {
					alert("show动画完成 ")
				});
			});		
			//隐藏  hide()
			$("#btn2").click(function(){
				$("#div1").hide(1000,function () {
					alert("hide动画 执行完成 ")
				});
			});	
			//切换   toggle()
			$("#btn3").click(function(){
				$("#div1").toggle(1000,function () {
					alert("toggle动画 完成 ")
				});
			});

			// var abc = function(){
			// 	$("#div1").toggle(1000,abc);
			// }
			// abc();

			//淡入   fadeIn()
			$("#btn4").click(function(){
				$("#div1").fadeIn(2000,function () {
					alert("fadeIn完成 ")
				});
			});	
			//淡出  fadeOut()
			$("#btn5").click(function(){
				$("#div1").fadeOut(2000,function () {
					alert("fadeOut完成 ")
				});
			});	
			
			//淡化到  fadeTo()
			$("#btn6").click(function(){
				$("#div1").fadeTo(2000,0.5,function () {
					alert('fadeTo完成 ')
				});
			});	
			//淡化切换  fadeToggle()
			$("#btn7").click(function(){
				$("#div1").fadeToggle(1000,function () {
					alert("fadeToggle完成 ")
				});
			});	
		})
</script>
	
	</head>
	<body>
		<table style="float: left;">
			<tr>
				<td><button id="btn1">显示show()</button></td>
			</tr>
			<tr>
				<td><button id="btn2">隐藏hide()</button></td>
			</tr>
			<tr>
				<td><button id="btn3">显示/隐藏切换 toggle()</button></td>
			</tr>
			<tr>
				<td><button id="btn4">淡入fadeIn()</button></td>
			</tr>
			<tr>
				<td><button id="btn5">淡出fadeOut()</button></td>
			</tr>
			<tr>
				<td><button id="btn6">淡化到fadeTo()</button></td>
			</tr>
			<tr>
				<td><button id="btn7">淡化切换fadeToggle()</button></td>
			</tr>
		</table>
		
		<div id="div1" style="float:left;border: 1px solid;background-color: blue;width: 300px;height: 200px;">
			jquery动画定义了很多种动画效果，可以很方便的使用这些动画效果
		</div>
	</body>

</html>

```

**练习 06、CSS_动画 品牌展示**

**需求：**

1.点击按钮的时候，隐藏和显示卡西欧之后的品牌。 

2.当显示全部内容的时候，按钮文本为“显示精简品牌” 

然后，小三角形向上。所有品牌产品为默认颜色。 

3.当只显示精简品牌的时候，要隐藏卡西欧之后的品牌，按钮文本为“显示全部品牌” 

然后小三形向下。并且把 佳能，尼康的品牌颜色改为红色（给 li 标签添加 promoted 样式即可）

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>品牌展示练习</title>
<style type="text/css">
* {
	margin: 0;
	padding: 0;
}

body {
	font-size: 12px;
	text-align: center;
}

a {
	color: #04D;
	text-decoration: none;
}

a:hover {
	color: #F50;
	text-decoration: underline;
}

.SubCategoryBox {
	width: 600px;
	margin: 0 auto;
	text-align: center;
	margin-top: 40px;
}

.SubCategoryBox ul {
	list-style: none;
}

.SubCategoryBox ul li {
	display: block;
	float: left;
	width: 200px;
	line-height: 20px;
}

.showmore , .showless{
	clear: both;
	text-align: center;
	padding-top: 10px;
}

.showmore a , .showless a{
	display: block;
	width: 120px;
	margin: 0 auto;
	line-height: 24px;
	border: 1px solid #AAA;
}

.showmore a span {
	padding-left: 15px;
	background: url(img/down.gif) no-repeat 0 0;
}

.showless a span {
	padding-left: 15px;
	background: url(img/up.gif) no-repeat 0 0;
}

.promoted a {
	color: #F50;
}
</style>
<script type="text/javascript" src="script/jquery-1.7.2.js"></script>
<script type="text/javascript">
	$(function() {
		// 基本初始状态
		$("li:gt(5):not(:last)").hide();

		// 给功能的按钮绑定单击事件
		$("div div a").click(function () {
			// 让某些品牌，显示，或隐藏
			$("li:gt(5):not(:last)").toggle();
			// 判断 品牌，当前是否可见
			if( $("li:gt(5):not(:last)").is(":hidden") ){
				// 品牌隐藏的状态 ：1 显示全部品牌    == 角标向下 showmore
				$("div div a span").text("显示全部品牌");

				$("div div").removeClass();
				$("div div").addClass("showmore");

				// 去掉高亮
				$("li:contains('索尼')").removeClass("promoted");

			} else {
				// 品牌可见的状态：2 显示精简品牌	 == 角标向上 showless
				$("div div a span").text("显示精简品牌");

				$("div div").removeClass();
				$("div div").addClass("showless");

				// 加高亮
				$("li:contains('索尼')").addClass("promoted");
			}

			return false;
		});
	});
</script>
</head>
<body>
	<div class="SubCategoryBox">
		<ul>
			<li><a href="#">佳能</a><i>(30440) </i></li>
			<li><a href="#">索尼</a><i>(27220) </i></li>
			<li><a href="#">三星</a><i>(20808) </i></li>
			<li><a href="#">尼康</a><i>(17821) </i></li>
			<li><a href="#">松下</a><i>(12289) </i></li>
			<li><a href="#">卡西欧</a><i>(8242) </i></li>
			<li><a href="#">富士</a><i>(14894) </i></li>
			<li><a href="#">柯达</a><i>(9520) </i></li>
			<li><a href="#">宾得</a><i>(2195) </i></li>
			<li><a href="#">理光</a><i>(4114) </i></li>
			<li><a href="#">奥林巴斯</a><i>(12205) </i></li>
			<li><a href="#">明基</a><i>(1466) </i></li>
			<li><a href="#">爱国者</a><i>(3091) </i></li>
			<li><a href="#">其它品牌相机</a><i>(7275) </i></li>
		</ul>
		<div class="showmore">
			<a href="more.html"><span>显示全部品牌</span></a>
		</div>
	</div>
</body>
</html>


```

## **jQuery** **事件操作** 

### jQuery 和原生 js 的区别

 **$( function(){} );**

**和**

**window.onload = function(){}** 

**的区别？** 

他们分别是在什么时候触发？ 

​             1、jQuery 的页面加载完成之后是浏览器的内核解析完页面的标签创建好 DOM 对象之后就会马上执行。 

​             2、原生 js 的页面加载完成之后，除了要等浏览器内核解析完标签创建好 DOM 对象，还要等标签显示时需要的内容加载 完成。

他们触发的顺序？ 

​          1、jQuery 页面加载完成之后先执行 

​          2、原生 js 的页面加载完成之后 

他们执行的次数？ 

​          1、原生 js 的页面加载完成之后，只会执行**最后一次**的赋值函数。 

​          2、jQuery 的页面加载完成之后是**全部把注册的 function 函数**，依次顺序全部执行。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<script type="text/javascript" src="../../script/jquery-1.7.2.js"></script>
<script type="text/javascript">

	//会在整个页面加载完毕之后调用
	window.onload = function () {
		alert("原生js的页面加载完成之后--1")
	}
	window.onload = function () {
		alert("原生js的页面加载完成之后--2")
	}
	window.onload = function () {
		alert("原生js的页面加载完成之后--3")
	}

	//会在当前文档加载完毕之后调用
	$(function () {
		alert("jquery的页面加载完成 之后--1")
	});
	// jquery的页面加载完成 之后
	$(function () {
		alert("jquery的页面加载完成 之后--2")
	});
	$(function () {
		alert("jquery的页面加载完成 之后--3")
	});

	//1、导入jquery的.js文件
	//2、可以在导入文件后的任意位置
	//   1）、可以写在head里面，如果写在head里面可能导致元素查找不到等问题
	//			1-1、只需要把代码写在$(function(){ jquery代码  })
	//   2）、可以写在head之后的任意位置，我们一般不采用这种写法。
	//	 3）、综合以上考虑，1-1
	//3、window.onload & $(function(){})
	//window.onload只可以使用一次
	//$(function(){})可以使用多次
</script>
</head>
<body>
	<button>我是按钮</button>
	
	<iframe src="http://localhost:8080"></iframe>
	<img src="http://localhost:8080/1.jpg" alt=""/>
</body>
</html>
```

### **jQuery** **中其他的事件处理方法：**

click()                               它可以绑定单击事件，以及触发单击事件 

mouseover()                   鼠标移入事件 

mouseout()                     鼠标移出事件 

bind()                               可以给元素一次性绑定一个或多个事件。 

one()                                 使用上跟 bind 一样。但是 one 方法绑定的事件只会响应一次。 

unbind()                           跟 bind 方法相反的操作，解除事件的绑定 

live()                                  也是用来绑定事件。它可以用来绑定选择器匹配的所有元素的事件。哪怕这个元素是后面                                              动态创建出来的也有效 

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<link href="css/style.css" type="text/css" rel="stylesheet" />
		<script type="text/javascript" src="../../script/jquery-1.7.2.js"></script>
		<script type="text/javascript">
		
			$(function(){
				//*1.通常绑定事件的方式
				//给元素绑定事件  
				//jquery对象.事件方法(回调函数(){ 触发事件执行的代码 }).事件方法(回调函数(){ 触发事件执行的代码 }).事件方法(回调函数(){ 触发事件执行的代码 })
				//绑定事件可以链式操作
				// $(".head").click(function(){
				// 	$(".content").toggle();
				// }).mouseover(function(){
				// 	$(".content").toggle();
				// });
				//
				//*2.jQuery提供的绑定方式：bind(type,[data],fn)函数把元素和事件绑定起来
				//type表示要绑定的事件   [data]表示传入的数据   fn表示事件的处理方法
				//bind(事件字符串,回调函数),后来添加的元素不会绑定事件
				//使用bind()绑定多个事件   type可以接受多个事件类型，使用空格分割多个事件
				 $(".head").bind("click mouseover",function(){
					$(".content").toggle();
				});
			
				
				//3.one()只绑定一次,绑定的事件只会发生一次one(type,[data],fn)函数把元素和事件绑定起来
				//type表示要绑定的事件   [data]表示传入的数据   fn表示事件的处理方法
			/* 	$(".head").one("click mouseover",function(){
					$(".content").toggle();
				}); */

				//4.live方法会为现在及以后添加的元素都绑定上相应的事件
			/**	$(".head").live("click",function(){
					$(".content").toggle();
				});
				
				$("#panel").before("<h5 class='head'>什么是jQuery?</h5>");
			*/
			});
		
		</script>
	</head>
	<body>
		<div id="panel">
			<h5 class="head">什么是jQuery?</h5>
			<div class="content">
				jQuery是继Prototype之后又一个优秀的JavaScript库，它是一个由 John Resig 创建于2006年1月的开源项目。jQuery凭借简洁的语法和跨平台的兼容性，极大地简化了JavaScript开发人员遍历HTML文档、操作DOM、处理事件、执行动画和开发Ajax。它独特而又优雅的代码风格改变了JavaScript程序员的设计思路和编写程序的方式。
			</div>
		</div>
	</body>

</html>

```

### **事件的冒泡**

**什么是事件的冒泡？** 

事件的冒泡是指，父子元素同时监听同一个事件。当触发子元素的事件的时候，同一个事件也被传递到了父元素的事件里去 响应。 

**那么如何阻止事件冒泡呢？** 

在子元素事件函数体内，return false; 可以阻止事件的冒泡传递。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Untitled Document</title>
		<style type="text/css">
			*{
				margin: 0;
				padding: 0;
			}
			body{
				font-size: 13px;
				line-height: 130%;
				padding: 60px;
			}
			#content{
				width: 220px;
				border: 1px solid #0050D0;
				background: #96E555;
			}
			span{
				width: 200px;
				margin: 10px;
				background: #666666;
				cursor: pointer;
				color: white;
				display: block;
			}
			p{
				width: 200px;
				background: #888;
				color: white;
				height: 16px;
			}
		</style>
		<script type="text/javascript" src="jquery-1.7.2.js"></script>
		<script type="text/javascript">
			$(function(){
				
				//冒泡就是事件的向上传导，子元素的事件被触发，父元素的响应事件也会触发
				//解决冒泡问题：return false;
				
				//给span绑定一个单击响应函数
				$("span").click(function(){
					alert("我是span的单击响应函数");
					return false;
				});
				
				//给id为content的div绑定一个单击响应函数
				$("#content").click(function(){
					alert("我是div的单击响应函数");
					return false;
				});
				
				//给body绑定一个单击响应函数
				$("body").click(function(){
					//alert("我是body的单击响应函数");
				});
				
				//取消默认行为
				/* $("a").click(function(){
					return false;
				}) */
				
			})
		</script>
	</head>
	<body>
		<div id="content">
			外层div元素
			<span>内层span元素</span>
			外层div元素
		</div>
		
		<div id="msg"></div>	
		
		<br><br>
		<a href="http://www.hao123.com" onclick="return false;">WWW.HAO123.COM</a>	
	</body>
</html>


```

### **javaScript** **事件对象** 

事件对象，是封装有触发的事件信息的一个 javascript 对象。 

我们重点关心的是怎么拿到这个 javascript 的事件对象。以及使用。 

如何获取呢 javascript 事件对象呢？ 

在给元素绑定事件的时候，在事件的 function( event ) 参数列表中添加一个参数，这个参数名，我们习惯取名为 event。 

这个 event 就是 javascript 传递参事件处理函数的事件对象。 

比如：//1.原生 javascript 获取 事件对象 

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<style type="text/css">

	#areaDiv {
		border: 1px solid black;
		width: 300px;
		height: 50px;
		margin-bottom: 10px;
	}
	
	#showMsg {
		border: 1px solid black;
		width: 300px;
		height: 20px;
	}

</style>
<script type="text/javascript" src="jquery-1.7.2.js"></script>
<script type="text/javascript">

	//1.原生javascript获取 事件对象
	// window.onload = function () {
	// 	document.getElementById("areaDiv").onclick = function (event) {
	// 		console.log(event);
	// 	}
	// }
	//2.JQuery代码获取 事件对象
	$(function () {
		// $("#areaDiv").click(function (event) {
		// 	console.log(event);
		// });
		//3.使用bind同时对多个事件绑定同一个函数。怎么获取当前操作是什么事件。

		$("#areaDiv").bind("mouseover mouseout",function (event) {
			if (event.type == "mouseover") {
				console.log("鼠标移入");
			} else if (event.type == "mouseout") {
				console.log("鼠标移出");
			}
		});


	});



</script>
</head>
<body>

	<div id="areaDiv"></div>
	<div id="showMsg"></div>

</body>
</html>
```



### **图片跟随**

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<style type="text/css">
	body {
		text-align: center;
	}
	#small {
		margin-top: 150px;
	}
	#showBig {
		position: absolute;
		display: none;
	}
</style>
<script type="text/javascript" src="script/jquery-1.7.2.js"></script>
<script type="text/javascript">
	$(function(){
		$("#small").bind("mouseover mouseout mousemove",function (event) {
			if (event.type == "mouseover") {
				$("#showBig").show();
			} else if (event.type == "mousemove") {
				console.log(event);
				$("#showBig").offset({
					left: event.pageX + 10,
					top: event.pageY + 10
				});
			} else if (event.type == "mouseout") {
				$("#showBig").hide();
			}
		});
	});
</script>
</head>
<body>

	<img id="small" src="img/small.jpg" />
	
	<div id="showBig">
		<img src="img/big.jpg">
	</div>

</body>
</html>
```

