



# Maven进阶



**pom**是项目对象模型(Project Object Module)，该文件可以被继承。

maven多模块管理其实就是让子模块的pom文件继承父工程的pom文件。

## 第一种方式

maven父工程必须遵循以下两点要求：

**1.packaging标签的文本内容必须设置为pom**

packaging标签的是指定打包的方式，默认是jar，如果pom文件中没有packaging标签name默认就是jar。

**2.把src删除掉**

```xml
<!--parent标签-->
<parent>
        <artifactId>_003-mavenweb</artifactId>
        <groupId>com.bjpowernode.maven</groupId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>../_003-mavenweb/pom.xml</relativePath>
    </parent>
```



父工程添加的依赖，所有子工程会无条件自动继承。

这样有可能会出现冗余，所以，父工程要加强管理子模块的所有依赖：

给所有父工程里的依赖加上<dependencyManagement>标签。

父工程管理所有依赖的版本号<properties>标签。

自定义依赖版本号，构成=依赖名称+version



## 第二种方式

把子工程的位置放到父工程文件里面。

其余操作和第一种一模一样。







# SpringBoot





## JavaConfig

使用java类作为xml配置文件的替代，是配置spring容器的纯java的方式。

在这个java类可以创建java对象，把对象放入spring容器中。

使用两个注解：

@Configuration：放在一个类的上面，表示这个类是作为配置文件。

@Bean：声明对象，把对象注入到容器中。

@Bean如果不指定对象的name属性，默认name是方法名。

```java
//SpingConfig类
@Configuration
public class SpringConfig {
    /*创建方法，方法的返回值是对象，
    * 在方法的上面加入@Bean
    * 方法的返回值对象就注入到容器中*/

    @Bean
    public Student createStudent(){
        Student s1 = new Student();
        s1.setName("王五");
        s1.setAge(25);
        s1.setGender("男");
        return s1;
    }

}
//测试类
@Test
public void test02(){
	ApplicationContext ctx = new 			AnnotationConfigApplicationContext(SpringConfig.class);
	Student student = (Student) ctx.getBean("createStudent");
	System.out.println("使用JavaConfig"+student);
}
```



## @ImportResource

@ImportResource的作用是导入其他的xml配置文件。

```java
@ImportResource(value = "classpath:applicationContext.xml")
```



## @PropertySource

用来读取properties属性配置文件，使用属性配置文件可以实现外部化配置，

在程序代码之外提供数据。

步骤：

​	1.在resources目录下，创建properties文件，使用k=v的格式提供数据

​	2.在PropertyResource指定properties文件的位置

​	3.使用@Value(value="${key}")



```java
@Configuration
@ImportResource(value = "classpath:applicationContext.xml")
@PropertySource(value = "classpath:config.properties")
@ComponentScan(basePackages = "com.bjpowernode.vo")
public class SpringConfig {
}
```



## SpringBoot入门

可以简化Spring，SpringMVC的使用，它的核心还是IOC容器。

特点：

​	创建spring应用

​	内嵌的tomcat，jetty，Undertow

​	提供了starter起步依赖，简化应用的配置。

​	尽可能去配置spring和第三方库。（自动配置）

​	提供健康检查，统计，外部化配置

​	不用生成代码，不用使用xml做配置



### 创建项目方式

第一种：使用Spring提供的初始化器，就是向导创建SpringBoot应用

国外的地址：https://start.spring.io

SpringBoot项目的结构：

<img src="C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220812181719225.png" alt="image-20220812181719225" style="zoom: 67%;" />



第二种方式：

国外的地址：https://start.springboot.io



### 注解的使用

@SpringBootApplication注解

这是一个复合注解：

```java
@SpringBootConfiguration 
//使用了@SpringBootConfiguration注解标注的类，可以作为配置文件使用，可以使用Bean声明对象，注入到容器
@EnableAutoConfiguration
//启用自动配置，把java对象配置好，注入到spring容器中
@ComponentScan
扫描器，找到注解，根据注解的功能创建对象，给属性赋值等等
```



### 两种配置文件

配置文件名称：application

扩展名有：properties(k=v)，yml(k: v)

使用application.properties,application.yml

**application.properties:**

```properties
#设置端口号
server.port=8082
#设置访问应用的上下文路径contextPath
server.servlet.context-path=/myboot
```

**application.yml:**

```yaml
server:
  port: 8083
  servlet:
    context-path: /myboot2
```



### 多环境配置

实际的开发中，项目会经历很多阶段(开发->测试->上线)，每个阶段的配置也会不同。

使用方式：创建多个配置文件，名称规则是：

**application-环境名称.properties/yml**

 

使用方法：可以创建多个配置文件实现多个场景的功能。

再创建一个配置文件用来激活要使用的配置文件。



**@Value：**

使用@Value来读取配置文件中的数据

```java
@Value("${server.port}")
private Integer port;

@Value("${server.servlet.context-path}")
private String contextPath;

@Value("${school.name}")
private String name;
```



**@ConfigurationProperties(prefix="")**

SchoolInfo类：

```java
@Component
@ConfigurationProperties(prefix = "school")
public class SchoolInfo {
    private String name;
    private String website;
    private String address;
```

HelloController类：

```java
@Resource
private SchoolInfo info;

@RequestMapping("/info")
@ResponseBody
public String queryInfo(){
    return info.toString();
```







### 使用容器

想通过代码从容器中获取对象。

ConfigurableApplicationContext接口是ApplicationContext的子接口。



**例子：**

Service实现类：

```java
@Service("userService")
public class UserServiceImpl implements UserService {


    @Override
    public void sayHello(String name) {
        System.out.println("执行了业务方法sayHello"+name);
    }
}
```

Application启动类：

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
      	//获取容器对象
        ConfigurableApplicationContext ctx = SpringApplication.run(Application.class,args);
        //从容器中获取java对象
        UserService userService = (UserService) ctx.getBean("userService");
        userService.sayHello("lisi");
    }
}
```



**CommandLineRunner接口，ApplicationRunner接口**

这两个接口都有run方法，执行时间在**容器对象创建好之后，自动执行run()方法**。

可以完成自定义的在容器对象创建好的一些操作。

```java
@SpringBootApplication
public class Application implements CommandLineRunner {
    public static void main(String[] args) {
        System.out.println("准备创建容器对象");
        //创建容器对象
        SpringApplication.run(Application.class,args);
        System.out.println("容器对象创建之后");
    }
    @Override
    public void run(String... args) throws Exception {
        //可做自定义的操作，比如读取文件，数据库等等
        System.out.println("在容器对象创建好，执行的方法");
    }
}
```





## 拦截器

框架中有系统的拦截器，还可以自定义拦截器。



实现自定义的拦截器：

1.创建springmvc框架的HandlerInterceptor接口

此接口有**三个方法**：

```java
public interface HandlerInterceptor {
    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return true;
    }

    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
    }

    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
    }
}
```

主要使用preHandle方法对请求进行拦截。



**编写拦截器：**

```java
public class LoginInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request,
                             HttpServletResponse response,
                             Object handler) throws Exception {
        System.out.println("执行了LoginInterceptor的preHandle");
        return true;
    }
}
```

**SpringBoot中注册拦截器：**

```java
@Configuration
public class MyAppConfig implements WebMvcConfigurer {

    //添加拦截器，注入到容器中
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //创建拦截器对象
        HandlerInterceptor interceptor = new LoginInterceptor();
        //指定的拦截的请求uri地址
        String path[] ={"/user/**"};
        String excludePath[] = {"/user/login"};
        registry.addInterceptor(interceptor)
                .addPathPatterns(path)
                .excludePathPatterns(excludePath);
    }
}
```





## 过滤器

Filter是Servlet规范中的过滤器，可以处理请求，对请求的参数，属性进行调整。

常常在过滤器中处理字符编码。

在框架中使用过滤器有两步：

1.创建自定义过滤器类

2.注册Filter过滤器对象

```java
//自定义过滤器
public class MyFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("执行了MyFilter，doFilter");
        filterChain.doFilter(servletRequest, servletResponse);

    }
}
```

```java
//注册Filter过滤器
@Configuration
public class WebApplicationConfig {
    @Bean
    public FilterRegistrationBean filterRegistrationBean(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new MyFilter());
        bean.addUrlPatterns("/user/*");
        return bean;
    }
}
```





## 字符集过滤器

CharacterEncodingFilter：解决post请求中乱码的问题

修改**application.properties**的内容，让自定义的过滤器起作用：

```properties
server.port=9001
server.servlet.context-path=/myboot
#让系统的CharacterEncodingFilter生效
server.servlet.encoding.enabled=true
#指定特定的编码方式
server.servlet.encoding.charset=UTF-8
#强制request,response都使用charset属性的值
server.servlet.encoding.force=true
```



## 集成MyBatis

使用MyBatis框架操作数据，在SpringBoot框架集成MyBatis

使用步骤：

**1.mybatis起步依赖：完成mybatis对象自动配置，对象放在容器中**

**2.pom.xml指定把src/main/java目录中的xml文件包含在classpath中**

**3.创建实体类Student**

```java
public class Student {
    private Integer id;
    private String name;
    private Integer age;
```

**4.创建Mapper接口 StudentMapper，查询学生的方法**

```java
//告诉MyBatis这是Mapper接口，会创建此接口的代理对象
@Mapper
public interface StudentMapper {
    Student selectById(@Param("stuId") Integer id);
}
```

**5.创建Mapper接口所对应的Mapper文件，xml文件，写sql语句**

```xml
<!--定义sql语句-->
<select id="selectById" resultType="com.bjpowernode.pojo.Student">
    select id,name,age from student where id=#{stuId}
</select>
```

**6.创建Service层对象，创建StudentService接口和他的实现类。去调用Dao对象的方法，完成数据库的操作**

```java
//Service接口
public interface StudentService {

    Student queryStudent(Integer id);
}
```

```java
//Service接口的实现类
@Service
public class StudentServiceImpl implements StudentService { 
    @Autowired
    private StudentMapper studentMapper;
    @Override
    public Student queryStudent(Integer id) {
        Student student = studentMapper.selectById(id);
        return student;
    }
}
```

**7.创建Controller对象，访问Service**

```java
@Controller
public class StudentController {
    @Autowired
    private StudentService studentService;
    @RequestMapping("/student")
    @ResponseBody
    public String queryStudent(Integer id){
        Student student = studentService.queryStudent(id);
        return student.toString();
    }
}
```

**8.写application.properties文件**

​	配置数据库连接信息

```properties
server.port=9001
server.servlet.context-path=/orm
#连接数据库
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/springdb?useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=123456
```



### **@MapperScan**

第一种方式：放在Mapper接口的上面，每个接口都需要使用这个注解

第二种方式：使用**@MapperScan**注解，在**Application类**中使用此注解**指定要扫描的包**

```java
//@MapperScan:找到Mapper接口和Mapper文件，
//basepackages: Mapper接口所在的包名
@SpringBootApplication
@MapperScan(basePackages ={"com.bjpowernode.mapper",
                           "com.bjpowernode.dao"})
public class Application {

    public static void main(String[] args) {
        
        SpringApplication.run(Application.class, args);
    }

}
```

**java文件和mapper.xml文件分开：**

1.把mapper.xml文件全部放在resources目录的自定义子目录下

2.在application.properties里配置位置。

```properties
#指定mapper文件的位置
mybatis.mapper-locations=classpath:mapper/*.xml
#指定mybatis的日志
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```





### 事务控制

**Spring框架中的事务：**

1.管理事务的对象：事务管理器（接口，有很多实现类）

2.声明式事务：在xml配置文件或者使用注解说明事务控制的内容

控制事务：隔离级别，传播行为，超时时间

3.事务处理方式：

- spring框架中的@Transactional
- aspectJ框架可以在xml配置文件中，声明事务控制的内容



**SpringBoot中的事务：**

1.在业务方法上面加入@Transactional，加入注解后，方法就有事务功能了。

2.在主启动类的上面，加入@EnableTransactionManager





## RESTful风格

**API：**

Application Programming Interface，应用程序接口

是一些预先定义的接口，用来提供某种功能，又无需访问源代码或理解内部机制

**接口(API)：**可以指访问servlet，controller的url，调用其他程序的函数

**架构风格：**api组织方式（样子）

传统方式：http://localhost:9001/orm/student?id=1

​					在地址上提供了访问的资源名称addStudent，在其后使用了get方式传递参数



**RESTful方式：**

Representational State Transfer，简称REST，表现层状态转移。

优点：更简介，更有层次

对资源的操作：

使用http中的动作（请求方式），表示对资源的操作（CRUD）

GET：查询资源 -- sql select

POST：创建资源 --sql insert

PUT：更新资源 --sql update

DELETE：删除资源 --sql delete



一句话概括REST：使用url表示资源，使用http动作操作资源。



注解：

**@PathVariable**：从url中获取数据

**@GetMapping**：支持get请求方式，等同于@RequestMapping(method=RequestMethod.GET)

```java
@RestController
public class MyRestController {
    //学习注解的使用
    //查询id=xxxx的学生
    /**
     * @PathVariable(路径变量)：获取url中的数据
     * 属性：value：路径变量名
     * 位置：放在控制器方法的形参前面
     */
    @GetMapping("/student/{stuId}")
    public String queryStudent(@PathVariable(value = "stuId") Integer studentId){
        return "查询学生studentId="+studentId;
    }
}
```

**@PostMapping**：支持post请求方式，等同于@RequestMapping(method=RequestMethod.POST)

```java
@PostMapping("/student/{name}/{age}")
public String createStudent(@PathVariable("name") String name,
                            @PathVariable("age") Integer age){
    return "创建资源 student:"+name+"#age="+age;
}
```

**@PutMapping：**支持put请求方式，等同于@RequestMapping(method=RequestMethod.PUT)

```java
/*更新资源
* 当路径变量名和形参名一样，@PathVariable中的value属性可以省略*/
@PutMapping("/student/{id}/{age}")
public String modifyStudent(@PathVariable Integer id,
                            @PathVariable Integer age){
    return "更新资源，执行put请求方式：id="+id+"#age="+age;
}
```

**@DeleteMapping：**支持delete请求方式，@RequestMapping(method=RequestMethod.DELETE)

```java
/*删除资源*/
@DeleteMapping("/student/{id}")
public String deleteStudentById(@PathVariable Integer id){
    return "删除资源id="+id;
}
```

**@RestController：**复合注解，是@Controller和ResponseBody组合。



在页面中支持put，delete请求

实现步骤：

1.application.properties(yml)：开启使用HiddenHttpMethodFilter过滤器

```properties
#启用支持put,delete
spring.mvc.hiddenmethod.filter.enabled=true
```

2.在请求页面中，包含_method参数，它的值是put,delete，发起这个请求使用post方式

```html
<h3>测试rest支持的请求方式</h3>
<from action="student/testdelete" method="post">
    <input type="hidden" name="_method" value="delete">
    <input type="submit" value="测试请求方式">
</from>
```





## 集成Redis

Redis的数据类型：string，hash，set，zset，list

SpringBoot中有一个RedisTemplate(StringRedisTemplate),处理和交互



使用步骤：

1.添加Redis依赖项

2.配置Redis服务器信息

```properties
spring.redis.host=localhost
spring.redis.port=6379
```



**对比StringRedisTemplate和RedisTemplate**

**StringRedisTemplate**：把k和v都作为String处理，使用的是String的序列化，可读性好。

**RedisTemplate**：把k和v经过序列化存到redis，k和v是序列化的内容，不能直接识别。（默认是JDK的序列化，可以设置成别的序列化方式）



**序列化**：把对象转换成可传输的字节的过程。

序列化的最终目的是为了对象可以跨平台存储和进行网络传输。

序列化只是一种拆装对象的规则，常见的有：

**JDK**,**JSON**,XML，Hessian，**Kryo**，Thrift等。。。





```java
/*使用RedisTemplate，在存取值之前，设置序列化*/
@PostMapping("/redis/{k}/{v}")
public String addString(String k, String v){
    //使用RedisTemplate
    //设置key的String的序列化
    redisTemplate.setKeySerializer(new StringRedisSerializer());
    //设置value的String的序列化
    redisTemplate.setValueSerializer(new StringRedisSerializer());
    redisTemplate.opsForValue().set(k,v);
    return "定义RedisTemplate对象的key，value的序列化";
}
```



**使用json序列化：**

```java
@PostMapping("/redis/addjson")
public String addJson(){
    Student student = new Student();
    student.setId(1001);
    student.setName("zhangsan");
    student.setAge(20);

    redisTemplate.setKeySerializer(new StringRedisSerializer());
    //把值作为json序列化
    redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer(Student.class));
    redisTemplate.opsForValue().set("mystudent",student);
    return "json序列化";
}

@PostMapping("/redis/getjson")
public String getJson(){
	redisTemplate.setKeySerializer(new StringRedisSerializer());
    //把值作为json序列化
    redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer(Student.class));
    Object obj = redisTemplate.opsForValue().get("mystudent");
    return "json反序列化="+obj;
}
```





## Thymeleaf

Thymeleaf模板引擎：用来解析生成视图，在服务器处理视图，将处理的结果输出到浏览器。

Thymeleaf特点：在html的属性中使用Thymeleaf语法



**SpringBoot集成Thymeleaf：**

1.加入thymeleaf，spring-web依赖

2.在controller中提供数据，数据放在request或者session作用域

3.在resources的templates目录中创建html文件，作为视图使用。在html文件中加入thymeleaf的获取数据。

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <p>第一个thymeleaf例子</p>
    <!--获取controller数据，显示在视图-->
    <p th:text="${myname}"> 显示姓名</p>
    <div th:text="${myage}"> 显示年龄</div>
    <br/>
    <p th:text="${mygender}"> 显示性别</p>
</body>
</html>
```

![image-20220828012704863](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220828012704863.png)



### 语法

**1.标准变量表达式**

在页面上获取数据的语法

格式：${key}

**controller代码：**

```java
/*表达式*/
    @GetMapping("/thy/exp")
    public String exp(Model model){
        //简单类型的数据
        model.addAttribute("name","周星");
        model.addAttribute("age",20);
        //添加对象类型的key
        model.addAttribute("user",new User("张三",20,"男"));
        return "exp";
    }
```

**页面代码：**

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <p>表达式</p>
    <h3>简单类型的值</h3>
    <p th:text="${name}"></p>
    <p th:text="${age}"></p>
    <p th:text="${gender}"></p>
    <h3>对象类型</h3>
    <p th:text="${user.name}"></p>
    <p th:text="${user.age}"></p>
    <p th:text="${user.gender}"></p>
</body>
</html>
```



**thymeleaf属性**

几乎所有的html标签，在他们的属性前面加入th，这些属性就可以看做是thymeleaf属性，这些属性的值可以使用表达式语法，例如${key}动态获取数据。



**判断：**

 th:if  语法<div th:if="boolean表达式">内容</div> （没有elseif,没有else）



**多条件判断：**

语法:

```html
<div th:switch="变量">
    <p th:case="值1">结果1</p>
    <p th:case="值2">结果2</p>
    <p th:case="*">默认</p>
</div>
```









































