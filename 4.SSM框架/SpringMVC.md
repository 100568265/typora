# SpringMVC

**什么是SpringMVC？**

它是基于MVC开发模式的框架，用来优化控制器，是Spring家族的一员，也具备IOC和AOP。

**什么是MVC？**

它是一种开发模式，模型视图控制器的简称。所有的web应用都是基于MVC开发。

**M-模型层**：实体类，业务逻辑层，数据访问层

**V-视图层**：html，JavaScript，vue等都是视图层，用来显示数据

**C-控制器**：用来接收客户端的请求，并返回响应到客户端的组件，Servlet就是组件。

**SpringMVC的优点**：

轻量级，基于MVC的框架

易于上手，容易理解，功能强大

具备IOC和AOP

完全基于注解开发



## 开发步骤

1. 新建项目，选择webapp模板
2. 修改目录：添加缺失的test,java,resources,并修改目录属性
3. 修改pom.xml文件，添加SpringMVC的依赖，添加Servlet的依赖
4. 添加springmvc.xml配置文件，指定包扫描，添加视图解析器

```xml
<!--添加包扫描-->
    <context:component-scan base-package="com.bjpowernode"></context:component-scan>
    <!--添加视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--配置前缀-->
        <property name="prefix" value="/admin/"></property>
        <!--配置后缀-->
        <property name="suffix" value=".jsp"></property>
    </bean>
```

5. 删除低版本的web.xml文件，新建web.xml

6. 在web.xml文件中注册SpringMVC框架(所有的web请求都是基于Servlet)

```xml
<!--注册SpringMVC框架-->
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc.xml</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>*.action</url-pattern>
</servlet-mapping>
```

7. 在webapp目录下新建admin目录，在admin目录下新建main.jsp页面，删除index.jsp页面，并新建，发送请求给服务器

8. 开发控制器(Servlet)，它是一个普通的类

9. 添加tomcat进行测试功能



## 分析web请求

web请求执行的流程        	核心处理器

index.jsp<--------------->DispatcherServlet<--------->SpringMVC处理器（普通方法）

xxxxx.jsp<--------------->DispatcherServlet<--------->SpringMVC处理器（普通方法）

DispatcherServlet要在web.xml文件中注册才可用。



## web请求的流程

index.jsp ------------------->one.action

1.提交数据到action(5种方式)

2.action方法的返回值

3.页面跳转的4种方式

4.携带数据跳转

## @RequestMapping

此注解就是来映射服务器访问的路径。

1.此注解可**加在方法上**，是为此方法注册一个可以访问的名称(路径)。

```xml
<a href="${pageContext.request.contextPath}/demo.action">访问服务器</a>
```

```java
@RequestMapping("/demo")
    public String demo(){
        System.out.println("服务器被访问到了");
        return "main";//可以直接跳到/admin/main.jsp页面上
    }
```



2.此注解可**加在类上**，相当于包名(虚拟路径)

帮助区分不同类中两个拥有相同方法名的两个方法。



3.此注解可区分get请求和post请求

```xml
<form action="${pageContext.request.contextPath}/req.action" method="post">
    <input type="submit" value="提交">
</form>
```

```java
@Controller
public class ReqAction {
    @RequestMapping(value="/req",method= RequestMethod.GET)
    public String req(){
        System.out.println("我是处理get请求。。。。。。。。。。。。");
        return "main";
    }
    @RequestMapping(value="/req",method=RequestMethod.POST)
    public String req1(){
        System.out.println("我是处理post请求的。。。。。。");
        return "main";
    }
}
```



### 5种数据提交方式的优化

**单个数据提交**

**对象封装提交**：在提交请求中，保证请求参数的名称与实体类中成员变量的名称一致，则可以自动创建对象，自动提交数据，自动类型转换，自动封装数据到对象中。

```xml
<h2>2.对象封装数据提交</h2>
<form action="${pageContext.request.contextPath}/two.action">
    姓名:<input name="uname"><br>
    年龄:<input name="uage"><br>
    <input type="submit" value="提交">
</form>
```

```java
@RequestMapping("/two")
    public String two(Users u){
        System.out.println(u);
        return "main";
    }
```

**动态占位符提交**：仅限于超链接或地址栏提交数据，一杠一值，一杠一大括号，使用注解**@PathVariable**来解析。（用来解析路径中的请求参数）

```xml
<a href="${pageContext.request.contextPath}/three/张三/22.action">动态提交</a>
```

```java
@RequestMapping("/three/{uname}/{uage}")
    public String three(
            @PathVariable
            String uname,
            @PathVariable
            int uage
    ){
        System.out.println("uname="+uname+"uage="+uage);
        return "main";
        }
```

**映射名称不一致**：提交请求参数与action方法的形参名称不一致，使用注解**@RequestParam**来解析。

**手工提取数据**：

```java
@RequestMapping("/five")
    public String five(HttpServletRequest request){
        String name = request.getParameter("uname");
        int age = Integer.parseInt(request.getParameter("uage"));
        System.out.println("name="+name+"age="+age);
       return "main";
    }
```



### 中文编码设置

在web.xml中设置中文编码过滤器

```xml
<!--中文编码过滤器
    private String encoding;
    private boolean forceRequestEncoding = false;
    private boolean forceResponseEncoding = false;-->
    <filter>
        <filter-name>encode</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encode</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```



### action方法的返回值

1.String：客户端资源的地址，自动拼接前缀和后缀，还可以屏蔽自动拼接字符串，可以指定返回的路径。

2.Object：返回json格式的对象。自动将对象或集合转为json使用的jackson工具进行转换，必须要添加jackson依赖。一般用于ajax请求。

3.void：无返回值，一般用于ajax请求。

4.基本数据类型，用于ajax请求。

5.ModelAndView：返回数据和视图对象，现在用的很少。



## **ajax请求操作**

1.添加jackson依赖

```xml
<dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.8</version>
    </dependency>
```

2.在webapp目录下新建js目录，添加jQuery函数库

3.在index.jsp页面上导入函数库

4.在action上添加注解@ResponseBody，用来处理ajax请求

5.在springmvc.xml文件中添加注解驱动`<mvc:annotationdriven/>`，它用来解析@ResponseBody的注解

```xml
<!--添加包扫描-->
    <context:component-scan base-package="com.bjpowernode"></context:component-scan>
    <!--不用添加视图解析器，因为是用来处理ajax请求的-->
    <!--必须添加注解驱动，因为是处理ajax请求-->
    <mvc:annotation-driven></mvc:annotation-driven>
```



### 4种跳转方式

请求转发和重定向的区别：

请求转发是基于服务器的跳转，重定向是基于客户端的跳转

`forward:` 和 `redirect:`可以屏蔽前缀后缀的拼接

**请求转发页面跳转：**

```java
@RequestMapping("/one")
    public String one(){
        System.out.println("这是请求转发页面跳转");
        return "main";//默认使用视图解析器拼接前缀后缀进行页面跳转/admin/main.jsp
    }
```

**请求转发action跳转：**

```java
@RequestMapping("/two")
    public String two(){
        System.out.println("这是请求转发action跳转");
        //forward:这组字符串可以屏蔽前缀和后缀的拼接
        return "/forward:/other.action";//默认使用视图解析器拼接前缀后缀进行页面跳转
    }
```

**重定向页面跳转：**

```java
@RequestMapping("/three")
    public String three(){
        System.out.println("这是重定向页面");
        return "redirect:/admin/main.jsp";
    }
```

**重定向action跳转：**

```java
@RequestMapping("/four")
    public String four(){
        System.out.println("这是重定向action");
        return "redirect:/other.action";
    }
```



### 默认参数类型

**SpringMVC的默认参数类型：**

不需要创建，直接拿来使用即可。

1. HttpServletRequest
2. HttpServletResponse
3. HttpSession
4. Model
5. Map
6. ModelMap

注意：Map,Model,ModelMap和request一样，都使用请求作用域进行数据传递，所以服务器端的跳转必须是请求转发。

```java
@RequestMapping("/data")
    public String data(HttpServletRequest request,
                       HttpServletResponse response,
                       HttpSession session,
                       Model model,
                       Map map,
                       ModelMap modelMap){
        //做一个数据，传到main.jsp页面上
        Users u = new Users("张三",25);
        //传递数据
        request.setAttribute("requestUsers", u);
        session.setAttribute("sessionUsers", u);
        model.addAttribute("modelUsers",u);
        map.put("mapUsers", u);
        modelMap.addAttribute("modelMapUsers", u);
        return "main";//请求转发方式跳转
    }
```



### 日期处理

**日期的提交处理：**

- 单个日期处理

要使用注解@DateTimeFormat，此注解必须搭配springmvc.xml文件中的`<mvc:annotationdriven>`标签

```java
@Controller
public class MyDateAction {
    SimpleDateFormat sf = new SimpleDateFormat("YYYY-MM-dd");
    @RequestMapping("/mydate")
    public String mydate(
            @DateTimeFormat(pattern = "yyyy-MM-dd")
                    Date mydate){
        System.out.println(mydate);
        System.out.println(sf.format(mydate));
        return "show";
    }
}
```

- 类中全局日期处理

注册一个注解，用来解析本类中所有的日期类型，自动转换。

```java
//注册一个全局的日期处理注解
    @InitBinder
    public void initBinder(WebDataBinder dataBinder){
        dataBinder.registerCustomEditor(Date.class, new CustomDateEditor(sf, true));
    }
    @RequestMapping("/mydate")
    public String mydate(Date mydate){
        System.out.println(mydate);
        System.out.println(sf.format(mydate));
        return "show";
    }
```

**日期的显示处理：**

在页面上显示美观的日期，必须使用JSTL。

步骤：

1. 添加依赖jstl
2. 在页面上导入标签库
3. 使用标签显示数据

如果是单个日期类型，直接转为美观的格式化的字符串。

如果是list中的实体类对象的成员变量是日期类型，则必须用jstl进行显示。

```xml
<h2>学生集合</h2>
<table witdh="500px" border="1">
    <tr>
        <th>姓名</th>
        <th>生日</th>
    </tr>
    <c:forEach items="${list}" var="stu">
        <tr>
            <td>${stu.name}</td>
            <td> <fmt:formatDate value="${stu.birthday}" pattern="yyyy-MM-dd"></fmt:formatDate> </td>
        </tr>

    </c:forEach>
</table>
```



## SpringMVC执行流程

DispatcherServlet->HandlerMapping->HandlerAdapter->ViewResolver

核心处理器



## 拦截器

针对请求和响应进行的额外的处理，在请求和响应的过程中添加**预处理**，**后处理**和**最终处理**。



### 执行时机

**preHandle():**在请求被处理之前进行操作，预处理

**postHandle():**在请求被处理之后，但结果还没有渲染前进行操作，可以改变响应结果，后处理

**afterCompletion():**	所有请求响应结束后执行善后工作，清理对象，关闭资源，最终处理



**两种实现方式:**

继承HandlerInterceptorAdapter的父类

**实现HandlerInterceptor接口（推荐使用)**



### 拦截器实现步骤

改造登录方法，在session中存储用户信息，用于进行权限验证

开发拦截器的功能，实现HandlerInterceptor接口，重写preHandle()方法。

```java
public class LoginInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //是否登陆过的判断
        if(request.getSession().getAttribute("users"==null)){
            //此时就是没有登录，打回到登陆页面，并给出提示
            request.setAttribute("msg", "您还没有登录，请先登录");
            request.getRequestDispatcher("WEB-INF/jsp/login.jsp").forward(request,response);
            return false;
        }
        return true;//放行请求
    }
}
```

在springmvc.xml文件中注册拦截器

```xml
<!--注册拦截器-->
<mvc:interceptors>
    <mvc:interceptor>
        <!--映射要拦截的请求-->
        <mvc:mapping path="/**"/>
        <!--设置放行的请求-->
        <mvc:exclude-mapping path="/showLogin"/>
        <mvc:exclude-mapping path="/login"/>
        <!--配置拦截器具体实现功能的类-->
        <bean class="com.bjpowernode.interceptor.LoginInterceptor"></bean>
    </mvc:interceptor>
</mvc:interceptors>
```





















