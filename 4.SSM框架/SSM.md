# MyBatis

为了减轻使用JDBC的复杂性。它是持久化层非常优秀的框架，数据访问层优秀的框架。



## 三层架构

**界面层**（服务员）：它用来接收客户端的请求，返回处理结果给客户端。

**业务逻辑层**（厨师）：项目的业务功能的实现。

**数据访问层**（采购员）：所有的数据库的操作实现。

注意：不允许跨层访问。



## 搭建MyBatis步骤

1. 建库建表
2. 新建maven工程，选择quickstart模板
3. 添加数据库可视化
4. 修改目录
5. 修改**pom.xml**文件，添加依赖，添加资源文件的指定
6. 添加**jdbc.properties**属性文件

```properties
jdbc.driverClassName=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC
jdbc.username=root
jdbc.password=123456
```

7. 添加**SqlMapConfig.xml**核心配置文件并开发

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!--读取属性文件properties-->
	<properties resource="jdbc.properties"></properties>
	<!--配置数据库的环境变量-->
	<environments default="development">
 	<environment id="development">
   	<!--配置事务管理器-->
   	<transactionManager type="JDBC">						      	</transactionManager>
   	<!--配置数据源-POOLED:使用数据库连接池-->
   <dataSource type="POOLED">
    <property name="driver" value="${jdbc.driverClassName}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
   </dataSource>
 </environment>
</environments>
	<!--注册Mapper.xml文件-->
<mappers>
   <mapper resource="StudentMapper.xml"></mapper>
</mappers>
</configuration>
```

8. 添加实体类（对应数据库中的表）

9. 添加**StudentMapper.xml**文件，数据库增删查改，核心文件中注册

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--mapper是整个文件的大标签，用来开始和结束xml文件
    属性：namespace:指定命名空间，用来区分不同mapper.xml文件中相同的id属性-->
<mapper namespace="watson">
    <!--查询全部学生
    List<Student> getAll();
    resultType:指定查询返回的结果集类型，如果是集合，则必须是泛型的类型
    parameterType:如果有参数，则通过它来指定参数的类型-->
	<select id="getById" resultType="com.bjpowernode.pojo.Student" parameterType="int">
    select id,name,email,age from student where id=#{id}
    </select>
</mapper>
```

10. 添加测试类

```java
@Test
public void testGetById() throws IOException {
	//读取核心配置文件
	InputStream in = 		Resources.getResourceAsStream("SqlMapConfig.xml");
	//得到SqlSessionFactory对象
	SqlSessionFactory factory = new 	SqlSessionFactoryBuilder().build(in);
	//取出sqlSession
	SqlSession sqlsession = factory.openSession();
	//完成查询
	Student stu = sqlsession.selectOne("watson.getById",1);
	System.out.println(stu);
	//关闭
	sqlsession.close();
    }
```

#### 优化测试类

```java
SqlSession sqlSession;
@Before//在所有的@Test方法执行前先执行的代码
public void openSqlSession() throws IOException{
    //使用文件流读取核心配置文件SqlMapConfig.xml
    InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
    //创建SqlSessionFactory工厂
    SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
    //取出sqlSession对象
    sqlSession = factory.openSession();
}
@after//在所有的@Test方法执行后执行的代码
sqlSession.close();
```

#### 实体类注册别名

```xml
SqlMapConfig.xml中添加标签
<!--单个注册-->
<typeAlias type="com.bjpowerno.pojo.Student" alias="student"></typeAlias>
<!--批量注册-->
<package name="com.bjpowernode.pojo"></package>
```

#### 设置日志输出

```xml
<settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
```



## MyBatis对象分析

**Resources**类

解析SqlMapConfig.xml文件，创建出相应的对象

```java
InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
```

**SqlSessionFactory**接口

使用ctrl+h快捷键查看本接口的子接口及实现类

**SqlSession**接口

DefaultSqlSession实现类



## 动态代理实现

在三层架构中，业务逻辑层要通过接口访问数据访问层的功能，动态代理可以实现。

1. UsersMapper.xml文件与UsersMapper.java的接口必须同一个目录下。
2. UsersMapper.xml文件与UsersMapper.java的接口文件名必须一致。
3. UserMapper.xml文件中标签的id值和UserMapper.java的接口方法名完全一致。
4. UserMapper.xml文件中标签的parameterType属性值和UserMapper.java的接口中方法的参数类型完全一致。
5. UserMapper.xml文件中标签的resultType值与UserMapper.java的接口中方法的返回值类型完全一致。
6. UserMapper.xml文件中namespace属性必须是接口的完全限定名称。com.bjpowernode.mapper.UsersMapper
7. 在sqlMapConfig.xml文件中注册mapper文件时，使用class=接口的完全限定名com.bjpowernode.mapper.UsersMapper



#### 动态代理访问步骤

1. 建表Users
2. 新建Maven工程，刷新可视化
3. 修改目录
4. 修改pom.xml文件，添加依赖
5. 添加jdbc.properties文件到resources目录下
6. 添加SqlMapConfig.xml文件
7. 添加实体类
8. 添加mapper文件夹，新建UsersMapper接口
9. 在mapper文件夹下新建UsersMapper.xml文件，完成增删改查功能
10. 添加测试类测试功能

```java
//取出动态代理的对象，完成接口中方法的调用，实则是调用xml文件中相应标签的功能
uMapper = sqlSession.getMapper(UsersMapper.class);
    }
@Test
public void testGetAll(){

//就是在调用接口的方法，mybatis框架已经把功能代理了出来
    List<Users> list = uMapper.getAll();
    list.forEach(users -> System.out.println(users));
    }
```

批量注册mapper文件

```xml
<mappers>
<!--<mapper class="com.bjpowernode.mapper.UsersMapper"></mapper>-->
	<package name="com.bjpowernode.mapper"/>
</mappers>
```



### #{}和${}

**#{}**

传参大部分使用#{}传参，底层使用PreparedStatement对象，是安全的数据库访问，防止sql注入。

#{}里面怎么写，看parameterType参数类型：

**简单类型**(8种基本类型+String)：#{}里面随便写。

**实体类的类型**：#{}里只能是类中成员变量的名称，而且区分大小写。



**${}**

字符串拼接/字符串替换

字符串拼接一般用于模糊查询中，建议少用，因为有sql注入的风险

${}里面怎么写，看parameterType参数类型：

**简单类型**：${}里面随便写

**实体类的类型**：${}里只能是类中成员变量的名称。（现在已经少用）



优化后的模糊查询：

使用函数concat()拼接，防止sql注入：

```java
<select id="getByNameGood" parameterType="string" resultType="users">
        select id,username,birthday,sex,address from users
        where username like concat('%',#{name},'%')
    </select>
```



字符串替换：

需求：模糊用户名和地址查询

```xml
<!--模糊用户名和地址查询
        如果参数超过一个就不写
List<Users> getByNameOrAddress(String columnName,String columnValue);
    -->
<select id="getByNameOrAddress" resultType="users">
        select id,username,birthday,sex,address from users
        where ${columnName} like concat('%',#{columnValue},'%')
    </select>
```



### 返回主键值

在插入语句结束后，返回自增的主键到入参的users对象的id属性中

```xml
<insert id="insert" parameterType="users">
	<selectKey keyProperty="id" resultType="int" order="AFTER">
        select last_insert_id()
    </selectKey>
    insert into users(username,birthday,sex,address) values(#{userName},#{birthday},#{sex},#{address})
</insert>
```



### UUID

全球唯一字符串



## 动态sql

1.什么是动态sql？

可以定义代码的片段，可以进行逻辑判断，循环处理，使条件判断更为简单



<sql>：用来定义代码片段，可以将所有的列名，或复杂的条件定义为代码片段，供使用时调用。

<include>：用来引用<sql>定义的代码片段。

```xml
<!--定义代码片段-->
    <sql id="allColumns">
        id,username,birthday,sex,address
    </sql>
    <!--//查询全部用户信息
    List<Users> getAll();-->
    <select id="getAll" resultType="users">
        select <include refid="allColumns"/> from users
    </select>
```

<if>：进行条件判断

<where>：进行多条件拼接，在查询，删除，更新中使用

```xml
 <!--//按指定的条件进行多条件查询
    List<Users> getByCondition(Users users);
    -->
    <select id="getByCondition" parameterType="users" resultType="users">
        select <include refid="allColumns"></include> from users
        <where>
            <if test="userName!=null and userName!=''">
                and username like concat('%',#{userName},'%')
            </if>
            <if test="birthday!=null">
                and birthday = #{birthday}
            </if>
            <if test="sex!=null and sex!=''">
                and sex=#{sex}
            </if>
            <if test="address!=null and address!=''">
                and address like concat('%',#{address},'%')
            </if>
        </where>
    </select>
```

<set>：有选择的更新，至少选择一列

```xml
<!--//有选择的更新
    int updateBySet(Users users);
    -->
    <update id="updateBySet" parameterType="users">
        update users
        <set>
            <if test="userName!=null and userName!=''">
                username=#{userName},
            </if>
            <if test="birthday!=null">
                birthday=#{birthday},
            </if>
            <if test="sex!=null and sex!=''">
                sex=#{sex},
            </if>
            <if test="address!=null and address!=''">
                address = #{address},
            </if>
        </set>
            where id=#{id}
    </update>
```

<foreach>：用来进行循环遍历，完成循环条件查询，批量删除，批量增加，批量更新

参数详解：

collection:用来指定入参的类型，list，map，array

item:每次循环遍历出来的值或对象

separator:多个值或对象或语句之间的分隔符

```xml
<!--//批量删除
    int deleteBatch(Integer[] arr);
    -->
    <delete id="deleteBatch">
        delete from users
        where id in (
            <foreach collection="array" separator="," item="id">
                #{id}
            </foreach>
            )
    </delete>
```



## 指定参数位置

如果入参是多个，可以通过指定参数位置进行传参。是实体类包含不住的条件，实体类只能封装住成员变量的条件，如果某个成员变量要有区间范围内的判断，或者有两个值进行处理，则实体类包不住。

例如：查询指定日期范围内的用户。

```xml
<!--//查询指定日期范围内的用户
    List<Users> getByBirthday(Date begin, Date end);
    -->
    <select id="getByBirthday" resultType="users">
        select <include refid="allColumns"></include>
        from users
        where birthday between #{arg0} and #{arg1}
    </select>
```



### **入参是map(重点)**

如果入参超过一个，使用map封装查询条件，语义更明确，查询条件更明确。

```xml
<!--//入参是map
    List<Users> getByMap(Map map);
    -->
    <select id="getByMap" resultType="users">
        select <include refid="allColumns"></include>
        from users
        where birthday between #{birthdayBegin} and #{birthdayEnd}
    </select>
```

```java
@Test
    public void testGetByMap() throws ParseException {
        Date begin = sf.parse("1999-01-01");
        Date end = sf.parse("2001-01-01");
        Map map = new HashMap<>();
        map.put("birthdayBegin", begin);
        map.put("birthdayEnd", end);
        List<Users> list = uMapper.getByMap(map);
        list.forEach(uu-> System.out.println(uu));
    }
```

### 返回值是一行Map

如果返回的数据实体类无法包含，可以使用map返回多张表中的若干数据，返回后这些数据之间没有任何关系，就是Object类型。返回的map的key就是列名/别名。

```xml
<!--//返回值是map，一行
    Map getReturnMap(Integer id);
    -->
<select id="getReturnMap" parameterType="int" resultType="map">
        select username,address from users
        where id = #{id}
</select>
```

```java
@Test
    public void testGetMap(){
        Map map = uMapper.getReturnMap(3);
        System.out.println(map);
        System.out.println(map.get("username"));
    }
```

### 返回值是多行Map

```xml
<!--//返回多行的map
    List<Map> getMuMap();
    -->
    <select id="getMuMap" resultType="map">
        select username,address
        from users
    </select>
```

## 表的关联关系

1. 一对多关联
2. 多对一关联
3. 一对一关联
4. 多对多关联

### **一对多关联**

客户和订单就是典型的关联关系。一个客户可以有多个订单。

使用一对多的关联关系可以满足查询客户的同时查询客户名下的所有订单。

<resultMap>映射：

```xml
<!--//根据客户id查询客户所有信息并查询该客户所有订单信息
    Customer getById(Integer id);
    //customer表的三个列
    private Integer id;
    private String name;
    private Integer age;
    //该客户名下的所有订单的集合
    private List<Orders> ordersList;
    -->
    <resultMap id="customermap" type="customer">
        <!--主键绑定-->
        <id property="id" column="cid"></id>
        <!--非主键绑定-->
        <result property="name" column="name"></result>
        <result property="age" column="age"></result>
        <!--ordersList绑定-->
        <collection property="ordersList" ofType="orders">
            <!--主键绑定-->
            <id property="id" column="oid"></id>
            <!--非主键绑定-->
            <result property="orderNumber" column="orderNumber"/>
            <result property="orderPrice" column="orderPrice"/>
        </collection>
    </resultMap>
    <select id="getById" parameterType="int" resultMap="customermap">
        select c.id cid,name,age,o.id oid,orderNumber,orderPrice,customer_id
        from customer c inner join
        orders o on c.id=o.customer_id
        where c.id=#{id}
    </select>
```



### 多对一关联

```xml
<!--//根据主键查询订单，并同时查询此订单的客户信息
    Orders getById(Integer id);
    -->
    <resultMap id="ordersmap" type="orders">
        <!--主键绑定-->
        <result property="id" column="id"></result>
        <!--非主键绑定-->
        <result property="orderNumber" column="orderNumber"></result>
        <result property="orderPrice" column="orderPrice"></result>
        <!--customer数据绑定-->
        <association property="customer" javaType="customer">
            <id property="id" column="id"></id>
            <result property="name" column="name"></result>
            <result property="age" column="age"></result>
        </association>
    </resultMap>
<select id="getById" parameterType="int" resultMap="ordersmap">
    select o.id oid,orderNumber,orderPrice,customer_id,c.id cid,name,age
        from orders o inner join customer c on o.customer_id = c.id
        where o.id=#{id}
</select>
```



## MyBatis事务处理

<transactionManager type="JDBC"></transactionManager>

可设置为自动提交：

```java
sqlSession = factory.openSession();//默认手动提交
sqlSession = factory.openSession(true);//自动提交
```



## 缓存

MyBatis框架提供一级缓存和二级缓存，默认开启一级缓存

缓存就是为了提高查询的效率。

使用缓存后查询的流程：

查询时先到缓存里查，如果没有则查询数据库，放缓存一份，再返回客户端，下次再查询直接从缓存拿。库中如果发生commit()操作，则清空缓存

一级缓存使用的是SqlSession的作用域，同一个SqlSession共享一级缓存的数据。

二级缓存使用的是mapper的作用域，不同的SqlSession只要访问的是同一个mapper.xml文件，则共享二级缓存作用域。



### 什么是ORM

**ORM(Object Relational Mapping)**：对象关系映射

MyBatis框架是ORM非常优秀的框架。

java语言中以对象的方式操作数据，存到数据库是以表的方式存储。

对象中的成员变量与表中的列之间的数据互换称为映射。

**持久化操作：**将对象保存到关系型数据库中，将关系型数据库中的数据读取出来以对象的形式封装。

MyBatis是持久化层优秀的框架。







# Spring

它是一个容器，它是整合其他框架的框架。

核心是IOC和AOP，它由20多个模块构成，在很多领域提供优秀的解决方案。

Spring主要的作用就是为代码解耦合。



## **Spring的特点**

**轻量级**：由20多个模块构成，每个jar包都很小。(<1M)。

对***代码无污染***。

**面向接口编程**：

使用接口，就是面向灵活，项目的可扩展性，可维护性都极高。

接口不关心实现类的类型，使用时接口指向实现类，切换实现类即可切换整个功能。

**AOP：**面向切面编程

公共的类提出去，需要的时候反射回去。底层原理是动态代理。

**IOC：**

控制反转(Inversion of Control)是一个概念，由Spring容器进行对象的创建和依赖注入，程序员在使用时直接取出使用。

反转：由Spring容器创建对象和依赖注入，将控制权从程序员手中夺走。



## 创建容器对象

```java
//如果想从spring容器中取出对象，则要创建容器对象，并启动
ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
//取出对象
Student stu = (Student) ac.getBean("stu");
//输出到控制台
System.out.println(stu);
```



## 基于xml的IOC

创建对象：

```xml
<bean id="stu" class="com.bjpowernode.pojo.Student"></bean>
```

给创建的对象赋值：

1. **使用setter注入：**

​		注入分为简单类型注入和引用类型注入：

​		**简单类型**注入使用**value**属性

​		**引用类型**注入使用**ref**属性

​		注意：使用setter注入必须提供无参构造方法，必须提供setxxx()方法。

```xml
<!--创建学生对象
    等同于Student stu = new Student();
    id:就是创建的对象名称
    class:就是创建的对象的类型，底层通过反射构建对象
-->
<bean id="stu" class="com.bjpowernode.pojo.Student">
    <property name="name" value="张三"></property>
    <property name="age" value="26"></property>
    <property name="school" ref="school"></property>
</bean>
<bean id="school" class="com.bjpowernode.pojo.School">
    <property name="name" value="清华大学"></property>
    <property name="address" value="北京海淀区"></property>
</bean>
```

2. **使用构造方法注入：**

Student stu = new Student("张三",22);

**a.使用构造方法的参数名称进行注入值**

```xml
<constructor-arg name="name" value="清华大学"></constructor-arg>
<constructor-arg name="address" value="海淀区"></constructor-arg>
```

**b.使用构造方法参数的下标注入值**

```xml
<constructor-arg index="0" value="清华大学"></constructor-arg>
<constructor-arg index="1" value="海淀区"></constructor-arg>
```

**c.使用默认的构造方法的参数的顺序注入值**

```xml
<bean id="stuSequence" class="com.bjpowernode.pojo.Student">
	<constructor-arg value="张三"></constructor-arg>
    <constructor-arg value="22"></constructor-arg>
</bean>
```



## 基于注解的IOC

也称为DI(Dependency Injection)，它是IOC的具体实现的技术。

基于注解的IOC，必须要在spring的核心配置文件添加**包扫描**。

单个包扫描：

```xml
<context:component-scan base-package="com.bjpowernode.controller"></context:component-scan>
<context:component-scan base-package="com.bjpowernode.service"></context:component-scan>
<context:component-scan base-package="com.bjpowernode.dao"></context:component-scan>
```

**1.创建对象的注解**



**@Component**：可以创建任意对象，对象名称默认类名的驼峰命名。也可以指定对象的名称@Component("指定名称")

**@Controller**：专门用来创建控制器的对象(Servlet)，这种对象可以接收用户的请求，可以返回处理结果给客户端

**@Service**：专门用来创建业务逻辑层的对象，负责向下访问数据访问层，处理完毕的结果返回给界面层

**@Repository**：专门创建数据访问层的对象，负责数据库的增删改查。



**2.依赖注入的注解**

值类型的注入

**@Value**：用来给简单类型注入值(8种基本类型+String)



引用类型的注入

**1.按类型注入：**

**@Autowired**：使用类型注入值，从整个Bean工厂中搜索同源类型的对象进行注入。也可注入**同源类型**。

**什么是同源类型？**

a.被注入的类型与注入的类型是**完全相同**的类型

b.被注入的类型与注入的类型(子)是**父子类**

c.被注入的类型与注入的类型(实现类)是**接口和实现类**的类型

**2.按名称注入：**

**@Autowired**

**@Qualifier**：使用名称注入值，从整个Bean工厂中搜索相同名称的对象进行注入。



## 配置文件拆分整合

为应用指定多个spring配置文件，当项目越来越大，需要多人合作开发，一个配置文件就存在很大隐患。

拆分配置文件的策略：

1.按层拆

2.按功能拆





## 面向切面编程AOP

AOP(Aspect Orient Programming)，面相切面编程

切面：公共的，通用的，重复的功能称为切面，面向切面编程就是将切面提取出来，单独开发，在需要调用的方法中通过动态代理的方式进行植入。



**Spring支持的AOP的实现**

Before通知：在目标方法被调用前调用

After通知：在目标方法被调用后调用

Throws通知：目标方法抛出异常时调用

Around通知：拦截对目标对象方法调用



### AOP常用术语

切面：重复的，公用的，通用的功能称为切面，例如：日志，事务，权限

连接点：就是目标方法，因为在目标方法中要实现目标方法的功能和切面功能

切入点：多个连接点构成切入点，切入点可是一个目标方法，可以是一个类中的所有方法，可以是某个包下所有类中的方法

通知(Advice)：来指定切入的时机，在目标方法执行前，执行后，出错时，环绕。



### AspectJ框架

一个优秀的面向切面框架，它扩展了Java语言，提供了强大的切面实现。

AspectJ常用通知类型：

前置通知**@Before**：在目标方法执行前切入切面功能，在切面方法中不可以获得目标方法的返回值，只能得到目标方法的签名。

后置通知**@AfterReturning**

环绕通知**@Around**

最终通知**@After**

定义切入点**@Pointcut**



**AspectJ的切入点表达式**

规范公式：

execution(访问权限 方法返回值 方法声明(参数) 异常类型)

简化公式：

execution( 方法返回值 方法声明(参数) )

用到的符号：

*****：代码任意个任意的字符（通配符）

**..**：如果出现在方法的参数中，代表任意参数

​	  如果出现在路径中，则代表本路径及其所有子路径

```java
execution(public * *(..))//任意的公共方法
execution(* set*(..))//任何一个以set开始的方法
execution(* com.xyz.service.impl.*.*(..))//任意类的任意方法任意参数
execution(* com.xyz.service..*.*(..))//任意的返回值类型
```



#### 前置通知

```java
@Aspect //交给AspectJ的框架去识别切面类
public class MyAspect {
    //所有切面的功能都是由切面方法来实现
    //可以将各种切面都在此类中开发
    //前置通知的前面方法规范：
    //访问权限是public
    //方法的返回值是void
    //方法没有参数
    @Before(value="execution(public * com.bjpowernode.s01.SomeServiceImpl.*(..))")
    public void myBefore(){
        System.out.println("切面方法中的前置通知功能实现");
    }
}
```

```xml
<!--创建业务对象-->
<bean id="someService" class="com.bjpowernode.s01.SomeServiceImpl"></bean>
<!--创建切面对象-->
<bean id="myAspect" class="com.bjpowernode.s01.MyAspect"></bean>
<!--绑定-->
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

AspectJ框架切换JDK动态代理和CGLib动态代理：

```xml
<aop:aspectj-autoproxy></aop:aspectj-autoproxy> //默认JDK代理
<aop:aspectj-autoproxy proxy-target-class="true"></aop:aspectj-autoproxy>
```



#### 后置通知

后置通知的方法可以获得目标方法的返回值，至于得到的返回值能不能更改，取决于返回值的类型：

简单类型(8种基本类型+String)：不可更改

引用类型：可以更改

```java
@Aspect
@Component
public class MyAspect {
    /*后置通知的方法规范：
    * 访问权限是public
    * 方法没有返回值void
    * 方法名称自定义
    * 方法有参数
    * 使用@AfterReturning*/
	@AfterReturning(value="execution(* com.bjpowernode.s02.*.*(..))",returning = "obj")
    public void myAfterReturning(Object obj){
        System.out.println("后置通知功能实现");
    }
}
```



#### 环绕通知

它是通过拦截目标方法的方式，在目标方法前后增强功能的通知。它是功能最强大的通知，一般事务使用此通知。它可以轻易改变目标方法的返回值。

```java
@Aspect
@Component
public class MyAspect {
    /*环绕通知方法规范：
    * 访问权限是public
    * 切面方法的返回值就是目标方法的返回值
    * 方法名自定义
    * 方法有参数，此参数就是目标方法
    * 回避异常
    * 使用@Around注解声明环绕通知
    * 参数：value:指定切入点表达式*/
    @Around(value="execution(* com.bjpowernode.s03.*.*(..))")
    public Object myAround(ProceedingJoinPoint pjp) throws Throwable {
        //前切功能实现
        System.out.println("环绕通知中的前置功能实现");
        //目标方法调用
        Object obj = pjp.proceed(pjp.getArgs());
        //后切功能实现
        System.out.println("环绕通知中的后置功能实现");
        return obj.toString().toUpperCase();//改变了目标方法的返回值
    }
}
```



#### 最终通知

@After，无论目标方法是否正常执行，最终通知的代码都会被执行。

```Java
@Aspect
@Component
public class MyAspect {
    /*最终通知方法规范：
    * 访问权限是public
    * 方法没有返回值
    * 方法没有参数，如果有也只能是JoinPoint
    * 使用@After注解表明是最终通知
    * 参数：value:指定切入点表达式*/
    @After(value="execution(* com.bjpowernode.s04.*.*(..))")
    public void myAfter(){
        System.out.println("最终通知的功能");
    }
}
```



#### @Pointcut

如果多个切面切入到同一个切入点，可以使用别名简化开发。

使用@Pointcut注解，创建一个空方法，此方法的名称就是别名。



## SM的整合

### 整合步骤

1. 建表
2. 新建项目，选择quickstart模板
3. 修改目录
4. 修改pom.xml文件，添加相关的依赖
5. 添加MyBatis相应的模板(SqlMapConfig.xml和XXXMapper.xml文件)
6. 添加`SqlMapConfig.xml`文件，并拷贝jbdc.properties到resources目录下
7. 添加`applicationContext_mapper.xml`
8. 添加`applicationContext_service.xml`
9. 添加Users实体类，Accounts实体类
10. 添加mapper包，添加`UsersMapper`接口和`UsersMapper.xml`文件并开发
11. 添加service包，添加`UsersService`接口和`UsersServiceImpl`实现类
12. 添加测试类功能测试



### 事务

**事务的两种处理方式**

注解式的事务

使用@Transactional注解完成事务控制，此注解可以添加到**类**上或**方法**上。

声明式事务，在配置文件中添加一次，整个项目遵循事务的设定。



#### 注解式事务

在`applicationContext_service.xml`文件中添加事务处理：

**1.添加事务管理器**

为什么添加事务管理器？

JDBC:	`Connection	con.commit();	con.rollback();`

MyBatis:`SqlSession	sqlSession.commit(); sqlSession.rollback();`

事务管理器用来生成相应技术的连接+执行语句的对象。

使用MyBatis框架，必须使用`dataSourceTransactionManager`类完成处理。

**2.添加注解驱动**

```xml
<!--1.添加事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	<!--因为事务必须关联数据库处理，所以要配置数据源-->
    <property name="dataSource" ref="dataSource"></property>
</bean>
<!--2.添加注解驱动-->
<tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
</beans>
```

 **3.在实现类中添加@Transactional注解**

```java
@Transactional(propagation = Propagation.REQUIRED,noRollbackForClassName = "ArithmeticException")
```

**propagation**：事务的传播特性

**noRollbackForClassName**：指定发生什么异常时不回滚，使用异常的名称

**noRollbackFor**：指定发生什么异常时不回滚，使用异常的类型

**rollbackForClassName**：指定发生什么异常时必须回滚

**timeout** = -1//连接超时设置，默认值-1，表示永不超时

**readOnly** = false//默认是false，如果是查询操作，必须设置为true

**isolation** = Isolation.DEFAULT//使用数据库自己的隔离级别



#### 事务隔离级别

**未提交读(Read Uncommitted)**：允许脏读，可能读取到其他会话未提交事务修改的数据。

**提交读(Read Committed)**：只能读取到已经提交的数据。

**可重复读(Repeat Read)**：在同一个事务内的查询都是事务开始时刻一致的。

**串行读(Serializable)**：完全串行化的读，每次都都要获得表级共享锁，读写相互阻塞。

**默认隔离级别 isolation** = isolation.DEFAULT

MySQL默认级别为可重复读'REPEATABLE-READ'

Oracle默认级别为读已提交READ COMMITTED



项目中的所有事务，必须添加到业务逻辑层上。

#### 事务传播特性

多个事务之间的合并，互斥等都可以通过设置事务的传播特性来解决。

常用的传播特性
  **PROPAGATION_REQUIRED**：必被包含事务(增删改必用)
  **PROPAGATION_REQUIRES_NEW**：自己新开事务，不管之前是否有事务
  **PROPAGATION_SUPPORTS**：支持事务，如果加入的方法有事务，则支持事务，如果没有，不单开事务
  **PROPAGATION_NEVER**：不能运行在事务中，如果包在事务中，抛异常
  **PROPAGATION_NOT_SUPPORTED**：不支持事务，运行在非事务的环境 



#### 声明式事务

Spring非常有名的事务处理方式。

要求项目中的方法命名有规范

完成增加操作包含 add save insert set

更新操作包含 update change modify

删除操作包含 delete drop remove clear

查询操作包含 select find search get



配置事务切面时可以使用通配符*来匹配所有方法

```xml
<!--导入applicationContext_mapper.xml-->
    <import resource="applicationContext_Mapper.xml"></import>
    <!--添加包扫描-->
    <context:component-scan base-package="com.bjpowernode.service.impl"></context:component-scan>
    <!--添加事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!--配置事务切面-->
    <tx:advice id="myAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="*select*" read-only="true"/>
            <tx:method name="*find*" read-only="true"/>
            <tx:method name="*search*" read-only="true"/>
            <tx:method name="*get*" read-only="true"/>
            <tx:method name="*insert*" propagation="REQUIRED"/>
            <tx:method name="*add*" propagation="REQUIRED"/>
            <tx:method name="*save*" propagation="REQUIRED"/>
            <tx:method name="*update*" propagation="REQUIRED"/>
            <tx:method name="*change*" propagation="REQUIRED"/>
            <tx:method name="*modify*" propagation="REQUIRED"/>
            <tx:method name="*drop*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <!--绑定切面和切入点-->
    <aop:config>
        <aop:pointcut id="mycut" expression="execution(* com.bjpowernode.service.impl.*.*(..))"/>
        <aop:advisor advice-ref="myAdvice" pointcut-ref="mycut"></aop:advisor>
    </aop:config>
```







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





