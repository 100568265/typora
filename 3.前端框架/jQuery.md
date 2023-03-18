# jQuery

一个JavaScript的函数库。它是一个快速的小巧的功能丰富的函数库。

专注于DOM操作。

注意：script标签一定要加上type属性："text/javascript"



用jQuery缩减js代码

```javascript
function fun1(){
			//得到文本框
			//var txt1 = document.getElementById("txt1");
			//以内容弹出
			//alert(txt1.value);
			//jquery实现相同的功能
			alert($("#txt1").val());
    		//$("#txt1")表示拿到id叫"myname"的对象
		}
```



## DOM和jQuery对象

**什么是DOM对象？**

DOM对象是**文档对象模型**，使用js语法创建的对象就是DOM对象，它具备DOM对象的属性和方法。

`var txt1 = document.getElementById("txt1");`txt1就是DOM对象

txt1.value使用的是DOM对象的属性

**什么是jQuery对象？**

jQuery对象就是使用jQuery语法创建的对象，它是一个数组，它具备jQuery对象的属性和方法。

`var txt1 = $("#myname"); `txt1就是jQuery对象

`txt1.val(); `使用的是jQuery对象的方法



**DOM对象转jQuery对象**

使用$()包DOM对象，即可完成转换。

```javascript
<script type="text/javascript">
		function btnClick(){
			//使用DOM方式得到DOM对象
			var dom = document.getElementById("btn");
			//转为jQuery对象
			var jObj = $(dom);
			//使用jQuery的方式操作内容
			alert(jObj.val());
		}
	</script>
```

**jQuery对象转DOM对象**

从jQuery数组中取出的每一个对象，自动转为DOM对象

1. 使用下标取出
2. 使用get(下标取出)

```javascript
<script type="text/javascript>">
		function btnClick(){
			//得到jQuery对象
			var jObj = $("#txt");
			//转为DOM对象
			var dom = jObj[0];
			//取出值进行平方计算
			var mul = dom.value*dom.value;
			//将结果赋值给文本框
			dom.value= mul;
			
		}
	</script>
```



## 选择器

选择器是用来元素定位的。在jQuery所有操作之前，必须要先定位到元素，再对其进行内容，样式，动画，事件等的处理。

### 基本选择器

**id选择器：**

只能返回一个值。

语法：$("#id名称")          

`$("#myname")`

**类选择器：**

根据元素的class属性进行定位，可以返回多个元素。

语法：$(".类名")

`$(".myclass")`

**标签选择器：**

使用元素的标签名进行定位。

语法：$("标签名")

`$("div")`，所有div的元素被定位得到。

**全选选择器：**

语法：`$("*")`选中页面上的所有元素

**组合选择器：**

将各种方式下的选择器组合在一起，通过逗号分隔。小心：id选择器不再唯一。

语法：`$("#myname",.myclass)`



### 层次选择器

```javascript
//var v=$("form input");// 只要是from元素下的所有input子元素，不管该子元素是否在另一个容器元素中
		//var v=$("form>input");// 仅仅是本form元素下的不在其他容器包裹中的的input子元素才可以，
		                        //如果input元素又被放到本from下另一个div的容器中，则不被选择
		//var v=$("form+input");//只取form元素一个同辈份的紧挨着的input元素,一定是同级元素
```



### 表单选择器

通过表单元素的type属性值进行选择定位

<input type=""> text文本框 password密码框 submit提交按钮 reset重置按钮 button普通按钮 checkbox复选框 radio单选框。。。

以下不是表单选择器

```HTML
<select></select>  <textarea></textarea> <button></button>
```

语法：`$(":表单类型")  $(":radio")`

```javascript
function fun3(){
			//读取checkbox的值
			var cks = $(":checkbox:checked");
			for(var i=0;i<cks.length;i++){
				alert(cks[i].value);
			}
		}
```



## 过滤器

### 基本过滤器

过滤器不可以单独存在，必须跟在选择器后面进行二次筛选。

first：选中元素中的第一个

last：选中元素中的最后一次

eq()：得到指定下标的元素

gt()：得到大于指定下标的元素

lt()：得到小于指定下标的元素

语法：$("选择器:过滤器")       `$("div:first")`



### 表单属性过滤器

专门针对表单元素的特殊的一些属性进行过滤

**enabled:**根据当前元素的有效性进行定位

**disabled：**根据当前元素的无效性进行定位

**checked：**单选复选按钮的选中状态进行定位

**selected：**下拉列表框被选中元素定位。注意:selected属性不是添加在select标签上的，所有使用时要select>option:selected

```javascript
$("#btn1").on("click",function(){
				//设置所有不可用文本框hello
				$(":text:disabled").val("hello");
			});
```



### 显示隐藏过滤器

:hidden匹配所有不可见元素,或者type为hidden的元素。

:visible匹配所有的可见元素。

:empty匹配所有不包含子元素或者文本的空元素。



### 内容限制过滤器

:contains("匹配的内容") 匹配包含给定文本的元素。

:has(selector) 匹配含有选择器所匹配的元素。

:header匹配如h1,h2,h3之类的标题元素。



### 子元素过滤器

匹配其父元素下的第N个子元素

:nth-child(2): 匹配每个父元素下的第N个子元素，下标从1开始。

:nth-child(odd): 匹配每个父元素下的所有奇元素，下标从1开始。

:nth-child(even): 匹配每个父元素下的所有偶元素，下标从1开始。

:first-child":匹配每个父元素下第一个子元素。

:last-child":匹配每个父元素下最后一个子元素。

:only-child":如果某个元素是父元素中唯一的子元素，将会被匹配。



## 函数

具备独立功能的代码模块。

- val()：用来针对表单元素的value属性值进行操作。
- text()：用来针对非表单元素的内容进行操作，不解析html标签。
- html()：针对非表单元素的内容进行操作，解析html标签。
- attr()：用来操作元素的属性值。例如：src，title，name....
- hide()：用来隐藏元素，使用元素的display属性设置为none。
- show()：用来显示元素，如果隐藏状态，则显示出来。
- remove()：从页面上删除元素。
- empty()：删除子元素。
- append()：在指定的元素某位进行追加内容。
- each()：是一对数组

语法：each(要遍历的数组,操作数组中的内容);

```javascript
//遍历DOM对象数组
$(":text").each(function(i,item){
alert("下标是:"+i+"内容是:"+item.value);
});
//遍历json对象
var stu={"name":"张三","age":22};
$.each(stu,function(key,value){
alert(key+"---"+value);
});
//遍历json数组
var stuarr=[{"name":"张三","age":22},
			{"name":"李四","age":23},
			{"name":"王五","age":24}];
$.each(stuarr,function(i,stu){
	$.each(stu,function(k,v){
		alert("下标是"+i+", 内容是:"+k+"---"+v);
});
});
```



## 事件

对页面元素的操作称为事件。

事件三要素：事件源（对谁进行操作），事件（发出了什么操作），事件处理程序（事件的响应）。



jQuery绑定事件语法

`$("选择器").on("事件名称",事件处理程序(){});`

监听事件语法：

`$("#btn1").click(function(){});`

事件名称就是JavaScript的事件去掉on的部分。

```javascript
$("#btn1").on("click",function(){});
```

**页面加载函数：**在页面所有元素加载完成之后，再执行页面加载函数中的代码。



## H5新特性

```javascript
日期：<input type="date"/><br>
颜色：<input type="color"/><br>
数字：<input type="number"/><br>
邮箱：<input type="email"/>
```













