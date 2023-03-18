**本阶段课程安排**

Dubbo-分布式框架(3次课)

Maven-继承与聚合(2小时)

Springboot-框架(6次课)

盈利宝项目(16次课)



# Dubbo

进程间通信

RPC概念：Remote Procedure Call

分布式：若干独立计算机的集合，分布式系统是建立在网络之上的。

Dubbo框架实现了RPC协议，能够在分布式项目中做服务调用。

微服务：如果服务功能是单一的，可以独立的部署运行的，称为微服务。

RPC:远程工程调用，是进程间的通信方式，是一种协议。实现从A服务器执行B服务器执行B服务器上运行的程序的方法，无需了解底层网络通信的细节。





## Dubbo的结构

Container：容器是Spring，管理对象。管理自定义对象，Dubbo框架使用的对象。

Provider：提供功能的叫提供者，通过接口和里面的方法提供功能。

Consumer：消费者角色，是一个项目，表示要使用提供者功能。



直连模式：消费者通过url地址访问提供者，适合学习，简单项目。

注册中心：通过注册中心，实现消费者和服务提供者的协调管理。



## 直连模式

**一.创建提供者：**

1.创建maven项目

2.加入dubbo框架依赖

3.创建订单的实体类Order类，需要实现序列化

4.创建业务接口和实现类，比如OrderService，OrderServiceImpl接口

5.写Spring的配置文件，声明dubbo的对象和自定义对象

6.创建启动类，运行Dubbo程序

7.打包程序，安装到maven仓库，目的是让消费者能够使用Order类，OrderService接口的类定义

```xml
<!--声明Dubbo的配置信息-->
<!--1.声明服务名称
    name:服务名称，自定义的字符串，最好是唯一值
    给Dubbo内部使用的，识别应用程序的名字-->
<dubbo:application name="order-service"></dubbo:application>
<!--声明RPC的具体协议-->
<dubbo:protocol name="dubbo" port="20880"></dubbo:protocol>
<!--3.暴露服务，让消费者调用
    必须使用暴露服务说明消费者使用的服务
    interface:暴露的接口的全限定名称
    registry:是否使用注册中心，不使用为N/A
    ref:接口的实现类对象的id-->
<dubbo:service interface="com.bjpowernode.service.OrderService" registry="N/A" ref="OrderServiceImpl"></dubbo:service>

<!--4.声明接口的实现类对象-->
<bean id="orderServiceImpl" class="com.bjpowernode.service.OrderServiceImpl"></bean>
```



**二.创建消费者：**

1.创建maven项目

2.加入dubbo依赖和提供者依赖（Order，OrderService接口）

3.创建spring配置文件，声明对远程对象的引用

4.创建启动，从spring容器中，获取远程接口的代理对象，通过代理对象执行方法，表示执行远程提供者服务的功能。