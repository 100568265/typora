# IO流

#### IO流的分类

字节流：按照字节方式读取数据，一次读取一个字节byte（8 bits），这种流什么类型的数据都可以读取。

字符流：一次读取一个字符，为了方便读取普通文本文件而存在的。只能读取纯文本。

#### IO流四大家族

`java.io.InputStream`字节输入流

`java.io.OutputStream`字节输出流

`java.io.Reader`字符输入流

`java.io.Writer`字符输出流



#### close()和flush()方法

所有的流都实现了`java.io.Closeable`接口，都是可关闭的，都有close()方法。

流是内存和硬盘间的通道，用完要关闭，否则会占用资源。

所有的输出流都实现了`java.io.Flushable`接口，都是可刷新的，都有flush()方法。输出流在输出之后，要记得flush()刷新一下，刷新的作用就是清空管道。没有flush()可能会导致数据丢失。

**注意：**只要以Stream结尾的都是字节流，以Reader/Writer结尾的是字符流。



#### java.io包下需要掌握的16个流

**文件专属：**

`java.io.FileInputStream`

`java.io.FileOutputStream`

`java.io.FileReader`

`java.io.FileWriter`

**转换流：**（将字节流转换成字符流）

`java.io.InputStreamReader`

`java.io.OutputStreamWriter`

**缓冲流专属：**

`java.io.BufferedReader`

`java.io.BufferedWriter`

`java.io.BufferedInputStream`

`java.io.BufferedOutputStream`

**数据流专属：**

`java.io.DataInputStream`

`java.io.DataOutputStream`

**标准输出流：**

`java.io.PrintWriter`

`java.io.PrintStream`

**对象专属流：**

`java.io.ObjectInputStream`

`java.io.ObjectOutputStream`



#### FileInputStream

文件字节输入流

**方法：**

- 流对象.read()

```java
FileInputStream fis = null;
        try {
            fis = new FileInputStream("src/hehe");
            byte[] bytes = new byte[4];
            int readCount = 0;
            while((readCount=fis.read(bytes))!=-1){
//把bytes数组转换成字符串，读到多少个就转换多少个          
System.out.print(new String(bytes,0,readCount));
            }
        }
```

- int available():返回流当中剩余没有读到的字节数量

```java
//这个方法可以直接设定数组的长度，不需要再循环了
byte[] bytes = new byte[fis.available()];
int readCount = fis.read(bytes);
System.out.println(readCount);//一次性全部读取，不需要循环
System.out.println(new String(bytes));//abcdef
```

注意：这种方式不太适合大文件，因为byte[]数组不能太大。

- long skip(long n):跳过几个字节不读

```java
fis.skip(3);//跳过三个字节不读
```



#### FileOutputStream

文件字节输出流，负责写，内存->硬盘

**方法：**

流对象.write()

```java
//这种方式会先将原文件情况，再重新写入
//fos = new FileOutputStream("myfile");//如果文件不存在会新建
//以追加的方式在文件末尾写入，不清空原文件内容
fos = new FileOutputStream("myfile",true);
//开始写
byte[] bytes = {97,98,99,100};
fos.write(bytes);
//写完之后最后一定要刷新
fos.flush();
```



**FileReader**

文件字符输入流

**FileWriter**

文件字符输出流

**BufferedReader**

带有缓冲区的字符输入流。

使用这个流的时候，不需要自定义byte数组，自带缓冲。

```java
BufferedReader br = new BufferedReader(new FileReader("src/hehe"));
            //当一个流的构造方法中需要一个流的时候，这个被传进来的流叫做节点流
            //外部负责包装的流叫做包装流
            String s = null;
            while((s=br.readLine())!=null){
                System.out.println(s);
            }
```



**BufferedWriter**

带有缓冲的字符输出流

**BufferedInputStream**

带有缓冲的字节输入流

**BufferedOutputStream**

带有缓冲的字节输出流

**InputStreamReader**

将字节流转换成字符流

**OutputStreamWriter**

将字节流转换成字符流

**DataOutputStream**

将数据连同数据的类型一并写入文件

**DataInputStream**

将数据连同数据的类型一并读取

#### **PrintStream**

标准字节输出流，默认输出到控制台，标准输出流不需要close

```java
//标准输出流不再指向控制台，指向"log"文件
PrintStream ps = new PrintStream(new FileOutputStream("log"));
//修改输出方向，将输出方向修改到"log"文件。
System.setOut(ps);
```



#### ObjectInputStream



#### ObjectOutputStream



#### File类

File对象代表文件和路径名的抽象表示形式。

一个File对象有可能是目录，也有可能是文件。

**常用方法：**

`对象.exists()`判断文件是否存在

`对象.mkdir()`以目录的形式新建

`对象.mkdirs()`以多重目录的形式新建

`对象.getParent()`获取文件的父路径

`对象.getAbsolutePath()`获取文件的绝对路径

`对象.isDirectory()`判断对象是否为路径

`对象.isFile()`判断对象是否为文件

`对象.lastModified()`获取最后修改时间

`对象.listFiles()`返回一个目录中文件的一个数组



#### ObjectOutputStream

将内存中的java对象拆分开，传输到硬盘文件中，叫序列化。Serialize

参与序列化的类型必须先实现Serializable接口。

transient关键字，表示不参与序列化。

#### ObjectInputStream

将拆分的对象组装，叫反序列化



java语言是采用什么机制来区分类的？

首先通过类名，其次通过序列化版本号。 `serialVersionUID`



























