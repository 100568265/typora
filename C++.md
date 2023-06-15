# C++基础



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
- 使用关键字`constexpr` 声明的常量表达式（C++11 新增的）
- 使用关键字`enum` 声明的枚举常量
- 使用#define 定义的常量（已摒弃，不推荐）

```cpp
//字面常量
int someNum = 10;				//10是一个字面常量
cout << "Hello World" << endl;	//Hello World 就是一个字符串字面常量

//使用const将变量声明为常量
const double pi = 22.0/7;
```





### constexpr

通过关键字`constexpr`，可让常量声明像函数：

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

枚举由一组称为枚举量（emum）的常量组成。

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







## 数组

数组是一系列元素。

数组中所有元素的类型都相同(C++)。



**注意：**

您可从只包含10 个元素的数组中取回索引为1001 的元素，但这样做将给程序带来安全和稳定性方面的风险。访问数组时，**确保不超越其边界是程序员的职责**。





**动态数组**

C++提供了`std::vector`，这是一种方便而易于使用的动态数组。

```cpp
std::vector<int> dynArray(3);   //dyn array of int
dynArray[0] = 365;
dynArray[1] = -421;
dynArray[2] = 789;
std::cout << "Number of integers in array: " << dynArray.size() << std::endl;
std::cout << "Enter an element to insert" << std::endl;
int newNum;
std::cin >> newNum;
dynArray.push_back(newNum);
std::cout << "Number of integers in array: " << dynArray.size() << std::endl;
std::cout << "Last element in array: ";
std::cout << dynArray[dynArray.size() - 1] << std::endl;
```







## 字符串



**1.C风格字符串**

```cpp
char sayHello[] = {'H', 'e', 'l', 'l', 'o','\0'};
```

该数组的最后一个字符为空字符`'\0'`。这也被称为字符串结束字符，因为它告诉编译器，字符串到此结束。

您在代码中使用**字符串字面量**时，编译器将负责在它后面添加‘\0’。

在数组中间插入`'\0'`并**不会改变数组的长度**，而只会导致将该数组作为输入的字符串处理将到这个位置结束。





**2.C++字符串**

使用`std::string`。

不同于字符数组（C 风格字符串实现），`std::string` 是动态的。



`length()`：获取字符串的长度

```cpp
string greetString ("Hello std::string!");
cout << greetString << endl;
cout << "Enter a line of text: " << endl;
string firstLine;
getline(cin, firstLine);
cout << "Enter another: " << endl;
string secondLine;
getline(cin, secondLine);
cout << "Result of concatenation: " << endl;
string concatString = firstLine + " " + secondLine;
cout << concatString << endl;
cout << "Copy of concatenated string: " << endl;
string aCopy;
aCopy = concatString;
cout << aCopy << endl;
cout << "Length of concat string: " << concatString.length() << endl;
```





## 左值和右值

**左值通常是内存单元**。在前面的示例中，变量实际上指向一个内存单元，属于左值。

另一方面，**右值可以是内存单元的内容**。

因此，所有的左值都可用作右值，但并非所有的右值都可用作左值。





## 按位运算符

NOT(~), AND(&), OR(|), XOR(^)

按位运算符返回的**并非布尔值**，而是对操作数**对应位**执行指定运算的结果。

`~`：将每一位取反。

`^`：对相应位执行XOR运算。



这个程序使用了一种还未介绍过的数据类型—`bitset`，旨在简化二进制数据的显示。

```cpp
#include <iostream>
#include <bitset>

using namespace std;
int main()
{
cout << "Enter a number (0 - 255): ";
unsigned short inputNum = 0;
cin >> inputNum;
bitset<8> inputBits (inputNum);
cout << inputNum << " in binary is " << inputBits << endl;

bitset<8> bitwiseNOT = (~inputNum);
cout << "Logical NOT ~" << endl;
cout << "~" << inputBits << " = " << bitwiseNOT << endl;

cout << "Logical AND, & with 00001111" << endl;
bitset<8> bitwiseAND = (0x0F & inputNum);
cout << "0001111 & " << inputBits << " = " << bitwiseAND << endl;
cout << "Logical OR, | with 00001111" << endl;
bitset<8> bitwiseOR = (0x0F | inputNum);
cout << "00001111 | " << inputBits << " = " << bitwiseOR << endl;

cout << "Logical XOR, ^ with 00001111" << endl;
bitset<8> bitwiseXOR = (0x0F ^ inputNum);
cout << "00001111 ^ " << inputBits << " = " << bitwiseXOR << endl;

return 0;
}
```





## 基于范围的for

```cpp
for (VarType varName : sequence)
{
// Use varName that contains an element from sequence
}
```

例如，给定一个整形数组someNums，可像下面这样使用基于范围的for 循环来读取其中的元素：

```cpp
int someNums[] = { 1, 101, -1, 40, 2040 };
for (int aNum : someNums)// range based for
    cout << "The array elements are " << aNum << endl;
```



通过使用关键字`auto` 来自动推断变量的类型，可编写一个通用的for 循环，对任何类型的数组elements 进行处理：

```cpp
for (auto anElement : elements) // range based for
    cout << "Array elements are " << anElement << endl;
```









## 函数

**函数参数：**

- 引用传递：将参数的实体传给函数
- 值传递：将参数的一份拷贝传给函数



**函数调用：**

函数被调用时，所有局部变量都在栈中实例化，即被压入栈中。

函数执行完毕时，这些局部变量都从栈中弹出，栈指针返回到原来的地方。





**内联函数**

常规函数调用被转换为CALL 指令，这会导致栈操作、微处理器跳转到函数处执行等。

听起来在幕后发生了很多事情，如果函数非常简单，类似于下面这样又如何呢？

```cpp
double GetPi()
{
return 3.14159;
}
```

相对于实际执行`GetPi( )`所需的时间，执行函数调用的开销可能非常高。

这就是C++编译器允许程序员将这样的函数声明为内联的原因。程序员使用关键字`inline` 发出请求，**要求在函数被调用时就地展开它们：**

```cpp
inline double GetPi()
{
return 3.14159;
}
```

编译器通常将该关键字视为请求，请求将函数的内容直接放到调用它的地方，以**提高代码的执行速度**。



将函数声明为内联的会导致代码急剧膨胀，在声明为内联的函数做了大量复杂处理时尤其如此。应尽可能少用关键字inline，仅当函数非常简单，需要降低其开销时，才应使用该关键字。







## 指针和引用



### 指针

指针是一种指向内存空间的特殊变量。

与大多数变量一样，除非对指针进行初始化，否则它包含的**值将是随机的**。

因此将指针初始化为`NULL`。`NULL` 是一个可以检查的值，且不会是内存地址：

```cpp
int &pointsToInt = NULL;
```

**未初始化的指针**可能导致程序访问**非法内存单元**，进而导致程序崩溃。





```cpp
//以下代码有什么错误？
int* pointToAnInt = new int;
int* pNumberCopy = pointToAnInt;	//pNumberCopy和pointToAnInt指向同一块内存空间
*pNumberCopy = 30;
std::cout << *pointToAnInt;
delete pNumberCopy;
delete pointToAnInt;
return 0;

//pNumberCopy和pointToAnInt指向同一块内存空间，delete时不能重复delete
```







**（++和−−）用于指针的结果**

将指针递增或递减时，其包含的地址将增加或减少指向的数据类型的`sizeof`。

会指向下一个数据项的地址。



### 动态分配内存

使用new 和delete 动态地分配和释放内存。

通常情况下，如果成功，new 将返回指向一个指针，指向分配的内存，否则将引发异常。

```cpp
int* pointToAnInt = new int; // get a pointer to an integer
int* pointToNums = new int[10]; // pointer to a block of 10 integers
```

使用`new` 分配的内存最终都需使用对应的`delete` 进行释放：

```cpp
delete pointToAnInt;
delete[] pointToNums;
```





**问：下面的两个声明有何不同？**

```cpp
int myNumbers[100];		//myNumbers是一个数组，指向这样的内存单元的开头
int* myArrays[100];		//myArrays是一个包含100个元素的指针数组，其中每个指针都可指向int或int数组
```





### const

const 指针有如下三种：

**指针包含的地址是常量，不能修改，但可修改指针指向的数据：**

```cpp
int daysInMonth = 30;
int* const pDaysInMonth = &daysInMonth;
*pDaysInMonth = 31; // OK! Data pointed to can be changed

int daysInLunarMonth = 28;
pDaysInMonth = &daysInLunarMonth; // Not OK! Cannot change address!
```



**指针指向的数据值为常量，不能修改，但可以修改指针包含的地址：**

```cpp
int hoursInDay = 24;
const int* pointsToInt = &hoursInDay;
int monthsInYear = 12;
pointsToInt = &monthsInYear; // OK!
*pointsToInt = 13; // Not OK! Cannot change data being pointed to
int* newPointer = pointsToInt; // Not OK! Cannot assign const to non-const
```



**指针包含的地址以及它指向的值都是常量：**

```cpp
int hoursInDay = 24;
const int* const pHoursInDay = &hoursInDay;
*pHoursInDay = 25; // Not OK! Cannot change data being pointed to
int daysInMonth = 30;
pHoursInDay = &daysInMonth; // Not OK! Cannot change address
```







### 引用

要声明引用，可使用引用运算符（&），如下面的语句所示：

```cpp
VarType original = Value;
VarType& RefVar = original;
```



**将关键字const 用于引用**
可能需要禁止通过引用修改它指向的变量的值，为此可在声明引用时使用关键字const：

```cpp
int original = 30;
const int& constRef = original;
constRef = 40; // Not allowed: constRef can’t change value in original
int& ref2 = constRef; // Not allowed: ref2 is not const
const int& constRef2 = constRef; // OK
```















# C++面向对象



```cpp
Human firstMan;		//Human类的一个实例
firstMan.dateOfBirth = "1970";

Human* firstWoman = new Human();
firstWoman->dateOfBirth = "1970";
```





## 构造和析构



**声明和实现构造函数**

构造函数是一种特殊的函数，它与类同名且不返回任何值。因此，Human 类的构造函数的声明类似于下面这样：

```cpp
class Human
{
public:
Human(); // declaration of a constructor
};
```

这个构造函数可在类声明中实现，也可在类声明外实现。在类声明中实现（定义）构造函数的代码类似于下面这样：

```cpp
// constructor implementation (definition)
Human::Human()
{
// constructor code here
}
```

`::`被称为作用域解析运算符





**包含初始化列表的构造函数**

初始化列表由包含在括号中的参数声明后面的冒号标识，冒号后面列出了各个成员变量及其初始
值。

```cpp
class Human
{
private:
string name;
int age;
public:
// two parameters to initialize members age and name
Human(string humansName, int humansAge)
:name(humansName), age(humansAge)
{
cout << "Constructed a human called " << name;
cout << ", " << age << " years old" << endl;
}
// ... other class members
};
```



**析构函数**

与构造函数一样，析构函数也是一种特殊的函数。构造函数在实例化对象时被调用，而析构函数
**在对象销毁时自动被调用**。





## **浅拷贝浅拷贝**

一个指针成员buffer，它指向动态分配的内存（这些内存是在构造函数中使用new 分配的，并在析构函数中使用delete[]进行释放）。复制这个类的对象时，将复制其指针成员，但不复制指针指向的缓冲区，其结果是两个对象指向同一块动态分配的内存。销毁其中一个对象时，delete[]释放这个内存块，导致另一个对象存储的指针拷贝无效。这种复制被称为浅复制，会威胁程序的稳定性。

```cpp
#include <iostream>
#include <string.h>
using namespace std;
class MyString
{
private:
	char* buffer;
public:
	MyString(const char* initString) // Constructor
	{
		buffer = NULL;
		if(initString != NULL)
		{
			buffer = new char[strlen(initString) + 1];
			strcpy(buffer, initString);
		}
    }
	~MyString() // Destructor
	{
		cout << "Invoking destructor, clearing up" << endl;
		delete [] buffer;
	}
	int GetLength()
	{ return strlen(buffer); }
    
	const char* GetString()
	{ return buffer; }
};

void UseMyString(MyString str)
{
	cout << "String buffer in MyString is " << str.GetLength();
	cout << " characters long" << endl;
	cout << "buffer contains: " << str.GetString() << endl;
	return;
}

int main()
{
	MyString sayHello("Hello from String Class");
	UseMyString(sayHello);
	return 0;
}
```

`sayHello.buffer` 包含的指针值被复制到str 中，即`sayHello.buffer` 和`str.buffer` 指向同一个内存单元

函数`UseMyString( )`返回时，变量str 不再在作用域内，因此被销毁。为此，将调用`MyString` 类的析构函数，而该析构函数使用delete[]释放分配给buffer 的内存。这将导致main( )中的对象`sayHello` 指向的内存无效。

而等main( )执行完毕时，`sayHello` 将不再在作用域内，进而被销毁。但这次第22 行对不再有效的内存地址调用delete（销毁str 时释放了该内存，导致它无效）。正是这种重复调用delete 导致了程序崩溃。





**使用深拷贝**

为`MyString` 类声明复制构造函数的语法如下：

```cpp
class MyString
{
MyString(const MyString& copySource); // copy constructor
};

MyString::MyString(const MyString& copySource)
{
	buffer = NULL;
	if(copySource.buffer != NULL)
	{
		// allocate own buffer
		buffer = new char [strlen(copySource.buffer) + 1];
		// deep copy from the source into local buffer
		strcpy(buffer, copySource.buffer);
		cout << "buffer points to: 0x" << hex;
		cout << (unsigned int*)buffer << endl;
	}
}
```

这里并非浅复制（复制指针的值），而是深复制，即将指向的内容复制到给当前对象新分配的缓冲区中。



**注意：**

类包含原始指针成员（char *等）时，务必编写复制构造函数和复制赋值运算符。

编写复制构造函数时，务必将接受源对象的参数声明为const 引用。



如果您编写类时需要包含字符串成员，用于存储姓名等，应使用std::string 而不是char*。在没有使用原始指针的情况下，您都不需要编写复制构造函数。这是因为编译器添加的默认复制构造函数将调用成员对象（如std::string）的复制构造函数。





**移动构造函数**

C++编译器严格地调用复制构造函数反而降低了性能，如果复制的对象很大，对性能的影响将很严重。为避免这种性能瓶颈，C++11 引入了移动构造函数。移动构造函数的语法如下：

```cpp
// move constructor
MyString(MyString&& moveSource)
{
	if(moveSource.buffer != NULL)
	{
		buffer = moveSource.buffer; // take ownership i.e. 'move'
		moveSource.buffer = NULL; // set the move source to NULL
	}
}

MyString sayHelloAgain(Copy(sayHello)); // invokes 1x copy, 1x move constructors
```





**创建单例类**

要创建单例类，关键字`static` 必不可少。









## 友元

不能从外部访问类的私有数据成员和方法，但这条规则不适用于友元类和友元函数。

```cpp
class Human
{
private:
	friend void DisplayAge(const Human& person);
	string name;
	int age;

public:
	Human(string humansName, int humansAge)
	{
		name = humansName;
		age = humansAge;
	}
};

void DisplayAge(const Human& person)
{
cout << person.age << endl;
}


int main()
{
	Human firstMan("Adam", 25);
	cout << "Accessing private member age via friend function: ";
	DisplayAge(firstMan);
    return 0;
}
```

函数`DisplayAge( )`是全局函数，还是Human 类的友元，因此能够访问Human 类的私有数据成员。





使用关键字friend 让外部类Utility 能够访问私有数据成员。

```cpp
class Human
{
private:
	friend class Utility;
	string name;
	int age;
    
public:
	Human(string humansName, int humansAge)
	{
		name = humansName;
		age = humansAge;
	}
};

class Utility
{
public:
	static void DisplayAge(const Human& person)
	{
		cout << person.age << endl;
	}
};
```











## 封装

**关键字public和private**

```cpp
class Human
{
private:
// Private member data:
int age;
string name;
public:
int GetAge()
{
return age;
}
void SetAge(int humansAge)
{
age = humansAge;
}
// ...Other members and declarations
};
```





## 继承

C++派生语法如下：

```cpp
class Base
{
// ... base class members
};
class Derived: access-specifier Base
{
// ... derived class members
};
```

其中access-specifier 可以是public（这是最常见的，表示派生类是一个基类）、private 或protected(表示派生类有一个基类）。



### **私有继承**

指定派生类的基类时使用关键字private：

```cpp
class Base
{
	// ... base class members and methods
};

class Derived: private Base // private inheritance
{
	// ... derived class members and methods
};
```

私有继承意味着即便是Base 类的公有成员和方法，也只能被Derived 类使用，**而无法通过Derived 实例来使用它们**。







### **保护继承**

将属性声明为**protected**时，相当于允许**派生类**和**友元类**访问它，但禁止在继承层次结构外部（包括main( )）访问它。





**调用基类中被覆盖的方法**

```cpp
Tuna myDinner;
myDinner.Swim(); // will invoke Tuna::Swim()
myDinner.Fish::Swim();
```





### **切除问题**

```cpp
Derived objDerived;
Base objectBase = objDerived;

void UseBase(Base input);
...
Derived objDerived;
UseBase(objDerived); // copy of objDerived will be sliced and sent
```

编译器将只复制`objDerived` 的Base 部分，即不是整个对象。

要避免切除问题，不要按值传递参数，而应以指向基类的指针或const 引用的方式传递。





### final禁止继承

```cpp
class Platypus final: public Mammal, public Bird
{
public:
	void Swim()
	{
		cout << "Platypus: Voila, I can swim!" << endl;
	}
};
```

除用于类外，还可将final 用于成员函数来控制多态行为.









## 多态

**使用虚函数实现多态行为**

通过使用关键字`virtual`，可确保编译器调用覆盖版本。



**虚函数的作用**

当基类中的函数被声明为虚函数时，派生类可以通过相同的函数签名（函数名称、参数类型和返回类型相同）来**覆盖基类的虚函数**。在通过**基类指针**或引用调用虚函数时，根据实际对象的类型，会**动态地选择调用派生类中的函数**。



如果一个函数没有被声明为虚函数，它就不能被子类直接覆盖。

在这种情况下，子类只能通过定义一个具有相同名称但不同的函数来隐藏（隐藏）基类的函数。隐藏的函数只会在通过子类的对象调用时被执行，而不会在通过基类指针或引用调用时发生多态性。

```cpp
class Fish
{
public:
	virtual void Swim()
	{
	cout << "Fish swims!" << endl;
	}
};

class Tuna:public Fish
{
public:
	// override Fish::Swim
	void Swim()
	{
		cout << "Tuna swims!" << endl;
	}
};
```

将派生类对象视为基类对象，并执行派生类的Swim( )实现。



将析构函数声明为虚函数，确保通过基类指针调用delete时，将调用派生类的析构函数。

```cpp
//基类Fish
class Fish
{
public:
	Fish()
	{
		cout << "Constructed Fish" << endl;
	}
	virtual ~Fish() // 虚析构
	{
		cout << "Destroyed Fish" << endl;
	}
};

//派生类Tuna
class Tuna:public Fish
{
public:
	Tuna()
	{
		cout << "Constructed Tuna” << endl;
	}
	~Tuna()
	{
		cout << "Destroyed Tuna" << endl;
	}
};

void DeleteFishMemory(Fish* pFish)
{
    delete pFish;
}

int main()
{
    Tuna* pTuna = new Tuna;
    DeleteFishMemory(pTuna);
}
```





### 抽象基类和纯虚函数

**不能实例化的基类**被称为抽象基类。

这样的基类只有一个用途，那就是从它派生出其他类。

```cpp
class AbstractBase
{
public:
	virtual void DoSomething() = 0; // pure virtual
};

//该声明告诉编译器，AbstractBase的派生类必须实现方法DoSomething()

class Derived: public AbstractBase
{
public:
	void DoSomething()
	{
		cout << "Implemented virtual function" << endl;
	}
};
```



### override覆盖

override 提供了一种强大的途径，让程序员能够明确地表达对基类的虚函数进行覆盖的意图，进而让编译器做**如下检查**：

- 基类函数是否是虚函数？
- 基类中相应虚函数的特征标是否与派生类中被声明为override 的函数完全相同？

```cpp
class Fish
{
public:
	virtual void Swim()
	{
		cout << "Fish swims!" << endl;
	}
};	

class Tuna:public Fish
{
public:
    // Error: no virtual fn with this sig in Fish
	void Swim() const override 
	{
		cout << "Tuna swims!" << endl;
	}
};
```



### final禁止覆盖

被声明为final 的类不能用作基类。

对于被声明为final 的虚函数，不能在派生类中进行覆盖。

```cpp
class Tuna:public Fish
{
public:
	// override Fish::Swim and make this final
	void Swim() override final
	{
		cout << "Tuna swims!" << endl;
	}
};
```

您可继承这个版本的Tuna 类，但不能进一步覆盖函数Swim()











## 运算符重载

**语法格式：**

```cpp
return_type operator operator_symbol (...parameter list...);
```



```cpp
Date operator ++(int)
{
    Date copy(month,day,year);
    ++day;
    return copy;
}
```





### <<运算符

cout 不知道如何解读Date 实例,然而，cout 能够很好地显示const char *：

```cpp
std::cout << "Hello world"; // const char* works!
```

因此，要让cout 能够显示Date 对象，只需添加一个返回const char*的运算符：

```cpp
operator const char*()
{
	ostringstream formattedDate; // assists string construction
	formattedDate << month << " / " << day << " / " << year;
	dateInString = formattedDate.str();
	return dateInString.c_str();
}
```

可在`cout` 语句中直接使用Date 对象，因为`cout` 能够理解`const char*`





### **==和!=运算符**

```cpp
bool operator==(const ClassType& param)
{
    //comparison code here, return true if equal, else false
}
```

例如：比较日期是否相等

```cpp
bool operator== (const Date& compareTo)
{
	return ((day == compareTo.day)
		&& (month == compareTo.month)
		&& (year == compareTo.year));
}
```





### **赋值运算符=**

```cpp
ClassType& operator=(const ClassType& copySource)
{
    if(this != &copySource) // protection against copy into self
	{
		// copy assignment operator implementation
	}
	return *this;
}
```





### 函数运算符

operator()让对象像函数，被称为函数运算符。

```cpp
class Display
{
public:
	void operator () (string input) const
	{
 		cout << input << endl;
	}
};

int main ()
{
	Display displayFuncObj;
	// equivalent to displayFuncObj.operator () ("Display this string! ");
	displayFuncObj ("Display this string! ");
    
	return 0;
}
```







## 类型转换运算符

**C++类型转换运算符**

4个C++类型转换运算符如下：

- static_cast
- dynamic_cast
- reinterpret_cast
- const_cast

这4 个类型转换运算符的使用语法相同：

```cpp
destination_type result = cast_operator<destination_type> (object_to_cast);
```





### static_cast

使用static_cast 可将指针向上转换为基类类型，也可向下转换为派生类型，如下面的示例代码所示：

```cpp
Base* objBase = new Derived();
Derived* objDer = static_cast<Derived*>(objBase); //ok
```



将`Derived*`转换为`Base*`被称为向上转换，无需使用任何显式类型转换运算符就能进行这种转换：

```cpp
Derived objDerived;
Base* objBase = &objDerived;
```



除用于向上转换和向下转换外，static_cast 还可在很多情况下将隐式类型转换为**显式类型**，以引起程序员或代码阅读者的注意：

```cpp
double pi = 3.14159265;
int num = static_cast<int>Pi;
```





### dynamic_cast

与静态类型转换相反，动态类型转换在运行阶段（即应用程序运行时）执行类型转换。

**典型语法：**

```cpp
destination_type* Dest = dynamic_cast<class_type*>(Source);

if(Dest) // Check for success of the casting operation
Dest->CallFunc ();
```

例如：

```cpp
Base* objBase = new Derived();
//perform a downcast
Derived* objDer = dynamic_cast<Derived*>(objBase);

if(objDer) //check for success
    objDer->CallDerivedFunction();
```







### reinterpret_cast

reinterpret_cast 是C++中与C 风格类型转换最接近的类型转换运算符。它让程序员能够将一种对象类型转换为另一种，**不管它们是否相关**；

```cpp
Base* objBase = new Base();
Unrelated* notRelated = reinterpret_cast<Unrelated*>(objBase);
// The code above compiles, but is not good programming!
```

这种类型转换实际上是强制编译器接受static_cast 通常不允许的类型转换，通常用于低级程序（如驱动程序）。

**警告：**

应尽量避免在应用程序中使用reinterpret_cast，因为它让编译器将类型X 视为不相关的类型Y，这看起来不像是优秀的设计或实现。





### const_cast

const_cast 让程序员能够关闭对象的访问修饰符const。

```cpp
void DisplayAllData (const SomeClass& object)
{
	SomeClass& refData = const_cast<SomeClass&>(object);
	refData.DisplayMembers(); // Allowed!
}
```









## 模板



### 模板函数

```cpp
template <typename objType>
const objType& GetMax(const objType& value1, const objType& value2)
{
	if (value1 > value2)
		return value1;
	else
		return value2;
}

//下面是使用模板的示例
int num1 = 25;
int num2 = 40;
int maxVal = GetMax <int> (num1, num2);
double double1 = 1.1;
double double2 = 1.001;
double maxVal = GetMax <double>(double1, double2);
```







### 模板类

```cpp
template <typename T>
class HoldVarTypeT
{
private:
	T value;
public:
	void SetValue (const T& newValue) 
    { value = newValue; }
	T& GetValue() {return value;}
};

//来看该模板类的一种用法
HoldVarTypeT <int> holdInt; 
holdInt.SetValue(5);
cout << "The value stored is: " << holdInt.GetValue() << endl;
```





**参数重载模板**

```cpp
template<typename Res, typename First, typename... Rest>
void Sum(Res& result, First val1, Rest... valN)
{
result = result + val1;
return Sum(result, valN ...);
}
```























# C++容器



**STL6大组件**

容器，算法，迭代器，仿函数，适配器，空间配置器



**顺序容器：**

`std::vector`, `std::deque`, `std::list`, `std::forward_list`

**关联容器：**

`std::set`, `std::map`

**容器适配器：**

`std::stack`, `std::queue`, `std::priority_queue`





## 迭代器

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



```cpp
vector <int>::iterator arrIterator = intArray.begin ();
```

上述迭代器类型定义看起来令人恐怖。如果您使用的是遵循C++11 的编译器，可将这行代码简化成下面这样：

```cpp
auto arrIterator = intArray.begin (); //compiler detects
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

vector和数据结构**数组**十分类似，也称为单端数组，动态数组。



### **vector构造**

```cpp
//1.默认构造
vector<int> v1; //默认构造
for(int i=0;i<10;i++)
{
    v1.push_back(i);
}


//2.通过区间方式进行构造
vector<int> v2(v1.begin(),v2.end());

//3.n个elem方式构造
vector<int> v3(10,100);		//10个100

//4.拷贝构造
vector<int> v4(v3);
```



### **vector赋值**

```cpp
vector<int> v1;
for(int i=0;i<10;i++)
{
	v1.push_back(i);
}

//1.等号赋值
vector<int> v2;
v2 = v1;

//2.区间赋值
vector<int> v3;
v3.assign(v1.begin(),v1.end());

//3.n个elem赋值
vector<int> v4;
v4.assign(10,100);
```





### vector容量和大小

| 函数                    | 功能                                            |
| ----------------------- | ----------------------------------------------- |
| `empty()`               | 判断容器是否为空                                |
| `capacity()`            | 容器的容量                                      |
| `size()`                | 返回容器中元素的个数                            |
| `resize(int num)`       | 重新指定容器长度为num                           |
| `resize(int num, elem)` | 重新制定容器长度为num，以**`elem`**值填充新位置 |



### vector插入和删除

| 函数                                              | 功能                                  |
| ------------------------------------------------- | ------------------------------------- |
| `push_back(ele)`                                  | 尾部插入元素                          |
| `pop_back()`                                      | 删除最后一个元素                      |
| `insert(const_iterator pos, ele)`                 | 迭代器指向位置pos插入元素             |
| `insert(const_iterator pos,int count, ele)`       | 迭代器指向位置pos插入count个元素`ele` |
| `erase(const_iterator pos)`                       | 删除迭代器指向的元素                  |
| `erase(const_iterator start, const_iterator end)` | 删除迭代器从start到end之间的元素      |
| `clear()`                                         | 删除容器中所有元素                    |



### vector数据存取

- 利用[]方式访问数组中元素

```cpp
for(int i=0;i<v1.size();i++)
{
    cout << v1[i] << " ";
}
```

- 获取第一个元素

```cpp
v1.front();
```

- 获取最后一个元素

```cpp
v1.back();
```

- 容器互换

```cpp
v1.swap(v2);

//巧用swap可以收缩内存空间
vector<int> v;
for(int i=0;i<10000;i++)
{
    v.push_back(i);
}

v.resize(3);
//巧用swap收缩内存
vector<int>(v).swap(v);
```





### vector预留空间

减少vector在动态扩展时的扩展次数

```cpp
v.reserve(100000);
```







## deque容器

功能：双端数组，可以对头端进行插入删除操作。



**vector和deque区别：**

- vector对于头部插入删除效率较低，数据量越大，效率越低
- deque对头部的插入删除速度比vector快
- vector访问元素时的速度比deque快



### deque构造

```cpp
//1.默认构造
deque<int> d1;
for (int i = 0; i < 10; i++) 
{
    d1.push_back(i);
}

//2.指定范围拷贝
deque<int> d2(d1.begin(),d1.end());

//3.n个elem
deque<int> d3(10,100);	//10个100

//4.拷贝构造
deque<int> d4(d3);
```





### deque赋值

```cpp
deque<int> d1;
for(int i=0;i<10;i++)
{
    d1.push_back(i);
}

//1.等号赋值
deque<int> d2 = d1;

//2.assign赋值
deque<int> d3;
d3.assign(d1.begin(),d1.end());

//3.n个elem
deque<int> d4;
d4.assign(10,100);	//10个100
```





### deque大小操作

| 函数               | 功能                                          |
| ------------------ | --------------------------------------------- |
| `empty()`          | 判断容器是否为空                              |
| `size()`           | 返回容器中元素的个数                          |
| `resize(num)`      | 重新指定容器的长度为num                       |
| `resize(num,elem)` | 重新指定容器的长度为num，以`elem`值填充新位置 |





### deque插入和删除

**两端插入操作：**

| 函数               | 功能                   |
| ------------------ | ---------------------- |
| `push_back(elem)`  | 在容器尾部添加一个元素 |
| `push_front(elem)` | 在容器头部插入一个元素 |
| `pop_back()`       | 删除容器最后一个元素   |
| `pop_front()`      | 删除容器第一个元素     |



**指定位置操作：**

| 函数                  | 功能                                              |
| --------------------- | ------------------------------------------------- |
| `insert(pos,elem)`    | 在pos位置插入一个`elem`元素的拷贝，返回数据的位置 |
| `insert(pos,n,elem)`  | 在pos位置插入n个`elem`数据，无返回值              |
| `insert(pos,beg,end)` | 在pos位置插入[beg,end)区间的数据，无返回值        |
| `clear()`             | 清空容器的所有数据                                |
| `erase(beg,end)`      | 删除[beg,end)区间的数据，返回下一个数据的位置     |
| `erase(pos)`          | 删除pos位置的数据，返回下一个数据的位置           |





### deque数据存取

| 函数            | 功能                       |
| --------------- | -------------------------- |
| `at(int index)` | 返回索引index所指的数据    |
| `operator[]`    | 返回索引index所指的数据    |
| `front()`       | 返回容器中第一个数据元素   |
| `back()`        | 返回容器中最后一个数据元素 |



### deque排序

算法：对beg和end区间内元素进行排序

`sort(iterator beg, iterator end)`

```cpp
deque<int> d;
d.push_back(10);
d.push_back(20);
d.push_back(30);
d.push_front(100);
d.push_front(200);
d.push_front(300);
//升序排序d
sort(d.begin(),d.end());
```





## stack容器

栈不允许有遍历行为



**stack常用接口**

**构造函数**

| 函数                       | 功能     |
| -------------------------- | -------- |
| `stack<T> stk;`            | 默认构造 |
| `stack(const stack &stk);` | 拷贝构造 |



**数据存取**

| 函数          | 功能                 |
| ------------- | -------------------- |
| `push(elem);` | 向栈顶添加元素       |
| `pop();`      | 从栈顶移除第一个元素 |
| `top();`      | 返回栈顶元素         |



**大小操作**

```cpp
empty();	//判断栈是否为空
size();		//返回栈的大小
```









## queue容器

**队列**是一种先进先出的数据结构，它有**两个出口**。

只有队头(front)和队尾(back)能被外界访问，因此不允许有遍历行为。



**构造函数**

```cpp
queue<T> que;				//默认构造
queue(const queue &que);	//拷贝构造
```



**数据存取**

```cpp
push(elem);		//往队尾添加元素
pop();			//从队头移除第一个元素
back();			//返回最后一个元素
front();		//返回第一个元素
```



**大小操作**

```cpp
empty();	//判断是否为空	
size();		//队列大小
```







## list容器

STL中的链表是一个**双向循环链表**



### list构造函数

| 函数原型                 | 功能                                       |
| ------------------------ | ------------------------------------------ |
| `list<T> lst;`           | 默认构造                                   |
| `list(beg, end);`        | 构造函数将[beg, end)区间中的元素拷贝给本身 |
| `list(n,elem);`          | 构造函数将n个`elem`拷贝给本身              |
| `list(const list &lst);` | 拷贝构造函数                               |





### list赋值和交换

| 函数原型                             | 功能                                   |
| ------------------------------------ | -------------------------------------- |
| `assign(beg,end);`                   | 将[beg, end)区间中的数据拷贝赋值给本身 |
| `assign(n,elem);`                    | 将n个`elem`拷贝赋值给本身              |
| `list& operator=(const list &list);` | 重载等号操作符                         |
| `swap(lst);`                         | 将`lst`与本身的元素互换                |





### list大小操作

| 函数原型            | 功能                                        |
| ------------------- | ------------------------------------------- |
| `size();`           | 返回容器中元素的个数                        |
| `empty();`          | 判断容器是否为空                            |
| `resize(num);`      | 重新指定容器的长度为num，以默认值填充新位置 |
| `resize(num,elem);` | 重新指定容器的长度为num，以`elem`填充新位置 |





### list插入和删除

| 函数原型               | 功能                                            |
| ---------------------- | ----------------------------------------------- |
| `push_back(elem);`     | 在容器尾部插入一个元素                          |
| `pop_back();`          | 删除容器中的最后一个元素                        |
| `push_front(elem);`    | 在容器头部插入一个元素                          |
| `pop_front();`         | 删除容器中的第一个元素                          |
| `insert(pos,elem);`    | 在pos位置插入元素`elem`的拷贝，返回新数据的位置 |
| `insert(pos,n,elem);`  | 在pos位置插入n个`elem`的数据，无返回值          |
| `insert(pos,beg,end);` | 在pos位置插入[beg,end)区间的数据，无返回值      |
| `clear();`             | 移除容器的所有数据                              |
| `erase(beg,end);`      | 删除[beg,end)区间的数据，返回下一个数据的位置   |
| `erase(pos);`          | 删除pos位置的数据，返回下一个数据的位置         |
| `remove(elem);`        | 删除容器中所有和`elem`值匹配的元素              |





### list数据存取

list是链表，迭代器是不支持随机访问的。

```cpp
front();	//返回第一个元素
back();		//返回最后一个元素
```





### list反转和排序

```cpp
reverse();	//反转链表
sort();		//给链表排序
```

**注意：**

所有不支持随机访问迭代器的容器，不可以用标准算法。

不支持随机访问迭代器的容器，内部会提供对应的一些算法。

```cpp
List<int> L1;
L1.sort();		//升序排序
```



**指定排序规则**

```cpp
bool compareRule(Person &p1,Person &p2)
{
    return p1.age < p2.age;		//按照年龄升序
}
L.sort(compareRule);
```









## set容器

**简介：**

所有元素都会在插入时自动被排序



**本质：**

set/multiset属于关联式容器，底层结构是二叉树实现。



**set和multiset区别：**

set不允许容器中有重复的元素

multiset允许容器中有重复的元素





### set构造和赋值

**构造：**

```cpp
set<T> st;			//默认构造
set<T>st(const set &st);	//拷贝构造
```

**赋值：**

```cpp
set& operator=(const set &st);	//重载等号操作符
```

**总结：**

set容器插入时用insert





### set大小和交换

```cpp
size();		//返回容器中元素个数
empty();	//判断容器是否为空
swap(st);	//交换两个集合容器
```







### set插入和删除

| 函数原型          | 功能                                                |
| ----------------- | --------------------------------------------------- |
| `insert(elem);`   | 在容器中插入元素                                    |
| `clear();`        | 清除所有元素                                        |
| `erase(pos);`     | 删除pos迭代器所指的元素，返回下一个元素的迭代器     |
| `erase(beg,end);` | 删除区间[pos,end)的所有元素，返回下一个元素的迭代器 |
| `erase(elem);`    | 删除容器中值为`elem`的元素                          |







### set查找和统计

| 函数原型      | 功能                                                         |
| ------------- | ------------------------------------------------------------ |
| `find(key);`  | 查找key是否存在，如果存在则返回该键的元素的迭代器。如果不存在，返回`set.end()` |
| `count(key);` | 统计key的元素个数                                            |







### set排序

set容器默认排序规则为从小到大，掌握如何改变排序规则

**技术点：**

利用仿函数，可以改变排序规则

```cpp
#include <set>

class MyCompare{
public:
    bool operator()(int v1, int v2)
    {
        return v1 > v2;
    }
};

//指定排序规则为从小到大
set<int,MyCompare>s2;
```









## map容器



 **map基本概念**

所有元素都会根据元素的键值自动排序。

map/multimap属于**关联式容器**，底层结构是用**二叉树**实现。



**优点：**可以根据key值快速的找到value值



**map和multimap的区别：**

- map不允许容器中有重复key值元素
- multimap允许容器中有重复key值元素







### map构造和赋值

```cpp
map<int,int> m;			//默认构造
map<int,int> m2(m);		//拷贝构造

//赋值操作
map<int,int>m3;
m3 = m2;
```







### map大小和交换

| 函数原型    | 功能               |
| ----------- | ------------------ |
| `size();`   | 返回容器中元素个数 |
| `empty();`  | 判断容器是否为空   |
| `swap(mp);` | 交换两个集合容器   |







### map插入和删除

| 函数原型          | 功能                                                |
| ----------------- | --------------------------------------------------- |
| `insert(elem);`   | 在容器中插入元素                                    |
| `clear();`        | 清除所有元素                                        |
| `erase(pos);`     | 删除pos迭代器所指的元素，返回下一个元素的迭代器     |
| `erase(beg,end);` | 删除区间[beg,end)的所有元素，返回下一个元素的迭代器 |
| `erase(key);`     | 删除容器中值为key的元素                             |



**三种插入方式：**

```cpp
map<int,int> m;
//第一种
m.insert(pair<int,int>(1,10));
//第二种
m.insert(make_pair(2,20));
//第三种(不建议使用)
m[4] = 40;
```





**删除**

```cpp
m.erase(m.begin());

m.erase(3);					//按照key删除

m.erase(m.begin(),m.end());	//按照区间删除
```





### map查找和统计

```cpp
find(key);	//查找key是否存在，若存在，返回该键的元素的迭代器；若不存在，返回map.end();
count(key);	//统计key的元素个数
```





### map排序

map容器默认排序规则为：按照key值进行，从小到大排序



**主要技术点：**

利用仿函数，可以改变排序规则

```cpp
class MyCompare
{
public:
    bool operator()(int v1,int v2)
    {
        //降序
        return v1 > v2;
    }
};

map<int,int,MyCompare> m;
m.make_pair(1,10);
m.make_pair(2,20);
m.make_pair(3,30);
m.make_pair(4,40);

for (auto it=m.begin();it!=m.end();it++)
{
	cout << "key = " << (*it).first << " value = " << (*it).second << endl;
}
```





## 函数对象(仿函数)

**概念：**

- 重载**函数调用操作符**的类，其对象常称为**函数对象**
- **函数对象**使用重载的()时，行为类似函数调用，也叫**仿函数**

**本质：**

函数对象(仿函数)是一个类，不是一个函数



### 函数对象调用

**特点：**

- 在使用时，可以像普通函数那样调用，可以有参数，可以有返回值
- 函数对象可以有自己的状态
- 函数对象可以作为参数传递


```cpp
class MyAdd
{
public:
    int operator()(int v1,int v2)
    {
        return v1 +v1;
    }
};

//1.函数对象在使用时，可以像普通函数那样调用，可以有参数，返回值
MyAdd myAdd;
cout << myAdd(10,10) << endl;

//2.函数对象超出普通函数的概念，可以有自己的状态
class MyPrint
{
public:
    MyPrint()
    {
        this->count = 0;
    }
    void operator()(string test)
    {
        cout << test << endl;
        this->count++;
    }
    int count;	//内部自己状态
};

MyPrint myPrint;
myPrint("hello world!");
myPrint("hello world!");

cout << "myPrint调用次数为: " << myPrint.count <<endl;	

//3.函数对象可以作为参数传递
void doPrint(MyPrint &mp, string test)
{
    mp(test);
}

MyPrint myPrint;
doPrint(myPrint,"Hello C++");
```





### 谓词

**概念：**

返回bool类型的仿函数称为谓词

如果operator()接受一个参数，称为一元谓词

如果operator()接受两个参数，称为二元谓词





#### 一元谓词

例：查找容器中有没有大于5的数字

```cpp
class GreaterFive{
public:
    bool operator()(int val)
    {
        return val > 5;
    }
};

void test08()
{
    vector<int> v;
    for (int i = 0; i < 10; ++i) {
        v.push_back(i);
    }
    //查找容器中大于5的数字
    //GreaterFive() 匿名的函数对象
    auto it = find_if(v.begin(),v.end(),GreaterFive());
    if(it == v.end())
    {
        cout << "未找到" << endl;
    }
    else{
        cout << "找到了大于5的数字: " << *it << endl;
    }
}
```





#### 二元谓词

如果operator()接受两个参数，称为二元谓词

```cpp
class MyCompare
{
public:
    bool operator()(int val1, int val2)
    {
        return val1 > val2;
    }
};

//二元谓词
void test09()
{
    vector<int> v;
    v.push_back(10);
    v.push_back(40);
    v.push_back(20);
    v.push_back(30);
    v.push_back(50);

    //排序
    sort(v.begin(),v.end(),MyCompare());
    printVector(v);
}
```





### 内建函数对象

**概念：**

STL内建了一些函数对象

**分类：**

- 算术仿函数
- 关系仿函数
- 逻辑仿函数

**用法：**

这些防函数所产生的对象，用法和一般函数完全相同

使用内建函数对象，需要引入头文件`#include <functional>`





#### 算术仿函数

| 函数                                | 功能       |
| ----------------------------------- | ---------- |
| `template<class T> T plus<T>`       | 加法仿函数 |
| `template<class T> T minus<T>`      | 减法仿函数 |
| `template<class T> T multiplies<T>` | 乘法仿函数 |
| `template<class T> T devides<T>`    | 除法仿函数 |
| `template<class T> T modules<T>`    | 取模仿函数 |
| `template<class T> T negate<T>`     | 取反仿函数 |

其中negate是一元仿函数，其余都是二元仿函数





```cpp
//negate 一元仿函数 取反仿函数
negate<int> n;
cout << n(50) <<endl;   //-50

//plus 二元仿函数 加法仿函数
plus<int> p;
cout << p(10,20) << endl;
```





#### 关系仿函数

**功能：**

实现关系对比

| 仿函数原型                                | 功能     |
| ----------------------------------------- | -------- |
| `template<class T> bool equal_to<T>`      | 等于     |
| `template<class T> bool not_equal_to<T>`  | 不等于   |
| `template<class T> greater<T>`            | 大于     |
| `template<class T> bool greater_equal<T>` | 大于等于 |
| `template<class T> bool less<T>`          | 小于     |
| `template<class T> bool less_equal<T>`    | 小于等于 |

**示例：**

```cpp
//大于
vector<int> v;
v.push_back(10);
v.push_back(30);
v.push_back(40);
v.push_back(20);
v.push_back(50);
printVector(v);
//降序
//greater<int>() 内建函数对象
sort(v.begin(),v.end(),greater<int>());
```

**总结：**关系仿函数中最常用的就是greater<>()







#### 逻辑仿函数

实现逻辑运算

| 函数原型                                | 功能   |
| --------------------------------------- | ------ |
| `template<class T> bool logical_and<T>` | 逻辑与 |
| `template<class T> bool logical_or<T>`  | 逻辑或 |
| `template<class T> bool logical_not<T>` | 逻辑非 |



```cpp
vector<bool> v;
v.push_back(true);
v.push_back(false);
v.push_back(true);
v.push_back(true);
v.push_back(false);
printVector(v);
cout << endl;
//利用逻辑非，将容器v搬运到容器2中，并执行取反操作
vector<bool> v2;
v2.resize(v.size());
//tranform搬运容器
transform(v.begin(),v.end(),v2.begin(),logical_not<bool>());
printVector(v2);
```





## STL常用算法

算法主要由头文件`<algoritm>`，`<functional>`，`<numeric>`组成。

`<algorithm>`是所有STL头文件最大的一个，范围设计到比较，交换，查找，遍历，复制，修改等等。

`<numeric>`体积很小，只包括几个在序列上面进行简单数学运算的模板函数

`<functional>`定义了一些模板类，用以声明函数对象





### 常用遍历算法

| 函数                                                         | 功能                   |
| ------------------------------------------------------------ | ---------------------- |
| `for_each(iterator beg, iterator end, _func)`                | 遍历容器               |
| `transform(iterator beg1, iterator end1, iterator beg2, _func);` | 搬运容器到另一个容器中 |







### 常用查找算法

| 算法          | 功能               |
| ------------- | ------------------ |
| find          | 查找元素           |
| find_if       | 按条件查找元素     |
| adjacent_find | 查找相邻重复元素   |
| binary_search | 二分查找法         |
| count         | 统计元素个数       |
| count_if      | 按条件统计元素个数 |



#### 1.find

**功能描述：**

查找指定元素，找到返回指定元素的迭代器，找不到返回结束迭代器end()



**函数原型：**

`find(iterator beg, iterator end, value);`

按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

beg：开始迭代器

end：结束迭代器

value：查找的元素



**示例：**

```cpp
#include <algorithm>

vector<int> v;
for (int i = 0; i < 10; ++i) 
{
	v.push_back(i);
}

auto it = find(v.begin(),v.end(),5);
```

利用find在容器中查找指定的元素，返回值是迭代器





#### 2.find_if

按条件查找元素

**函数原型**

`find_if(iterator beg, iterator end, _Pred);`

按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

`beg`开始迭代器

`end`结束迭代器

`_Pred`函数或者谓词(返回bool类型的仿函数)



**示例：**

```cpp
class GreaterFive{
public:
    bool operator()(int val){
        return val > 5;
    }
};

vector<int> v;
for (int i = 0; i < 10; ++i) {
	v.push_back(i);
}
auto it =find_if(v.begin(),v.end(),GreaterFive());
```







#### 3.adjacent_find

查找相邻重复元素



**函数原型：**

`adjacent_find(iterator beg, iterator end);`

//查找相邻重复元素，返回相邻元素的第一个位置的迭代器

//beg开始迭代器

//end结束迭代器



**示例：**

```cpp
auto pos = adjacent_find(v.begin(),v.end());
```





#### 4.binary_search

查找指定元素是否存在



**函数原型：**

`bool binary_search(iterator beg,iterator end,value);`

查找指定的元素，查到返回true，否则false

注意：在无序序列中不可用



**示例：**

```cpp
//查找容器中是否有元素9
binary_search(v.begin(),v.end(),9);
```





#### 5.count

统计元素个数



**函数原型：**

`count(iterator beg, iterator end, value);`

统计元素出现次数



**示例：**

```cpp
//查找值为40的元素个数
int num = count(v.begin(),v.end(),40);
```

对于自定义的数据类型，必须重载==





#### 6.count_if

按条件统计元素个数



**函数原型：**

`count_if(iterator beg, iterator end, _Pred);`

按条件统计元素出现次数



**示例：**

```cpp
//计算容器中大于20的元素个数
class Greater20
{
public:
    bool operator()(int val)
    {
        return val > 20;
    }
};

int num = count_if(v.begin(),v.end(),Greater20());
```







### 常用排序算法

**算法简介：**

| 算法           | 功能                               |
| -------------- | ---------------------------------- |
| sort           | 对容器内元素进行排序               |
| random_shuffle | 洗牌，指定范围内的元素随即调整次序 |
| merge          | 容器元素合并，并存储到另一个容器中 |
| reverse        | 反转指定范围的元素                 |



#### 1.sort

```cpp
//利用sort进行升序
sort(v.begin(),v.end());
//改编为降序
sort(v.begin(),v.end(),greater<int>());
```





#### 2.random_shuffle

洗牌，指定范围内的元素随即调整次序

`random_shuffle(iterator beg, iterator end);`



**示例：**

```cpp
//利用洗牌算法打乱顺序
random_shuffle(v.begin(),v.end());
```





#### 3.merge

两个容器元素合并，并存储到另一个容器中。

这两个容器必须是有序的。



**函数原型：**

`merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

`dest` ：目标容器开始迭代器



**示例：**

```cpp
//目标容器
vector<int> vTarget;
//提前给目标容器分配内存
vTarget.resize(v1.size()+v2.size());

merge(v1.begin(),v1.end(),v2.begin(),v2.end(),vTarget.begin());
```





#### 4.reverse

将容器内的元素进行反转

**函数原型**：

`reverse(iterator beg, iterator end);`



**示例：**

```cpp
reverse(v.begin(),v.end());
```





### 常用拷贝和替换算法

| 算法       | 功能                                     |
| ---------- | ---------------------------------------- |
| copy       | 容器内指定范围的元素拷贝到另一个容器中   |
| replace    | 将容器内指定范围的旧元素修改为新元素     |
| replace_if | 容器内指定范围满足条件的元素替换为新元素 |
| swap       | 互换两个容器的元素                       |





#### 1.copy

容器内指定范围的元素拷贝到另一个容器中

`copy(iterator beg, iterator end, iterator dest);`

`beg` 开始迭代器

`end` 结束迭代器

`dest` 目标其实迭代器



**示例：**

```cpp
vector<int> v2;
v2.resize(v1.size());
copy(v1.begin(),v1.end(),v2.begin());
```





#### 2.replace

将容器内指定范围的旧元素修改为新元素

`replace(iterator beg, iterator end, oldVal, newVal);`

**示例：**

```cpp
//将所有的20替换为2000
replace(v.begin(),v,end(),20,2000);
```

replace会替换区间内满足条件的元素





#### 3.replace_if

容器内指定范围满足条件的元素替换为新元素

`replace_if(iterator beg,iterator end,_pred,newVal);`

按条件替换元素，满足条件的替换成指定元素



**示例：**

```cpp
//把所有大于30的元素替换为3000
class Greater30
{
public:
    bool operator()(int val)
    {
        return val >= 30;
    }
}

replace_if(v.begin(),v.end(),Greater30(),3000);
```





#### 4.swap

互换两个容器的元素

`swap(container c1, container c2);`

**示例：**

```cpp
swap(v1,v2);
```







### 常用算术生成算法

| 算法       | 功能                 |
| ---------- | -------------------- |
| accumulate | 计算容器元素累计总和 |
| fill       | 向容器中添加元素     |



#### **accumulate**

计算容器元素累计总和

`accumulate(iterator beg, iterator end, value);`

`beg` 起始迭代器

`end` 结束迭代器

`value` 起始值

**示例：**

```cpp
int total = accumulate(v.begin(),v.end(),0);
```





#### **fill**

向容器中添加元素

`fill(iterator beg, iterator end, value);`

**示例：**

```cpp
//容器中元素全部填充为100
vector<int> v;
v.resize(10);
fill(v.begin(),v.end(),100);
```







### 常用集合算法

| 算法             | 功能             |
| ---------------- | ---------------- |
| set_intersection | 求两个容器的交集 |
| set_union        | 求两个容器的并集 |
| set_difference   | 求两个容器的差集 |



#### set_intersection

求两个容器的交集

该函数返回交集容器的最后一个元素的迭代器



**函数原型：**

`set_intersection(v1.begin(),v1.end(),v2.begin(),v2.end(),vTarget.begin());`



**示例：**

```cpp
vector<int> vTarget;
vTarget.resize(min(v1.size(),v2.size()));
//获取交集
auto itEnd = set_intersection(v1.begin(),v1.end(),v2.begin(),v2.end(),vTarget.begin());

for_each(vTarget.begin(),itEnd,myPrint);
```

**总结：**

求交集的两个集合必须是有序序列

目标容器开辟空间需要从两个容器中取较小值

sec_intersection返回值是交集最后一个元素的位置





#### set_union

求两个集合的并集

**函数原型：**

`set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

`dest` 目标容器开始迭代器



**示例：**

```cpp
//最特殊情况：两个容器没有交集，并集就是两个容器size相加
vector<int> vTarget;
vTarget.resize(v1.size()+v2.size());

set_union(v1.begin(),v1.end(),v2.begin(),v2.end(),vTarget.begin());
```

**总结：**

求并集的两个集合必须是有序序列

目标容器开辟空间需要两个容器size相加

set_union返回值是并集最后一个元素的位置





#### set_difference

求两个容器的差集



**分两种情况：**

V1和V2容器的差集

V2和V1容器的差集



**函数原型：**

`set_difference(iteartor beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

**示例：**

```cpp
//最特殊的情况是两个容器没有交集，取第一个容器的size作为开辟空间
vTarget.resize(v1.size());
set_difference(v1.begin(),v1.end(),v2.begin(),v2.end(),vTarget.begin());
```

**总结：**

求差集的两个集合必须是有序序列

set_difference返回值是差集最后一个元素的位置











