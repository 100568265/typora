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



























