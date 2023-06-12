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

























