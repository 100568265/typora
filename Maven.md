# Maven

**什么是Maven？**

它是一个自动化的构建工具。



## **构建步骤：**

1. **清理：**将上次编译的内容全部删除，清理环境，为下次编译做准备。
2. **编译：**将.java文件编译生成.class文件。
3. **测试：**每个功能/模块/系统完成后都要进行测试。
4. **报告：**测试完成后生成报告结果，以便程序分析，功能实现，扩展。
5. **打包：**生成对应的jar包/war包。
6. **安装：**将jar包安装到本地仓库，或将项目安装到客户的环境中。
7. **部署：**运行项目。



## Maven核心概念

### **pom：**

Project Object Model,项目对象模型

它是Maven项目的核心配置文件，以pom.xml形式存在，所有的Maven项目都有这个文件。

### **约定的目录结构：**

maven_001_demo

​	**src**

​		**main**(java源码文件及配置文件)

​			**java**(源文件)

​				com.bjpowernode.controller

​				com.bjpoernode.service

​				com.bjpowernode.mapper

​				com.bjpowernode.pojo

​			**resources**(所有的配置文件)

​				applicationContext_service.xml

​				applicationContext_mapper.xml

​				jdbc.properties	

​		**test**(测试文件)

​			**java**

​				test

​			**resources**

pom.xml



### 坐标：

所有的功能是以坐标的形式存在于仓库中。想引用功能必须得知道坐标。坐标也称为gav坐标。

<groupid>com.bjpowernode</groupid>组织名，一般是公司域名的倒写。

<artifactid>maven_001_demo</artifactid>项目或模块名称

<version>1.0.1</version>版本编号



### 依赖：

就是添加jar包的引用。先从本地仓库找，再到指定的远程仓库找资源，并下载到本地仓库，添加到本项目中。

使用的是一组标签：

<dependencies>

​	<dependency>

​		<groupId>mysql</groupId>

​		<artifactId>mysql-connector-java</artifactId>

​		<version>5.1.32</version>

​	</dependency>

</dependencies>

一个大标签（dependencies）内有多套标签。



### 仓库：

用来存放jar包的位置。可以是本地，可以是远程。

**本地仓库：**本机上一个指定的磁盘的位置。里面存放着所有的jar包。

**远程仓库：**不在本机上，在网络上的一个地址存放着jar包。

​	中央仓库：全世界程序员共享的一个仓库地址。

​	中央仓库镜像：各大洲都有中央仓库的镜像，每个洲的程序自动从本洲的镜像中下载jar包，分流访问量。

**中央仓库地址：http://mvnrepository.com/**

​	标杆企业的仓库(阿里)：阿里分享的自己多年各种项目开发积累下的jar包地址。

​	私服：本公司的一个服务器上存放的jar包的位置。使用局域网。



**生命周期：**

对应每个构建的步骤都会有响应的生命周期。

生命周期底层对应的一个一个maven指令。

在idea的工具中，生命周期的命令对应一个个操作按钮。

**插件：**

Mavan管理了各种插件，用来继承项目的构建。包括编译插件，包括tomcat，包括自定义插件。

**继承：**

子工程继承父工程的约定的规范。

**聚合：**

将多个子工程整合成一个大工程



## Maven配置环境变量

下载Maven工具。

JAVA_HOME:配置jdk的路径

MAVEN_HOME:配置Maven工具的主目录

将JAVA_HOME和MAVEN_HOME配置在系统环境变量path路径中。



## 依赖的范围

compile,test,provided









