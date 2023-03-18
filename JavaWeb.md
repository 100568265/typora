# JavaSE

计算机存储数据的最小单位：字节 1Byte = 8 bits

**8大基本数据类型：**

byte, short, int, long, float, double, boolean, char

**整数**的字面量默认为**int**类型。

**小数**的字面量默认为**double**类型。

关键字不能作为类名或者变量名使用。



**a+=b 和a=a+b的区别**

![image-20220912235628828](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220912235628828.png)

a+=b运算：a+b的结果会自动转换成a的类型。



**java中类的加载时机：**

1. 创建对象的时候(new)
2. 访问类的静态变量时
3. 访问类的静态方法
4. 反射，Class.forName
5. 初始化一个类的子类，会先初始化子类的父类
6. 虚拟机启动时，定义了main()方法的那个类





## 选择循环

### switch

**注意事项：**

1.case给出的值不允许重复，而且只能是字面量，不能是变量

2.表达式类型**不支持**double，float，long

3.不要忘记写break，否则会出现**穿透现象**。

4.穿透现象是一种特性，不是缺点，有专门的使用场景。



### for，while

**for和while循环怎么选择:**

在**确定**循环次数的时候，建议使用**for**循环

在**不确定**循环次数时，建议使用**while**循环



**break：**只能用于结束所在循环，或者结束所在switch分支的执行。

**continue：**只能在循环中使用。



### Random随机数

**功能：**随机生成一个数

**用法：**

1.导包 java.util.Random

2.创建随机对象

3.调用**nextInt()**方法，可以返回一个整型的随机数

```java
Random r = new Random();
int data = r.nextInt(10);//0-9不包含10的
System.out.println(data);//打印生成的随机数data
//生成区间随机数的方法
int data2 = r.nextInt(10)+1;//生成1-10之间的随机数
```





## 数组

数组就是用来存储**一批同种类型数据**的**内存区域**。

数组属于**引用数据类型**。



**静态初始化数组：**

定义数组的时候直接给数组赋值。

```java
double[] scores = new double[]{1,2,3,4,5,6,7,8,9};
int[] ages = new int[]{12,24,36,48};//完整写法
int[] ages2 = {12,24,36};//简化写法
```



**数组的动态初始化：**

定义数组的时候，只确定元素的类型和数组的长度，数据之后再存入。

```java
int[] arr = new int[3];//长度为3的int类型数组
```



**元素默认值：**

| 数据类型 | 明细                         | 默认值 |
| -------- | ---------------------------- | ------ |
| 基本类型 | byte, short, char, int, long | 0      |
|          | float, double                | 0.0    |
|          | boolean                      | false  |
| 引用类型 | 类，接口，数组，String       | null   |



## 方法

**基本类型**的参数传递是：**数据值传递**

**引用类型**的参数传递是：**地址值传递**



**方法重载：**

同一个类中，出现多个同名方法，但是形参列表不同。

**形参不同是指：**形参的个数，类型，顺序不同。不关心形参的名称。

**作用：**

可读性好，**方法名相同**是为了提示是**同一类型的功能**。通过**形参不同**实现**功能差异化**。





## String类

字符串类型，可以定义**字符串变量**指向**字符串对象**。

String被称为**不可变字符串类型**，它的对象在创建后不能被更改。



**String是不可变字符串的原因？**

String变量每次修改都是产生并指向了新的字符串对象。



**字符串构造器：**

```java
//方式1:直接构造字符串
String s1 = "我是你爹";
//方式2:根据字符数组的内容，创建字符串对象
char[] chars = {'a','b','中','心'};
String s2 = new String(chars);//拼接成字符串"ab中心"
//方式3:根据字节数组的内容，创建字符串对象
byte[] bytes = {97,98,99,65,66,67};
String s3 = new String(bytes);//abcABC
```



**区别：**

以" "方式给出的字符串对象，在**字符串常量池**中存储，而且相同内容只会在其中存储一份。

通过构造器new对象，每new一次都会产生**新对象**，放在**堆内存**中。



![image-20220914223552006](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220914223552006.png)



**==和equals的区别**

==判断的是内存地址，equals()比较内容是否一致。

**结论：**==不适合用作字符串比较



**开发中什么时候用==比较数据？**

**基本数据类型**可以用==比较。



**String常用API：**

**charAt, toCharArray, subString, replace, contains, startsWith, split**

```java
//1. public char charAt():获取某个索引位置处的字符。
char c = name.charAt[2];
//2. public char[] toCharArray()方法:把字符串转换成字符数组。
char[] chars = name.toCharArray();
//3. public String subString(int beginIndex, int endIndex),截取内容(包前不包后)
name2.subString(0,9);//截取下标0-8的字符
//4. public String replace(CharSequence target, CharSequence replacement)方法:替换指定字符串
String name3 = "金三胖是个大傻逼，金三胖nmsl!";
String rs3 = name3.replace("金三胖","***");
//5. public boolean contains(CharSequence s)方法:判断是否包含
name3.contains("金三胖");
//6. public boolean startsWith(String prefix)方法:是否以..开始
name3.startsWith("金三胖");
//7. public String[] split(String s)方法:按照某个内容把字符串分割成字符串数组返回
String name4 = "王宝强,贾乃亮,陈羽凡";
String[] name4.split(",");
```



## ArrayList

**数组的特点：**

数组定义完成后，类型确定，长度固定。

在个数不确定，而且要进行增删数据时，数组是不太合适的。

**集合的特点：**

集合的大小不固定，类型也可以选择不固定。

集合非常适合做元素个数不确定，且要进行增删操作的业务场景。

集合中**只能**存储**引用数据类型**，不支持基本数据类型。

ArrayList是**集合**的一种，它支持**索引**。

```java
//1.创建ArrayList集合对象
ArrayList list = new ArrayList();
//2.添加数据
list.add("呵呵");
//3.给指定索引位置插入元素
list.add(1,"赵敏");
```



**泛型约束ArrayList：**

ArrayList<String>：此集合只能操作字符串类型的元素。

ArrayList<Integer>：此集合只能操作整数类型的元素。

```java
ArrayList<String> list = new ArrayList<>();
```



**常用API：**

![image-20220915000606934](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220915000606934.png)

```java
//1. public E get(int index):获取某个索引位置处的元素值
String e = list.get(3);
//2. public int size():获取集合的大小(元素个数)
list.size();
//3.完成集合的遍历
for(int i=0;i<list.size();i++){
    System.out.println(list.get(i));
}
//4. public E remove(int index):删除索引位置处的元素值，并返回此元素值
String e2 = list.remove();
//5. public boolean remove(Object o):直接删除元素值，删除成功返回true
System.out.println(list.remove("mybatis"));
//6. public E set(int index,E element):修改某个索引处的元素值,返回修改前的值
String e3 = list.set(0,"贾乃亮");
```



## 面向对象

**类：**是对象共同特性的描述（设计图）

**对象：**是真是存在的实例

**this：**代表当前对象的地址。出现在构造器和成员方法中。

**成员变量&局部变量：**

![image-20220914205247492](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220914205247492.png)



### static

**静态成员变量：**有static修饰，属于类，可以被共享访问。

**实例成员变量：**无static修饰，属于对象。



**静态成员方法：**有static修饰，属于类，建议用类名访问。

**实例成员方法：**无static，属于对象，只能用对象访问。



**static访问注意事项：**

- 静态方法只能访问静态的成员，不可以直接访问实例成员。
- 实例方法可以访问静态的成员，也可以访问实例成员。
- 静态方法中是不能出现this关键字的。



**静态代码块：**static修饰，属于类，与类一起优先加载一次，自动触发执行。

作用：可以用于初始化静态资源。





### 继承

提高代码复用性，减少代码冗余，增强类的功能扩展性。

**继承后子类构造器的特点：**

子类的全部构造器默认会先访问父类的无参构造器，然后再执行自己。



**super：**代表父类存储空间的标识。

**this(...)：**借用本类兄弟构造器

**注意：this(...)和super(...)只能在构造器的第一行！！**



### 权限修饰符

同一个包下的类，互相可以直接访问。

不同包下的类，需要先导包才能访问。

![image-20220916235836142](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220916235836142.png)



### final

- 修饰类：该类不能被继承。
- 修饰方法：该方法不能被重写。
- 修饰变量：该变量不能再次赋值。

final**修饰变量**的注意事项：

变量是基本类型，数据值不能改变。

变量是引用类型，地址值不能改变，但是指向的对象的内容可以改变。



### 枚举

枚举是java中的一种**特殊类型**。

**作用：**是为了做信息**标志**和信息**分类**。

```java
修饰符 enum 枚举名{
    //第一行罗列枚举实例的名称。
    SPRING,SUMMER,FALL,WINTER;
}
```



### 匿名内部类

本质上是一个没有名字的局部内部类，定义在方法，代码块中。

**作用：**方便创建子类对象，为了简化代码。

```java
Employee a = new Employee(){
    public void work(){ 
    }
};
a.work();
```

**特点：**

- 是一个没有名字的类
- 匿名内部类写出来就会产生一个匿名内部类的对象
- 匿名内部类的对象类型相当于当前new的类型的子类类型





## 常用API

### Object

**toString()：**默认返回当前对象的内存地址：类全限定名@内存地址

**equals()：**默认比较两个对象地址是否相同



**Objects常用方法**

![image-20220917013758699](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917013758699.png)



### StringBuilder

StringBuilder是一个可变的字符串类，可以看做成一个对象容器。

**作用：**操作字符串，如拼接，修改等。提高效率。



**StringBuilder常用方法：**

![image-20220917014243286](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917014243286.png)

**注意：**StringBuilder只是拼接字符串的手段：效率高。

最终的结果还是要恢复成**String**类型。



### System

System不能被实例化，是静态方法。

**常用方法：**exit(), currentTimeMillis(), arraycopy()

![image-20220917020139182](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917020139182.png)





### Date

```java
//日期对象创建：
Date date = new Date();
//获取时间毫秒值
Long time = date.getTime();
//时间毫秒值恢复成日期对象，第一种：
Date d = new Date(time);
//第二种：
d.setTime(time);
```



**SimpleDateFormat类**

- 可以对Date对象或时间毫秒值**格式化**成自定义的形式

  ![image-20220917160637279](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917160637279.png)

  ```java
  //1.日期对象
  Date d = new Date();
  System.out.println(d);
  //2.格式化这个日期对象(指定格式化的形式)
  SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss EEE");
  //3.格式化日期对象成自定义的字符串形式
  String rs = sdf.format(d);
  System.out.println(rs);
  //4.格式化时间毫秒值
  long time1 = System.currentTimeMillis();
  String rs2 = sdf.format(time1);
  System.out.println(rs2);
  ```



- 可以把字符串的时间形式**解析**成时间对象。(字符串-->时间对象)

![image-20220917160413478](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917160413478.png)

```java
//2021年8月06日 11:11:11 往后 2天 14小时 49分 06秒后的时间是多少？
String dateStr = "2021年08月06日 11:11:11";
//1.字符串解析成时间对象
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
Date a = sdf.parse(dateStr);
//2.获取时间毫秒值+2天
long time = a.getTime() + (2*24*3600+14*3600+19*60+6)*1000;
//3.格式化这个时间毫秒值
String s = sdf.format(time);
System.out.println(s);
```





### 包装类

自动装箱和自动拆箱。

包装类的变量默认值可以是null，容错率更高。



**把字符串类型的数值转换成真实的数据类型：**

```java
int age = Integer.valueOf("26");
double score = Double.valueOf("99.3");
```





### 正则表达式

![image-20220917164934213](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917164934213.png)

![image-20220917164945229](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917164945229.png)





### Arrays类

![image-20220917165513160](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917165513160.png)

```java
int[] arr = {10,2,55,23,24,100};
//1.返回数组内容的 toString(数组)
String rs = Arrays.toString(arr);

//2.排序的API(默认升序)
Arrays.sort(arr);

//3.二分搜索技术
Arrays.binarySearch(arr,55);

//4.自定义数组排序规则-比较器
//要求降序排序，自定义比较器(只支持引用类型)
Integer[] ages = {34,12,42,23};
Arrays.sort(ages, new Comparator<Integer>() {
     @Override
     public int compare(Integer o1, Integer o2) {
       		//指定比较规则
            if(o1>o2){
                return 1;
            } else if (o1<o2) {
                return -1;
            }
            //return o1-o2;//默认升序
            return o2-o1;//降序
        }
    });
```



### Lambda表达式

jdk8的新特性

**作用：**简化匿名内部类的代码写法

Lambda表达式只能简化**函数式接口**的匿名内部类的写法。

**函数式接口：**

必须是接口，且仅有一个抽象方法。

![image-20220917172348663](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917172348663.png)





## Collection

**数组适合的场景：**业务数据个数是固定的，且都是同一批数据类型的时候。

**集合适合的场景：**数据的个数不确定，需要进行增删元素的时候。



**集合类体系结构：**

**Collection** 单列：每个元素只包含一个值。

**Map** 双列：每个元素是(key,value)**键值对**。

![image-20220917174044603](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917174044603.png)



### Collection-API

![image-20220917175746222](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917175746222.png)

```java
Collection<String> list = new ArrayList<>();
//1.添加元素，成功返回true
list.add("java");
list.add("java");
list.add("黑马");
//2.清空集合的元素
list.clear();
//3.判断集合是否为空
System.out.println(list.isEmpty());
//4.获取集合的大小
System.out.println(list.size());
//5.判断集合是否包含某元素
System.out.println(list.contains("java"));
//6.删除某个元素，如果重复默认只删除第一个
System.out.println(list.remove("java"));
//7.把集合转成数组
Object[] arrs = list.toArray();
//8.合并集合-把c2的全部元素拷贝到c1中
c1.addAll(c2);
```



### 遍历方式



**1.迭代器**

![image-20220917180020207](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917180020207.png)

```java
Iterator<String> it = lists.iterator();
while (it.hasNext()){
	String element = it.next();
	System.out.println(element);
	}
```



**2.foreach**

**格式：**

for(元素数据类型 变量名：数组或集合){

​		//在此处使用变量即可，该变量就是元素

}

```java
Collection<String> list = new ArrayList<>();
for(String ele:list){
    System.out.println(ele);
}
```



**3.lambda表达式**

![image-20220917200729048](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917200729048.png)

```java
lists.forEach(s -> System.out.println(s));
```

或者

```java
lists.forEach(System.out::println);
```





### 数据结构

**常见数据结构：**

栈，队列，数组，链表，二叉树，二叉查找树，平衡二叉树，红黑树...



**栈：**

后进先出，先进后出

**队列：**

先进先出，后进后出。后端入队列，前端出队列

**数组：**

查询速度快，地址值和索引定位。

删除效率低。

添加效率极低。

**链表：**

内存中不连续存储的，每个元素节点包含**下个元素的地址**。

查询慢，无论查询哪个数据都得从头开始找。

增删快。

**二叉树：**

父节点有**两个分支**：左子节点，右子节点。

**二叉查找树：**

小的存左边，大的存右边，一样的不存

**平衡二叉树：**

**任意节点**的左右两个子数的高度差不超过1，任意节点的左右两个子树都是一颗平衡二叉树

**红黑树：**

每一个节点可以是红或黑，红黑树不是通过高度平衡的。它的平衡通过“红黑规则”实现。

红黑规则：

- 根节点必须是黑色。
- 如果一个节点没有子节点或父节点，则该节点指针属性为Nil，视为叶节点，为黑色。
- 如果某一个节点是红色的，name它的子节点必须是黑色的。
- 对每一个节点，从该节点到其所有后代叶节点的简单路径上，均包含相同数目的黑节点。





### List系列

**List系列集合特点：**

ArrayList，LinkedList：有序，可重复，有索引。



**List集合的遍历方法：**

迭代器

增强for

Lambda表达式

for循环（因为List集合存在索引）



**ArrayList底层原理：**

ArrayList底层基于数组实现，根据索引定位元素快，增删需要做元素的移位操作。

第一次创建集合并添加第一个元素的时候，在底层创建一个默认长度为10的数组。



size()：当前集合已有的元素的长度。	



**LinkedList底层原理：**

底层数据结构是双链表，**查询慢**，**首尾操作**的**速度极快**。有很多操作首尾元素的特有方法。

![image-20220917222532084](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220917222532084.png)

LinkedList可以完成队列和栈的结构实现。



**并发修改异常：**

**a. 迭代器遍历并删除**

要使用迭代器删除当前位置的元素，保证不后移。

```java
it.remove();
```

**b. foreach遍历删除**

也会出现同样的问题，只能用**迭代器来遍历删除**。

**c. lambda表达式**

因为内部也是用foreach，所以也有bug。

**d. for循环**

没有异常错误，但是数据会漏删。但是有解决方案：**从后往前删**



类可以继承，集合之间不能继承。

**?**在**使用泛型**的时候可以代表一切集合类型。

<? extends Car>: ?必须是Car或者其子类 **泛型上限**

<? super Car>：?必须是Car或者其父类 **泛型下限**





### Set系列

**Set集合特点：**

**无序：**存取顺序不一致

**不重复：**可以去重复

**无索引：**没有带索引的方法



**Set集合实现类特点：**

- **HashSet：**无序，不重复，无索引。
- **LinkedHashSet：有序**，不重复，无索引。
- **TreeSet：可排序**，不重复，无索引。



**HashSet：**

HashSet底层采取**哈希表**存储的数据。

哈希表是一种对于增删查改性能都较好的结构。



**哈希值：**是JDK根据**对象的地址**，按照某种规则算出来的**int**类型的数值。

Object类API： **hashCode()：**返回对象的哈希值。

**对象的哈希值特点：**

- 同一个对象多次调用hasCode()方法返回的哈希值是相同的。
- 默认情况下，不同对象的哈希值是不同的。



**Set集合底层原理：**

JDK8之前，哈希表：数组+链表

JDK8开始，哈希表：数组+链表+红黑树



**Set集合去重复原理：**先判断哈希值，再判断equals

对象内容一样的哈希值就相同。





**TreeSet集合默认的规则：**

对于数值类型：Integer，Double，默认按照大小进行升序排序。

对于字符串类型：默认按照首字符的编号升序排序。

对于自定义类型：必须自定义排序规则。



方式1：让自定义类实现Comparable接口重写里面的compareTo方法。

方式2：TreeSet集合有参数构造器，可以设置Comparator接口对应的比较器对象。





### 可变长参数

```java
public static void sum(int...nums){
    
}
//1.可以不传参数
//2.可以传多个参数
//3.可以传一个数组
//注意：一个形参列表只能传一个可变参数
```





### 集合工具类

**Collections集合工具类**

![image-20220918004355991](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220918004355991.png)

![image-20220918004612068](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220918004612068.png)





## Map

Map集合是一种键值对集合,key=value

**Map集合整体格式：**

Collection集合的格式：[元素1,元素2,元素3...]

Map集合的完整格式：{key1=value1, key2=value2, key3=value3, ...}



![image-20220918020837732](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220918020837732.png)

**Map特点：**

- Map集合是由键决定的。
- Map集合的键是无序，不重复，无索引。
- Map集合后面重复的键对应的值会覆盖前面的键的值。
- Map集合的键值对都可以为null。



**Map集合实现类特点：**

**HashMap：**元素按照键是无序，不重复，无索引。值不做要求（与Map体系一致）

```java
Map<String,Integer> maps = new HashMap<>();//一行经典代码
maps.put("耐克",1000);
maps.put("阿迪",900);
maps.put("阿迪",1000);//覆盖前面的数据
maps.put(null,null);//是可以的
```

**LinkedHashMap：**元素按照键**有序**，不重复，无索引。值不做要求。

**TreeMap：**键是**排序**的，不重复的，无索引的，值不做要求。



### Map API

![image-20220918021715595](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220918021715595.png)



### Map遍历

**方式1：键找值**

- 先获取Map集合的全部键的Set集合。
- 遍历键的Set集合，然后通过键提取对应值。

![image-20220918022813999](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220918022813999.png)

```java
//1.先拿到集合的全部键
Set<String> keys = maps.keySet();
//2.遍历每个键，拿到对应的值
for(String key:keys){
    int value = maps.get(key);
    System.out.println(key+"===>"+value);
}
```



**方式2：键值对**

通过调用Map的方法 entrySet()把Map集合转换成**Set集合形式**。

Set集合形式： **Set<Map.Entry<String,Integer>> 集合名;**

![image-20220918024542118](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220918024542118.png)

```java
//1.把Map集合转换成Set集合
Set<Map.Entry<String,Integer>> entries = maps.entrySet();
//2.开始遍历
for(Map.Entry<String,Integer> entry:entries){
    String key = entry.getKey();
    String value = entry.getValue();
    System.out.println(key+"===>"+value);
}
```



**方式3：Lambda表达式**

![image-20220918024627149](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220918024627149.png)

```java
maps.forEach((k,v) -> {
    System.out.println(k+"--->"+v);
});
```



**注意：**HashSet的底层就是HashMap，只是HashSet只保留了键数据，去掉了值数据。



### 不可变集合

有些业务场景下需要有不可变集合对象。

集合的数据项在创建的时候提供，并且在整个声明中秋都不可变，否则报错。

![image-20220918032935205](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220918032935205.png)





## Stream流

**Stream流思想：**

1. 先得到集合或者数组的Stream流
2. 把元素放上去
3. 再用Stream流**简化API**来方便的**操作元素**。



**集合获取Stream流：**

可以使用Collection接口的默认方法**stream()**生成流

```java
//Collection集合获取流
Collection<String> list = new ArrayList<>();
Stream<String> s = list.stream();

//Map集合获取流
Map<String,Integer> maps = new HashMap<>();
//键流
Stream<String> keyStream = maps.keySet().stream();
//值流
Stream<Integer> valueStream = maps.values().stream();
//键值对流
Stream<Map.Entry<String,Integer>> keyAndValueStream = maps.entrySet().stream();
```



**获取数组流：**

![image-20220921095317623](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921095317623.png)

```java
String[] names = {"aa","bb","cc"};
//方法1
Stream<String> nameStream = Arrays.stream(names);
//方法2
Stream<String> nameStream2 = Stream.of(names);
```



**Stream常用API：**

![image-20220921102330969](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921102330969.png)

**filter()：**过滤元素

```java
list.stream().filter(s -> s.startsWith("E"));
```

**forEach()：**遍历元素

```java
list.stream().filter(s -> s.startsWith("E")).forEach(s -> System.out.println(s));
```

**count()：**计数

```java
long size = list.stream().filter(s -> s.length()==3).count();
```

**limit()：**取前几个元素

```java
list.stream().filter(s -> s.startsWith("E").limit(2).forEach(System.out::println));
```

**skip()：**跳过前几个元素

```java
list.stream().filter(s -> s.startsWith("E").skip(2).forEach(System.out::println));
```

**map()：**加工方法

```java
//Map加工方法：第一个参数原材料，第二个参数是加工后的结果
//给集合元素的前面都加上一个"我的"
list.stream().map(s -> "我的" + s).forEach(a -> System.out.println(a));
```

**concat()：**合并流

```java
Stream<String> s1 = list.stream().filter(s -> s.startsWith("张"));
Stream<String> s2 = Stream.of("java1","java2");
Stream<String> s3 = Stream.concat(s1,s2);
```



**Stream流的收集方法**

![image-20220921103001880](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921103001880.png)

![image-20220921103016352](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921103016352.png)





## 异常

异常是在编译或执行的过程中可能出现的问题。**注意：语法错误不是异常**

异常一旦出现，如果没有提前处理，程序就会退出JVM而终止。



**常见异常：**

1. 数组索引越界异常：ArrayIndexOutOfBoundsException
2. 空指针异常：NullPointerException
3. 类型转换异常：ClassCastException
4. 数字操作异常：ArithmeticException
5. 数字转换异常：NumberFormatException





## IO流



### File类

File类可以定位文件：进行删除，获取文本本身信息等操作。

File类不能读写文件。



**File类创建对象**

![image-20220921112144420](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921112144420.png)



绝对路径：从盘符开始

相对路径：默认到当前工程下的目录

```java
//创建File对象
File f = new File("C:\\Users\\52677\\Desktop\\密码.txt");
//获取文件大小
System.out.println(f.length());
//判断路径是否存在
System.out.println(f.exists());
```



**判断文件类型，获取文件信息功能：**

![image-20220921113828132](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921113828132.png)



**创建文件：**

![image-20220921114129501](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921114129501.png)

**删除文件：**

![image-20220921113938486](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921113938486.png)

**遍历：**

![image-20220921114421453](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921114421453.png)

```java
File f1 = new File("D:\\course");
File[] files = f1.listFiles();
for(String file:files){
	System.out.println(file);
}
```

**listFiles方法注意事项：**

- 当调用者不存在时，返回null
- 当调用者是一个文件时，返回null
- 当调用者是一个空文件夹时，返回一个长度为0的数组





### 字符集

**String编码：**

把文字转换成字节(使用指定的编码)

![image-20220921173439035](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921173439035.png)

**String解码：**

![image-20220921173500527](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921173500527.png)



**IO流四大类：**

![image-20220921174421028](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921174421028.png)

**注意：**输入输出是以**内存**为基准



### 字节流

**字节输入流**

```java
InputStream is = new FileInputStream();
//读取一个字节数组
byte[] buffer = new byte[3];
int len;//记录每次读取的字节数
while((len = is.read(buffer))!=-1){
    //读取多少倒多少
    System.out.print(new String(buffer,0,len));
}
```



**字节输出流**

![image-20220921232629661](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921232629661.png)

![image-20220921232642645](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220921232642645.png)

```java
OutputStream os = new FileOutputStream("data.txt");
//一个一个放入字节
os.write('a');
os.write('b');
os.write('c');
//放入字节数组
byte[] buffer = {'a','97','98','99'};
os.write(buffer);
os.flush();
os.close();
```



**总结：**

**字节流适合做一切文件数据的拷贝**，任何文件的底层都是字节。只要前后文件格式，编码一致，就没有任何问题。





### 字符流

**字节流读取中文输出可能会存在什么问题？**

会乱码，或者内存溢出

**读取中文字符，哪个流更合适？**

字符流，最小单位是按照单个字符读取的。



**字符输入流：**

![image-20220922000344212](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922000344212.png)

```java
//每次读取一个字符返回
Reader fr = new FileReader("data.txt");
int code;
while((code=fr.read())!=-1){
	System.out.print((char)code);
}
```

```java
//每次读取一个字符数组
Reader fr = new FileReader("data.txt");
char[] buffer = new char[1024];//1k个字符
int len;
while((len=fr.read())!=-1){
    String rs = new String(buffer,0,len);
    System.out.print(rs);
}
```



**字符输出流：**

![image-20220922001223539](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922001223539.png)

![image-20220922001238775](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922001238775.png)





### 缓冲流

缓冲流也称为高级流。

自带缓冲区，可以提高原始字节，字符流读写数据的性能。

![image-20220922002049520](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922002049520.png)



**字节缓冲流**

![image-20220922002356182](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922002356182.png)

字节缓冲流自带了8KB的缓冲区





**字符缓冲流**

BufferedReader

*新增功能：读取一行![image-20220922091957883](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922091957883.png)*



BufferedWriter

*新增功能：换行*

![image-20220922091529805](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922091529805.png)

```java
Writer fw = new FileWriter("data.txt");
BufferedWriter bw = new BufferedWriter(fw);//把低级流放入缓冲流
bw.write('a');
bw.newLine();//换行
bw.('b');
```





### 转换流

**问题引出：**不同编码读取存在乱码问题



**字符输入转换流：**InputStreamReader，可以把原始字节流按照**指定编码**转换成字符输入流。

构造器：`InputStreamReader(InputStream in, String charsetName)` 

可以解决字符流读取不同编码乱码的问题。



**字符输出转换流：**OutputStreamWriter，把原始的字节流按照**指定编码**转成字符输出流。







### 序列化

把内存中的对象存储到磁盘中，称为对象序列化。

对象如果要序列化，必须实现Serializable接口。



**对象字节输出流：**ObjectOutputStream

```java
FileOutputStream fos = new FileOutputStream("t.tmp");
//使用对象字节输出流包装低级管道
ObjectOutputStream oos = new ObjectOutputStream(fos);
oos.writeInt(12345);
oos.writeObject("Today");
oos.writeObject(new Date());
oos.close();
```



**对象字节输入流：**ObjectInputStream

```java
FileInputStream fis = new FileInputStream("t.tmp");
//使用对象字节输入流包装低级管道
ObjectInputStream ois = new ObjectInputStream(fis);
//调用对象字节输入流的反序列化方法
int i = ois.readInt();
String today = (String) ois.readObject();
Date date = (Date) ois.readObject();
//关闭流
ois.close();
```

**注意：**transient修饰的成员变量不参与序列化。

序列化的版本号与反序列化的版本号必须一致，才不会出错。





### 打印流

打印流可以实现方便，高效的打印数据到文件中。

**PrintStream**

![image-20220922100657366](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922100657366.png)

```java
//创建一个打印流对象
PrintStream ps = new PrintStream(data.txt);
ps.println(97);
ps.println('a');
ps.println(23.3);
ps.println(true);
ps.println("我是打印流，写什么类型就打印什么类型");
ps.flush();
ps.close();
```



**PrintWriter**

打印功能上与PrintStream没有区别。

PrintStream继承自字节输出流OutputStream，支持写字节数据的方法。

PrintWriter继承自字符输出流Writer，支持写字符数据出去。





## 线程

**线程(thread)**是一个程序内部的一条执行路径。



### 创建方式

**方式一：**继承Thread类

```java
//1.定义一个类继承Thread类
public class MyThread extends Thread{
    //2.重写run方法，里面定义线程以后的执行内容
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程执行输出"+i);
        }
    }
}

public class ThreadDemo1 {
    public static void main(String[] args) {
        //3.new一个新线程对象
        Thread t = new MyThread();
        //调用start方法启动线程(执行的还是run方法)
        t.start();

        for (int i = 0; i < 5; i++) {
            System.out.println("主线程执行输出"+i);
        }
    }
}
```

**注意：**

**为什么用start()不用run？**

因为直接用run会被当成普通的方法执行，而不会被当成线程。

**主线程代码不要放在子线程前面？**

因为这样主线程会直接先跑完，相当于是一个单线程的效果了。

**方式一优缺点：**

优点：编码简单

缺点：已经继承了Thread类，不能再继承别的类了





**方式二：**实现Runnable接口

1. 定义线程任务类MyRunnable实现Runnable接口，重写run()方法
2. 创建MyRunnable任务对象
3. 把MyRunnable任务对象交给Thread处理
4. 调用线程对象start()方法启动线程

![image-20220922105713808](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922105713808.png)

```java
//1.定义一个线程任务类，实现Runnable接口
public class MyRunnable implements Runnable{
    //2.重写run方法，定义线程任务
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println("子线程执行"+i);
        }
    }
}

public class ThreadDemo02 {
    public static void main(String[] args) {
        //3.创建一个任务对象
        Runnable target = new MyRunnable();
        //4.把任务对象交给Thread处理
        Thread t = new Thread(target);
        //5.启动线程
        t.start();
    }
}
```

**使用匿名内部类简化写法：**

```java
public class ThreadDemo02 {
    public static void main(String[] args) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 10; i++) {
                    System.out.println("子线程执行输出"+i);
                }
            }
        }).start();
    }
}

//lambda表达式
new Thread(() -> {
    for (int i = 0; i < 10; i++) {
    	System.out.println("子线程执行输出"+i);
}).start();
```



**方式二优缺点：**

优点：可以继续继承别的类和实现接口，扩展性强

缺点：编程多一层对象包装，如果线程有执行结果是不可以直接返回的。





**方式三：**利用Callable，FutureTask接口实现

前两种方式都存在一个问题：

他们重写的run方法均不能直接返回结果



1. 得到任务对象

- 定义类实现Callable接口，重写call方法，封装要做的事情。
- 用FutureTask把Callable对象封装成线程任务对象。

2. 把线程任务对象交给Thread处理
3. 调用Thread的start方法启动线程
4. 线程执行完毕后，通过FutureTask的get方法获取执行结果

![image-20220922115119046](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922115119046.png)



### 常用方法

getName，setName，currentThread，sleep

![image-20220922120024734](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922120024734.png)

![image-20220922120253313](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922120253313.png)





### 线程同步

多个线程同时操作同一个公共资源且修改资源的时候，可能出现业务安全问题。



**线程同步的核心思想**

加锁，把共享资源进行上锁，每次只能一个线程进入访问完毕以后解锁，然后其他线程才能进来。

```java
synchronized(同步锁对象){
操作共享资源的代码(核心代码)
	}
```

**锁对象要求：**理论上，锁对象只要对于当前同步执行的线程来说是同一个对象即可。



![image-20220922125304654](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922125304654.png)

**只能用锁对象进行调用！！**





### 线程池

线程池是一个可以复用线程的技术。

创建新线程的开销很大，会严重影响系统的性能。



**线程池实现的API**

ExecutorService接口，代表了线程池

使用ExecutorService的实现类ThreadPoolExecutor自创建一个线程池对象

```java
ThreadPoolExecutor(int corePoolSize,//指定线程池的线程数量(核心线程)，不能小于0
                   int maximumPoolSize,//指定线程池可支持的最大线程数
                   long keepAliveTime,//指定临时线程的最大存活时间，不能小于0
                   TimeUnit unit,//指定存活时间的单位
                   BlockingQueue<Runnable> workQueue,//指定任务队列
                   ThreadFactory threadFactory,//指定用哪个线程工厂
                   RejectedExecutionHandler handler//指定任务满了新任务怎么办
                  )
```



**临时线程创建时机？**

新任务提交时发现核心线程在忙，任务队列也满了，并且可以创建临时线程，此时才会创建临时线程。

**什么时候开始拒绝任务？**

核心线程和临时线程在忙，任务队列也满了，新的任务过来才会拒绝。

```java
ExecutorService pool = 
    new ThreadPoolExecutor(3,5,6,
                           TimeUnit.SECONDS,
                           new ArrayBlockingQueue<>(5),
                           Executors.defaultThreadFactory(),
                           new ThreadPoolExecutor.AbortPolicy());
pool.execute(target);
```





### 定时器

**Timer定时器**

![image-20220922142413435](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922142413435.png)

**Timer的问题：**

- Timer是单线程，处理多个任务按照顺序执行，存在延时与设置定时器的时间有出入。
- 可能因为其中某个任务的异常使Timer线程死掉，从而影响后续任务执行。





**ScheduledExecutorService定时器**

为了弥补Timer的缺陷。

```java
ScheduledExecutorService pool = Executors.newScheduledThreadPool(3);
//开启定时任务
pool.scheduleAtFixedRate(new TimerTask() {
	@Override
	public void run() {
    	System.out.println(Thread.currentThread().getName()+"执行输出");
	}
},0,2000,TimeUnit.SECONDS);
```

基于线程池，某个任务的执行情况不会影响其他定时任务的执行。



### 生命周期

Java线程的6种状态

![image-20220922144803450](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220922144803450.png)





































# MySQL

**关系型数据库：**

简单说，关系型数据库是由多张能**互相连接**的二维表组成的数据库。

**SQL：** Structured Query Language，结构化查询语言，一门操作关系型数据库的编程语言。

1.SQL语句可以单行或多行书写，以分号结尾

2.MySQL数据库的SQL语句不区分大小写，关键字建议大写

3.单行注释：**#**，多行注释：**/* */**



**SQL分类：**

- **DDL**(Data Definition Language):数据定义语言，来**定义数据库对象**：数据库，表，列
- **DML**(Data Manipulation Language):数据操作语言，用来**增删改**。
- **DQL**(Data Query Language):数据查询语言，用来**查询数据**。
- **DCL**(Data Control Language):数据控制语言，定义访问权限和安全级别，及创建用户



## DDL

**DDL操作数据库：**

**1.查询**

```sql
show databases;
```

**2.创建数据库db1**

```sql
create database if not exists db1;
```

**3.删除数据库db1**

```sql
drop database if exists db1;
```

**4.使用数据库db1**

```sql
use db1;
```

5.查询当前使用的数据库

```sql
select database();
```



**DDL操作表：**

- 创建(**C**reate)
- 查询(**R**etrieve)
- 修改(**U**pdate)
- 删除(**D**elete)



**创建：**

```sql
#创建表
CREATE TABLE 表名(
    字段名1 数据类型1,
    字段名2 数据类型2,
    ...
    字段名n 数据类型n
);
#创建表
CREATE TABLE tb_user(
    id int,
    username varchar(20),
    password varchar(32)
);
```

**查询：**

```sql
#查询当前数据库的表
SHOW TABLES;
#查询表结构
DESC 表名称;
```

**修改：**

```sql
```



# JavaWeb

Client/Server 客户端-服务器

Browser/Server 浏览器-服务器



**B/S结构系统优点和缺点：**

**优点：**

成本低，升级方便。

不需要安装特定的客户端软件。

**缺点：**

速度慢（服务器的负载比较高），体验差

不安全。。。



### BS通信原理

**IP地址：**相当于计算机在网络中的身份证号，在同一个网络中，IP地址是唯一的。

**端口号：**一个端口代表一个软件，在同一台计算机上，端口号具有唯一性。

**URL：**统一资源定位符



### 对象生命周期

Servlet对象由WEB容器(Tomcat)管理。WEB容器创建的Servlet对象会被放到一个Hashmap集合中。

服务器启动的时候，Servlet对象并不会被实例化。



**Servlet对象生命周期：**

Servlet**无参构造方法**执行。

Servlet对象的**init方法**执行。

Servlet对象的**service方法**执行。

Servlet对象的**destroy方法**执行。

























































































# Linux



## 常用命令

**磁盘管理：**

cd	切换目录

ll		以列表形式展示当前目录下的文件以及文件夹

ls		列出当前目录下目录以及文件

ll -a	以列表形式展示当前目录下的文件及文件夹和隐藏的文件

ls -a	列出当前目录下目录及文件和隐藏文件

dir		列出当前目录下的目录及文件

mkdir	创建文件夹

clear	滚屏

Ctrl+C	退出操作



**文件管理：**

mv	文件重命名或将文件移动到指定目录

rm	删除文件(不能删除文件夹)

rm -rf	删除文件或目录

touch	创建空文件或将文件的最后修改时间改为当前时间

cat		将整个文件内容输出到控制台(只能查看,不可编辑)



## 文件权限

<img src="C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220614102631733.png" alt="image-20220614102631733" style="zoom: 50%;" />

-rw-rw-rw  -表示文件
lrwxrwxrwx. l表示软链接（快捷方式）  
drw-rw-rw  d表示目录文件夹
srw-rw-rw  s表示socket套接字文件
brw-rw-rw  Block device块设备
crw-rw-rw  Character Device字符设备

(s、b、c一般在dev目录才有)

r--read 读权限 4
w--write 写权限 2
x--execute 执行权限 1
rwx=7
常见644、755、777三种权限



## 文档处理

grep	文本搜索	cat 文件名 | grep 过滤内容
sort	排序，cat aa.log |  sort
uniq	去除相邻重复的行，cat aa.log |  sort | uniq
wc	cat aa.log | wc 依次输出 行数、单词数、字符数

/关键字：按斜杠/键，可以输入想搜索的字符



## 防火墙操作命令

**查看防火墙状态：**systemctl status firewalld
让防火墙可用：systemctl anable firewalld
让防火墙不可用：systemctl disable firewalld
启动防火墙：systemctl start firewalld
**关闭防火墙：**systemctl stop firewalld













































# Spring

以**IoC**和**AOP**为内核的分层的JavaSE/EE应用轻量级框架。



## 配置文件



**1.Bean标签基本配置**

用于配置对象交由**Spring**来创建。

默认情况下它调用的是类的**无参构造**，没有无参构造则不能创建。

```xml
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"></bean>
```

**基本属性：**

id：Bean实例在Spring容器中的唯一标识

class：Bean的全限定名称

scope：指对象的作用范围，取值如下：

![image-20220926162153767](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220926162153767.png)

**当scope取值为singleton时：**

Bean的实例化个数：1

Bean的实例化机时机：Spring核心文件被加载时，Bean被实例化。

Bean的生命周期：

- 对象创建：当应用加载，创建容器时，对象就被创建了
- 对象运行：只要容器在，对象一直活着
- 对象销毁：当应用卸载，销毁容器时，对象就销毁了



**当scope取值为prototype时：**

Bean的实例化个数：多个

Bean的实例化机时机：当调用getBean()方法时实例化Bean

Bean的生命周期：

- 对象创建：使用对象时，创建新的对象实例
- 对象运行：只要容器在，对象一直活着
- 对象销毁：对象长时间不用，垃圾回收器回收



![image-20220926170856711](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220926170856711.png)



## 相关API



**ApplicationContext的实现类：**

1. ClasspathXmlApplicationContext：从类的根路径下加载配置文件（推荐）
2. FileSystemXmlApplicationContext：从磁盘路径加载
3. AnnotationConfigApplicationContext：当使用注解配置容器对象时，使用它创建容器。



**getBean()方法：**

参数可以写id名，也可以写类型。

getBean("id名");         getBean(xxx.class)





## 配置数据源

**1.数据源(连接池)的作用：**

为了提高程序性能。

- 需要实现实例化数据源，初始化部分连接资源
- 使用连接资源时从数据源中获取
- 使用完毕后归还数据源



常见的数据源：DBCP,C3P0,BoneCP，Druid



**2.抽取配置文件：**

把数据源的配置信息写到jdbc.properties里面。

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test
jdbc.username=root
jdbc.password=123456
```

可以将数据源的初始化交给Spring容器

```xml
<!--加载外部的properties文件-->
<context:property-placeholder location="classpath:jdbc.properties"/>

<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="${jdbc.driver}"/>
    <property name="jdbcUrl" value="${jdbc.url}"/>
    <property name="user" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>
```





## 注解开发

ApplicationContext.xml：

```xml
<!--配置组件扫描-->
<context:component-scan base-package="com.itheima"/>
```

扫描这些包，解析里面的**注解**，使注解生效

**@Component：**使用在类上实例化Bean             @Controller @Service @Repository

![image-20220926175232606](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220926175232606.png)



**Spring新注解：**

![image-20220926223844603](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220926223844603.png)



**纯注解开发：**

主配置类(SpringConfiguration)：

```java
@Configuration //指定当前类是一个Spring配置类
@ComponentScan("com.itheima") //扫描包
@Import({DataSourceConfiguration.class}) //导入其他的子配置类，比如数据源配置类
public class SpringConfiguration {
}
```

数据源配置类(DataSourceConfiguration)：

```java
//把properties文件里的值赋给私有属性。
//初始化数据源对象时再调用这些属性。
@PropertySource("classpath:jdbc.properties") 
public class DataSourceConfiguration {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;
    @Bean("dataSource")//代表spring会将当前方法的返回值以指定名称存储到spring容器中
    public DataSource getDataSource() throws PropertyVetoException {
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setDriverClass(driver);
        dataSource.setJdbcUrl(url);
        dataSource.setUser(username);
        dataSource.setPassword(password);
        return dataSource;
    }
}
```





## AOP

Aspect Oriented Programming，意为面向切面编程，是通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。

AOP可以对**业务逻辑**的各个部分进行**隔离**，从而使得业务逻辑各部分之间的**耦合度降低**，提高程序的复用性。



**举例理解AOP：**比如现在要实现CRUD四个功能，每个功能我都想对其进行增强，即加入日志。那么如果用常规办法，只能在CRUD四个业务里都分别加入日志功能。

而如果使用AOP面向切面编程，只需要编写一次日志功能，通过配置的方式连接日志功能代码和CRUD代码，要使用的时候直接把切面拿来用，这样本来要写四次的日志功能，现在只需要写一次。



**AOP底层实现**

在运行期间，Spring通过动态代理技术动态的生成代理对象，代理对象方法执行时进行增强功能的介入，在去调用目标对象的方法，从而完成功能的增强。



**常用的动态代理技术**

- JDK代理：基于接口的动态代理技术
- cglib代理：基于父类的动态代理技术

![image-20220927033735213](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220927033735213.png)



**AOP相关概念**

**Target(目标对象)：**代理的目标对象

**Proxy(代理)：**一个类被AOP织入增强后，就产生一个结果代理类

**Jointpoint(连接点)：**这些点指的是方法，因为spring只支持方法的连接点

**Pointcut(切入点)：**指要对哪些Jointpoint进行增强

**Advice(通知/增强)：**拦截到Jointpoint之后要做的事情就是Advice

**Aspect(切面)：**是切入点和通知的结合





## **aspectj**

spring集成了第三方aspectj的配置。

```xml
<!--目标对象-->
<bean id="target" class="com.itheima.aop.Target"></bean>
<!--切面对象-->
<bean id="myAspect" class="com.itheima.aop.MyAspect"></bean>
<!--配置织入,告诉框架哪些方法需要哪些增强-->
<aop:config>
    <!--声明切面-->
    <aop:aspect ref="myAspect">
        <!--切面=切点+通知-->
        <aop:before method="before" pointcut="execution(public void 	 com.itheima.aop.Target.save())"></aop:before>
    </aop:aspect>
</aop:config>
```



**切点表达式**

表达式语法：

```xml
execution([修饰符] 返回值类型 包名.类名.方法名(参数))
```

- 访问修饰符可以忽略
- 返回值类型，包名，类名方法名可以用*代表任意
- 参数列表可以用..表示任意个参数

修改上面的切入点表达式：

```xml
<aop:before method="before pointcut="execution(* com.itheima.*.*(..))"/>
```



**通知的类型**

通知的配置语法：

```xml
<aop:通知类型 method="切面类方法名" pointcut="切点表达式"></aop:通知类型>
```

![image-20220927171029455](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220927171029455.png)





**基于注解开发AOP：**

通知的配置语法：@通知注解("切点表达式")



注解aop开发步骤：

1. 使用@Aspect标注切面类
2. 使用@通知注解标注通知方法
3. 在配置文件中配置aop自动代理`<aop:aspectj-autoproxy/>`





## 事务控制

编程式事务控制相关对象：

**1.PlatformTransactionManager**

此接口是spring的事务管理器，不同Dao层技术有不同的实现类。



**2.TransactionDefinition**

TransactionDefinition是事务的定义信息对象，有如下方法：

![image-20220927173414305](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220927173414305.png)



**事务隔离级别：**

- ISOLATION_DEFAULT                             默认
- ISOLATION_READ_UNCOMMITTED     读未提交
- ISOLATION_READ_COMMITTED           读已提交
- ISOLATION_REPEATABLE_READ           可重复读 
- ISOLATION_SERIALIZABLE                    



**事务传播行为：**

REQUIRED：如果当前没有事务，就新建一个事务，如果已经存在一个事务，就加入到这个事务

SUPPORTED：支持当前事务，如果当前没有事务就以非事务方式执行(没有事务)



**3.TransactionStatus**

事务的状态信息





**声明式事务**

采用声明的方式来处理事务，就是指在配置文件中声明，用声明来代替代码式的处理事务。

事务管理不侵入开发的组件，因为它属于系统层面的服务，不是业务逻辑的一部分。

在不需要事务管理时，只要在设定文件上修改，即可移去事务管理，**不用改代码**重新编译。



**xml的方式：**

```xml
<!--目标对象 内部的方法就是切点-->
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
    </bean>
<!--配置平台事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.dataSource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--通知 事务的增强-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--设置事务的属性信息-->
        <tx:attributes>
            <tx:method name="transfer" isolation="REPEATABLE_READ" propogation="REQUIRED" read-only="false"/>
            <tx:method name="save" isolation="REPEATABLE_READ" propogation="REQUIRED" read-only="false"/>
            <tx:method name="findAll" isolation="REPEATABLE_READ" propogation="REQUIRED" read-only="true"/>
            <tx:method name="update*" isolation="REPEATABLE_READ" propogation="REQUIRED" read-only="false"/>
        </tx:attributes>
    </tx:advice>

    <!--配置事务的aop织入-->
    <aop:config>
        <aop:advisor advice-ref="txAdvice" poincut="execution(* com.itheima.service.impl.*.*(..))"/>
    </aop:config>
```

**总结：**

- 平台事务管理器配置
- 事务通知的配置
- 事务aop织入的配置



**注解的声明式事务控制：**

自定义的bean用注解，非自定义的用xml，因为非自定义的拿不到源码

使用方式：在要控制的方法上加上**@Transactional**注解

```java
@Transactional(isolation=Isolation.READ_COMMITTED,propagation=Propagation.REQUIRED)
public void transfer(String outMan, String inMan, double money){
    accountDao.out(outMan,money);
    accountDao.in(inMan,money);
}
```

在xml文件上加入**事务的注解驱动**：

```xml
<tx:annotation-driven transaction-manager="transactionManager"/>
```

**知识要点：**

- 平台事务管理器(xml配置)
- 事务通知的配置(@Transactional注解配置在方法上)
- 事务注解驱动`<tx:annotation-driven/>`









# SpringMVC

SpringMVC封装了Servlet的共有行为。

![image-20220926235215423](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220926235215423.png)



## 组件解析

**执行流程：**

![image-20220926235633271](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220926235633271.png)



**注解解析：**

@RequestMapping：

**作用：**用于建立**请求URL**和**处理请求方法**之间的对应关系。

**属性：**

- value：用于指定请求的URL
- method：用于指定请求的方式
- params：用于指定限制请求参数的条件，支持简单的表达式

​	例如：

​	params={"accountName"}，表示请求参数必须有accountName

​	params={"money!100"}，表示请求参数中money不能是100





## 数据响应

**1.页面跳转**

- **直接返回字符串**

![image-20220927002046431](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220927002046431.png)

- **通过ModelAndView对象返回**

Model代表数据模型，View代表视图

**2.回写数据**

- **直接返回字符串**

Web阶段，客户端访问服务器端，如果想直接回写字符串作为响应体返回的话：

response.getWriter().print("hello world!")即可，那在**Controller中怎么回写？**

通过**@ResponseBody**直接告知SpringMVC，方法返回的字符串不是跳转，而是直接在http响应体返回。此注解加在controller方法上。

如果想直接返回**json格式**的字符串：需要导入**jackson**转换工具。

```java
@RequestMapping("/quick9")
@ResponseBody
public String save() throws IOException{
    User user = new User();
    user.setUsername("lisi");
    user.setAge(25);
    //使用json的转换工具将对象转换成json格式的字符串再返回
    ObjectMapper objectMapper = new ObjectMapper();
    String json = objectMapper.writeValueAsString(user);
    return json;
}
```

- **返回对象或集合**

在方法上添加@ResponseBody就可以返回json格式的字符串，但是这样配置比较麻烦。

因此，可以使用mvc的注解驱动代替上述配置：

```xml
<!--mvc的注解驱动-->
<mvc:annotation-driven/>
```

在MVC中，**处理器映射器**，**处理器适配器**，**视图解析器**称为SpringMVC三大组件。

<mvc:annotation-driven/>**自动加载**RequestMappingHandlerMapping(处理映射器)和RequestMappingHandlerAdapter(处理适配器)，同时自动集成jackson进行对象或集合的json格式字符串的转换。





## 获得请求数据



**1.获得请求参数**

客户端请参数的格式是：name=value&name=value...

服务器端要获得请求的参数，有时还需要进行数据的封装，SpringMVC可以接收如下类型参数：

- 基本类型参数
- POJO类型参数
- 数组类型参数
- 集合类型参数



**2.获得基本类型参数**

Controller中的业务**方法的参数名**要和**请求参数的name**一致，**参数值会自动映射匹配**

http://localhost:8080/user/quick11?username=zhangsan&age=18

```java
@RequestMapping("/quick11")
@ResponseBody
public void save(String username,int age) throws IOException{
    System.out.println(username);
    System.out.println(age);
}
```



**3.获得POJO类型参数**

Controller中的业务方法的**POJO属性名**要和**请求参数的name**一致，**参数值会自动映射匹配**

http://localhost:8080/user/quick10?username=zhangsan&age=18

![image-20220927012655113](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220927012655113.png)

```java
public class User{
    private String username;
    private int age;
    gtter/setter...
}
@RequestMapping("/quick10")
@ResponseBody
public void quickMethod(User user) throws IOException{
    System.out.println(user);
}
```



**4.获得数组类型参数**

Controller中的业务方法的**数组名称**要和**请求参数的name**一致，**参数值会自动映射匹配**

```java
@RequestMapping("/quick10")
@ResponseBody
public void quickMethod(String[] strs) throws IOException{
    System.out.println(Arrays.asList(strs));//数组作为集合打印
}
```



**5.获得集合类型参数**

```java
@RequestMapping("/quick14")
@ResponseBody
public void save14(@RequestBody List<User> userList) throws IOException{
    System.out.println(userList);
}
```



**6.请求数据乱码问题**

配置全局过滤的filter



**7.参数绑定注解@RequestRaram**

当请求的参数名称和Controller的业务方法参数名**不一致**时，需要用@RequestParam.

**value：**请求参数名称

**required：**指定的请求参数是否必须包括，默认是true，提交时没有此参数会报错

**defaultValue：**当没有指定请求参数时，则使用指定的默认值赋值

```java
@RequestMapping("/quick15")
@ResponseBody
public void quickMethod(@RequestParam("name") String username){
    System.out.println(username);
}
```



**8.获取Restful风格的参数**

Restful请求风格是使用url+请求方式表示一次请求目的，HTTP协议里四个操作方式的动词：

- GET：用于获取资源
- POST：用于新建资源
- PUT：用于更新资源
- DELETE：用于删除资源

```java
//localhost:8080/user/quick17/zhangsan
@RequestMapping(value="/quick17/{username}")//解析地址中的zhangsan放入到里面
@ResponseBody
public void save(@PathVaraible(value="username") String username){
    System.out.println(username);
}
```



**9.自定义类型转换器**

SpringMVC默认已经提供了一些常用的类型转换器，例如客户端里的字符串转换成int类型进行参数设置。

但不是所有的数据类型都提供了，需要自定义。例如：日期类型的数据就需要自定义转换器。

1. 自定义转换器类实现Converter接口
2. 在配置文件中声明转换器
3. 在<annotation-driven>中引用转换器



**10.获得Servlet相关API**

可以在方法的形参里声明常用的对象：

- HttpServletRequest
- HttpServletResponse
- HttpSession

因为写在形参里的对象会被自动注入到容器，不用自己实例化对象。





## 异常处理

**1.异常处理思路**

系统的Dao，Service，Controller出现都通过throws Exception向上抛出，最后交由SpringMVC前端控制器交给异常处理器进行异常处理。

![image-20220927030759447](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220927030759447.png)



**2.异常的处理方式**

- 使用SpringMVC提供的简单异常处理器SimpleMappingExceptionResolver
- 实现Spring的异常处理接口HandlerExceptionResolver自定义自己的异常处理器

自定义异常处理：

- 创建异常处理器类实现HandlerExceptionResolver接口
- 配置异常处理器
- 编写异常页面
- 测试异常跳转



























































# MyBatis

**xxxMapper.xml**，这个文件专门写sql语句的配置文件(一个表一个)



**SqlSession**是专门执行SQL语句的，是一个java程序和数据库之间的一次会话。

需要SqlSessionFactory工厂生产SqlSession对象。

一个sqlSessionFactory对象对应一个environment(数据库)





## 核心配置文件

**MyBatis核心配置文件层级关系：**

![image-20220928000451326](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220928000451326.png)

**1.properties标签：**

可以提取数据库的配置信息到一个单独的jdbc.properties文件中

**3.typeAliases标签：**

类型别名是为Java类型设置一个短的名字。

![image-20220928001637607](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220928001637607.png)

mybatis框架已经提前设置好一些**常用的类型别名**，比如包装类可以直接写成简单类型类的写法

例如：Integer ->int



**dataSource标签：**

数据源标签，就是数据库连接池

属性：

- **POOLED：**使用mybatis自己实现的数据库连接池
- **UNPOOLED ：**不使用数据库连接池，每一次请求过来，都是创建新的Connection对象
- **JNDI：**集成其他第三方的数据库连接池

```xml
<!--mybatis-config.xml配置环境-->
<environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <!--数据源dataSource(数据库连接池)-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url"
                          value="jdbc:mysql://localhost:3308/ssm"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
```





### 添加日志组件

**日志输出级别：**

logback日志级别包括五个：TRACE < DEBUG < INFO < WARN <ERROR

如果用logback就不用配置settings，但是必须写logback.xml配置文件。（主要配置日志格式，日志级别）

**mybatis-config.xml具体配置**

```xml
<!--settings标签-->
<settings>
    <!--mybatis标准日志实现-->
    <setting name="logImpl" value="STDOUT_LOGGING"></setting>
</settings>
```





### 事务管理器

在mybatis-config.xml文件中，可以配置**事务管理**。

<transactionManager tpye = "JDBC"/>

type属性有两个值：**MANAGED，JDBC**

JDBC事务管理器：mybatis框架自己管理，采用原生的JDBC代码去管理

MANAGED：事务交给其他容器管理，mybatis不再负责







## 映射文件

xxxMapper.xml映射文件

**namespace属性：**

在xxxMapper.xml文件中有一个namespace，这个属性是用来指定命名空间的，用来防止id重复。



**插入操作：**

- **paramaterType**属性指定要插入的数据类型
- Sql语句中使用**#{实体属性名}**方式引用实体中的属性值
- 插入操作使用的API是**sqlSession.insert**("命名空间.id"，实体对象);
- 插入操作涉及数据库变化，要使用sqlSession对象显示的**提交事务**，**sqlSession.commit()**

```xml
<!--插入操作-->
<insert id="save" parameterType="com.itheima.pojo.User">
        insert into t_user values(#{id},#{username},#{password})
</insert>
```

```xml
<!--修改操作-->
<update id="update" parameterType="com.itheima.pojo.User">
    update t_user set username = #{username}, password= #{password}
    where
    id = #{id}
</update>
```

**删除操作：**

Sql语句中使用#{任意字符串}方式引用传递的单个参数

删除操作使用的是**sqlSession.delete**("命名空间.id"，Object);

```xml
<!--删除操作-->
<delete id="delete" parameterType="java.lang.Integer">
    delete from t_user where id=#{id}
</delete>
```

**查询操作：**

List<User> userList = sqlSession.selectList("userMapper.findAll");

```xml
<select id="findAll" resultType="com.itheima.pojo.User">
    select * from t_user
</select>
```



















## 占位符传值

在mybatis中，#{}是占位符。



1. 使用**Map集合**可以给sql语句的占位符传值：

占位符里写Map集合的key

```xml
<insert id="insertCar">
    insert into t_car(id,car_num,brand,guide_price,produce_time)
    values(null,#{carNum},#{brand},#{guidePrice},#{produceTime});
</insert>
```

2. 使用**pojo类**给sql语句的占位符传值：

占位符里写pojo类的属性名，(严格来说写的是pojo类的get方法的方法名)



**update操作：**

```xml
<update id="updateById">
    update t_car 
    set car_num=#{carNum}, brand=#{brand},guide_price=#					 	 {guidePrice},produce_time=#{produceTime}
    where
    id = #{id};
</update>
```

**select操作(查一个)：**selectOne方法

```xml
<select id="selectById" resultType="com.powernode.mybatis.pojo.Car">
    select * from t_car where id = #{id}
</select>
```

mybatis底层执行了select语句之后，返回一个结果集，封装java对象。

这里需要指定结果集封装成什么类型的java对象。

注意：数据库中的**列名**要和pojo类中的**属性值**匹配，否则查不到数据。



**select操作(查所有)：**selectList方法

```xml
<select id="selectAll" resultype="com.powernode.mybatis.pojo.Car">
    select id,car_num,brand,guide_price,produce time,car_type
    from
    t_car
</select>
```

注意：resultType还是指定要封装的结果集的类型，不是指定List类型。

因为是多个数据，所以用List集合接数据。





























# SpringBoot



## Maven进阶

**pom**是项目对象模型(Project Object Module)，该文件可以被继承。

maven多模块管理其实就是让子模块的pom文件继承父工程的pom文件。



**第一种方式：**

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



**第二种方式：**

把子工程的位置放到父工程文件里面。

其余操作和第一种一模一样。



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





**过滤器：**

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







# RabbitMQ

**什么是消息队列：**

消息队列是一种应用间的通信方式，消息发送后可以立即返回，由消息系统来确保消息的可靠传递。**发布者**只管把消息发布到MQ中而**不用管谁来取**，消息**使用者**只管从MQ中取出消息而**不管发布者是谁**。



**为什么使用消息队列：**

业务解耦，最终一致性，广播，错峰流控等等。



### 基本操作

**1.启动RabbitMQ：**

rabbitmq-server start &

**2.停止服务：**

rabbitmqctl stop

**3.RabbitMQ管控台地址：**

http://192.168.60.133:15672





### MQ机制

所有MQ产品从模型抽象上来说都是一样的过程：

消费者(consumer)订阅每个队列。生产者(producer)创建消息，然后发布到队列(queue)中，最后将消息发送到监听的消费者。

![image-20220907212249081](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220907212249081.png)

**1.Message**

消息，消息是不具体的，它由消息头和消息体组成。消息体是不透明的，而消息头则由一系列的可选属性组成，这些属性包括routing-key（路由键）、priority（相对于其他消息的优先权）、delivery-mode（指出该消息可能需要持久性存储）等。

**2.Publisher**

消息的生产者，也是一个向交换器发布消息的客户端应用程序。 

**3.Exchange**

交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。 

**4.Binding**

绑定，用于消息队列和交换器之间的关联。一个绑定就是基于路由键将交换器和消息队列连接起来的路由规则，所以可以将交换器理解成一个由绑定构成的路由表。

**5.Queue**

消息队列，用来保存消息直到发送给消费者。它是消息的容器，也是消息的终点。一个消息可投入一个或多个队列。消息一直在队列里面，等待消费者连接到这个队列将其取走。 

**6.Consumer**

消息的消费者，表示一个从消息队列中取得消息的客户端应用程序。

**7.Broker**

表示消息队列服务器实体。



**AMQP 中的消息路由：**

生产者把消息发布到 Exchange 上，消息最终到达队列并被消费者接收，而 Binding 决定交换器的消息应该发送到那个队列

![image-20220907213715720](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220907213715720.png)



### Direct交换机

要求消息需要携带RoutingKey(路由键)，交换机接收到消息后，会读取路由表(Bindings)，如果消息的RoutingKey和路由表中的BindingKey完全一致则将消息转发到对应的队列中。

![image-20220907223040830](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220907223040830.png)





### Fanout交换机

1.Fanout交换机速度是最快的，因此常常用它替换其他的交换机。

2.可以利用Fanout模拟广播模式实现消息推送



### Topic交换机

要求消息需要携带RoutingKey，在路由表中的BindingKey可以使用通配符*或#。

注意：

1. *表示必须要匹配一个任意的单词
2. #表示可以匹配0个或任意个任意的单词
3. RoutingKey和BindingKey中的多个单词需要使用.进行分割。例如aa.bb或aa.*

![image-20220907224132532](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220907224132532.png)





### 消息发送

```java
```















