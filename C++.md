# Linux



## 常用命令

- mkdir - make directories
- touch - create files
- rm - remove files or directories
- rm -rf 	删除目录
- cp - copy files and directories

```shell
#cp命令来复制一个文件
cp /home/zjs/myfile ./
# cp -r 表示递归复制，复制文件夹的时候需要加-r
#复制/home/zjs/myfolder文件夹到根目录下
cp -r /home/zjs/myfolder /
```

- mv - move(rename) files

作用：移动文件到新的位置，或者重命名文件

用法：mv需要移动的文件路径

```shell
#移动当前目录下myfile到根目录/下
mv myfile /myfile

#移动当前目录下myfolder文件夹到根目录下
mv myfolder /myfolder

#移动当前目录下myfile到根目录/下，并重命名为myfile007
mv myfile myfile007
```

- man - an interface to the system reference manuals

作用：包含了Linux中全部命令手册

用法：man [命令]

含义：查看命令使用手册，按q退出

```shell
#查看ls命令的手册
man ls
#查看cd命令的手册
man cd
#查看man命令的手册
man man
```

- reboot - reboot the machine
- shutdown - power-off the machine







## 编译过程

**1.预处理-Pre-Processing**			//.i文件

```shell
# -E 选项指示编译器仅对输入文件进行预处理
g++ -E test.cpp -o test.i	
```

**2.编译-Compiling**						//.s文件

```shell
# -s 编译选项告诉g++在为C++代码产生了汇编语言文件后停止编译
# g++ 产生的汇编语言文件的缺省扩展名是 .s
g++ -S test.i -o test.s
```

**3.汇编-Assembling**					//.o文件

```shell
# -c 选项告诉 g++仅把源代码编译为机器语言的目标代码
#缺省时 g++ 建立的目标代码文件有一个 .o 的扩展名
g++ -c test.s -o test.o		
#注意：这里的-c必须是小写
```

**4.链接-Linking**							//bin文件

```shell
# -o 编译选项来为将产生的可执行文件用指定的文件名
g++ test.o -o test
```





## 重要编译参数

**-g	编译带调试信息的可执行文件**

```shell
# -g 选项告诉 GCC 产生能被 GNU 调试器使用的调试信息，以调试程序
#产生带调试信息的可执行文件test
g++ -g test.cpp -o test
```

**-O[n]	优化源代码**

```shell
#优化，例如省略掉代码中从未使用的变量，直接将常量表达式用结果值代替等等，这些操作会缩减目标文件所包含的代码量，提高最终生成的可执行文件的运行效率。

# -O 选项告诉 g++ 对源代码进行基本优化
# -O2 选项告诉 g++ 产生尽可能小和尽可能快的代码。
# 如 -O2,-O3,-On (n常为0-3)
# -O0 代表不做优化 -O1 为默认优化 -O2 进行一些额外的调整工作，如指令调整等
#选项将使编译的速度比使用 -O 时慢，但通常产生的代码执行速度会更快

#使用 -O2优化源代码，并输出可执行文件
g++ -O2 test.cpp
```

**-l 和 -L 指定库文件 | 指定库文件路径**

```shell
# -l参数(小写)就是用来指定程序要链接的库，-l参数紧接着就是库名
# 在/lib和/usr/lib和/usr/local/lib里的库直接用-l参数就能链接

# 链接glog库
g++ -lglog test.cpp

#如果库文件没放在上面三个目录里，需要使用-L参数(大写)指定库文件所在目录
# -L参数跟着的是库文件所在的目录名

#链接mytest库，libmytest.so在/home/bing/mytestlibfolder目录下
g++ -L/home/bing/mytestlibfolder -lmytest test.cpp
```

**-I	(大写的i)	指定头文件搜索目录**

```shell
# -I
# usr/include目录一般是不用指定的，gcc知道去哪里找，但是如果头文件不在usr/include里，就需要用-I参数指定了，比如头文件放在/myinclude目录里，那编译命令行就要加上	-I/myinclude参数。-I参数可以用相对路径

g++ -I/myinclude test.cpp
```

**-Wall	打印警告信息**

```shell
#打印出gcc提供的警告信息
g++ -Wall test.cpp
```

**-w	关闭警告信息**

```shell
#关闭所有警告信息
g++ -w test.cpp
```

**-std=c++11**

```shell
#使用c++11 编译标准 test.cpp
g++ -std=c++11 test.cpp
```

**-o	指定输出文件名**

```shell
#指定即将产生的文件名
#指定输出可执行文件名为test
g++ test.cpp -o test
```

**-D	定义宏**

```shell
# 在使用gcc/g++编译的时候定义宏
# 常用场景：
# -DDEBUG 定义DEBUG宏，可能文件中有DEBUG宏部分的相关信息，用 -DDEBUG来选择开启或关闭DEBUG
```





**生成库文件并编译**

链接静态库生成可执行文件①：

```shell
##进入src目录下
$cd src

#汇编，生成swap.o文件
g++ Swap.cpp -c -I../include
#生成静态库libSwap.a
ar rs libSwap.a Swap.o

##回到上级目录
$cd ..

#链接，生成可执行文件:staticmain
g++ main.cpp -Iinclude -Lsrc -o staticmain
```

链接动态库生成可执行文件②：

```shell
##进入src目录下
$cd src

#生成动态库libSwap.so
g++ Swap.cpp -I../include -fPIC -shared -o
libSwap.so
```









## GDB调试器

GDB(GNU Debugger)是一个用来调试C/C++程序的功能强大的调试器，是Linux系统开发C/C++最常用的调试器

VSCode是通过调用GDB调试器来实现C/C++的调试工作的



GDB主要功能：

设置断点

使程序在指定的代码上暂停执行，便于观察

单步执行程序，便于调试

查看程序中变量值的变化

动态改变程序的执行环境

分析崩溃程序产生的core文件





### 调试参数

调试开始：执行gdb [exefilename]，进入gdb调试程序。

```shell
$(gdb)help(h)# 查看命令帮助，具体命令查询在gdb中输入help + 命令
$(gdb)run(r)# 重新开始运行文件(run-text: 加载文本文件，run-bin: 加载二进制文件)
# 单步执行，运行程序，停在第一行执行语句
$(gdb)start
$(gdb)list(T)# 查看原代码(list-n,从第n行开始查看代码。1ist+ 函数名: 查看具体函数)
$(gdb)set
# 设置变量的值
$(gdb)next(n)#单步调试(逐过程，函数直接执行)
$(gdb)step(s)# 单步调试 (逐语句: 跳入自定义函数内部执行)
$(gdb)backtrace(bt) # 查看函数的调用的栈顺和层级关系
$(gdb)frame(f) # 切换函数的栈帧
$(gdb)info(i)# 查看函数内部局部变量的数值
$(gdb)finish# 结束当前函数，返回到函数调用点
$(gdb)continue(c)# 继续运行
s(gdb)print(p) # 打印值及地址
```







## VSCode

**高频使用快捷键：**

| 功能                  | 快捷键           |
| --------------------- | ---------------- |
| 转到文件/其他常用操作 | Ctrl + P         |
| 打开命令面板          | Ctrl + Shift + P |
| 打开终端              | Ctrl + `         |
| 转到定义处            | F12              |
| 撤销操作              | Ctrl + Z         |
| 关闭当前文件          | Ctrl + W         |
| 当前行上/下移动       | Alt + Up/Down    |
| 变量统一重命名        | F2               |



**快捷键：编辑器与窗口管理**

1. 打开一个新窗口：Ctrl + Shift + N
2. 关闭窗口：Ctrl + Shift + w
3. 同时打开多个编辑器(查看多个文件)
4. 新建文件：Ctrl + N
5. 文件之间切换：Ctrl + Tab
6. 切出一个新的编辑器：Ctrl + \



**快捷键：格式调整**

1. 代码格式化：Ctrl + Shift + i
2. 移动到行首：Home
3. 移动到行尾：End
4. 编辑器全屏：F11
5. 查找替换：Ctrl + H
6. 注释掉选中代码：ALT+SHIFT+A



































## Makefile

![img](https://img-blog.csdnimg.cn/20201109214319194.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0tQMTk5NQ==,size_16,color_FFFFFF,t_70)



```makefile
target ... : prerequisites ...
    command
    ...
    ...
```

**target**

可以是一个object file（目标文件），也可以是一个执行文件，还可以是一个标签（label）。对于标签这种特性，在后续的“伪目标”章节中会有叙述。

**prerequisites**

生成该target所依赖的文件和/或target

**command**

该target要执行的命令（任意的shell命令）



8个.o文件，三个.h文件：

```makefile
objects = main.o kbd.o command.o display.o \
    insert.o search.o files.o utils.o

edit : $(objects)
    cc -o edit $(objects)

main.o : defs.h
kbd.o : defs.h command.h
command.o : defs.h command.h
display.o : defs.h buffer.h
insert.o : defs.h buffer.h
search.o : defs.h buffer.h
files.o : defs.h buffer.h command.h
utils.o : defs.h

.PHONY : clean
clean :
    rm edit $(objects)
```





### make的工作方式

GNU的make工作时的执行步骤如下：（想来其它的make也是类似）

1. 读入所有的Makefile。
2. 读入被include的其它Makefile。
3. 初始化文件中的变量。
4. 推导隐晦规则，并分析所有规则。
5. 为所有的目标文件创建依赖关系链。
6. 根据依赖关系，决定哪些目标要重新生成。
7. 执行生成命令。



### 文件搜寻

Makefile文件中的特殊变量 `VPATH` 就是完成这个功能的，如果没有指明这个变量，make只会在当前的目录中去找寻依赖文件和目标文件。如果定义了这个变量，那么，make就会在当前目录找不到的情况下，到所指定的目录中去找寻文件了。

```makefile
VPATH = src:../headers
```

上面的定义指定两个目录，“src”和“../headers”，make会按照这个顺序进行搜索。目录由“冒号”分隔。（当然，当前目录永远是最高优先搜索的地方）



### 伪目标

最早先的一个例子中，我们提到过一个“clean”的目标，这是一个“伪目标”，

```makefile
clean:
	rm *.o temp
```

当然，为了避免和文件重名的这种情况，我们可以使用一个特殊的标记“.PHONY”来显式地指明一个目标是“伪目标”，向make说明，不管是否有这个文件，这个目标就是“伪目标”。

```makefile
.PHONY : clean
```

只要有这个声明，不管是否有“clean”文件，要运行“clean”这个目标，只有“make clean”这样。于是整个过程可以这样写：

```makefile
.PHONY : clean
clean :
	rm *.o temp
```







### 嵌套执行make

在一些大的工程中，我们会把我们不同模块或是不同功能的源文件放在不同的目录中，我们可以在每个目录中都书写一个该目录的Makefile，这有利于让我们的Makefile变得更加地简洁，而不至于把所有的东西全部写在一个Makefile中，这样会很难维护我们的Makefile，这个技术对于我们模块编译和分段编译有着非常大的好处。

例如，我们有一个子目录叫subdir，这个目录下有个Makefile文件，来指明了这个目录下文件的编译规则。那么我们总控的Makefile可以这样书写：

```makefile
subsystem:
    cd subdir && $(MAKE)
```

定义$(MAKE)宏变量的意思是，也许我们的make需要一些参数，所以定义成一个变量比较利于维护。这两个例子的意思都是先进入“subdir”目录，然后执行make命令。











## CMake

### 语法特性

基本语法格式：指令(参数1 参数2 ...)

- 参数使用**括号**括起
- 参数之间使用**空格**或**分号**分开

**指令是大小写无关的，参数和变量是大小写有关的**

```cmake
set(Hello hello.cpp)
add_executable(hello main.cpp hello.cpp)
ADD_EXECUTABLE(hello main.cpp ${HELLO})
```

变量使用**${}**方式取值，但是在IF控制语句中是直接使用变量名





### 重要指令

- **cmake_minimum_required** - 指定CMake的最小版本要求

语法：cmake_minimum_required(VERSION versionNumber [FATAL_ERROR])

```cmake
# CMake最小版本要求为2.8.3
cmake_minimum_required(VERSION 2.8.3)
```

- **project** - 定义工程名称，并可指定工程支持的语言

语法：project(projectname [CXX] [C] [Java])

```cmake
#指定工程名为HELLOWORLD
project(HELLOWORLD)
```

- **set** - 显式的定义变量

语法：set(VAR [VALUE] [CACHE TYPE DOCSTRING[FORCE]])

```CMAKE
#定义src变量，其值为main.cpp hello.cpp
set(SRC sayhello.cpp hello.cpp)
```

- **include_directories** - 向工程添加多个特定的**头文件**搜索路径

语法：include_directories([AFTER|BEFORE][SYSTEM]dir1 dir2 ...)

```cmake
#将usr/include/myincludefolder和./include添加到头文件搜索路径
include_directories(/usr/include/myincludefolder ./include)
```

- **link_directories** - 向工程添加多个特定的**库文件**搜索路径

语法：link_directories(dir1 dir2 ...)

```cmake
#将usr/lib/myfolder 和 ./lib 添加到库文件搜索路径
link_directories(usr/lib/myfolder ./lib)
```

- **add_library** - 生成库文件

语法：add_library(libname[SHARED|STATIC|MODULE][EXCLUDE_FROM_ALL] source1 source2 ... sourceN)

```cmake
#通过变量SRC生成libhello.so共享库
add_library(hello SHARED ${SRC})
#SRC变量是预先定义的，比如可以把很多个.cpp文件声明给SRC变量，再用SRC变量把这些.cpp文件生成为共享库.so文件
```

- **add_compile_options** - 添加编译参数

语法：add_compile_options(<option>...)

```cmake
#添加编译参数 -Wall -std=c++11 -o2
add_compile_options(-Wall -std=c++11 -o2)
```

- **target_link_libraries** - 为target添加需要链接的共享库

语法：target_link_libraries(target library1<debug|optimized> library2...)

```cmake
#将hello动态库文件链接到可执行文件main
target_link_libraries(main hello)
#相当于指定g++编译器-l参数
```

- **add_subdirectory** - 向当前工程添加存放源文件的子目录，并可以指定中间二进制和目标二进制存放的位置

语法：add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL])

```CMAKE
#添加src子目录，src中需有一个CMakeLists.txt
add_subdirectory(src)
```

- **aux_source_directory** - 发现一个目录下所有的源代码文件并将列表存储在一个变量中，这个指令临时被用来自动构建源文件列表

语法：aux_source_directory(dir VARIABLE)

```cmake
#定义SRC变量，其值为当前目录下所有的源代码文件
aux_source_directory(. SRC)
#编译SRC变量所代表的源代码文件，生成main可执行文件
add_executable(main ${SRC})
```





### 常用变量

- **CMAKE_C_FLAGS**	gcc编译选项

- **CMAKE_CXX_FLAGS**	g++编译选项

```cmake
#在CMAKE_CXX_FLAGS编译选项后追击-std=c++11
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
```

- **CMAKE_BUILD_TYPE**	编译类型(Debug,Release)

```cmake
#设定变异类型为debug，调试时需要
set(CMAKE_BUILD_TYPE Debug)
#设定编译类型为release，发布时需要
set(CMAKE_BUILD_TYPE Release)
```

- **CMAKE_BINARY_DIR**

​		**PROJECT_BINARY_DIR**

​		**<projectname>_BINARY_DIR**

1. 这三个变量指代的内容是一致的。
2. 如果是in source build，指的就是工程顶层目录
3. 如果是out-of-source编译，指的是工程编译发生的目录
4. PROJECT_BINARY_DIR跟其他指令稍有区别，不过现在可以理解为一致的

- **CMAKE_SOURCE_DIR**

​		**PROJECT_SOURCE_DIR**

​		**<projectname>_SOURCE_DIR**

1. 这三个变量指代的内容是一致的，不论采用何种编译方式，都是工程顶层目录
2. 也就是在in source build时，他跟CMAKE_BINARY_DIR等变量一致
3. PROJECT_SOURCE_DIR跟其他指令稍有区别，现在也可以理解为一致的

- **CMAKE_C_COMPILER：指定C编译器**
- **CMAKE_CXX_COMPILER：指定C++编译器**
- **EXECUTABLE_OUTPUT_PATH：可执行文件输出的存放路径**
- **LIBRARY_OUTPUT_PATH：库文件输出的存放路径**





### CMake编译工程

CMake目录结构：项目主目录存在一个CMakeLists.txt文件

**两种方式设置编译规则：**

1. 包含源文件的子文件夹**包含**CMakeLists.txt文件，主目录的CMakeLists.txt通过add_subdirectory添加子目录即可
2. 包含源文件的子文件夹**未包含**CMakeLists.txt文件，子目录编译规则体现在主目录的CMakeLists.txt中



**编译流程**

**在linux平台使用CMake构建C/C++工程的流程如下：**

1. 手动编写CMakeLists.txt
2. 执行命令cmake PATH生成Makefile(PATH是顶层CMakeLists.txt所在的目录)
3. 执行命令make进行编译



```shell
#important tips
.	#表示当前目录
./	#表示当前目录

..	#表示上级目录
../	#表示上级目录
```





**两种构建方式**

- 内部构建(in-source build)：不推荐使用

内部构建会在同级目录下产生一大堆中间文件，这些文件不是我们最终所需要的，和工程源文件放在一起会显得杂乱无章

```cmake
##内部构建
#在当前目录下，编译本目录的CMakeLists.txt，生成Makefile和其他文件
cmake .
#执行make命令，生成target
make
```



- **外部构建(out-of-source build)： **	**推荐！！！**

将编译输出文件与源文件放到不同目录中

```cmake
##外部构建
#1.在当前目录下，创建build文件夹
mkdir build
#2.进入到build文件夹
#3.编译上级目录的CMakeLists.txt，生成Makefile和其他文件
cmake ..
#4.执行make命令，生成target
make
```









# C语法



## 预处理指令

#include	包含

#define	宏定义

```c
//预处理器定义常量
//格式 : #define name value
#define PI 3.1415926
```

#if	如果，则预定义

#else	否则预定义

#elif	否则如果，则预定义

#endif	结束条件判断

```c
#if SYS == 1
	#include "ibm.h"
#elif SYS == 2
	#include "vax.h"
#elif SYS == 3
	#include "mac.h"
#else 
	#include "general.h"
#endif
```



#ifdef	如果定义了，则

#ifndef	如果没定义，则

#undef	取消定义

```c
#ifdef MAVIS
	#include "horse.h"//如果已经用#define定义了MAVIS，则执行下面的指令
	#define STABLE 5
#else
	#include "cow.h"//如果没有定义MAVIS，则执行下面的指令
	#define STABLE 15
#endif
```





## 预定义宏

```c
__DATE__			//预处理的日期 Mmm dd yyyy 例如： Nov 11 2022
__FILE__			//表示当前源代码文件名的字符串字面量
__LINE__			//表示当前源代码文件中行号的整型常量
__STDC__			//设置为1时，表明实现遵循C标准
__STDC_HOSTED__		//本机环境设置为1，否则设置为0
__STDC_VERSION__	//支持C99标准，设置为199901L，支持C11标准，设置为201112L
__TIME__			//翻译代码的时间，格式为hh:mm:ss
```



**#line和#error:**

```c
#line指令重置__LINE__和__FILE__宏报告的行号和文件名。
#line 1000	//把当前行号重置为1000
#line 1000 "cool.c"	//把行号重置为10,把文件名重置为cool.c
```

#error指令让预处理器发出一条错误消息，消息包含指令中的文本。可能的话，编译过程应该中断。这样可以使用#error指令：

```c
#if __STDC_VERSION__ !=201112L
#error Not C11
#endif
```





## #typedef

给数据类型起别名。





## extern "C"

试想这样的情况:一个库文件已经用C写好了而且运行得很良好，这个时候我们需要使用这个库文件，但是我们需要使用C++来写这个新的代码。如果这个代码使用的是C++的方式链接这个C库文件的话，那么就会出现链接错误.我们来看一段代码:首先，我们使用C的处理方式来写一个函数，也就是说假设这个函数当时是用C写成的:

```c++
//f1.c
extern "C"
{
void f1()
{
return;
}
}

//这个extern表示f1函数在别的地方定义，这样可以通过
//编译，但是链接的时候还是需要
//链接上原来的库文件.
extern void f1();
int main()
{
f1();
return 0;
}
//也就是说，在编译test.cxx的时候编译器是使用C++的方式来处理f1()函数的，但是实际上链接的库文件却是用C的方式来处理函数的，所以就会出现链接过不去的错误:因为链接器找不到函数。

```





# C++语法

变量存在的意义：方便我们管理内存空间

常量：用于记录程序中不可更改的数据

C++定义常量的两种方式：

1. #define 宏常量
2. const修饰的变量



## 数据类型

数据类型存在的意义：给变量分配**合适的内存空间**

char < short < int < long < long long 

short至少2 bytes

long至少4 bytes

long long 至少8 bytes



### **1.整型**

| 数据类型  | 占用空间 |           取值范围           |
| :-------- | :------- | :--------------------------: |
| short     | 2 bytes  | -2^15 ~ 2^15-1(-32768~32767) |
| int       | 4 bytes  |        -2^31 ~ 2^31-1        |
| long      | 4 bytes  |        -2^31 ~ 2^31-1        |
| long long | 8 bytes  |        -2^63 ~ 2^63-1        |



**sizeof**关键字

sizeof(数据类型/变量)





### **2.浮点型**

单精度float，至少32位

双精度double，至少48位

科学计数法：

```c++
float f2 = 3e2;//3*10^2
float f3 = 3e-2//3*0.1^2
```



### 3.字符型

```c++
char ch = 'a';
//ASCII码常见值：A-65 a-97
```



### 4.字符串

两种风格的字符串：

1. C风格字符串：

```c++
char str1[] = "hello world";
```

2. C++风格字符串：

```c++
string str2 = "hello world";
```



### 5.布尔类型

bool类型只有两个值： true和false

```c++
bool flag = true;
```



### 数据的输入

```c++
//把用户输入的数据赋值给a
int a = 10;
cin >> a;
```







## 程序流程结构

C/C++支持最基本的三种程序运行结构：顺序结构，选择结构，循环结构



```c++
//系统随机生成随机数
int num = rand()%100+1;
//添加随机数种子，利用当前系统时间生成随机数，防止每次随机数都一样
srand((unsigned int)time(NULL));
```





### break语句

1. 出现在switch语句中：退出switch语句

2. 出现在循环中：退出循环
3. 出现在嵌套循环中：退出内层循环(如果break写在内层循环里)



### continue

跳过当前循环尚未执行的语句，直接执行下一次的循环。



### goto语句

语法：`goto 标记;`

解释：如果标记的名称存在，执行到goto语句时，会跳转到标记的位置







## 数组

数组就是一个集合，里面放了相同类型的数据元素。

数组是由连续的内存位置组成的。



### 一维数组

一维数组定义的三种方式：

```c++
int arr[10];
int arr[10] = {值1,值2,...};
int arr[] = {值1,值2,...};
//sizeof()函数可以统计整个数组占用内存大小
sizeof(arr);
```



一维数组名称用途：

1. 可以统计整个数组在内存中的长度
2. 可以获取数组在内存中的首地址

注意：数组名是常量，不可以进行赋值操作。



两种拿到数组地址的方法：

```c++
double wages[3] = {0};
short stacks[3] = {0};
double * pw = wages;//数组的名字就是地址
short * ps = &stack[0];//取地址符，数组的第一个元素就是地址
```





### 二维数组

```c++
int arr[2][3];//定义一个2行3列的二维数组
```







## 函数

C++的函数必须写在main函数上方。



### 函数的声明

正常情况下，自定义的函数是不能写在main函数下面的。

但是如果对函数进行声明，则自定义函数就可以写在main函数的下面了。

注意：声明可以写多次，但是定义只能有一次。

```c++
//函数的声明
int sum(int num1, int num2);
//main函数
int main() {
	int a = 3;
	int b = 4;
	int result = sum(a, b);
	cout << result << endl;
	system("pause");
	return 0;
}
//函数的定义
int sum(int num1, int num2) {
	return num1 + num2;
}
```



### 函数的参数

**函数默认参数：**

如果自己传入数据，就用自己的数据，如果没有，则用默认值。

如果某个位置已经有了默认参数，那么从这个位置往后，必须有默认值

声明和实现只能有一个默认参数



**站位参数：**

```c++
void func(int a, int){
    cout << "this is func" << endl;
}
```





### 函数重载

满足条件：

- **同一个作用域下**
- **函数名称相同**
- **参数列表不同**(参数类型不同 / 参数个数不同 / 参数顺序不同)



**注意事项：**

1.引用作为重载的条件

2.函数重载碰到默认参数

```c++
void func(int &a){
    
}
void func(const int &a){
    //可以构成重载，两个参数的类型不同。
}
```





### 分文件编写

函数分文件编写一般有4个步骤：

1. 创建后缀名为.h的头文件
2. 创建后缀名为.cpp的源文件
3. 在头文件中写函数的声明
4. 在源文件中写函数的定义







## 指针

可以通过指针来**保存一个地址**。

```c++
int a =10;
int *p;//定义指针
p = &a;//让指针记录变量a的地址
```

**解引用：**

指针前加一个*号，找到指针指向的内存中的数据。



**指针所占内存空间：**

32位下占4个字节，64位下占8个字节。



**空指针：**

指针变量指向内存中编号为0的空间

用途：初始化指针变量

注意：空指针的内存是不可以访问的

0-255之间的内存编号是系统占用的，因此不可以访问



**野指针：**

指针变量指向非法的内存空间



### const修饰*p

1. const修饰指针 ---常量指针

```c++
//常量指针：指针的指向可以修改，指针指向的值不可以修改
const int* p = &a;
*p=20;//错误，指针指向的值不可修改
p = &b;//正确，指针的指向可以修改
```

2. const修饰常量 ---指针常量

```c++
//指针的指向不可以改，指针指向的值可以改
int * const p = &a;
p = &a;//错误，指针的指向不可以修改
*p = 20;//正确，指向的值可以改
```

3. const既修饰指针，又修饰常量

```c++
const int* const p = &a;
//指针的指向和指向的值都不可以改
```



### 指针和数组

```c++
int arr[10] = {1,2,3,4,5,6,7,8,9,10};
int *p = arr;//arr就是数组首地址
//利用指针遍历数组
for(int i=0;i<10;i++){
    cout<<*p<<endl;//*p解引用拿到数组的元素
    p++;//指针向后移4个字节
}
```



### 指针和参数

```c++
void swap(int *p1, int *p2){
    int temp = *p1;
    *p1 = *p2;
    *p2 = temp;
}
```







## 结构体

结构体属于用户自定义的数据类型，允许用户存储不同的数据类型

语法：`struct 结构体名 {结构体成员列表};`

```c++
struct Student {
	//成员列表
	string name;
	int age;
	double score;
};

int main() {
	struct Student s1 = {"张三",19,80};
    return 0;
}
```





### 结构体数组

```c++
struct Student {
	string name;
	int age;
	double score;
};

int main() {
	//创建结构体数组
	struct Student stuArray[3] = {
		{"张三",19,70},
		{"李四",20,90},
		{"王五",29,60}
	};
	//给结构体数组中的元素赋值
	stuArray[2].name = "赵六";
	cout << stuArray[2].name << endl;
	system("pause");
	return 0;
}
```



### 结构体指针

```c++
//1.创建学生结构体变量
struct student s = {"张三",21,97};
//2.通过指针指向结构体变量
struct student *p = &s;
//3.通过指针访问结构体变量中的数据
cout << "姓名:" << p->name<<"年龄:"<<p->age<<endl;
```





### 结构体嵌套

作用：结构体中的成员可以是另一个结构体

例如：每个老师辅导一个学员，一个老师的结构体中，记录一个学生的结构体

示例：

```c++
//定义学生结构体
struct student {
	string name;
	int age;
	double score;

};
//定义老师结构体
struct teacher {
	int id;
	string name;
	int age;
	struct student stu;
};
//创建老师结构体对象
int main() {
	teacher t1 = { 1000,"李老师",50,{"张同学",26,70}};
	teacher* p = &t1;
	cout << p->id << p->name << p->age << p->stu.name <<p->stu.age<<p->stu.score<< endl;
	system("pause");
	return 0;
}
```





### 做函数参数

```c++
void printStudent(struct student *p){
    p->name = "王五";//此时改变的是指针p指向的地址中的值
}
```

注意：如果是值传递，在函数体内修改变量不会影响实际参数。

​			如果是引用传递，在函数体内修改变量会影响实参中的变量值。





### 结构体const

思考：值传递和地址传递的优劣？

值传递是复制一份数据，传到函数的形参中，不会改变原数据的值。

地址传递是拿到内存中的地址，这样不需要再浪费内存创建副本，减少内存开销，但是如果在函数内对参数进行操作，就会改变原变量的值。

如果既想节省空间，使用地址传递，又想不改变原参数的值，就要使用const关键字：

```c++
void printStudents(const student *s){
    //不能对参数s进行修改
}
```

总结：使用指针是为了节省空间，不用再复制一份。

​			使用const是为了防止函数对数据误操作/修改。







## 内存分区

C++程序在执行时，将内存划分为4大区域：

- 代码区：放函数体的二进制代码，由操作系统进行管理
- 全局区：全局变量，静态变量，常量
- 栈区：由编译器自动分配释放，存放函数的参数值，局部变量等
- 堆区：由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收



**内存四区意义：**

不同区域存放的数据，赋予不同的生命周期，更灵活的编程。



### **程序运行前**

在程序编译后，生成了exe可执行程序，未执行前程序分为两个区域。

**代码区：**

​			存放CPU执行的机器指令

​			代码区是**共享的**，共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可

​			代码区是**只读的**，使其只读的原因是防止程序意外的修改了它的指令

**全局区：**

全局变量和静态变量存放在此

全局区还包含了常量区，字符串常量和其他常量

该区域的数据在程序结束后由**操作系统释放**





### 程序执行后

**栈区：**

​	有编译器自动分配释放，存放函数的参数值，局部变量等

​	注意：**不要返回局部变量的地址**，栈区开辟的数据由编译器自动释放



**堆区：**

**由程序员分配释放**，若程序员不释放，程序结束时由操作系统回收

在C++中主要利用**new**在堆区开辟内存





### new运算符

C++中用new操作符在堆中开辟数据

堆区开辟的数据，由程序员手动开辟，手动释放，释放利用delete操作符

利用new创建的数据，会返回该数据对应类型的指针

```c++
int* func() {
	//利用new关键字 可以将数据开辟到堆区
	//指针本质也是一个局部变量，放在栈上
	//指针保存的数据存放在堆区
	int * p = new int(10);
	return p;
}
int main() {
	int* p = func();
	cout << *p << endl;
	cout << *p << endl;
	cout << *p << endl;
	delete p;//释放内存
	cout << *p << endl;//此时会报错，因为已经
	system("pause");
	return 0;
}
```



**注意：**

- 不要使用delete来释放不是new分配的内存
- 不要使用delete释放同一块内存两次
- 如果使用new[]为数组分配内存，则应使用delete[]来释放
- 对空指针应用delete是安全的





## 引用

作用：给变量起别名

语法：`数组类型 &别名 = 原名`

```c++
int a = 10;
int &b = a;
```

注意：

1. 引用必须要初始化

2. 一旦初始化后，不可以改变



### 做函数参数

```c++
//交换函数
//1.值传递
void mySwap01(int a,int b){//值传递，形参不会修饰实参
    int temp = a;
    a = b;
    b = temp;
}
//2.地址传递
void mySwap02(int *a,int *b){//地址传递，形参会修饰实参
    int temp = *a;
    *a = *b;
    *b = temp;
}
//3.引用传递
void mySwap03(int &a,int &b){//引用传递，形参会修饰实参
    int temp = a;
    a = b;
    b = temp;
}
```





### 引用做函数返回值

```c++
//1.不要返回局部变量的引用
int& test01() {
	int a = 10;//局部变量存放在栈区中
	return a;
}
//2.函数的调用可以作为左值
int& test02() {
	static int a = 10;//静态变量，存放在全局区,程序结束后内存才会释放
	return a;
}
int main() {
	int &ref = test01();
	cout << "ref=" << ref << endl;//第一次结果正确，因为编译器有保留
	cout << "ref=" << ref << endl;//第二次结果错误，因为a的内存已经释放
    
    int &ref2 = test02();
    cout << "ref2" << ref2 <<endl;//10
    test02() = 1000;//函数的调用可以为左值
    cout << "ref2" <<endl;//1000
}
```

**总结：**

1. 不要返回局部变量的引用

2. 函数的调用可以作为左值





### 引用的本质

引用的本质在c++内部实现是一个指针常量

![image-20221021005708507](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221021005708507.png)

C++推荐使用引用，因为语法方便





### 常量引用

使用场景：用来修饰形参，**防止误操作**

```c++
//引用使用的场景，通常用来修饰形参
void showValue(const int& v){
    //v+=10;
    cout << v <<endl;
}

int main(){
    //int& ref = 10; 引用本身需要一个合法的内存空间，因此错误。
    //加入const就可以，编译器优化代码，int temp=10; const int& ref = temp;
    const int& ref = 10;//值和引用都不能改了
}
```







## 面向对象



### 封装

类中的属性和行为统一称为成员：成员属性，成员方法

#### 访问权限

1. public	成员	类内和类外都可以访问
2. protected   成员   类内可以访问 类外不可以   子类可以访问父类中的保护内容
3. private   成员   类内可以访问 类外不可以    子类不能访问父类中的保护内容

**总结：**

​	类内和类外可以访问public内容

​	类外不可以访问private和protected内容





**struct和class的区别？**

struct默认权限是公共

class默认权限是私有



#### 成员属性

成员属性可以设置为私有，然后对外开放一些函数操作私有属性。

优点1：将所有成员属性设置为私有，可以自己控制读写权限

优点2：对于写权限，可以检测数据的有效性

```c++
class Person {
private:
	string name;
	int age;
	string lover;

public:
	//姓名可读可写
	void setName(string name) {
		this->name = name;
	}
	string getName() {
		return this->name;
	}
	//年龄只读
	int getAge() {
		this->age;
	}
	//情人只写
	int setLover(string lover) {
		this->lover = lover;
	}
};
```



### 对象的构造

#### 构造和析构

对象的初始化和清理：

```c++
class Person{
    //1.构造函数,没有返回值
    //2.函数名与类名相同
    //3.构造函数可以有参数，可以发生重载
    //4.在创建对象时，构造函数会自动调用，且只调用一次
    //5.编译器会自动提供一个无参构造
public:
    Person(){}
	}
	//析构函数，进行清理的操作
	//没有返回值
	//函数名和类名相同 在名称前加上~
	//析构函数不可以有参数，不可以重载
	//在对象销毁前，会自动调用析构函数，而且只会调用一次
	~Person(){
    }

int main(){
    Person p;
}
```





#### 构造分类

两种分类方式：

1. 按参数分类：有参构造和无参构造
2. 按类型分类：普通构造和拷贝构造

三种调用方式：

1. 括号法
2. 显示法
3. 隐式转换法

```c++
class Person {
private:
	string name;
	int age;
public:
	//无参构造
	Person() {

	}
	//有参构造
	Person(string name, int age) {
		this->name = name;
		this->age = age;
	}
	//拷贝构造函数
	Person(const Person &p) {
		//将传入的人身上的所有属性拷贝到我身上
		name = p.name;
		age = p.age;
	}
};

//调用
void test01() {
	//1.括号法
	Person p;//默认构造函数调用，调用默认构造不要加()
	Person p2("张三", 20);//有参构造函数
	Person p3(p2);//拷贝构造函数
	//2.显示法
	Person p1;
	Person p2 = Person("张三", 20);//有参构造
	//3.隐式转换法
	Person p4 = 10;//相当于写了 Person p4 = Person(10);
}
```





#### 拷贝构造

C++中拷贝构造函数调用时机通常有三种情况：

- 使用一个已经创建完毕的对象来初始化一个新对象
- **值传递**的方式给函数参数传值
- **值方式**返回局部对象





#### 构造函数调用规则

默认情况下，C++编译器至少给一个类添加三个函数

1. 默认无参构造函数
2. 默认析构函数
3. 默认拷贝构造函数，对属性进行值拷贝



**规则：**

如果用户定义有参构造函数，C++不再提供无参构造函数。(和Java一样)

如果用户定义拷贝构造函数，C++不再提供其他构造函数。





#### 深拷贝和浅拷贝

浅拷贝：简单的赋值操作

深拷贝：在堆区重新申请空间，进行拷贝操作



浅拷贝带来的问题：堆区的内存重复释放

浅拷贝的问题要用深拷贝来解决：

```c++
//自己实现拷贝构造函数解决浅拷贝带来的问题
Person(const Person &p){
    cout << "Person 拷贝构造函数调用" << endl;
    m_Age = p.m_Age;
    //m_Height = p.m_Height; 编译器默认实现这行代码，浅拷贝
    //重写-深拷贝操作,在堆内存重新申请空间，防止堆区的内存重复释放
    m_Height = new int(*p.m_Height);
}
~Person(){
    //析构代码，将堆区开辟数据做释放操作
    if(m_Height!=NULL){
        delete m_Height;//析构
        m_Height = NULL;//防止野指针问题
    }
}
```

**总结：**如果属性有在**堆区**开辟的，一定要自己提供拷贝构造函数，防止浅拷贝带来的问题。





#### 初始化列表

C++提供了初始化列表语法，用来初始化属性

语法：`构造函数()：属性1(值1)，属性2(值2)...{}`

```c++
class Person{
private:
    int a;
    int b;
    int c;
public:
    //传统初始化操作
    /*Person(int a,int b,int c){
        this->a = a;
        this->b = b;
        this->c = c;
    }*/
    //初始化列表初始化属性
    Person():a(10),b(20),c(30)
};

void test01(){
    //Person p(10,20,30);
    Person p;
}
```





#### static

**静态成员变量：**

- 所有对象共享同一份数据
- 在编译阶段分配内存
- 类内声明，类外初始化

**静态成员函数：**

- 所有对象共享同一个函数
- 静态成员函数只能访问静态成员变量

因为非静态成员变量是属于对象的，用静态函数调用，无法区分该变量属于哪个对象







### this和const

C++中，类的成员变量和成员函数是分开存储

只有非静态成员变量才属于类的**对象**上



空对象占用内存空间为1。

C++编译器会给每个空对象也分配一个字节空间，是为了区分空对象占内存的位置。

每个空对象也应该有个独一无二的内存地址。

```c++
class Person{
    int m_A;//非静态成员变量				属于类的对象上
    static int m_B;//静态成员变量			不属于类的对象上
    void func(){}//非静态成员函数			不属于类的对象上	
    static void func2(){}//静态成员函数	不属于类的对象上
}
```

**总结：**只有非静态成员变量属于类的对象上





**this指针：**

思考：通过对象模型，我们知道成员变量和成员函数是分开存储的，也就是每一个非静态成员函数只会诞生一份函数实例，多个同类型对象只会共用一块代码。

问题：这块代码是如何区分是哪个对象调用的自己呢？

C++通过this指针解决，**this指针指向调用成员函数的那个对象**。



**this指针作用：**

1. 解决名称冲突
2. 返回对象本身用*this





**const修饰成员函数**

**常函数：**

- 成员函数加const后称为常函数
- 常函数内不可以修改成员属性
- 成员属性声明时加关键字mutable，在常函数中依然可以修改



**常对象：**

- 声明对象前加const称该对象为常对象
- 常对象只能调用常函数



常函数示例：

```c++
class Person {
public:
	int m_A;
	mutable int m_B;//特殊变量
	//this指针的本质是 指针常量 指针的方向是不可以修改的
	//const Person * const this
	//在成员函数后面加const，修饰的是this指针，让指针指向的值也不可修改
	void showPerson() const{
		//this->m_A = 100; 报错，不能修改
		this->m_B = 100;//可以修改 mutable修饰
	}
};
```

常对象示例：

```c++
void test02(){
    const Person p;//实例化一个常对象
    //p.m_A = 100; 报错
    p.m_B = 100;//成功修改，mutable修饰的特殊值
    p.showPerson();//常对象可以调用常函数
    //p.func(); 报错，常对象不能调用普通函数，因为普通函数可以修改属性
}
```





### friend友元

生活中你的家客厅(public)，你的卧室(private)。

程序中有些私有属性也想让类外的特殊的一些函数或者类进行访问，就需要用到友元技术。



**友元的三种实现：**

- 全局函数做友元
- 类做友元
- 成员函数做友元



```c++
//全局函数做友元
//在类的最上方声明友元函数
class Building{
    //goodGay全局函数是Building好朋友，可以访问Building中私有成员
    friend void goodGay(Building *building);
public:
	string SittingRoom;
private:
	string BedRoom;
}
void goodGay(Building *building){
    cout << "好基友全局函数正在访问：" << building.SittingRoom << endl;
	cout << "好基友全局函数正在访问：" << building.BedRoom << endl;
}
```



```c++
//类做友元
class Building{
    //goodGay类是本类的好朋友，可以访问本类的私有成员
    friend class goodGay;
}
```



```c++
//成员函数做友元
class Building{
    //告诉编译器，goodGay类中的visit()成员函数是Building的好朋友
    //可以访问私有内容
    friend void goodGay::visit();
}
```





### 运算符重载



**1.加号运算符重载**

```c++
class Person {
public:
	int m_A;
	int m_B;
	//1.成员函数重载+号
	Person operator+(Person &p) {
		Person temp;
		temp.m_A = this->m_A + p.m_A;
		temp.m_B = this->m_B + p.m_B;
		return temp;
	}
};
//2.全局函数重载+号
Person operator+(Person& p1, Person& p2) {
	Person temp;
	temp.m_A = p1.m_A + p2.m_A;
	temp.m_B = p1.m_B + p2.m_B;
	return temp;
}
void test01(){
   	Person p1;
	Person p2;
	p1.m_A = 10;
	p1.m_B = 10;
	p2.m_A = 10;
	p2.m_B = 10;
    //成员函数重载本质调用
    //Person p3 = p1.operator+(p2);
	Person p3 = p1 + p2;
    //全局函数本质调用
    //Person p3 = operator+(p1,p2);
}
```





**2.左移运算符重载**

作用：可以输出自定义类型，类似重写toString()方法

```c++
class Person {
    //为保证重载左移运算符可以输出私有成员属性，需要用友元来保证此方法可以访问Person类私有属性
	friend ostream& operator<<(ostream& cout, Person p);
private:
	int m_A;
	int m_B;
};
//只能利用全局函数重载左移运算符
ostream &operator<<(ostream &cout,Person p) {
	//简化 cout<<p
	cout << "m_A=" << p.m_A << endl << "m_B=" << p.m_B << endl;
	return cout;
}
```



**3.递增运算符重载**

```c++
class MyInteger {
	friend ostream& operator<<(ostream& cout, MyInteger myint);
private:
	int m_Num;
public:
	MyInteger() {
		m_Num = 0;
	}
	//重载前置++运算符
	//这里返回引用是为了保证操作的是同一个数据
	MyInteger& operator++() {
		//先做++运算，再将自身返回
		m_Num++;
		return *this;
	}
	//重载后置++运算符
	//int代表了站位参数，可以用于区分前置后置递增
	MyInteger operator++(int) {
		//先记录当时的结果
		//再递增
		//返回当时记录的结果
		MyInteger temp = *this;
		m_Num++;
		return temp;
	}
};
```

总结：前置递增返回的是引用，后置递增返回的是值





**4.赋值运算符重载**

C++编译器至少给一个类添加4个函数

1. 默认构造函数
2. 默认析构函数
3. 默认拷贝函数
4. 赋值运算符operator=，对属性进行值拷贝



思考：如果不重载赋值运算符会出现什么问题？

```c++
class Person {
public:
	Person(int age) {
		m_Age = new int(age);
	}
	int *m_Age;
	~Person() {
		if (m_Age != NULL) {
			delete m_Age;
			m_Age = NULL;
		}
	}
};
void test03() {
	Person p1(18);
	Person p2(20);
	p2 = p1;//赋值操作,这里是浅拷贝
	//因为这里是浅拷贝，所以在析构函数调用时，会存在堆内存重复释放的问题
	cout << "p1的年龄为：" << *p1.m_Age << endl;
}
```

因此，需要重载赋值运算符解决浅拷贝的问题：

```c++
class Person{
    ...
    Person& operator=(Person &p){
        //因为要把p1的值深拷贝给p2，但是p2本身是有值的
        //所以应该先判断是否有属性在堆区，如果有先释放干净，然后再深拷贝
        if(m_Age!=NULL){
            delete m_Age;
            m_Age=NULL;
        }
        //深拷贝
        m_Age = new int(*p.m_Age);
        //返回对象本身
        return *this;
    }
}
```





**5.关系运算符重载**

```c++
class Person {
public:
	Person(string name, int age) {
		this->name = name;
		this->age = age;
	}
	//重载==运算符
	bool operator==(Person &p) {
		if (this->name == p.name && this->age == p.age) {
			return true;
		}
		return false;
	}
	string name;
	int age;
};
```





**6.函数调用运算符重载**

- 函数调用运算符()也可以重载
- 由于重载后使用的方式非常像函数的调用，因此称为仿函数
- 仿函数没有固定写法，非常灵活



```c++
class MyPrint {
public:
	//重载函数调用运算符
	void operator()(string test) {
		cout << test << endl;
	}
};

void MyPrint02(string test) {
	cout << test << endl;
}
void test01() {
	MyPrint myPrint;
	myPrint("hello world!");//仿函数
	MyPrint02("hello world!");//函数调用
}
```







### 继承

语法：`子类 : 父类`

子类的成员包含两大部分：

1. 从父类继承过来的
2. 自己增加的成员



#### 继承方式

- 公共继承
- 保护继承
- 私有继承

![image-20221023143223506](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221023143223506.png)

**总结：**

父类的私有成员，子类哪种方式都无法继承

公共继承中，父类的成员在子类不变

保护继承中，父类的公共成员在子类中变成保护成员

私有继承中，父类中成员在子类中变成私有成员





#### 对象模型

父类中所有非静态成员属性都会被子类继承下去

父类中私有成员属性被编译器隐藏了，因此访问不到，但的确被继承下去了



```shell
#利用开发人员命令提示工具查看对象模型：
#跳转文件路径 cd 具体路径下
cd /d1 reportSingleClassLayout类名 文件夹
```



**继承中构造和析构顺序：**

先构造父类，再构造子类

先析构子类，再析构父类



#### 继承语法

**访问父类同名成员：**

如果通过子类对象，访问到父类中的同名成员，需要加作用域

```c++
//同名成员
子类对象.父类名::同名成员;
s.Base::m_A;
//同类成员函数
s.Base::func();
//如果子类中出现和父类同名的成员函数，子类的同名成员会隐藏掉父类中所有的同名成员函数
//如果想访问到父类中被隐藏的同名成员函数，需要加作用域
//相当于子类重写了父类的方法
s.Base::func(100);
```





**继承同名静态成员处理方式：**

```c++
//通过类名访问
cout << Son::m_A << endl;
//第一个::代表通过类名访问，第二个::代表访问父类作用域下
cout << Son::Base::m_A << endl;
```





**多继承语法：**

```c++
class 子类:继承方式 父类1,继承方式 父类2,...
class Son:public Father1,public Father2...
```

C++实际开发中不建议使用多继承





#### 菱形继承

两个子类继承同一个父类，然后又有某个类继承了这两个子类。

![image-20221023151028248](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221023151028248.png)

**菱形继承问题：**

1. 羊和陀分别继承了动物的数据，当草泥马使用数据时，会产生二义性
2. 草泥马继承自动物的数据继承了两份，但其实这份数据一份就够了





解决方法：利用虚继承解决菱形继承的问题。

继承之前 加上 **virtual**变成虚继承。

```c++
//动物类
class Animal {
public:
	int age;
};
//羊类-继承动物类
class Sheep : virtual public Animal {};
//陀类-继承动物类
class Camel : virtual public Animal {};
//羊驼类-继承羊类和陀类
class Alpaca : public Sheep, public Camel {};
//测试函数
void test01(){
    Alpaca yt;
    yt.Sheep::age = 10;
    yt.Camel::age =20;
    //虚继承解决这个问题后，age只有一份了
    cout << yt.age << endl;//20
}
```







### 多态

**静态多态：**函数重载和运算符重载

**动态多态：**子类和虚函数实现运行时多态



**静态多态和动态多态的区别：**

静态多态的函数地址早绑定 - 编译阶段确定函数地址

多态多态的函数地址晚绑定 - 运行阶段确定函数地址



**动态多态执行条件：**

1. 子类继承父类
2. 子类重写父类虚函数



父类的指针/引用指向子类的对象



```c++
vfptr - 虚函数指针: v - virtual	f - function	ptr - pointer
```

虚函数指针指向虚函数表 vftable





#### 抽象类

纯虚函数语法： `virtual 返回值类型 函数名(参数列表) = 0;`

当类中有了纯虚函数，这个类也称为抽象类



**抽象类特点：**

- 无法实例化对象
- 子类必须重写抽象类中的纯虚函数，否则也属于抽象类





#### 纯虚析构

多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码

解决方式：将父类中的析构函数改为虚析构和纯虚析构



**虚析构和纯虚析构共性：**

- 可以解决父类指针释放子类对象
- 都需要有具体的函数实现

**虚析构和纯虚析构区别：**

- 如果是纯虚析构，该类属于抽象类，无法实例化对象



虚析构语法：

`virtual ~类名(){}`

纯虚析构语法：

`virtual ~类名() = 0;`
`类名::~类名(){}`



**总结：**

1. 虚析构或纯虚析构就是用来解决通过父类指针释放子类对象
2. 如果子类中没有堆区数据，可以不写虚析构或纯虚析构
3. 拥有纯虚析构函数的类也属于抽象类







### 文件操作

程序运行时产生的数据都属于临时数据，程序一旦结束都会被释放。

通过文件可以将数据持久化

C++中对文件操作需要包含头文件<fstream>



文件类型分为：

1. 文本文件，文件以文本的ASCII码存储在计算机中
2. 二进制文件，文件以文本的二进制形式存储在计算机中



**操作文件的三大类：**

1. `ofstream`：写操作
2. `ifstream`：读文件
3. `fstream`：读写文件

```c++
//1.包含头文件
#include <fstream>
//2.创建流对象
ofstream ofs;
//3.打开文件
ofs.open("文件路径",打开方式);
//4.写数据
ofs << "写入的数据";
//5.关闭文件
ofs.close();
```



**文件打开方式：**

| 打开方式    | 解释                         |
| ----------- | ---------------------------- |
| ios::in     | 为读文件而打开文件           |
| ios::out    | 为写文件而打开文件           |
| ios::ate    | 初始位置：文件尾             |
| ios::app    | 追加方式写文件               |
| ios::trunc  | 如果文件存在，先删除，再创建 |
| ios::binary | 二进制方式                   |

注意：文件打开方式可以配合使用，利用|操作符

例如：用二进制方式写文件 `ios::binary |ios::out`



**读文件示例：**

```c++
//2.创建ifs对象
ifstream ifs;
//3.打开文件
ifs.open("text.txt", ios::in);
	
if (!ifs.is_open()) {
		cout << "文件打开失败！" << endl;
		return;
}
//4.读数据
string buffer;
while (getline(ifs, buffer)) {
		cout << buffer << endl;
}
//5.关闭文件
ifs.close();
```



**写二进制文件：**

```c++
//2.创建ifs对象
ofstream ofs;
//3.打开文件
ofs.open("person.txt", ios::out|ios::binary);
//4.读数据
Person p = { "张三",18 };
ofs.write((const char*)&p, sizeof(Person));
//5.关闭文件
ofs.close();
```

总结：

文件输出流对象可以通过write函数，以二进制的方式写数据



**二进制读文件：**

```c++
//2.创建ifs对象
ifstream ifs;
//3.打开文件
ifs.open("person.txt", ios::in|ios::binary);
if (!ifs.is_open()) {
	cout << "文件打开失败！" << endl;
	return;
}
//4.读数据
Person p;
ifs.read((char*)&p, sizeof(Person));
//5.关闭文件
ifs.close();
```







## 模板

C++另一种编程思想，称为**泛型编程**，主要利用的技术就是模板

C++提供两种模板机制：函数模板和类模板



### 函数模板

函数模板作用：

建立一个通用函数，其函数返回值类型和形参类型可以不具体指定，用一个**虚拟的类型**代表。

**语法：**

```c++
template<typename T>
函数声明或定义
```

**解释：**

template - 声明创建模板

typename - 表明其后面的符号是一种数据类型，可以用class代替

T - 通用的数据类型，名称可以替换，通常为大写字母



**示例：**

```c++
//函数模板-交换两个数
template<class T> //声明一个模板，T是一个通用的数据类型，typename可以替换成class
void mySwap(T &a, T &b) {
	T temp = a;
	a = b;
	b = temp;
}
```

```c++
//使用模板函数方法
//1.自动类型推导
mySwap(a,b);
//2.显示指定类型
mySwap<int>(a,b);
```

**注意：**

1. 自动类型推导，必须推导出一致的数据类型T才可以使用
2. 模板必须要确定出T的数据类型才可以使用



**普通函数和模板函数的区别：**

1. 普通函数调用可以发生隐式类型转换
2. 函数模板 用自动类型推导，不可以发生隐式类型转换
3. 模板函数 用显示指定类型，可以发生隐式类型转换



**普通函数和模板函数调用规则：**

1. 优先调用普通函数
2. 可以通过**空模板参数列表**来强制调用函数模板
3. 函数模板也可以发生重载
4. 如果函数模板可以产生更好的匹配，优先调用函数模板

```c++
//优先调用普通函数
myPrint(a, b);
//通过空模板参数列表，调用函数模板
myPrint<>(a, b);
```

总结：既然提供了函数模板，就不要提供普通函数了，容易出现二义性





**模板的局限性：**

利用具体化的模板，可以解决自定义类型的通用化

学习模板并不是为了写模板，而是为了在STL中运用系统提供的模板







### 类模板

**语法：**

```c++
template<typename T>
类
```



**类模板与函数模板的区别：**

1. 类模板没有自动类型推导的使用方式
2. 类模板在模板参数列表中可以有默认参数

```c++
template<class nameType,class ageType>
class Person{
public:
    string name;
    int age;
    Person(nameType name,ageType age){
        this->name = name;
        this->age = age;
    }
}
//类模板只能显示指定类型
Person<string,int>p("孙悟空",1000);
```



**类模板成员函数创建时机：**

**普通类**中的成员函数**一开始**就可以创建

**类模板**中的成员函数在**调用时**才创建





**类模板对象做函数参数：**

**三种方式：**

1. 指定传入的类型 - 直接显示对象的数据类型
2. 参数模板化  - 将对象中的参数变成模板进行传递
3. 整个类模板化 - 将这个对象类型 模板化进行传递

```c++
//1.指定传入类型
void printPerson1(Person<string,int>&p) {
	p.showPerson();
}
void test01() {
	Person<string, int>p("wukong", 100);
	printPerson1(p);
}

//2.参数模板化
template<class T1,class T2>
void printPerson2(Person<T1, T2>& p) {
	p.showPerson();
	cout << "T1的类型为:" << typeid(T1).name() << endl;
	cout << "T2的类型为:" << typeid(T2).name() << endl;
}
void test02() {
	Person<string, int>p("猪八戒", 90);
	printPerson2(p);
}

//3.整个类模板化
template<class T>
void printPerson3(T &p) {
	p.showPerson();
}
void test03() {
	Person<string, int>p("唐僧", 30);
}
```

**总结：**第一种最常用





**类模板与继承：**

- 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
- 如果不指定，编译器无法给子类分配内存
- 如果想灵活指定出父类中T的类型，子类也需变为类模板

```c++
template<class T>
class Base {
	T var;
};
class Son:public Base<int> {//错误，必须要知道父类中的T类型，才能继承给子类
};
//如果想灵活指定父类中T类型，子类也需要变类模板
template<class T1,class T2>
class Son2 :public Base<T2> {
	T1 obj;
};
```





**类模板成员函数类外实现：**

```c++
//类模板成员函数类外实现
template<class T1,class T2>
class Person {
public:
	T1 name;
	T2 age;
	Person(T1 name, T2 age);//构造函数声明
	void showPerson();//成员函数类内声明
};
//构造函数类外实现
template<class T1,class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->name = name;
	this->age = age;
}

//成员函数类外实现
template<class T1,class T2>
void Person<T1,T2>::showPerson(){
    cout << "姓名:" << this->name << "\t年龄:" << this->age << endl; 
}
```





**类模板分文件编写：**

**问题：**类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到



解决方法1：直接包含.cpp源文件

解决方法2：将声明和实现写到同一个文件中，并更改后缀名为.hpp





**类模板与友元：**

全局函数类内实现 - 直接在类内声明友元即可

全局函数类外实现 - 需要让编译器知道全局函数的存在

```c++
//提前让编译器知道Person的存在
template<class T1,class T2>
class Person;
//类外实现,加空模板参数列表
template<class T1,class T2>
void printPerson2<>(Person<T1,T2>p){
    cout << "姓名:" << p.name << "\t年龄:" << p.age << endl;
}
//类模板
template<class T1,class T2>
class Person{
    //全局函数 类内实现
    //因为是友元，所以他不是成员函数
    friend void printPerson(Person<T1,T2>p){
        cout << "姓名:" << p.name << "\t年龄:" << p.age << endl;
    }
    
    //全局函数 类外实现
    friend void printPerson2(Person<T1,T2>p);
};

```

全局函数类外实现：

类外实现要写在类模板的前面，并且让编译器知道Person的存在











# STL标准库

Standard Template Library - 标准模板库

STL六大组件：**容器，算法，迭代器，仿函数**，适配器，空间配置器



1. 容器：各种数据结构。例如：vector，list，deque，set，map等，用来存放数据
2. 算法：各种常用算法，如：sort，find，copy，for_each等。
3. 迭代器：扮演了容器和算法之间的胶合剂
4. 仿函数：行为类似函数，可作为算法的某种策略
5. 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西
6. 空间配置器：负责空间的配置与管理













# 通信

7层结构-IEEE

应用层

。。。

数据链路层：保证数据的可靠的，正确的传输

物理层：负责传输数据

有线：485，232，LAN，CAN

无线：4G，5G

带宽：每秒钟能传多少位

网线：8根线，13收，26发。另外四根为控制线。

电压传输信号，带宽=速度(波特率)，距离(分辨率)



问题：

1.重发

2.中断次数

3.发送间隔



网口：IP地址+port端口 = 唯一标签

串口：com1+com2+....+波特率 = 唯一

链路层协议：inner...,modbus,103,104



com.9600       物理层

modbus		 链路层      编程通信协议

设备(应用层)	设备模板--->工程



**Modbus通讯协议：**

是应用于**电子控制器上的一种通用语言**。通过此协议，控制器相互之间、控制器经由网络（例如以太网）和其它设备之间可以通信。

此协议定义了一个**控制器能认识使用的消息结构**,而不管它们是经过何种网络进行通信的。它描述了一控制器请求访问其它设备的过程，如果回应来自其它设备的请求，以及怎样侦测错误并记录。它制定了消息域格局和内容的公共格式。

1. **两种传输模式**

控制器能设置为两种传输模式（ASCII或RTU）中的任何一种在标准的Modbus网络通信。用户选择想要的模式，包括串口通信参数（波特率、校验方式等），在配置每个控制器的时候，在一个Modbus网络上的所有设备都必须选择相同的传输模式和串口参数。

**ASCII模式**

| 地址 | **功能代码** | **数据数量** | **数据1** | **...** | **数据n** | **LRC高字节** | **LRC低字节** | **回车** | **换行** |
| :--- | ------------ | ------------ | --------- | ------- | --------- | ------------- | ------------- | -------- | -------- |

**RTU模式**

| **地址** | **功能代码** | **数据数量** | **数据1** | **...** | **数据n** | **CRC低字节** | **CRC高字节** |
| -------- | ------------ | ------------ | --------- | ------- | --------- | ------------- | ------------- |





## 基本概念

**dll文件**

Dynamic Link Library，动态链接库。在windows系统中，许多应用程序并不是一个完整的可执行文件，它们被分割成一些相对独立的动态链接库，也就是dll文件，放置于系统中，当执行某一个程序时，相应的DLL文件就会被调用。

**so文件**

so文件就跟.dll差不多。

一般来说，so文件就是动态链接库，都是C或C++编译出来的，与Java相比通常是Class文件(字节码)。

Linux下的so文件是不能直接运行的，一般来讲，so文件称为共享库。



**波特率**

波特率：是**码元传输速率单位**，他说明单位时间传输了多少个码元。

如果在数字传输过程中，用0V表示数字0，5V表示数字1，那么每个码元有两种状态0和1. 每个码元代表一个二进制数字。此时的每秒码元数和每秒二进制代码数是一样的，这叫两相调制，波特率等于比特率。

如果在数字传输过程中，0V、2V、4V和6V分别表示00、01、10和11，那么每个码元有四种状态00、01、10和11. 每个码元代表两个二进制数字。此时的每秒码元数是每秒二进制代码数是一半的，这叫四相调制，波特率等于比特率一半。



**码元(Symbol)**

码元又称为“符号”，即"Symbol”。这是维基百科上对码元所做的解释:

A symbol is a state or significant condition of the communication channel that persists for a fixed period of time. A sending device places symbols on the channel at a fixed and known symbol rate, and the receiving device has the job of detecting the sequence of symbols in order to reconstruct the transmitted data. 直译：符号是通信信道持续一段固定时间的状态或重要条件。 发送设备以固定且已知的符号率将符号放置在信道上，而接收设备有检测符号序列的任务，以重建发送的数据。” 









## UART

Universal Asynchronous Receive Transmitter,通用异步收发器

是一种通用的串行，异步通信总线。该总线有两条数据线，可以实现全双工的发送和接收。在嵌入式系统中常用于主机与辅助设备之间的通信



**UART存在的问题：**

- **电气接口不统一**

UART只是对信号的时序进行了定义，而未定义接口的电气特性

- **抗干扰能力差**

UART一般直接使用TTL信号来表示0和1，但TTL信号的抗干扰能力较差，数据在传输过程中很容易出错

- **通信距离极短**

因为TTL信号的抗干扰能力较差，所以其通信距离也很短，一般只能用于一个电路板上的两个不同芯片之间的通信





### RS232

该标准规定采用一个标准的连接器，标准中对连接器的**每一个引脚**的作用加以规定，还对信号的电平加以规定。

- **接口**

RS-232接口一般只使用RXD,TXD,GND三条线。

- **信号**

该标准规定逻辑1的电平为-5到-15v，逻辑0的电平为+5到+15v。



**RS232存在的问题：**

接口的信号电平比较高，容易损坏接口电路的芯片。又因为与TTL电平不兼容，所以需要使用电平转换芯片才能与TTL电路连接。



### RS485

传输距离长，抗干扰能力强。

该标准允许连接多个收发器，可利用单一的RS485接口方便的建立起一个设备网络。



- 信号

差分信号进行数据传输，两线电压差为+2到+6v表示逻辑1，-2到-6v表示逻辑0。



- 接口

RS485采用两线制，这种接线方式为总线式拓扑结构，在同一总线上可以同时存在多个节点。

因为采用两线制，数据的发送和接收都要使用这对差分信号线，**发送和接收不能同时进行**，所以只能采用半双工的方式工作，编程时也要加以处理。









## IIC总线

IIC总线是一种串行，半双工总线。

主要用于近距离，低速的芯片之间的通信。

IIC总线有两根双向的信号线，一根SDA用于收发数据，一根时钟线SCL用于通信双方时钟同步。

IIC总线硬件结构简单，成本较低。



**主从机制：**

IIC总线是一种多主机总线，连接在IIC总线上的器件分为主机和从机。

主机有权发起和结束一次通信，而从机只能被主机呼叫。

当总线上有多个主机同时启用总线时，IIC也具备**冲突检测和仲裁**的功能来防止错误的产生。

每个连接到IIC总线上的器件有一个唯一的地址(7bits)，每个器件都可以作为主机或从机。





**IIC总线通信过程：**

1. 主机发送起始信号启用总线

2. 主机发送一个字节的数据，用来**指明从机地址**(高7位)和**后续字节的传送方向**(最后一位)

3. 被寻址的从机发送应答信号回应主机

4. 发送器发送一个字节数据

5. 接收器发送应答信号回应发送器

   ... ...(循环步骤4,5)

   通信完成后主机发送停止信号释放总线









## CAN总线



### 报文帧种类

**数据帧：**用于发送节点向接收节点传送数据，是使用最多的帧类型

**远程帧：**用于接收节点向某个发送节点请求数据

**错误帧：**用于当某节点检测出错误时向其他节点通知错误的帧

**过载帧：**用于接收节点向发送节点通知自身接收能力的帧

**帧间隔：**将数据帧或远程帧与前面的帧分离的帧



数据帧：标准帧和扩展帧

![image-20221114090241778](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221114090241778.png)



![image-20221118155420761](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221118155420761.png)



![image-20221114100021808](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221114100021808.png)







## socketCAN

设置参数：接口，波特率

```c
#define ip_cmd_set_can0_params "ip link set can0 type can bitrate 1000000 triple-sampling on"
#define ip_cmd_can0_up         "ifconfig can0 up"
#define ip_cmd_can0_down       "ifconfig can0 down"
 
// 使用系统调用函数运行以上命令，也可以自行在终端中运行
system(ip_cmd_set_can0_params); // 设置参数
system(ip_cmd_can0_up);  // 开启can0接口
```







## Modbus协议



**硬件层协议：解决0和1的可靠传输，常有RS232、RS485、CAN、IIC、SPI …**
**软件层协议：解决传输目的,常有Modbus、TCP/IP、CANopen …**



### 通信过程

Modbus是**一主多从**的通信协议。

Modbus通信中**只有一个设备可以发送请求**。其他从设备接收主机发送的数据来进行响应，从机是任何外围设备，如I/O传感器，阀门，网络驱动器，或其他测量类型的设备。从站处理信息和使用Modbus将其数据发送给主站。

**也就是说,不能Modbus同步进行通信,主机在同一时间内只能向一个从机发送请求，总线上每次只有一个数据进行传输,即主机发送,从机应答,主机不发送,总线上就没有数据通信。**

从机不会自己发送消息给主站，只能回复从主机发送的消息请求。

![在这里插入图片描述](https://img-blog.csdnimg.cn/6545b8a54f6841309802ceb9586b17bf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWuWwj-aXiw==,size_20,color_FFFFFF,t_70,g_se,x_16)





### 存储区

存储的数据类型可以分为 ：布尔量 和 16位寄存器

- **布尔量**比如IO口的电平高低，灯的开关状态等。
- **16位寄存器**比如 传感器的温度数据，存储的密码等。



**Modbus协议规定了4个存储区 分别是0 1 3 4区 其中1区和4区是可读可写，1区和3区是只读。**

| 区号 | 名称       | 读写           | 地址范围    |
| ---- | ---------- | -------------- | ----------- |
| 0区  | 输出线圈   | 可读可写布尔量 | 00001-09999 |
| 1区  | 输入线圈   | 只读布尔量     | 10001-19999 |
| 3区  | 输入寄存器 | 只读寄存器     | 30001-39999 |
| 4区  | 保持寄存器 | 可读可写寄存器 | 40001-49999 |

并且Modbus还给每个区都划分了**地址范围，** **主机向从机获取数据时，只需要告诉从机数据的起始地址，还有获取多少字节的数据，从机就可以发送数据给主机**

Modbus数据模型规定了具体的地址范围，每一个从机，都有实际的物理存储，跟modbus的存储区相对应，**主机读写从机的存储区，实际上就是对从机设备对应的实际存储空间进行读写。**





### 协议类型

Modbus可以在各种介质上传输，那么他的传输模式也分为三种。包括ASCII、RTU(远程终端控制系统)、TCP三种报文类型。

串行端口存在多个版本的Modbus协议，而最常见的是下面四种：

- Modbus-Rtu
- Modbus-Ascii
- Modbus-Tcp
- ModbusPlus



Modbus RTU是一种紧凑的，**十六进制表示数据的方式**

Modbus协议使用**串口传输时**可以选择**RTU或ASCII模式**，并规定了消息、数据结构、命令和应答方式并需要对数据进行校验。ASCII 模式采用LRC校验，RTU模式采用16 位CRC校验。通过**以太网传输时使用TCP**，这种模式不使用校验，因为TCP协议是一个面向连接的可靠协议。

举一个简单的例子，如果我们需要发送一个数字`10` 那么RTU模式下，只需要发送` 0x0A` 总线上传输数据形式为： 0000 1010





### RTU协议

Modbus报文帧结构：

一个报文就是一帧数据，一个数据帧就一个报文： 指的是一串完整的指令数据，本质就是一串数据。

Modbus报文是指主机发送给从机的一帧数据，其中包含着**从机的地址**，**主机想执行的操作**，**校验码**等内容。

![在这里插入图片描述](https://img-blog.csdnimg.cn/1ee51a10afa14700a83c43099ccda2b3.png)

| 从站地址 | 功能码 | 数据    | CRC/LRC |
| -------- | ------ | ------- | ------- |
| 1 byte   | 1 byte | N bytes | 2 bytes |

- **帧结构 = 从机地址 + 功能码 + 数据 + 校验**



**从机地址:** 每个从机都有唯一地址，占用一个字节,范围0-255,其中有效范围是1-247,其中255是广播地址(广播就是对所有从机发送应答)

**功能码:** 占用一个字节,功能码的意义就是,知道这个指令是干啥的,比如你可以查询从机的数据,也可以修改从机的数据,所以不同功能码对应不同功能.

**数据:** 根据功能码不同,有不同功能，比方说功能码是查询从机的数据，这里就是查询数据的地址和查询字节数等。

**校验:** 在数据传输过程中可能数据会发生错误，CRC检验检测接收的数据是否正确





### 功能码

Modbus协议同时规定了二十几种功能码，但是常用的只有8种，用于对存储区的读写，如下表所示：

| 功能码 | 功能说明       |
| ------ | -------------- |
| 01H    | 读取输出线圈   |
| 02H    | 读取输入线圈   |
| 03H    | 读取保持寄存器 |
| 04H    | 读取输入寄存器 |
| 05H    | 写入单线圈     |
| 06H    | 写入单寄存器   |
| 0FH    | 写入多线圈     |
| 10H    | 写入多寄存器   |

当然我们用的最多的就是03和06 一个是读取数据，一个是修改数据。





### CRC校验

错误校验（CRC）域占用两个字节包含了一个**16位的二进制值**。CRC值由传输设备计算出来，然后附加到数据帧上，接收设备在接收数据时重新计算CRC值，然后与接收到的CRC域中的值进行比较，**如果这两个值不相等，就发生了错误。**



### 遥信遥测遥控

**遥测：**(遥测信息) 远程测量，采集并传送运行参数，包括各种电气量（线路上的电压、电流、功率等量值）和负荷潮流等。 

**遥信：**(遥信信息) 远程信号，采集并传送各种保护和开关量信息。 

**遥控：**(遥控信息) 远程控制，接受并执行遥控命令，主要是分合闸，对远程的一些开关控制设备进行远程控制。





## IEC103规约

103规约是**继电保护设备信息接口配套标准**。

**遥测 telemetering：**

将对象参量的近距离测量值传输至远距离的测量站来实现远距离测量的技术。遥测是利用传感技术。

遥测主要用于集中检测分散的或难以接近的被测对象，如被测对象距离遥远，所处环境恶劣，或处于高速运动状态。遥测信息是RTU采集到的电力系统运行的实时参数，如发电机出力，母线电压，系统中的潮流，有功负荷和无功负荷，线路电流，电度量等测量量信息。



**遥控(YK)：**

远程控制，接收并执行遥控命令，主要是分合闸，对远程的一些开关控制设备进行远程控制。



**遥调(YT)：**

远程调节，接受并执行遥调命令，对远程的控制量设备进行远程调试，如调节发电机输出功率。





### 帧格式

IEC60870-5-103规约有和两种报文格式，**固定帧长报文**主要用于传送“召唤、命令、确认、应答”等信息，**可变帧长报文**主要用于传送“命令”和“数据”等信息。



**固定帧长格式：**

| 启动字符10H     |
| --------------- |
| 控制域（C）     |
| 地址域（A）     |
| 帧校验和（CS）  |
| 结束字符（16H） |

 **可变帧长格式：**

| 启动字符68H              |
| ------------------------ |
| 长度L                    |
| 长度L（重复）            |
| 启动字符68H              |
| 控制域（C）              |
| 地址域（A）              |
| 链路用户数据（可变长度） |
| 帧校验和（CS）           |
| 结束字符（16H）          |



#### **控制域**

![image-20221110110927547](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221110110927547.png)

**RES：**保留位，恒为0

**PRM：**启动标志位，主站到子站为1，子站到主站为0

**FCB：**帧计数位，主站每向从站发送新一轮的"发送/确认"或"请求/响应"传输服务时，将FCB取反。

主站为每一个从站保存一个FCB的拷贝，若超时未收到应答，则主站重发，重发报文的FCB保持不变，重发次数不超过3次。

若重发3次仍未收到预期应答，则结束本轮传输服务。

**ACD：**有一级数据标识

**FCV：FCB有效位**

**DFC：**流量控制标识，DFC=0表示从站可以接收数据，DFC=1表示从站缓冲区已满，无法接收新数据





#### 地址域

主站与之通信的每一个从站的地址





#### ASDU

可变帧长的**链路规约数据单元**(LDPU)由两部分组成：链路控制规约信息(LPCI)和应用服务数据单元(ASDU)。ASDU即是可变帧长格式中的**链路用户数据**，而LPCI则是指可变帧长帧格式中除链路用户数据外的其他部分。ASDU的结构如下：

![image-20221110113533664](C:\Users\52677\AppData\Roaming\Typora\typora-user-images\image-20221110113533664.png)





**ASDU解释**

ASDU5：标识报文

ASDU6：对时

ASDU7：启动总查询

ASDU8：总查询结束

ASDU21：启动通用分类服务总查询

ASDU10：通用分类服务总查询结束





### 传输过程















