# AJAX

**什么是ajax？**

它是**局部刷新**。它的核心对象是XMLHttpRequest,它可以异步的发送请求给服务器，并且从服务器端接回数据。

**什么是局部刷新？**

它是由浏览器的内置对象XMLHttpRequest发请求和接收响应。地址栏保持不变，当前浏览器显示的内容是原始的内容和新返回内容的综合。



## 异步请求对象

**XMLHttpRequest的功能：**

在页面已经加载的情况下向服务器端发送请求。

在页面已经加载的情况下从服务器端接收响应。

在页面已经加载的情况下进行局部刷新数据显示。

AJAX=Asynchronous JavaScript And XML

JavaScript用来进行数据渲染

XML：是一种数据传输和存储的格式，现在已经被json替代。



## ajax实现5步

1. **新建异步对象**

var xmlHttp = new XMLHttpRequest();

2. **绑定回调函数（多次被调用）**

从服务器返回的数据，在回调函数中显示出来。回调函数在整个请求响应的过程中会被反复调用（初始化，发送请求，有数据回来，数据渲染）。

必须要进行判断，才可以进行数据回显。

```java
if(xmlHttp.readyState==4 && xmlHttp.status==200){
//5.进行数据回显
}
```

xmlHttp.readyState状态值：

0：表示异步对象被创建

1：异步对象初始化

2：发送请求

3：服务器有数据返回

4：可以进行数据渲染

xmlHttp.status状态值：

200：请求和响应一切正常

404：目标资源不可访问

400：请求参数映射出错

500：服务器端的语法错误

3. **异步对象初始化**

xmlHttp.open("请求提交的方式get,post","目标资源的地址","是否是异步,默认是异步");

XMLHttp.open("get","${pageContext.request.contextPath}/demo");

4. **发送请求**

xml.Http.send();

5. **取出数据回显**

服务器返回的数据在xmlHttp.responseText属性中。



## jQuery封装ajax请求

$.ajax({

​	url:"服务器提交的目标资源的地址",

​	type:"提交请求的方式get,post",

​	data:"以json格式提交到服务器的数据",

​	asnyc:"同步还是异步，默认异步",

​	dataType:"服务器端返回客户端的数据类型",

​	success:function(){

​	相当于回调函数的功能

},

​	error:function(){

​	出错了，到这里

}

});



## JSON

json：JavaScript对象表示法（JavaScript Object Notation），JSON是存储和交换文本信息的语法。JSON比XML更小，更快，更易解析。



### JSON的结构

只有JSON对象和JSON数组。



**什么是JSON对象？**

以大括号起始和结束，里面是键值对。键值对之间以冒号分隔，多个键值对之间以逗号分隔。

`var stu={"name":"张三","age":"25"};`

**什么是JSON数组？**

就是以中括号开始和结束，里面是多个json对象。

```json
var stuList = [{"name":"张三","age":"25"},

				{"name":"李四","age":"26"},

				{"name":"王五","age":"27"}];
```



### **JSON的七种值类型：**

- String：字符串类型
- number：数值类型
- true：
- false：
- null：
- Object：对象类型 var classes = {"name":"2115","teacher":{"name":"张阿荣","age":"22","teachage":"20"}}
- array：数组类型 



### JSON库(jar包)

Student stu=new Student("张三",22);         **Servlet中**

转换工具json库 ----> 

var stu = {"name":"张三","age":22};			 **JavaScript中**

jackson:性能好，规范好，SpringMVC使用的工具。

```json
ObjectMapper om = new ObjectMapper();
om.writeValueAsString(转换的对象);
```

​	注意：jar包必须在WEB-INF目录下的lib下。



## JSTL

JSTL: Java server pages Standard Tag Library，即JSP标准标签库













