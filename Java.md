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



**2.增强for**

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



**3.lambda表达式-forEach**

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
- 如果某一个节点是红色的，那么它的子节点必须是黑色的。
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



## SQL语法



### DDL

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

**5.查询当前使用的数据库**

```sql
select database();
```



**DDL操作表：**



**创建表：**

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

**删除表：**

```sql
#删除表
DROP TABLE if exists 表名;
#删除指定表，并重新创建该表
TRUNCATE TABLE 表名;
```

**查询表：**

```sql
#查询当前数据库的表
SHOW TABLES;
#查询表结构
DESC 表名称;
```

**添加字段：**

```sql
#添加字段
ALERT TABLE 表名 ADD 字段名 类型(长度)[COMMENT][约束]
#例：
ALERT TABLE emp ADD nickname varchar(255) '昵称';
```

**修改字段：**

```sql
#删除字段
ALERT TABLE 表名 DROP 字段名;
ALERT TABLE emp DROP username;
```

**修改表名：**

```sql
ALTER TABLE emp RENAME TO employee;
```



### DML

Data Manipulation Language：INSERT UPDATE DELETE

**添加数据：**

```sql
#给指定字段赋值
INSERT INTO 表名(字段1，字段2，...) VALUES(值1，值2,...);
#给所有字段赋值
INSERT INTO 表名 VALUES(值1，值2,...);
```

**更新数据：**

```sql
UPDATE 表名 SET 字段名1=值1,字段名2=值2,... WHERE 条件;
```

**删除数据：**

```sql
DELETE FROM 表名 WHERE 条件;
```





### DQL

**语法：**

![image-20221003105625680](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221003105625680.png)

```sql
#1.查询指定字段 name,workno,age
SELECT name,workno,age FROM emp;
#2.字段起别名
SELECT workaddress as '工作地址' FROM emp;
#3.去除重复查询DISTINCT
SELECT DISTINCT workaddress FROM emp;
```



**条件查询where：**

<>： 不等于

**LIKE** 占位符 

一个_代表一个字符 

%代表任意个字符

```sql
#查询姓名为两个字的员工 _ %
SELECT * FROM emp WHERE name LIKE '__';
#查询身份证号最后一位是X的员工信息
SELECT * FROM emp WHERE idcard LIKE '%X';
```



**聚合函数：**

count，max，min，avg，sum

语法： SELECT 聚合函数(字段列表) FROM 表名;

注意：**null**值不参与聚合函数运算



**分组查询：**

where和having的区别：

执行时机不同：where是分组前过滤，而having是对分组后的数据进行过滤

判断条件不同：where不能对聚合函数进行判断，而having可以

```sql
#根据性别分组，统计男女员工的数量
SELECT gender,count(*) FROM emp group by gender;
#根据性别分组，统计男女员工的平均年龄
SELECT gender,avg(age) FROM emp group by gender;
#查询年龄小于45的员工，并根据工作地址进行分组，获取员工数量大于等于3的工作地址
SELECT workaddress,count(*) FROM emp WHERE age<45 GROUP BY workadrress HAVING count(*) >=3;
```



**排序查询：**默认升序

```sql
#根据年龄升序排序
SELECT * FROM emp order by age asc;
```



**分页查询：**

LIMIT 起始索引，查询数





### 函数



**字符串函数：**

**CONCAT**(S1,S2,...Sn)：字符串拼接

**LOWER**(str),**UPPER**(str)：字符串全部换成大(小)写

**LPAD**(str,n,pad) **RPAD**(str,n,pad)：左填充，右填充

**TRIM**(str)：去掉字符串头部和尾部的空格

**SUBSTRING**(str,start,len)：截取字符串



**数值函数：**

ceil：向上取整

floor：向下取整

mod：取模

rand：求0-1的随机数

round：四舍五入



**日期函数：**

![image-20221003201235496](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221003201235496.png)



**流程函数：**

![image-20221004002236976](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221004002236976.png)



### **约束**

![image-20221004002844473](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221004002844473.png)



**外键约束：**

外键用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性。



删除/更新行为：

![image-20221004014001387](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221004014001387.png)





## 多表查询

一对多，两张表，多的表加外键

多对多，三张表，关系表两个外键



**笛卡尔积：**

指的是数学中，A集合和B集合所有的组合情况。

```sql
SELECT * FROM emp,dept where emp.dept_id=dept.id;
```



**多表查询分类：**

![image-20221007065335244](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007065335244.png)



### 内连接查询

内连接查询的是两张表交集的部分。

**隐式内连接：**

```sql
SELECT 字段列表 FROM 表1,表2 WHERE 条件...;
```

**显式内连接：**

```sql
SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 连接条件...;
```





### 外连接查询

**左外连接：**

```sql
SELECT 字段列表 表1 LEFT [OUTER] JOIN 表2 ON 条件...;
```

相当于查询左表的**所有数据**以及和表2交集部分的数据



**右外连接：**

```sql
SELECT 字段列表 表1 RIGHT [OUTER] JOIN 表2 ON 条件...;
#相当于查询右表的所有数据以及和表1交集部分的数据
```

注意：右外连接也可以改成左外连接的写法，左外在实际开发中用的比较多。





### 自连接查询

```sql
SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件...;
```

自连接查询，可以是内连接，也可以是外连接。





### 联合查询

union查询，就是把多次查询的结果合并起来，形成一个新的查询结果集。

```sql
SELECT 字段列表 FROM 表A...
UNION[ALL]
SELECT 字段列表 FROM 表B...;
```

**注意：**

如果把ALL关键字删掉，就可以去重。

多张表的列数必须保持一致。





### 子查询

概念：SQL语句中嵌套SELECT语句，称为嵌套查询，也叫子查询

```sql
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2);
```

根据子查询结果不同，分为：

- 标量子查询(子查询结果为单个值)
- 列子查询(子查询结果为一列)
- 行子查询(子查询结果为一行)
- 表子查询(子查询结果为多行多列)



标量子查询-例子：

查询在"方东白"入职之后的员工信息

a.查询方东白的入职日期

b.查询指定入职日期之后入职的员工信息

```sql
SELECT entrydate FROM emp WHERE name='方东白';
SELECT * FROM emp WHERE entrydate > '某个日期';
#两条合并
SELECT * FROM emp WHERE entrydate > (SELECT entrydate FROM emp WHERE name='方东白');
```



列子查询-例子：

![image-20221007073110970](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007073110970.png)

查询比财务部所有人工资都高的员工信息：

```sql
#a.查询所有财务部的人员工资
SELECT id FROM dept WHERE name='财务部';
SELECT salary FROM emp WHERE dept_id=(SELECT id FROM dept WHERE name='财务部');
#b.比财务部所有人工资都高的员工信息
SELECT * FROM emp WHERE sal > ALL(SELECT salary FROM emp WHERE dept_id=(SELECT id FROM dept WHERE name='财务部'));
```



行子查询-例子：

查询与"张无忌"的薪资及直属领导相同的员工信息？

```sql
#a.查询张无忌的薪资及直属领导
SELECT salary,managerid FROM emp WHERE name='张无忌';
#b.查询薪资和直属领导是...的员工信息
SELECT * FROM emp WHERE (salary,managerid)=(SELECT salary,managerid FROM emp WHERE name='张无忌');
```



表子查询-例子：

查询与"鹿杖客","宋远桥"的职位和薪资相同的员工信息？

```sql
#a.查询"鹿杖客","宋远桥"的职位和薪资
SELECT job,salary FROM emp WHERE name='鹿杖客' or name='宋远桥';
#b.查询与...职位和薪资相同的员工信息
SELECT * FROM emp WHERE (job,salary) IN (SELECT job,salary FROM emp WHERE name='鹿杖客' or name='宋远桥');
```

**注意：**这里因为是两个员工，所以是多行数据，因此不能像上个例子一样使用=，而是使用**in**关键词





## 事务

事务是**一组操作的集合**，它是一个**不可分割**的工作单位。这些操作要么同时成功，要么同时失败。



**转账操作：**

1.查询张三账户余额

```sql
select * from account where name = '张三';
```

2.将张三账户余额-1000

```sql
update account set money=money-1000 where name ='张三';
```

3.将李四账户余额+1000

```sql
update account set money=money+1000 where name = '李四';
```



### **事务操作**

```sql
#查看/设置事务提交方式
SELECT @@autocommit;
SET @@autocommit=0;
#提交事务
COMMIT;
#回滚事务
ROLLBACK;
```

以下这种方式不会改变事务的提交方式：

```sql
#开启事务
START TRANSACTION 或 BEGIN;
#提交事务
COMMIT;
#回滚事务
ROLLBACK;
```





### 四大特性

原子性(**A**tomicity)：事务是不可分割的最小操作单元。

一致性(**C**onsistency)：事务完成时，必须使所有的数据都保持一致状态。

隔离性(**I**solation)：数据库系统提供隔离机制，保证事务不受外部并发操作影响。

持久性(**D**urability)：事务一旦提交或回滚，它对数据库中的改变就是永久的。





### 隔离级别

并发事务问题：

**脏读：**一个事务读到另外一个事务还没有提交的数据。

![image-20221007091538901](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007091538901.png)

**不可重复读：**一个事务先后读取同一条记录，但两次读取的数据不同。

![image-20221007090949027](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007090949027.png)

**幻读：**一个事务按照条件查询数据时，没有对应的数据行，但是在插入操作时，又发现这行数据已经存在。(因为这里解决了不可重复读问题，所以一次事务内查询的的数据是一致的，所以查询不到事务B插入的id=1的数据，但又是真实存在的，因此无法完成第三步的插入操作)

![image-20221007091502053](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007091502053.png)



**事务隔离级别：**

![image-20221007091729458](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007091729458.png)

```sql
#查看事务隔离级别
SELECT @@TRANSACTION_ISOLATION;
#设置事务隔离级别
SET [SESSION|GLOBAL] TRANSACTION ISOLATION LEVEL{
	READ UNCOMMITTED|READ COMMITTED|REPEATABLE READ|SERIALIZABLE
}
```

注意：事务隔离级别越高，数据越安全，但是性能越低







## 存储引擎

存储引擎就是存储数据，建立索引，更新/查询数据等技术的实现方式。

存储引擎是**基于表**的，所以存储引擎也称为表类型。



**InnoDB**存储引擎：

5.5之后的默认存储引擎。兼顾高可靠性和高性能的通用存储引擎。

DML操作遵循ACID模型，支持**事务**；

**行级锁**，提高并发访问性能；

支持**外键**约束，保证数据的完整性和正确性



**MyISAM：**

不支持事务，不支持外键

支持表锁，不支持行级锁

访问速度快



**Memory：**

存储在内存中，hash索引(默认)



**InnoDB和MyISAM的区别？**

- InnoDB支持事务，MyISAM不支持
- InnoDB支持行锁，MyISAM只支持表锁
- InnoDB支持外键，MyISAM不支持外键





## 索引

index是帮助MySQL**高效获取数据**的**数据结构**(有序)。

![image-20221007101344759](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007101344759.png)



**索引的优缺点：**

![image-20221007101517279](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007101517279.png)



### **索引结构**

二叉树缺点：顺序插入时，会形成一个链表，查询性能大大降低。大数据量下，层级较深，检索速度慢。

B-Tree(多路平衡查找树)

**B+Tree**

以一颗最大度数为4的b+tree为例：

所有的数据都会出现在叶子节点

叶子节点形成一个单向链表

![image-20221007103057424](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007103057424.png)



**为什么InnoDB使用B+树？**

**相对于二叉树**，层级更少，搜索效率高。

**相对于B树**，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值减少，指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低。

**相对于Hash索引**，B+Tree支持范围匹配以及排序操作。





### 索引分类

![image-20221007104307634](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007104307634.png)

在InnoDB中，根据索引的存储形式，又可以分为以下两种：

![image-20221007104445108](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007104445108.png)

如果存在主键，主键索引就是聚集索引。

如果不存在主键，将使用第一个唯一索引为聚集索引。

如果表没有主键，或没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引。

![image-20221007105227409](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007105227409.png)

聚集索引叶子节点存储的是行数据。

![image-20221007105303448](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221007105303448.png)

二级索引叶子节点存储的是对应的主键。





### 索引语法

```sql
#创建索引
CREATE [UNIQUE|FULLTEXT] INDEX index_name ON table_name(index_col_name,...);
#查看索引
SHOW INDEX FROM table_name;
#删除索引
DROP INDEX index_name ON table_name;
```





### 性能分析

**1.SQL执行频率：**

```sql
show global status like 'Com________';
```

**2.慢查询日志：**

慢查询日志记录了所有**超过指定时间**的所有SQL语句的日志。默认10秒。

**3.profile详情：**

```sql
#查看每一条SQL的耗时基本情况
show profiles;
#查看指定query_id的SQL语句各个阶段的耗时情况
show profile for query query_id;
#查看指定query_id的SQL语句CPU的使用情况
show profile cpu for query query_id;
```

**4.explain执行计划：**

```sql
#直接在select语句之前加上关键字explain/desc
EXPLAIN SELECT 字段列表 FROM 表名 WHERE 条件;
```

EXPLAIN执行计划各字段含义：

**Id：**

select查询的序列号，表示查询中执行select语句的顺序(id相同，执行顺序从上到下，id不同越大优先级越高)

**select_type：**

表示SELECT的类型，常见的有SIMPLE，PRIMARY(主查询)，UNION(后面的查询语句)，SUBQUERY

**type(重点)：**

表示连接类型，性能**由高到低**为NULL，system，const，eq_ref，range，index，all

**possible_key(重点)：**

显示可能应用在这张表上的索引，一个或多个

**key(重点)：**

实际用到的索引

**key_len(重点)：**

使用到的索引的字节数，该值为**索引字段最大可能长度**

**rows：**

MYSQL认为必须要执行查询的行数，在inndb中，这是一个预估值，不总是准确的。

**filtered：**

表示返回结果的行数占需读取行数的百分比，filtered的值越大越好。





### 索引规则

**1.最左前缀法则：**

如果索引了多列(联合索引)，要遵守最左前缀法则。最左前缀法则指的是查询从索引的**最左列开始**，并且**不能跳过索引中的列**。

如果跳过某一列，索引将部分失效(后面的字段索引失效)。



**2.索引失效：**

**范围查询：**

联合索引中，出现范围查询，范围查询右侧的列索引失效。

```sql
#这种情况age>30右侧的列索引会失效
explain select * from tb_user where profession='软件工程' and age>30 and status='0'
#尽量使用>=或<=，索引则不会失效
explain select * from tb_user where profession='软件工程' and age>=30 and status='0'
```

**运算操作：**

不要在索引列上进行**运算**操作，**索引将失效**。

```sql
#对索引列进行运算，导致索引失效
explain select * from tb_user where substring(phone,10,2)='15';
#如果phone的类型是varchar字符，不加单引号会造成索引失效，但不会报错。
explain select * from tb_user where phone=177999015;
```

**模糊查询：**

如果是**头部模糊**，索引失效。

**or连接条件：**

用or分割开的条件，如果or前的条件中的列有索引，后面的列没有索引，索引会失效。

```sql
#id和age都需要添加索引，否则单个列的索引会失效
explain select * from tb_user where id=10 or age=23;
```

**数据分布影响：**

如果MySQL评估使用索引比全表扫描还慢，则不使用索引。



**SQL提示**

![image-20221010132632882](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221010132632882.png)



**覆盖索引：**

尽量使用覆盖索引(查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到)，减少select *

using index condition：查找使用了索引，但是需要回表查询数据

using where,using index：查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表查询数据

回表查询：既要扫描聚集索引，又要扫描辅助索引



**前缀索引：**

当字段类型为字符串时，有时候需要索引很长的字符串，这会让索引变得很大，查询时浪费大量磁盘IO，影响查询效率。

此时可以只将字符串的一部分前缀建立索引，可以大大节省索引空间，从而提高索引效率。

语法：

```sql
create index idx_xxx on table_name(column(n));
```

前缀长度可以根据索引的选择性来决定，选择性=不重复的索引值/数据表的总记录数，1为最高，性能也最好。

```sql
#可以通过count函数来计算索引的选择性
select count(distinct substring(email,1,8))/count(*) from tb_user;
```



### **设计原则**

1. 针对数据量大，且查询比较频繁的表建立索引。
2. 针对常作为查询，排序，分组操作的字段建立索引。
3. 尽量选择区分度高的列作为索引，尽量建立唯一索引，因为区分度越高，使用索引的效率越高。
4. 如果是字符串类型字段过长，可以建立前缀索引。
5. 尽量使用联合索引，减少单列索引，查询时，**联合索引**很多时候可以覆盖索引，**节省存储空间**，避免回表，提高查询效率。
6. 要控制索引的数量，索引会影响增删改的效率。
7. 如果索引列不能存储NULL值，建表时请使用NOT NULL，当优化器知道每列是否包含NULL值时，它可以更好的确定哪个索引最有效地用于查询。







## SQL优化



### insert优化

**1.批量插入**

```sql
insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry');
```

**2.手动事务提交**

```sql
start transaction;
insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry');
insert into tb_test values(4,'Tom'),(5,'Cat'),(6,'Jerry');
insert into tb_test values(7,'Tom'),(8,'Cat'),(9,'Jerry');
commit;
```

**3.主键顺序插入**

```sql
主键顺序插入:1 2 3 4 5 6 7 8 9
主键乱序插入:7 8 5 6 9 2 1 3 4
```

**4.大批量插入**

如果需要一次性插入大批量数据，使用insert语句插入性能较低，此时可以使用MySQL数据库提供的**load**指令进行插入。





### 主键优化

数据组织方式

在InnoDB存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为**索引表组织**。

![image-20221012002840180](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221012002840180.png)





**页分裂：**

![image-20221017221646563](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221017221646563.png)

**页合并：**

当页中删除的记录达到MERGE_THRESHOLD(默认为**页的50%**)，InnoDB会开始寻找最靠近的页看看是否可以将两个页合并以优化空间使用。

![image-20221017222022061](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221017222022061.png)



**主键设计原则：**

满足业务需求的情况下，尽量**降低主键的长度**。

因为二级索引保存的就是主键，二级索引比较多。所以如果主键长度很大会浪费很多磁盘空间。





### order by

Using firesort：通过表的索引或全表扫描，读取满足条件的数据行，然后在**排序缓冲区**sort buffer中完成排序操作，而不是通过索引直接返回排序结果的排序都叫FileSort排序。

Using index：通过有序索引顺序扫描直接返回有序数据，这种情况即为using index，不需要额外排序，操作效率高。



总结：

1. 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则。
2. 尽量使用覆盖索引。
3. 多字段排序，一个升序一个降序，此时需要注意联合索引在创建时的规则(ASC/DESC)
4. 如果不可避免的出现filesort，大数据量排序时，可以适当增大排序缓冲区大小sort_buffer_size。



### group by

在分组操作时，可以通过索引来提高效率。

分组操作时，索引的使用也是满足最左前缀法则。



### limit

对于limit，在大数据量时进行分页，越往后性能越低。

可以通过覆盖索引和子查询的形式来优化。



### count

count()是一个聚合函数，对于返回的结果集，一行一行的判断，如果count函数的参数**不是NULL**，累计值就加1，否则不加，最后返回累计值。

用法：count(*)，count(主键)，count(字段)，count(1)

count(主键)：InnoDB会**遍历整张表**，把每一行的主键都取出来返回给服务层。拿到主键后，直接按行进行累加(主键不可能为NULL)

count(字段)：分为没有not null约束和有not null约束

count(1)：遍历整张表，但不取值。

count(*)：不会取值，服务层直接累加。

**count(*)** > count(1) >count(主键)>count(字段)

总结：尽量使用count(*)





### update

有索引的是行级锁。没有索引的是表锁。

总结：使用update时要根据索引字段进行更新，否则行锁会升级为表锁。效率会降低。

而且这个索引不能失效。







## 视图

**介绍：**

视图view是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。

通俗的讲，视图只保存了查询的SQL逻辑，不保存查询结果。所以在创建视图时，主要的工作就落在创建这条SQL查询语句上。



创建视图：

```sql
CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [WITH[CASCADED|LOCAL] CHECK OPTION]
#例如
create or replace view stu_v_1 as select id,name from student where id<=10;
```

查询：

```sql
show create view 视图名称;
#可以把视图当成一张表进行查询
select * from 视图名称;
```

修改视图：

```sql
#方式1
create [or replace] view 视图名称 as select语句 
#方式2
alter view 视图名称 as select语句
```

删除视图：

```sql
drop view if exists 视图名称;
```



**视图的检查选项：**

当使用WITH CHECK OPTION子句创建视图时，MYSQL会通过视图检查正在更改的每个行，例如插入更新，删除，以使其符合视图的定义。

MYSQL允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致性。为了确定检查的范围，mysql提供了两个选项：CASCADED和LOCAL，默认值为CASCADED。

```sql
#cascaded关键词
create or replace view 
```



视图应用案例：

1.为了保证数据库表的安全性，开发人员在操作tb_user时，只能看到的用户的基本字段，屏蔽掉手机号和邮箱两个字段。

```sql
create view tb_user_view as select id,name,profession,age,gender,status,createtime from tb_user;
```

2.查询每个学生所选修的课程(三张表联查)，为了简化操作，定义一个视图。

```sql
#多表联查
select s.name,s.no,c.name from student s,student_course sc,course c where s.id=sc.student and sc.courseid=c.id;
#可以把这个多表联查封装到一个视图里
create view tb_stu_course_view as select s.name,s.no,c.name from student s,student_course sc,course c where s.id=sc.student and sc.courseid=c.id;
```











## 存储过程



























































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

**4.typeHandler标签：**

可以重写**类型转换器**或创建自定义的类型处理器。

**6.plugins标签：**

1. pom.xml导入分页助手坐标
2. mybatis-config.xml配置分页助手插件

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



**#{}和${}的区别：**

**#{}：**底层使用PreparedStatement，先进行SQL语句的编译，然后再给SQL语句的占位符?传值

**${}：**底层使用Statement。特点：先进性SQL语句的拼接，然后再编译。

${}的使用场景：表名拼接





### 动态sql

有些时候业务逻辑复杂时，SQL是动态变化的。

- if

```xml
<select id="findByCondition" parameterType="user" resultType="user">
        select * from t_user
            <where>
                <if test="id!=0">
                    and id=#{id}
                </if>
                <if test="username!=null">
                    and username=#{username}
                </if>
                <if test="password!=null">
                    and password=#{password}
                </if>
            </where>
    </select>
```

- foreach

```java
List<Integer> ids = new ArrayList<>();
ids.add(1);
ids.add(2);
List<User> userList = mapper.findByIds(ids);
```

```xml
<select id="findByIds" parameterType="list" resultType="user">
        select * from t_user
            <where>
                <foreach collection="list" open="id in(" close=")" item="id" separator=",">
                    #{id}
                </foreach>
            </where>
    </select>
```



**sql片段抽取**

```xml
<sql id="selectUser">select * from t_user</sql>
<include refid="selectUser"></include>
```





## 高级映射

### **多对一映射关系**

**1.怎么分主表和副表？**

在前面的是主表。主表映射的对象就是主对象，副表的就是副对象。

**第一种方式：**级联属性映射。

```xml
<resultMap id="studentResultMap" type="Student">
    <id property="sid" column=sid/>
    <result property="sname" column="sname">
 	<result property="clazz.cid" column="cid"/>
    <result property="clazz.cname" column="cname"/>
</resultMap>

<select id="selectById" resultMap="studentResultMap">
    select s.sid,s.sname,c.cid,c.cname
    from t_student left join t_clazz on s.cid=c.cid
    where s.sid=#{sid}
</select>
```

**第二种方式：**association

```xml
<resultMap id="studentResultMapAssociation" type="Student">
    <id property="sid" column="sid"/>
    <result property="sname" column="sname"/>
    <!--association：一个Student对象关联一个Clazz对象
		property：提供要映射的pojo类的属性名-->
    <association property="clazz" javaType="Clazz">
        <id property="cid" column="cid"/>
        <result property="cname" column="cname"/>
    </association>
</resultMap>
```

**第三种方式：**两条sql语句，分步查询（常用，优点：可复用，支持延迟加载）

```xml
<resultMap id="stuResultMapByStep" type="Student">
    <id property="sid" column="sid"/>
    <result property="sname" column="sname"/>
    <association property="clazz" 
                 select="这里需要指定另外第二步SQL语句的ID"
                 column="cid"/>
</resultMap>
```

**mybatis中怎么开启延迟加载？**

**association**标签中添加**fetchType="lazy"**

实际开发中的模式：把全局延迟加载打开，如果某一步不需要延迟加载，设置fetchType="eager"







### **一对多映射关系**

一个班级对应多个学生对象。

**第一种方式：**collection

```xml
<resultMap id="clazzResultMap" type="Clazz">
    <id property="cid" column="cid"/>
    <result property="cname" column="cname"/>
    <!--一对多，这里是collection
		ofType属性用来指定集合中的元素类型-->
    <collection property="stus" ofType="Student">
        <id property="sid" column="sid"/>
        <result property="sname" column="sname"/>
    </collection>
</resultMap>
<select id="selectByCollection" resultMap="clazzResultMap">
    select c.cid,c.cname,s.sid,s.sname
    from t_clazz left join t_stu on c.cid=s.cid
    where c.sid=#{cid}
</select>
```

**第二种方式：**分步查询

```xml
<!--分步查询第一步：根据班级的cid获取班级信息-->
<resultMap id="clazzResultMapStep" type="Clazz">
    <id property="cid" column="cid"/>
    <result property="cname" column="cname"/>
    <collection property="stus"
                select="这里需要指定另外第二步SQL语句的ID"
                column=""/>
</resultMap>
```





## 缓存

**MyBatis的缓存机制：**

执行DQL(查询)语句的时候，将查询结果放到缓存当中。如果下次还是执行完全相同的sql，直接从缓存中拿数据。



**有哪些缓存技术？**

- 字符串常量池
- 整数型常量池
- 线程池
- 连接池



**回顾：**sqlSession的作用域是一个会话，SqlSessionFactory的作用域是整个数据库，这也是为什么二级缓存的范围比一级缓存要大。



**一级缓存：**

**默认开启**，不用做任何配置。

原理：只要使用同一个SqlSession对象执行同一条语句，就会走一级缓存。

思考：什么时候不走缓存？

SqlSession对象不是同一个。

查询条件不一样，肯定也不走缓存。

**什么时候一级缓存失效？**

第一次DQL和第二次DQL查询之间做了以下任意两件事：

1. **手动清空**，clearCache()方法
2. 执行了**增删改**任意语句，不管操作哪张表，都会清空一级缓存。



**二级缓存：**

二级缓存的范围是SqlSessionFactory，也就是整个数据库

**二级缓存条件：**

1. 在相应的Mapper.xml文件下添加`<cache/>`标签
2. 使用二级缓存的实体类对象必须是可序列化的
3. SqlSession对象关闭或提交之后，一级缓存中的数据才会被写入到二级缓存中。


**二级缓存失效：**

增删改出现，就会失效



## 分页查询

实际上每一次在进行分页请求发送的时候，前端都要发送两个数据：

页码pageNum，每页显示条数pageSize

假设已知页码pageNum和pageSize，第一个数字怎么动态的获取？

startIndex=(pageNum-1)*pageSize

**mysql中应该怎么写？**

```sql
select * from t_car limit 0,3;
```





## 注解式开发

**原则：**简单sql可以注解，复杂sql用xml

不用写XXXmapper.xml，直接在接口的方法上加上注解



**@Insert注解：**

```java
@Insert("insert into t_car values(null,#{carNum},#{brand},#{guidePrice})")
int insert(Car car);
```

**@Delete注解：**

```java
@Delete("delete from t_car where id=#{id}")
int deleteById(Long id);
```

**@Update注解：**

```java
@Update("update t_car set car_num=#{carNum},brand=#{brand},guide_price=#{guidePrice} where id=#{id}")
int update(Car car);
```

**@Select注解：**

```java
@Select("select * from t_car where id=#{id}")
Car selectById(Long id);
```









# SpringBoot

可以简化Spring，SpringMVC的使用，它的核心还是IOC容器。

**特点：**

​	创建spring应用

​	内嵌的tomcat，jetty，Undertow

​	提供了starter起步依赖，简化应用的配置。

​	尽可能去配置spring和第三方库。（自动配置）

​	提供健康检查，统计，外部化配置

​	不用生成代码，不用使用xml做配置

国外的地址：https://start.spring.io

国外的地址：https://start.springboot.io



**starter：**

SpringBoot中常见项目名称，定义了当前项目使用的所有依赖坐标，达到**减少依赖配置**的目的。

**parent：**

所有SpringBoot项目要继承的项目，定义了若干个坐标版本号(依赖管理)，**减少依赖冲突**。





## Rest风格

![image-20220930040914063](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20220930040914063.png)



**@RestController** = @ResponseBody + @Controller

@PostMapping = @RequestMapping(METHOD="post")

**注意：**Get Put Delete同理

**@PathVariable：**路径变量注解，用来从请求的url中获取参数



**Rest例子：**

```java
@GetMapping("/student/{studentId}")
public Student getStudent(@Pathvariable("studentId")Integer id){
    Student student = new Student();
    student.setId(id);
}
```





## 核心注解

**@SpringBootApplication**

这是一个**组合注解**，由三个注解组成：

**@SpringBootConfiguration：**表明当前类是一个配置类

**@EnableAutoConfiguration：**启动自动配置，把spring和第三方库的对象都创建好，通过检查类路径的依赖，根据依赖推测项目要使用的功能，然后创建完成功能所需要的对象，放到容器中。

**@ComponentScan：**组件扫描器，通过扫描@Component注解，创建他们的对象。默认扫描根包（启动类所在的包）。



启动类执行run()方法，run()返回容器对象ApplicationContext。





## 核心配置文件

**application.properties（yaml）**

**yaml语法格式：**

1. 区分大小写
2. 支持层级关系
3. 缩进不能tab，只能空格
4. 冒号后面必须有空格



**读取值的方法：**@Value("${key}")



**多环境：**可以配置多个环境(application.properties)，然后再用一个properties文件来指定激活哪个配置环境。

```properties
spring.profiles.active=dev
```



**@ConfigurationProperties(prefix="")：**使用一个类存储配置文件中的数据







## 拦截器

定义类实现接口**HandlerInterceptor**，重写方法，常用的**preHandle()**方法：

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

**注册类中注册拦截器：**

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





## 字符集过滤器

**CharacterEncodingFilter**处理request和response中的乱码问题。

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





## MyBatis集成

```properties
server.port=8080
server.servlet.context-path=/mybatis
#配置数据库信息
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/springdb?useUnicode=true&characterEncoding=UTF-8&serverTimeZone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=123456

#指定mybatis文件的位置
mybatis.mapper-locations=classpath:mappers/**/*.xml

#配置mybatis日志
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
mybatis.configuration.map-underscore-to-camel-case=true
```



**注解开发：**

在启动类中加入**@MapperScan**注解扫描所有mapper的包

```java
@MapperScan("com.powernode.mapper","com.powernode.mapper2")
```





**事务：**

在入口类使用注解@EnableTransactionManagement

在访问数据库的Service方法上添加注解@Transactional即可









# Redis

键值数据库

SQL：structured query language(结构化，关系型，SQL语法查询，ACID)

NoSQL：非关系型数据库(非结构化，无关联的，非SQL，BASE)



Redis特征：

键值类型，value支持多种不同数据结构，功能丰富

单线程，每个命令具备原子性

低延迟，速度快

支持数据持久化

主从集群，分片集群



前台启动：redis-server

后台启动：./redis-server &



## Redis入门

修改redis.conf文件：vim redis.conf

```shell
#监听的地址，默认是127.0.0.1，会导致只能在本地访问，修改为0.0.0.0则可以在任意ip访问，生产环境不要设置为0.0.0.0
bind 0.0.0.0
#守护进程，修改为yes后即可后台执行
demonize yes
#密码
requirepass 123456
#工作目录，默认是当前目录，日志，持久化等文件会保存在这个目录
dir .
#数据库数量，设置为1，代表只能有一个库，默认有16个
databases 1
#指定日志文件，默认为空
logfile "redis.log"
#指定redis能使用的最大内存
maxmemory 512mb
```



**基础操作：**

```shell
#后台启动redis
./redis-server &
#前台启动redis
./redis-server
#后台启动并输出日志到nohup.out文件
nohup ./redis-server &
#关闭redis服务
src目录下执行 ./redis-cli shutdown
```



**Redis客户端：**

redis命令行客户端：redis安装完成后自带的客户端：redis-cli

```shell
#直接连接redis
./redis-cli
#指定ip和端口连接redis
./redis-cli -h 127.0.0.1 -p 6379 -a 123456
#exit退出命令行客户端
```





## Redis数据类型

**基本类型：**String，Hash，List，Set，SortedSet

**特殊类型：**GEO,BitMap，HyperLog



**通用命令：**

**keys：**查看符合模板的所有key，**不建议**在生产环境设备上使用(会阻塞请求)

**del：**删除一个指定的key

**exists：**判断一个key是否存在

**expire：**给一个key设置有效期，有效期到期时该key会被自动删除

**TTL：**查看一个key的剩余有效

```shell
#查看所有key
keys *
#删除一个指定的key
del k1
#判断一个key是否存在
exists k1
#给一个key设置有效期
expire k1 20
#查看一个key的有效期 -1代表永久有效
ttl k1
```



### String

![image-20221015151040088](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221015151040088.png)



key的结构：

Redis的key允许有多个单词形成层级结构，多个单词之间用冒号：隔开，格式如下：

```shell
项目名:业务名:类型:id
heima:user:1
heima:product:1
```





### Hash

Hash类型，也叫散列，其value是一个无序字典，类似于Java中的HashMap结构

![image-20221015153919421](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221015153919421.png)

- HSET key field value：添加或修改hash类型key的field的值

```shell
HSET heima:user:3 age 21
```

- HGET key field：获取一个hash类型key的field的值

```shell
HGET heima:user:3 age
```

- HMGET：批量获取多个hash类型key的field的值

```shell
HMGET heima:user:4 name age gender
```

- HGETALL：获取一个hash类型的key中的所有的field和value

```shell
HGETALL heima:user:4
```

- HKEYS：获取一个hash类型的key中的所有的field

```shell
HKEYS heima:user:4
```

- HVALS：获取一个hash类型的key中的所有的value

```shell
HVALS heima:user:4
```

- HINCRBY：让一个hash类型key的字段值自增并指定步长

```shell
HINCRBY heima:user:4 age 2
```

- HSETNX：添加一个hash类型的key的field值，前提是这个field不存在，否则不执行

```shell
HSETNX heima:user:4 gender female
```







### List

类似于Java的LinkedList，可以看做是一个双向链表。

有序，元素可以重复，插入和删除快，查询速度一般

![image-20221015162942647](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221015162942647.png)





### Set

与Java中的HashSet类似，可以看做是一个value为null的HashMap。

特征：无序，元素不可重复，查找快，支持交集，并集，差集等功能

![image-20221015163955945](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221015163955945.png)







### SortedSet

可排序的集合，功能上类似于Java的TreeSet，但是底层结构有很大差别

![image-20221015164820077](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221015164820077.png)





## Redis客户端



### 1.Jedis客户端

使用Jedis获取Redis缓存中的数据：

```java
private Jedis jedis;
@Before
public void setUp() {
    jedis = new Jedis("192.168.60.134", 6379);
    jedis.select(0);
}
@Test
public void testString() {
    String result = jedis.set("name", "zhangsan");
    System.out.println("result=" + result);
    //获取数据
    String name = jedis.get("name");
    System.out.println("name=" + name);
}
@After
public void tearDown(){
    if(jedis!=null){
        jedis.close();
    }
}
```



**Jedis连接池：**

推荐使用**Jedis连接池**代替Jedis的直连方式。

因为Jedis本身是**线程不安全**的，并且频繁的创建和销毁连接有性能损耗，因此



**1.创建连接工厂：**

```java
public class JedisConnectionFactory {
    private static final JedisPool jedisPool;
    static {
        JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
        //最大连接
        jedisPoolConfig.setMaxTotal(8);
        //最大空闲连接
        jedisPoolConfig.setMaxIdle(8);
        //最小空闲连接
        jedisPoolConfig.setMinIdle(0);
        //设置最长等待时间,ms
        jedisPoolConfig.setMaxWaitMillis(1000);
        jedisPool = new JedisPool(jedisPoolConfig,"192.168.60.134",6379,1000);
    }
    public static Jedis getJedis(){
        return jedisPool.getResource();
    }
}
```

**2.建立连接：**

```java
private Jedis jedis;
@Before
public void setUp() {
    jedis = JedisConnectionFactory.getJedis();
    jedis.select(0);
}
```





### 2.Spring Data Redis客户端



**RedisTemplate：**

![image-20221016103854885](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221016103854885.png)



**SpringDataRedis快速入门：**

**StringRedisTemplate：**

为了节省空间，要求**只能存String**类型的key和value。当需要存储Java对象时，**手动完成**对象的序列化和反序列化。

1.引入依赖

2.配置文件

```yaml
spring:
  redis:
    host: 192.168.60.134
    port: 6379
    lettuce:
      pool:
        max-active: 8
        max-idle: 8
        min-idle: 0
        max-wait: 100
```

3.配置StringRedisTemplate序列化器

![image-20221016131820039](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221016131820039.png)







## 企业实战



### 1.短信登录



数据库表：

![image-20221016134910740](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221016134910740.png)



**业务流程图：**

![image-20221016141619725](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221016141619725.png)

























# RabbitMQ

1.什么是消息队列？

是一种应用间的通信方式。消息发送后可以立即返回，由消息系统来确保消息的可靠传递。



## 基础操作



**1.启动和关闭**

rabbitmq-server start&

rabbitmqctl stop



**2.发送接收机制**

所有MQ产品从模型抽象上来说都是一样的过程：

消费者订阅某个队列。生产者创建消息，然后发布到队列中，最后将消息发送到监听的消费者。

![image-20221005125311716](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221005125311716.png)





## 交换机

不同的交换机对于读取bindings的规则不同。

目前有四种类型：direct，fanout，topic，headers。



### 1.direct

要求消息需要携带RoutingKey(路由键)，Exchange接收到消息后，会读取Bindings，如果消息的RoutingKey和路由表中的BindingKey完全一致，则将消息转发到对应的队列中。

![](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221005131223318.png)



### 2.fanout

不要求信息携带RoutingKey，交换机和队列仅通过名称进行简单绑定。速度最快。

![image-20221005131718961](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221005131718961.png)



### 3.topic

要求携带RoutingKey，路由表中的BindingKey可以使用通配符*或者#。

1. *表示必须要匹配一个任意的**单词**
2. #表示可以匹配0个或者任意单词
3. 单词之间用.进行分割


![image-20221005132535670](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221005132535670.png)





## 消息发送

```java
//创建连接工厂，用于配置RabbitMQ的连接信息
ConnectionFactory factory = new ConnectionFactory();
factory.setHost("192.168.60.133");
factory.setPort(5672);//5672 是broker端口，15672是管控台端口
factory.setUsername("root");
factory.setPassword("123456");
Connection connection = null;
Channel channel = null;
try {
    connection = factory.newConnection();
    channel = connection.createChannel();
    /*声明队列
    * 参数1:队列名称 取值任意，如果队列名在MQ存在，则放弃创建
    * 参数2:是否持久化
    * 参数3:是否排外，true表示排外，如果有消费者监听这个队列，则不允许再有消费者监听这个队列
    * 参数4:自动删除，没有消费者监听时，队列会自动删除
    * 参数5:队列属性设置，通常null即可*/
    String queueName = "myQueue";
    channel.queueDeclare(queueName,true,false,false,null);
    /*声明交换机
    *参数1 交换机名称 取值任意
    *参数2 交换机类型 取值为String 类型：direct fanout topic headers
    *参数3 是否持久化的
    */
    String exchangeName = "directExchange";
    channel.exchangeDeclare(exchangeName,"direct",true);
    /*将队列和交换机进行绑定
    *参数1 目的地名称(队列名称)
    *交换机名称
    *绑定时的BindingKey
    */
    channel.queueBind(queueName,exchangeName,"directKey");
    
    /*发送消息
    * 参数1:交换机名称
    * 参数2:消息携带的RoutingKey，没有指定交换机，所以被识别为队列名称
    * 参数3:消息的属性设置(消息头)通常为null即可
    * 参数4:具体消息数据取值为byte数组*/
    String message = "测试消息";
    channel.basicPublish(exchangeName,"directKey", null, message.getBytes());
    System.out.println("消息发送成功---"+message);
```



## 消息接收

```java
```



## fanout广播

广播模式可能会有很多接受者，对速度的要求高，所以选择速度最快的fanout来模拟广播模式。

**广播的特性**：消息一对多，所有人拿到的消息都是一模一样的。



**queueDeclare()** 没有指定参数，则会声明一个随机的，非持久化的，排外的，自动删除的队列。

**getQueue()**  获取随机队列名称





## boot整合

```properties
#配置RabbitMQ的连接信息(单击版)
spring.rabbitmq.host=192.168.60.133
spring.rabbitmq.port=5672
spring.rabbitmq.username=root
spring.rabbitmq.password=123456
```



**1.整合发送：**

RabbitMQ配置类：声明队列，声明交换机，绑定队列和交换机

```java
@Configuration
public class RabbitMQConfig {
    @Bean
    public Queue bootQueue(){
        //在MQ中声明队列，参数为队列名称
        //同时将这个对象定义到spring的应用上下文容器中
        return new Queue("bootQueue");
    }
    @Bean
    public DirectExchange directBootExchange(){
        //在MQ中声明交换机，参数为交换机名称
        //同时将这个对象定义到spring的应用上下文容器中
        return new DirectExchange("directBootExchange");
    }
    @Bean
    public Binding directBinding(){
        /**
         * 将队列和交换机进行绑定 同时这个对象会定义到spring的应用上下文容器中
         * 参数1 目的地名称
         * 参数2 目的地类型 取值为枚举类型
         * 参数3 交换机名称
         * 参数4 绑定时的BindingKey
         * 参数5 绑定时的属性设置
         */
        return new Binding("bootQueue", Binding.DestinationType.QUEUE,"directBootExchange",null,null);
    }
}
```

SendService类编写业务方法-发送：

```java
/*注入AMQP模板工具类接口对象，利用这个对象可以完成对AMQP的基本操作
* 例如消息的发送和接收*/
@Resource
private AmqpTemplate amqpTemplate;
/*注入RabbitMQ的模板工具类对象，利用这个对象可以完成对RabbitMQ的基本操作
* 例如消息发送和接收以及确认模式等等
* RabbitTemplate是AmqpTemplate的子类*/
@Resource
private RabbitTemplate rabbitTemplate;
public void directSend(){
    /**
     * 发送消息到MQ服务
     * 参数1 交换机名称
     * 参数2 消息携带的RoutingKey
     * 参数3 具体消息数据取值，任意类型建议使用String，复杂格式建议使用Json格式的字符串
     */
   amqpTemplate.convertAndSend("directBootExchange","bootKey","boot测试消息");
}
```



**2.整合接收：**

业务方法：接收服务类

```java
@Resource
private AmqpTemplate amqpTemplate;
/**
 * @RabbitListener 用于标记当前方法是RabbitMQ的消息监听方法
 * 用于持续监听队列接收消息
 * 属性：
 *  queues 用于指定需要监听的队列名称 取值为String数组
 *  这个监听自带消费者确认模式，如果当前方法抛出异常后将消息放回到队列尾部
 *  如果正常接收则将消息出队
 * @param message 接收到的具体消息数据 取值类型参考发送时
 */
@RabbitListener(queues = {"bootQueue"})
public void directListener(String message){
    System.out.println(message);
}
```



**3.整合广播：**

```java
@RabbitListener(bindings = {@QueueBinding(value = @Queue(),exchange = @Exchange(name = "fanoutBootExchange",type = "fanout"))})
    public void fanoutListener(String message){
        System.out.println(message);
    }
```





## 死信队列

回退库存问题：

![image-20221006170650435](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221006170650435.png)

加入死信队列：

![image-20221006172454001](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221006172454001.png)

订单队列和死信队列：

创建订单队列的自动删除必须选false，因为要转发给死信队列

```java
//订单队列
@Bean
public Queue orderQueue(){
    //定义Map集合，用于存放队列的属性
    Map<String,Object> arguments = new HashMap<>();
    //指定死信交换机的名称，当消息超时后，会自动将消息投递到这个交换机中
    arguments.put("x-dead-letter-exchange", "deadLetterExchange");
    //指定消息超时后投递到死信交换机时所携带的全新的routingKey
    arguments.put("x-dead-letter-routing-key", "deadLetter");
    return new Queue("orderQueue", true, false, false, arguments);
}
@Bean
public DirectExchange orderExchange(){
    return new DirectExchange("orderExchange");
}
@Bean
public Binding orderBind(){
    return new Binding("orderQueue", Binding.DestinationType.QUEUE, "orderExchange", "orderKey", null);
}

//死信队列
@Bean
public Queue deadLetterQueue(){
    return new Queue("deadLetterQueue");
}
@Bean
public DirectExchange deadLetterExchange(){
    return new DirectExchange("deadLetterExchange");
}
@Bean
public Binding deadLetterBind(){
    return new Binding("deadLetterQueue", Binding.DestinationType.QUEUE, "deadLetterExchange", "deadLetter", null);
}
```

监听器监听死信队列，判断数据状态决定对商品库存的操作

```java
@Component
public class OrderMessageListener {
    @RabbitListener(bindings = @QueueBinding(value = @Queue("deadLetterQueue"),exchange=@Exchange(name = "deadLetterExchange"),key = {"deadLetter"}))
    public void orderDeadLetterMessageListener(String message){
        //这里接收到消息后，要根据数据库中的数据状态决定是否要进行商品库存回退
        System.out.println(message);
    }
}
```







# SpringCloud

**什么是微服务？**

- 是系统架构上的一种设计风格，将一个独立的系统拆分成多个小型服务。
- 这些小型服务都在独立的进程中运行。
- 服务之间通过基于HTTP的RESTful API进行通信协作。



**SpringCloud的整体架构**

![image-20221006202801316](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221006202801316.png)

Server Provider：暴露服务的服务提供方

Service Consumer：调用远程服务的服务消费方

Eureka Server：服务注册中心和服务发现中心





## 消费者

**RestTemplate**类：

基于HTTP访问远程服务

```java
@RestController
public class ConsumerController {
    @Resource
    private RestTemplate restTemplate;
    @RequestMapping("/test")
    public Object test(){
        /**
         * getForEntity()方法:使用Http协议Get方式访问远程服务，对应服务提供者的@RequestMapping和@GetMapping
         * 参数1：具体要访问的URL地址路径
         * 参数2：本次请求的响应数据封装类型
         * 参数3 本次地址路径的动态数据 取值为可变长度的Object类型数据
         * 返回值 ResponseEntity<T> 用于封装响应，可以用这个对象获取响应状态码，响应头
         */
        String url ="http://localhost:8080/test";
        ResponseEntity<String> responseEntity = restTemplate.getForEntity(url, String.class);
        return "第一个SpringCloud消费者---"+responseEntity.getBody();
    }
}
```





## 注册中心

将服务所在主机，端口，版本号，通信协议等信息登记到注册中心上。

Eureka注册中心，使用Eureka的客户端连接到Eureka的服务端，并维持心跳连接。

**CAP概念：**

C：一致性

A：可用性

P：分区容错性

ZooKeeper保证CP，Eureka保证AP

Eureka优先保证高可用。



### 基础使用

**1.搭建服务中心**

在application.properties文件中配置Eureka服务注册中心信息：

```properties
#内嵌定时tomcat的端口
server.port=9100
#设置该服务注册中心的hostname
eureka.instance.hostname=localhost
#由于我们目前创建的应用是一个服务注册中心，而不是普通的应用，默认情况下，这个应用会向注册中心（也是它自己）注册它自己，设置为false表示禁止这种自己向自己注册的默认行为
eureka.client.register-with-eureka=false
#表示不去检索其他的服务，因为服务注册中心本身的职责就是维护服务实例，它不需要去检索其他服务
eureka.client.fetch-registry=false
#指定服务注册中心的位置
eureka.client.service-url.defaultZone=http://${eureka.instance.hostname}:${server.port}/eureka
```

在Spring Boot入口类上添加@EnableEurekaServer注解，用于开启Eureka注册中心服务端：

```java
@SpringBootApplication
//激活EurekaServer
@EnableEurekaServer
public class EurekaServer {
  public static void main(String[] args) {
    SpringApplication.*run*(EurekaServer.class, args);
  }
}
```



**2.向服务中心注册服务：**

```properties
#指定Tomcat的端口号需要避免和其他的Tomcat端口冲突
server.port=8081
#指定服务名字 这个名称将在服务消费者时被调用 取值任意，建议使用模块名，不同的服务服务名要具有唯一性
spring.application.name=02-eureka-client-provider
#指定eureka的访问地址
eureka.client.service-url.defaultZone=http://localhost:9100/eureka
```

激活Eureka中的EnableEurekaClient功能：

```java
@SpringBootApplication
//激活EurekaClient，可选注解
@EnableEurekaClient
public class Provider {
    public static void main(String[] args) {
        SpringApplication.run(Provider.class, args);
    }
}
```

编写控制器提供服务:

```java
@RestController
public class ProviderController {
    @RequestMapping("/test")
    public Object test(){
        return "服务提供者";
    }
}
```



**3.从注册中心发现服务**

标记当前RestTemplate支持负载均衡，默认使用Ribbon的负载均衡，默认轮询策略

```java
@Configuration
public class RestTemplateConfig {
    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

消费者类拿取提供者的数据：

```java
@Resource
private RestTemplate restTemplate;
@RequestMapping("/test")
public Object test(){
    /**
     * EUREKA-CLIENT-PROVIDER 需要访问的服务在注册中心的服务名
     */
    String url ="http://EUREKA-CLIENT-PROVIDER/test";
    ResponseEntity<String> responseEntity = restTemplate.getForEntity(url, String.class);
    return "第一个SpringCloud消费者---"+responseEntity.getBody();
}
```



### 集群搭建

Eureka注册中心高可用集群就是各个注册中心**相互注册**。

9100端口：

```properties
#指定服务注册中心的位置,将当前端口为9100的注册中心注册到端口号为9200的注册中心
eureka.client.service-url.defaultZone=http://eureka9200:9200/eureka
```

9200端口：

```properties
#指定服务注册中心的位置,将当前端口为9100的注册中心注册到端口号为9200的注册中心
eureka.client.service-url.defaultZone=http://eureka9200:9200/eureka
```

**配置本地DNS映射：**

修改本地hosts文件配置：C:\Windows\System32\drivers\etc\hosts

**127.0.0.1 eureka9100**

**127.0.0.1 eureka9200**

 



### 负载均衡

解决高并发量的手段。

负载均衡设备将请求相对均衡的发布到各个服务器。

SpringCloud对Ribbon做了二次封装，可以让我们使用**RestTemplate**的服务请求，自动转换成客户端负载均衡的服务调用。



**Ribbon实现客户端负载均衡**

1.启动多个服务提供者实例并注册到一个服务注册中心。

2.服务消费者通过被**@LoadBalanced**注解修饰过的**RestTemplate**来调用服务提供者。

```java
@Configuration
public class RestTemplateConfig {
    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```



**Ribbon负载均衡策略**

RandomRule	随机策略

RoundRobinRule	轮询策略

AvailabilityFilteringRule	先过滤掉故障的服务，对剩下的服务继续轮询

WeightedResponseTimeRule	权重计算

RetryRule	重试

BestAvailableRule	先过滤掉故障的服务，再选一个并发量最小的服务

ZoneAvoidanceRule	综合判断





## RestTemplate

### 返回数据

ResponseEntity<T>封装响应

```java
@RequestMapping("/getUser")
public Object getUser(){
    /**由于远程服务返回的是符合Json对象格式的字符串数据
    因此可以使用数据库类型封装响应信息
    1.String，使用String封装数据后利用一些JSON工具转换成需要的Java对象
    2.实体类，Spring会将Json对象格式的字符串转换成Java对象
    3.Map集合，Spring会使用Json的属性名作为key，使用属性对应的数据作为value
    */
    String url = "http://localhost:8081/getUser";
    ResponseEntity<User> responseEntity = restTemplate.getForEntity(url,User.class);
    User body = responseEntity.getBody();
    return "服务消费者----"+body.getName()+body.getAge();
}
```





### get请求

对于服务提供者，控制器仅仅是一个普通的控制器

获取请求参数方式和SpringMVC完全一致

```java
@RequestMapping("/params")
public Object params(String name,Integer age){
    //返回User Spring会将User对象转换成对应的Json对象格式的字符串数据返回
    return new User(name+"---提供者",age);
}
```

消费者控制器：

```java
@RequestMapping("/params01")
public Object params01(){
    /**本次地址路径的动态数据，取值为可变长度的Object类型数据
    {0}和{1}为地址路径的占位符，需要后期动态赋值
    0和1位参数的索引，必须和参数的索引完全对应
    */
    String url = "http://localhost:8081/params?name={0}&age={1}";
    ResponseEntity<List> responseEntity = restTemplate.getForEntity(url,User.class);
    User body=responseEntity.getBody();
    return "服务消费者"+body;
}
```



**getForObject()：**

与getForEntity使用类似，getForObject只能返回响应体，不能获取响应头以及响应状态码。

返回的具体类型取决于getForObject方法的参数





### post请求

**postForObject()：**

提供者实现：

```java
@PostMapping("/postTest")
public Object postTest(String  name,Integer age){
    return new User(name+"---提供者",age);
}
```

消费者实现：

```java
@RequestMapping("/postTest")
public Object postTest(){
    /**
     * postForObject 使用Http协议 以Post方式提交请求，访问远程服务，
     * 对应服务提供者的@PostMapping或@RequestMapping
     * 参数 1 请求地址路径
     * 参数 2 请求体封装数据，用于封装具体请求参数,取值类型为Map且value泛型需要是List
     * 参数 3 本次响应的数据封装类型
     * 参数 4 地址路径动态数据取值为Object可变长度类型
     */
    String url="http://localhost:8081/postTest";
    //创建LinkedMultiValueMap，Map集合的子类，且value泛型为List 
    LinkedMultiValueMap params=new LinkedMultiValueMap();
    //如果key为name在map中存在，则将赵六存入key为name所对应List集合的尾部
    //如果key为name在map中不存在，则创建一个List集合将赵六设置到这个List中
    // 在将List存入Map中key为name
    params.add("name","赵六");
    params.add("age",26);
    User body= restTemplate.postForObject(url,params, User.class);
    return "服务消费者-----"+body;
}
```





### put请求

put请求主要针对数据的修改，但是由于没有返回值类型，因此不知道本次修改是否成功，不推荐使用。(除非远程服务明确使用了@PutMapping)



### delete请求

delete请求也没有返回值。用于数据的删除







## Hystrix



**故障蔓延问题：**

在微服务架构中，很容易造成服务故障的蔓延，引发整个微服务系统瘫痪不可用。

为了解决此问题，微服务架构中引入了一种叫熔断器的服务保护机制。



**熔断器：**

当被调用方没有响应，调用方直接返回一个错误响应即可，而不是长时间的等待，这样避免调用时因为等待而线程一直得不到释放，避免故障在分布式系统间蔓延。



### 异常熔断

**1.修改pom文件，追加hystrix的起步依赖**

```xml
<!--Spring Cloud熔断器起步依赖-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

**2.修改主程序类**

在入口类中使用**@EnableCircuitBreaker**注解开启断路器功能。

也可以使用一个名为**@SpringCloudApplication**的注解代替主类上的三个注解；

```java
@SpringBootApplication
@EnableEurekaClient
//激活熔断器
@EnableCircuitBreaker
public class Consumer {
    public static void main(String[] args) {
        SpringApplication.run(Consumer.class, args);
    }
}
```

**3.修改test方法**

为test方法添加熔断注解**@HystrixCommand**

```java
/**
 * @HystrixCommand 用于标记当前方法支持Hystrix的服务熔断，
 * 如果当前方法抛出异常或执行超时（默认1000毫秒）都会触发服务熔断
 *  属性：
 *   fallbackMethod 用于指定当前类中的一个本地方法的方法名称，
 *   如果当前方法触发服务熔断以后，将自动调用fallbackMethod所指定的方法，
 *   利用这个方法的返回值来替代当前方法的真实返回内容
 */
@HystrixCommand(fallbackMethod = "error")
@RequestMapping("/test01")
public Object test01(){
    String url="http://06-hystrix-provider/test01";
    ResponseEntity<String>responseEntity= restTemplate.getForEntity(url,String.class);
    String body=responseEntity.getBody();
    return "服务消费者-----"+body;
}
```

**4.熔断处理方法**

在消费者控制器类中，创建自定义服务熔断处理方法error

```java
public  String  error(Throwable throwable){
    return "触发服务熔断了";
}
```



### 超时熔断

默认超时时间是1000毫秒。

更改默认设置

```java
/**
 * @HystrixCommand 用于标记当前方法支持Hystrix的服务熔断，如果当前方法抛出异常或执行超时（默认1000毫秒）都会触发服务熔断
 *  属性：
 *   commandProperties 用于指定熔断的属性 取值为@HystrixProperty 数组
 *     @HystrixProperty 用于配置hystrix熔断器的某个属性
 *       属性：
 *         name 用于指定熔断器的属性名
 *           execution.isolation.thread.timeoutInMilliseconds 指定熔断的超时时间，默认值为1000毫秒
 *         value 用于指定属性名所对应的属性值
 *           6000 配合属性名表示设置熔断器的超时时间为6000毫秒，要求当前方法必须在6000毫秒内完成响应
 */
@HystrixCommand(fallbackMethod = "error",commandProperties = {@HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "6000")})
@RequestMapping("/test02")
public Object test02(){
    String url="http://06-hystrix-provider/test02";
    ResponseEntity<String>responseEntity= restTemplate.getForEntity(url,String.class);
    String body=responseEntity.getBody();
    return "服务消费者-----"+body;
}
```



### 服务降级

降级处理方法，当服务触发熔断后的一种处理方式。

降级处理有很多种处理方法，例如：

1. 将请求存入MQ稍后完成
2. 日志或错误信息存入数据库
3. 日志或错误信息存入Redis
4. 写日志记录到本地磁盘

```java
/**
只要触发服务熔断，那么一定会抛出异常
要根据不同的异常做不同的降级处理
如果远程服务出现异常统一返回HttpServerErrorException$InternalServerError 表示服务器内部错误
如果是超时抛出HystrixTimeoutException，表示服务没有在指定时间内完成响应
如果是服务内部异常则抛出准确的异常信息例如NullPointerException
*/
public String error(Throwable throwable){
    //根据业务需求写逻辑代码
    return "触发服务熔断了，异常类型:"+throwable.getClass()
        +"异常信息:"+throwable.getMessage();
}
```



**异常忽略：**

**ignoreExceptions** 异常忽略，当服务出现了被忽略的异常时，不会触发服务熔断的，而是将异常抛给服务的调用处。

注意：不能忽略 Throwable Exception RuntimeException，因为这可能会导致所有的异常都不会触发熔断器

```java
@HystrixCommand(fallbackMethod = "error",ignoreExceptions = {NullPointerException.class})
@RequestMapping("/test03")
public Object test03(){
    String str=null;
    System.out.println(str.length());
    String url="http://06-hystrix-provider/test02";
    ResponseEntity<String>responseEntity= restTemplate.getForEntity(url,String.class);
    String body=responseEntity.getBody();
    return "服务消费者-----"+body;
}
```



### 自定义异常熔断

```java
/**
 * 自定义异常熔断处理，专门为远程服务进行异常熔断
 * 需要继承熔断器的抽象父类
 * 抽象父类的泛型用于决定使用什么数据类型封装远程服务的返回值以及触发熔断后降级方法的返回值类型
 */
public class MyHystrixCommand extends HystrixCommand<String> {
    private RestTemplate restTemplate;
    private String url;
    public MyHystrixCommand(RestTemplate restTemplate,String url) {
        //调用父类有参构造设置熔断器属性
super(HystrixCommand.Setter.withGroupKey(HystrixCommandGroupKey.Factory.asKey("")));
        this.restTemplate=restTemplate;
        this.url=url;
    }
    //来自抽象父类的方法，用于访问远程服务，这个方法不能手动调用
    protected String run() throws Exception {
        return restTemplate.getForObject(url,String.class);
    }
    //来自抽象父类的方法，当run方法抛出异常后会自动调用这个方法进行降级处理，这个方法不能手动调用
    protected String getFallback() {
        return "远程服务触发熔断了";
    }
}
```





## Feign

Feign是Netflix公司开发的一个**声明式**的WebService客户端。

**WebService客户端**： 能使不同机器上的不同应用**相互交换数据**。

**声明式**：只需要定义规则，不需要实现。

Feign整合了**Ribbon**和**Hystrix**两个组件，让我们的开发工作变得更加简单。





### 集成Feign

**1.创建提供者：**

创建服务提供者项目07-feign-provider

添加依赖包

修改配置文件：

```properties
#指定Tomcat的端口号需要避免和其他的Tomcat端口冲突
server.port=8081
#指定服务名字 这个名称将在服务消费者时被调用 取值任意，建议使用模块名，不同的服务服务名要具有唯一性
spring.application.name=07-feign-provider
#指定eureka的访问地址
eureka.client.service-url.defaultZone=http://localhost:9100/eureka
```



**2.创建控制器**：

```java
@RestController
public class ProviderController {
    @RequestMapping("/test")
    public Object test(){
        return "服务提供者";
    }
}
```



**3.消费者实现：**

创建项目07-feign-consumer

添加Feign的起步依赖

修改配置文件：

```properties
#指定Tomcat的端口号需要避免和其他的Tomcat端口冲突
server.port=8081
#指定服务名字 这个名称将在服务消费者时被调用 取值任意，建议使用模块名，不同的服务服务名要具有唯一性
spring.application.name=07-feign-provider
#指定eureka的访问地址
eureka.client.service-url.defaultZone=http://localhost:9100/eureka
```

修改主程序类：

```java
//程序入口类激活Feign的支持
@SpringBootApplication
@EnableEurekaClient
//激活Feign
@EnableFeignClients
public class Consumer {
    public static void main(String[] args) {
        SpringApplication.run(Consumer.class, args);
    }
}
```

自定义声明式服务消费接口RemoteProviderService:

```java
/**
 * @FeignClient 标记当前接口是声明式服务消费接口，
 *              SpringCloud会为这个接口创建动态代理对象。
 *              并定义到Spring的应用上下文容器中
 *  属性：
 *    name 用于指定需要访问的远程服务在注册中心中的服务名称
 */
@FeignClient(name = "07-feign-provider")
public interface RemoteProviderService {
    /**
     * @RequestMapping 用于通知SpringCloud需要使用什么方式访问哪个具体请求
     * @return 远程的响应数据
     */
    @RequestMapping("/test")
    String test();
}
```

创建控制器ConsumerController访问远程服务：

```java
@RestController
public class ConsumerController {
    @Resource
    private RemoteProviderService remoteProviderService;
    @RequestMapping("/test")
    public Object test(){
        String body=remoteProviderService.test();
        return "服务消费者-----"+body;
    }
}
```



### 返回数据



提供者控制器：

```java
@RestController
public class ProviderController{
    @RequestMapping("/test")
    public Object test(){
        return "服务提供者";
    }
    @RequestMapping("/getUser")
    public Object getUser(){
        return new User("张三",23);
    }
    @RequestMapping("/getUserList")
    public Object getUserList(){
        List<User> list = new ArrayList<>();
        list.add(new User("张三",23));
        list.add(new User("李四",24));
        return list;
    }
}
```



声明式服务消费方法

```java
@FeignClient(name="07-FEIGN-PROVIDER")
public interface RemoteProviderService{
    //@RequestMapping 用于通知SpringCloud需要使用什么方式访问哪个具体请求
    @RequestMapping("/test")
    String test();
     //因为远程服务返回的是符合Json对象格式的字符串数据，因此可以使用实体类或Map或String
    @RequestMapping("/getUser")
    User getUser();
    //因为远程服务返回的是符合Json对象数组格式的字符串数据，因此可使用List或Set或String作为返回值来封装响应数据
    @RequestMapping("/getUserList")
    List<User> getUserList(); 
}
```



消费者控制器-ConsumerController.java：

```java
@RestController
public Class ConsumerController{
    @Resource
    private RemoteProviderService remoteProviderService;
    @RequestMapping("/test")
    public Object test(){
        String body = remoteProviderService.test();
        return "test服务消费者---"+body;
    }
    @RequestMapping("/getUser")
    public Object getUser(){
        User body = remoteProviderService.getUser();
        return "getUser服务消费者---"+body.getName()+body.getAge();
    }
     @RequestMapping("/getUserList")
    public Object getUser(){
   		List<User> body = remoteProviderService.getUserList();
        return "getUserList服务消费者---"+body;
    }
}
```

总结：用户访问消费者应用上的地址，消费者控制器会调用**声明式服务消费接口方法**来抓取提供者控制器对应页面上的数据，返回给消费者控制器，最终拿到数据。





### 传递参数

**第一种方式**

提供者实现：

```java
@RequestMapping("/params")
public Object params(String name,Integer age){
    return  new User(name+"---提供者",age);
}
```

消费者实现：

修改声明式服务消费接口RemoteProviderServicet添加params01和params02方法

```java
/**
 * 地址路径中的{name}和{age}为请求地址路径的动态数据需要后期动态赋值
 * @PathVariable 用于标记当前形参为地址路径的数据，要求形参名称于占位符的名字完全相同
 */
@RequestMapping("/params?name={name}&age={age}")
User params01(@PathVariable String name, @PathVariable Integer age);
```



**第二种方式**

```java
/**
@RequestParam 用于标记当前方法形参作为请求的参数
SpringCloud会默认使用形参名称作为请求的参数名,使形参对应的数据作为请求参数值
*/
@RequestMapping("/params")
User params02(@RequestParam String name,@RequestParam Integer age);
```



**第三种方式**

如果要传递多个参数，可以使用实体类,Map,List
SpringCloud会将形参存入请求体传递给服务提供者
但是要求服务提供者必须用对应类型并且标记控制器为@RequestBody，否则无法接受请求参数

```java
@RequestMapping("/paramsObj")
User paramsObj(User user);
```



**第四种方式**：json传递

```java
/**
提供者控制器类
*/
@RequestMapping("/paramsJson")
public Object paramsJson(String paramsJson){
    User user = JSONObject.parseObject(paramsJson,User.class);
    return new User(user.getName()+"---提供者",user.getAge());
}
```

```java
/**
服务消费接口
*/
@RequestMapping("/paramsJson")
User paramsJson(@RequestParam String paramsJson);
```

```java
/**
消费者控制器类
*/
@Resource
private RemoteProviderService remoteProviderService;
@RequestMapping("/paramsJson")
public Object paramsJson(){
    User user = new User();
    user.setName("王五");
    user.setAge(25);
    String userJson = JSONObject.toString(user);
    User body = remoteProviderService.paramsJson(userJson);
    return body;
}
```





### 服务熔断

修改声明式服务消费接口RemoteProviderService，的@FeignClient注解

```java
@FeignClient(name = "07-feign-provider",fallback = MyFallback.class)
```

fallback属性： 用于指定当前接口的服务降级处理类，这个类是当前接口的子类，如果当前接口中的任意方法出现异常，则会自动调用这个类中的对应方法进行降级处理

```java
/**
 * 自定义熔断处理类
 * 这个类必须要实现某个声明式服务消费接口
 * 当前类必须要定义到Spring的应用上下文容器中
 */
@Component
public class MyFallback implements RemoteProviderService {
    @Override
    public String test() {
        return "test方法触发熔断了";
    }
}
```

修改application.properties配置文件

```properties
#激活Feign对Hystrix的支持
feign.hystrix.enabled=true
```



### **异常信息**

这样做可以根据异常信息进行熔断处理。



修改声明式服务消费接口RemoteProviderService的@FeignClient注解：

```java
@FeignClient(name = "07-feign-provider",fallbackFactory = MyFallbackFactory.class)
/**fallbackFactory属性：指定一个用于创建熔断处理类对象的工厂类,利用工厂类返回的对象我们可以获取触发熔断的异常信息*/
```

创建**自定义熔断处理类**MyFallbackFactory：

```java
/**
 * 自定义熔断处理类
 * 这个类必须要实现FallbackFactory接口
 * 接口可以指定一个泛型，用于决定为那个声明式服务消费接口创建熔断处理类
 * 当前类必须要定义到Spring的应用上下文容器中
 */
@Component
public class MyFallbackFactory implements FallbackFactory<RemoteProviderService> {
    //来自接口中的方法，用于创建某个声明式服务消费接口的熔断处理类对象
    //参数为触发熔断时的异常信息
    public RemoteProviderService create(Throwable throwable) {
        return new RemoteProviderService() {
            @Override
            public String test() {
                return "<br>test方法触发熔断了<br>异常类型:"+throwable.getClass()+"<br>异常信息："+throwable.getMessage();
            }
            @Override
            public User getUser() {
                return null;
            }
            @Override
            public List<User> getUserList() {
                return null;
            }
```



### 超时熔断

**注意：**由于消费者使用Hystrix服务熔断，要求必须在1000毫秒内返回响应否则将触发服务熔断。

修改消费者的application.properties文件增加超时时间

```properties
#配置Hystrix的超时时间为6000毫秒  hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=6000    #配置Ribbon的超时时间为6000毫秒， # 要求远程服务必须在6000毫秒内完成响应  ribbon.ReadTimeout=6000
```

**注意：**由于Feign在底层使用了Ribbon进行负载均衡，而Ribbon也有超时设置，所以如果需要配置超时熔断时，必须同时指定Hystrix以及Ribbon的超时







## Gateway



### 三大概念

**1.Route(路由)**

由一个ID，一个目的URL，一组断言工厂，一组Filter组成。

如果路由断言为真，说明**请求URL**和**配置路由**匹配。

**2.Predicate(断言)**

Java8中的断言函数。Gateway允许开发者去**定义匹配**来自Http Request中的**任何信息**。

**3.Filter(过滤)**

局部过滤和全局过滤。经常利用全局过滤器做接口安全认证，限流，IP拦截等。



网关配置

```properties
erver.port=9000
#启用gateway，默认值为true
spring.cloud.gateway.enabled=true
#配置路由规则
#routes是一个List集合类型，集合泛型为RouteDefinition，用于配置路由规则
#[0]表示第一个路由规则
#id为RouteDefinition对象中的属性，取值任意，必须唯一
spring.cloud.gateway.routes[0].id=first-route
#uri为RouteDefinition对象中的属性，用于指定请求需要转发到哪个地址
spring.cloud.gateway.routes[0].uri=http://localhost:8080/test
#配置一个断言，predicates取值为List集合，
#Path为路径断言，要求访问网关的test请求时会转发到目标服务http://localhost:9000/test中
#**表示任意子路径下的任意请求
spring.cloud.gateway.routes[0].predicates[0]=Path=/test/**
```





### 断言



#### 路径断言

当访问网关的某个特定的路径时即可实现请求的转发

```properties
spring.cloud.gateway.routes[0].id=first-route
spring.cloud.gateway.routes[0].uri=http://localhost:8081/test
spring.cloud.gateway.routes[0].predicates[0]=Path=/test/**
```

#### 时间断言

```properties
spring.cloud.gateway.routes[0].id=first-route
spring.cloud.gateway.routes[0].uri=http://localhost:8081/test
#在特定时间之后可以访问
spring.cloud.gateway.routes[0].predicates[0]=After=2022-08-12T10:07:17.773+08:00[Asia/Shanghai]
#在特定时间之前可以访问
spring.cloud.gateway.routes[0].predicates[0]=Before=2022-08-12T10:07:17.773+08:00[Asia/Shanghai]
#在特定时间段内可以访问
spring.cloud.gateway.routes[0].predicates[0]
=Between=2022-08-10T10:07:17.773+08:00[Asia/Shanghai],
2022-08-12T10:07:17.773+08:00[Asia/Shanghai]
```

#### Cookie

当请求中的Cookie携带某个特定数据时可以访问

```properties
spring.cloud.gateway.routes[0].id=first-route
spring.cloud.gateway.routes[0].uri=http://localhost:8081/test
spring.cloud.gateway.routes[0].predicates[0]=Cookie=username,zhangsan
spring.cloud.gateway.routes[0].predicates[1]=Cookie=password,abc
```

#### 头断言

当请求头**Header**中携带特定数据时可以路由转发

```properties
spring.cloud.gateway.routes[0].id=first-route
#请求中携带了自定义的请求头token数据为123456时即可路由转发
spring.cloud.gateway.routes[0].uri=http://localhost:8081/test``spring.cloud.gateway.routes[0].predicates[0]=Header=token,123456
#如果没有特定请求头数据则为Header=token
```

#### query

要求请求中必须携带特定请求参数名才可路由转发

```properties
spring.cloud.gateway.routes[0].id=first-route
spring.cloud.gateway.routes[0].uri=http://localhost:8081/test
spring.cloud.gateway.routes[0].predicates[0]=Query=username
```

#### Host断言

主机域名断言，要求必须使用**特定的域名**地址访问网关才可路由准发

```properties
spring.cloud.gateway.routes[0].id=first-route
spring.cloud.gateway.routes[0].uri=http://localhost:8081/test
spring.cloud.gateway.routes[0].predicates[0]=Host=gateway:9000
```

#### 权重断言

根据权重值将请求分发到不同的服务器中

```properties
#第一个路由规则
spring.cloud.gateway.routes[0].id=first-route
spring.cloud.gateway.routes[0].uri=http://localhost:8081/test
spring.cloud.gateway.routes[0].predicates[0]=Weight=group1,8
#第二个路由规则
spring.cloud.gateway.routes[1].id=user-service-router2 
spring.cloud.gateway.routes[1].uri=http://localhost:8082/test 
spring.cloud.gateway.routes[1].predicates[0]=Weight=group1,2
```

注释：80%的概率发送到8081，20%的概率发送到8082。用在实现**负载均衡**上。





### 过滤器

#### 接口安全认证

通常会暴露一个网关地址。

```java
/**
 * 自定义网关过滤器并实现父接口
 * GlobalFilter接口表示当前过滤器为全局过滤器所有路由必须经过
 * Ordered表示过滤器的排序接口，用于决定过滤器的执行先后顺序
 */
public class AuthFilter implements GlobalFilter, Ordered {
    /**
     * 过滤器的具体执行逻辑
     */
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        //认证工程后继续执行网关链式处理，可能进行下一个过滤器或直接进入远程服务
        //获取用于请求对象
        ServerHttpRequest request = exchange.getRequest();
        //获取有的请求参数
        MultiValueMap<String, String> params = request.getQueryParams();
        //获取请求中的appId参数，返回List，请求如果没有携带这个参数则返回null
        List<String> list = params.get("appId");
        if(list==null||!"123".equals(list.get(0))){
            //获取响应对象
            ServerHttpResponse response = exchange.getResponse();
            //返回响应状态码401 表示身份认证失败
            response.setStatusCode(HttpStatus.UNAUTHORIZED);
            //设置响应头，指定响应编码以及响应数据格式
            response.getHeaders().add("context-type","text/html;charset=utf-8");
            byte[] bytes = "请求非法，请检查appId".getBytes();
            //将响应数据转换成指定数据类型
            DataBuffer data = response.bufferFactory().wrap(bytes);
            //返回响应信息
            return response.writeWith(Mono.just(data));
        }
        return chain.filter(exchange);
    }

    /**
     * 返回过滤器执行序号
     * 返回值越小执行优先级越高
     * @return
     */
    @Override
    public int getOrder() {
        return 0;
    }
}
/**
     * 返回过滤器执行序号
     * 返回值越小执行优先级越高
     * @return
     */
    @Override
    public int getOrder() {
        return 0;
    }
}
```



#### IP拦截

可以通过修改getOrder()的序号来决定过滤器的执行顺序。

```java
String serverHostName = exchange.getRequest().getHeaders().getHost().getHostName();
//获取来访者客户端IP，也就是浏览器所在机器的IP
String clientIp=exchange.getRequest().getRemoteAddress().getHostName();
List<String> ipList = new ArrayList<String>();
for(int i=2;i<=255;i++){
    ipList.add("192.168.137."+i);
}
//IP拦截认证，进入if视为被封禁的ip地址
//实际项目可以到Redis或者数据库中查找
if (ipList.contains(ClientIp)) {
   //说明是黑名单里面的ip
   ServerHttpResponse response = exchange.getResponse();
   response.setStatusCode(HttpStatus.OK);
   response.getHeaders().add("content-type","text/html;charset=utf-8");
   byte[] bytes = "对不起，由于违规操作，IP不可使用".getBytes();
   DataBuffer data = response.bufferFactory().wrap(bytes);
   return response.writeWith(Mono.just(data));
    
@Override
public int getOrder() {
   return 1;
```







# 面试题总结

## 基础

**1.JDK1.8的新特性？**

接口的默认方法：java8允许给接口添加一个非抽象的实现方法，用default修饰。为了兼容lambda表达式

lambda表达式：简化编程

函数式接口：



**2.抽象类和接口的区别？**

1. 描述概念用抽象类，描述特征用接口
2. 抽象类可以定义构造器，接口不能定义构造器
3. 抽象类可以有具体方法和抽象方法，接口全部都是抽象方法
4. 抽象类是单继承，接口是多实现
5. 抽象类中可以有静态方法，接口不能有静态方法
6. 抽象类可以定义成员变量，接口中定义的成员变量实际都是常量



**3.聊一下java的集合类？**

集合（容器）是为了解决在Java中**无法确定保存对象个数**或者是否需要**更复杂的方式来存储对象**的问题。



**4.HashMap为什么用红黑树？**

1.8之后，java对HasMap做了改进，链表长度大于8时，将后面的数据存在红黑树，当链表长度退回6的时候，改回链表。



**5.集合类怎么解决高并发问题？**

**线程非安全的集合类：**ArrayList，LinkedList，HashSet，TreeSet，HashMap，TreeMap

**线程安全的集合类：**Vector，HashTable，虽然效率没有**JUC**中的集合高，但大部分环境够用

**高性能线程安全的集合类：**ConcurrentHashMap



**ConcurrentHashMap和HashMap的区别？**

JDK1.8采用CAS和synchronized来保证并发安全，数组+链表+红黑树。

synchronized只锁定当前链表或红黑树的首节点，这样只要hash不冲突，就不会产生并发，效率又提升N倍。

**ConcurrentHashMap线程安全的具体实现方式？**

**CopyOnWriteArrayList聊一下？**



**6.简述自定义异常的应用场景？**

当你不想把你的错误直接暴露给前端，控制项目的后期服务



**7.Object类中常用的方法？**

toString，hashCode，equals，clone，finalized，wait，notify，notifyAll

toString：定义一个对象的字符串的表现形式，Object类的规则是类的全路径名+@对象的哈希码



**8.hashCode和equals方法的区别？**

如果equals比较的对象内容相同，hashCode肯定相同。

但是如果hashCode如果相等，内容值不一定相同。



hashCode介绍：

hashCode()的作用是获取哈希码，实际上返回一个int整数。哈希码的作用是确定该对象在**哈希表中**的**索引位置**。Java中的任何类都包含有hashCode函数。哈希表存储的是键值对。

特点是：能够快速检索出所需要的对象。



**9.HashSet如何检查重复？**

对象加入HashSet时，HashSet会先计算对象的hashCode值来判断对象加入的位置，看该位置是否有值，如果没有值，说明不重复。如果有值，会再调用equals()来检查两个对象是否真的相同。如果不同，会重新散列到其他位置。这样大大减少了equals的次数，提升了执行速度。



hashCode的默认行为是对堆上的对象产生独特值，如果没有重写hashCode，则该class的两个对象无论如何都不会相等，即使这两个对象指向相同的数据。



**10.重载和重写的区别？**

重载发生在同一个类中，重载是方法名相同，参数列表不同。**方法返回值和修饰符可以不同。**

重写是子类重写父类的方法，方法名和参数列表都相同。

重写注意事项：

1. 子类不能比父类抛出更多的异常
2. 子类访问修饰符范围不能小于父类
3. 如果父类方法是private则子类不能重写此方法



**11.深拷贝和浅拷贝？**

指对象的拷贝，一个对象存在两种类型的属性，基本数据类型和引用数据类型。

浅拷贝指，只会拷贝基本数据类型的值，以及实例对象的**引用地址**。

深拷贝指，既拷贝基本数据类型的值，也会针对实例对象的引用地址所**指向的对象进行复制**，内部的属性指向的对象不是同一个。



**12.ArrayList和LinkedList？**

ArrayList：底层是**动态数组**，数组是连续的内存存储，对内存的要求高。适合下标访问。

LinkedList：底层是链表，可以分散在内存中存储，适合插入和删除，不适合查询，因为需要逐一遍历。

LinkedList遍历，如果使用for循环，性能消耗极大，不能这样用。



**13.List和Set的区别？**

List：有序，可重复，有索引，允许多个null元素对象，可以下标访问

Set：无序，不重复，无索引，只允许一个null元素对象，只能Iterator访问



**14.为什么局部内部类和匿名内部类只能访问局部final变量？**

final修饰类：不能被继承

final修饰方法：方法不能重写，可以覆盖

final修饰变量：变量一旦赋值，不可更改



局部内部类/匿名内部类在编译之后会生成两个class文件，内部类和外部类是同一个级别的。

**外部类的变量会复制一份到内部类**里面。因为如果外部的运行完毕，但内部类还没有运行完毕，如果内部类访问外部类的变量就会出问题。

如果在内部类中修改了成员变量，方法中的局部变量也得跟着变，因此必须要把局部变量设置为final



**15.String，StringBuffer，StringBuilder的区别？**

String是不可改变的，如果尝试修改，会产生一个新的字符串对象重新指向引用。

StringBuffer是线程安全的。

StringBuilder是线程不安全的，底层没有加锁，因此在单线程下StringBuilder效率更高。

Builder和Buffer可以用来做字符串拼接，



**16.HashMap的扩容原理？**

java1.7：数组+链表

java1.8：数组+链表，数组长度超过8，转为红黑树



**17.HashMap和HashTable的区别？**

HashMap线程不安全，HashTable是线程安全的，里面的每个方法都加了锁，执行效率低。

JDK8开始，链表高度到8，数组长度到64，链表转为红黑树，元素以内部类node节点存在。

长度低于6，又会退化回链表。



**18.ConcurrentHashMap扩容原理？**

1.7版本的ConcurrentHashMap基于Segment分段实现的。

每个Segement相对于一个小型的HashMap。

1.8版本ConcurrentHashMap不再基于Segment实现。

当某个线程进行put时，如果发现ConcurrentHashMap正在进行扩容那么该线程一起扩容。

ConcurrentHashMap是支持多个线程同时扩容的。





## 并发

**1.如何理解volatile关键字？**

并发三大特性：原子性，有序性，可见性。

volatile关键字修饰对象的属性。

在对这个属性进行修改时，会直接将CPU高级缓存的数据写回到主内存，对这个变量的读取也会从内存中读取。



**2.sleep，wait，join，yield的区别？**

锁池：所有需要竞争同步锁的线程都会放在锁池中。

等待池：当调用wait方法后，线程会放到等待池，等待池的线程不去竞争同步锁。只有调用了notify方法才会把等待池中的线程放到锁池中去竞争锁。



1. sleep是Thread类的静态本地方法，wait则是Object类的本地方法。
2. sleep方法不会释放lock，但是wait会释放，而且会加入到等待队列中。
3. sleep方法不依赖于同步器synchronized，但是wait需要依赖synchronized关键字。
4. sleep不需要被唤醒，但是wait需要被唤醒，不指定时间需要被别人中断。
5. sleep一般用于当前线程休眠，或者轮询暂停操作，wait则用于多线程之间的通信。
6. sleep会让出CPU执行时间，wait则不一定，wait后可能还是有机会重新竞争到锁继续执行。



yield是执行后线程直接进入就绪状态，马上释放了cpu的执行权，但是依然保留了cpu的执行资格。

join是执行后线程进入阻塞状态，比如在B线程中调用A线程的join，那么B线程会进入阻塞队列，直到线程A结束或中断线程。



**3.ThreadLocal的底层原理？**

ThreadLocal是**线程本地存储机制**，可以利用该机制将**数据缓存在某个线程内部**，该线程可以在任意时刻，任意方法中获取缓存的数据。



**4.Synchronized和ReentrantLock的区别？**

synchronized是一个关键字，ReentrantLock是一个类

synchronized会自动加锁和释放锁，ReentrantLock需要程序员手动加锁和释放

synchronized的底层是JVM层面的锁，ReentrantLock是API层面的锁

synchronized是非公平锁，ReentrantLock可以手动选择公平锁和非公平锁

synchronized锁的对象，锁信息保存在对象头，ReentrantLock通过代码int类型的state标识锁的状态

synchronized底层有一个锁升级的过程







## HTTP协议

**HTTP请求协议包：**

在浏览器准备发送请求时，负责创建一个http请求协议包。

浏览器将请求信息以**二进制形式**保存在Http请求协议包各个空间。

由**浏览器**负责将http请求协议包**推送到指定的服务端计算机**。

**HTTP响应协议包：**

http服务器在定位到被访问的资源文件之后，负责创建一个http响应协议包。

http服务器将定位文件内容或文件命令以**二进制形式**写入到**http响应协议包**各个空间。

由http服务器负责将http响应协议包**推送回发送请求的浏览器**上。



**HTTP请求协议内部空间**

自上而下划分为4个空间：

```http
请求行:[
	url:请求地址
	method:请求方式(GET/POST)
]
请求头:[
	请求参数信息[GET]
]
空白行:[
	没有任何内容，起到隔离的作用
]
请求体:[
	请求参数信息[POST]
]
```



HTTP响应协议包内部空间

```http
状态行:[
	Http状态码
]
响应头:[
	context-type:指定浏览器采用对应编译器对响应体二进制数据进行解析
]
空白行:[
	没有任何内容，起到隔离的作用
]
响应体:[
	可能是被访问静态资源文件内容
	可能是被访问的静态资源文件命令
	可能是被访问的动态资源文件运行结果
	都是以二进制的形式存在
]
```





