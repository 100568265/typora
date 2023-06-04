# C++语法



## 变量

**1.如果要使用固定宽度的整型，必须包含头文件`<cstdint>`**



**2.使用列表初始化避免缩窄转换错误**

C++11引入了列表初始化来禁止缩窄

```cpp
int largeNum = 5000000;
short anotherNum{ largeNum }; // error! Amend types
int anotherNum{ largeNum }; // OK!
float someFloat{ largeNum }; // error! An int may be narrowed
float someFloat{ 5000000 }; // OK! 5000000 can be accomodated
```



**3.auto自动推断类型**

如果您使用的编译器支持C++11 和更高版本，可不显式地指定变量的类型，而使用关键字auto.

这让**编译器去决定变量的类型**，而编译器将根据初始值来确定合适的类型.

```cpp
auto largeNumber = 2500000000000;
```



**4.常量**

在C++中，常量可以是：

- 字面常量
- 使用关键字const 声明的常量
- 使用关键字constexpr 声明的常量表达式（C++11 新增的）
- 使用关键字enum 声明的枚举常量
- 使用#define 定义的常量（已摒弃，不推荐）

```cpp
//字面常量
int someNum = 10;				//10是一个字面常量
cout << "Hello World" << endl;	//Hello World 就是一个字符串字面常量

//使用const将变量声明为常量
const double pi = 22.0/7;
```





### constexpr

通过关键字constexpr，可让常量声明像函数：

```cpp
constexpr double GetPi() {return 22.0 / 7;}
//在一个常量表达式中，可使用另一个常量表达式：
constexpr double TwicePi() {return 2 * GetPi();}
```



```cpp
//使用常量表达式来计算pi 的值
#include <iostream>
constexpr double GetPi() { return 22.0 / 7; }
constexpr double TwicePi() { return 2 * GetPi(); }

int main()
{
using namespace std;
const double pi = 22.0 / 7;

cout << "constant pi contains value " << pi << endl;
cout << "constexpr GetPi() returns value " << GetPi() << endl;
cout << "constexpr TwicePi() returns value " << TwicePi() << endl;
return 0;
}
```





### enum

需要定义一种变量，其可能取值由您指定。

枚举由一组称为枚举量（emumerator）的常量组成。

```cpp
enum CardinalDirections
{
North,
South,
East,
West
};
```

如果要声明一个变量，用于存储方向，可以像下面这样做：

```cpp
CardinalDirections windDirection = South;
```

编译器将枚举量（Voilet 等）转换为整数，每个枚举量都比前一个大1。您可以指定起始值，如果没有指定，编译器认为**起始值为0**，因此North 的值为0。

```cpp
//使用枚举量指示基本方位
#include <iostream>
using namespace std;
enum CardinalDirections
{
North = 25,
South,
East,
West
};

int main()
{
cout << "Displaying directions and their symbolic values" << endl;
cout << "North: " << North << endl;			//25
cout << "South: " << South << endl;			//26
cout << "East: " << East << endl;			//27
cout << "West: " << West << endl;			//28
CardinalDirections windDirection = South;
cout <<  windDirection << endl;	//26
return 0;
}
```





















































# C++容器



**STL6大组件**

容器，算法，迭代器，仿函数，适配器，空间配置器





容器，迭代器例子：

```cpp
//创建了一个vector容器，数组
std::vector<int> v;
v.push_back(10);
v.push_back(20);
v.push_back(30);
v.push_back(40);
//通过迭代器访问容器中的数据
std::vector<int>::iterator itBegin = v.begin(); //起始迭代器
std::vector<int>::iterator itEnd = v.end(); //指向容器中最后一个元素下一个位置
//第一种遍历方式
while (itBegin!=itEnd)
{
    std::cout << *itBegin++ << "\t";
}
std::cout << std::endl;
//第二种遍历方式
for(std::vector<int>::iterator it=v.begin();it<v.end();it++)
{
    std::cout << *it << "\t";
}
std::cout << std::endl;
//第三种遍历方式
std::for_each(v.begin(),v.end(), myPrint);
```





## string容器

string是C++风格的字符串，string本质上是一个类。



**string和char*的区别：**

- `char*`是一个指针
- string是一个类，类内部封装了`char*`，管理这个字符串，是一个`char*`型的容器



**特点：**

string类内部封装了很多成员方法

例如：find，copy，delete，replace，insert



### 构造函数

```cpp
std::string s1;  					//默认构造
const char* str = "hello world";	//C语言风格
std::string s2(str);				//C风格字符串构造
std::string s3(s2);					//拷贝构造
std::string s4(10,'a');				//10个字符a拼接成一个字符串
```





### 赋值操作

```cpp
string str1;
str1 = "hello world";
string str2 = str1;

//把字符串s的前n个字符赋值给当前字符串
string str5;
str5.assign("hello C++",5);		//str5是"hello"
```





### 字符串拼接

```cpp
string str1 = "我";
str1 += "爱玩游戏";

//append方式
string s3 = "I";
str3.append(" love ");
//截取字符串然后拼接
str3.append(str2,4,3);
```





### 查找和替换

**功能描述：**

- 查找：查找指定字符串是否存在
- 替换：在指定的位置替换字符串

**函数原型：**

```cpp
int find(const string& str, int pos = 0) const;			//查找str第一次出现位置,从pos开始查找
int find(const char* s, int pos = e) const;				//查找s第一次出现位置,从pos开始查找
int find(const char* s, int pos, int n) const;			//从pos位置查找s的前n个字符第一次位置
int find(const char c, int pos = 0) const;				//查找字符c第一次出现位置
int rfind(const string& str, int pos = npos) const;		//查找str最后一次位置,从pos开始查找
int rfind(const char* s, int pos = npos) const;			//查找s最后一次出现位置,从pos开始查找//从pos查找s的前n个字符最后一次位置
int rfind(const char* s, int pos, int n) const;			//从pos查找s的前n个字符最后一次位置
int rfind(const char c, int pos = 0) const;				//查找字符C最后一次出现位置
string& replace(int pos, int n, const string& str);		//替换从pos开始n个字符为字符串str
string& replace(int pos, int n,const char* s);			//替换从pos开始的n个字符为字符串s
```





### 字符串比较

**比较方式：**

- 字符串比较是按字符的ASCII码进行对比，=返回0，>返回1，<返回-1

**函数原型：**

```cpp
int compare(const string &S) const;		//与字符串s比较
int compare(const char *s) const;		//与字符串s比较
```





### 字符存取

1. 通过`[]`访问单个字符

2. 通过`at`访问单个字符

```cpp
string str = "hello";
//1.通过[]访问单个字符
for(int i=0;i<str.size();i++)
	cout << str[i] << " ";
//2.通过at访问单个字符
for(int i=0;i<str.size();i++)
    cout << str.at(i) << " ";
```





### 插入和删除

```cpp
string& insert(int pos, const char* s);			//插入字符串
string& insert(int pos, const string& str);		//插入字符串
string& insert(int pos, int n, char c);			//在指定位置插入n个字符
string& erase(int pos, int n = npos);			//删除从Pos开始的n个字符
```





### 字串获取

```cpp
string str = "abcedf";
string subStr = str.substr(1,3);		//bcd
```







## vector容器

























