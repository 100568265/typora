# 反射Reflect



##### 反射机制概述

通过java语言中的反射机制可以操作字节码文件。有点类似于黑客（可以读，改字节码文件）。通过反射可以操作代码片段（.class文件）

`反射机制的包：java.lang.reflect.*;`

##### 反射机制相关的类有哪些？

`java.lang.Class;` 代表整个字节码，代表一个类型

`java.lang.reflect.Method;`代表字节码中的方法字节码

`java.lang.reflect.Constructor;`  代表字节码中构造方法字节码

`java.lang.reflect.Field;` 代表字节码中的属性字节码



##### 获取Class的三种方式

1. 第一种：

   `Class.forName();`

   - 静态方法

   - 方法的参数是一个字符串

   - 字符串需要的是一个完整的类名，必须带上包名
   - `Class.forName()`方法会导致类加载，类加载时静态代码块会执行。

   ```java
   Class c1 = Class.forName("java.lang.String");
   Class c2 = Class.forName("java.util.Date");
   Class c3 = Class.forName("java.lang.Integer");
   ```

2. 第二种：

   `对象.getClass();`

   java中任何一个对象都有`getClass()`方法

   ```java
   String s = "abc";
   Class x = s.getClass();
   ```

3. 第三种：

   `Class  c =String.Class;`

   java中任何一种类型，都有.class属性。

   ```java
   Class c = String.class;//c代表String类型
   Class k = Date.class;//k代表Date类型
   Class f = int.class;//f代表int类型
   Class e = double.class;//e代表double类型
   ```

##### 通过反射实例化对象

`newInstance()`会调用User类的无参构造方法，完成对象创建

```java
Class c = Class.forName("com.bjpowernode.java.User");
Object obj = c.newInstance();//JDK9以后过时
```

##### 获取类路径下（src）文件的绝对路径

```java
//解释：Thread.currentThread() 当前线程路径
//getContextClassLoader() 线程对象的方法，可以获取当前线程的类加载器对象
//getResource() 获取资源，这是类加载器对象的方法，当前线程的类加载器默认从类的根路径加载资源
String path = Thread.currentThread().getcontextClassLoader().getResource("classinfo.properties").getPath();
//采用以上的代码可以拿到一个文件的绝对路径
```

##### 验证反射机制的灵活性

```java
//获取文件绝对路径
String path = Thread.currentThread)().getContextLoader().getResource("chapter25/class.info.properties").getPath();
//通过IO流读取class.info.properties文件
FileReader reader = new FileReader(path);
//创建属性类对象Map
Properties pro = new Properties();//key value是String
//加载
pro.load(reader);
//关闭流
reader.close();
//通过key获取value
String className = pro.getProperty("className");
//System.out.println(className);

//通过反射机制实例化对象
Class c = Class.forName(className);
Object obj = c.newInstance();
//System.out.println(obj);
```

只要修改配置文件properties里面的内容，就可以控制java代码中所创建的类。

```properties
className=java.util.Date
```



##### 资源绑定器

java.util包下提供了一个资源绑定器，便于获取属性配置文件内容。

使用此方式的时候，属性配置文件xxx.properties必须放到类路径下。

路径后面的扩展名不能写

```java
ResourceBundle bundle = ResourceBundle.getBundle("classinfo");
```





