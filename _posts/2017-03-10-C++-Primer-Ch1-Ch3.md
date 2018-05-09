---
title: "C++ Primer Ch1-Ch3"
date: 2017-03-10 00:18:29
categories:
- C++
- C++11
- C++ primer
- 笔记
---


# C++ Primer Ch1-Ch3
@(Coding)[笔记, C++]

## 第一章 开始
1. 
```c++
int main(){
return 0;
}
```

2. 函数四要素：返回值，函数名，形参列表，函数体
语句块{}
3. 一种类型定义了数据元素的类型和运算，分为内置类型和类类型
4. 标准输入输出iostream
流：字符序列，时间调用顺序生成或消耗。
cout, cin, cerr：标准错误, clog
```C++
#include<iostream>	//<>优先系统头文件或标准库,通常不带。h，""优先项目内
std::cout <<"Hello world" <<std::endl;
//<< 输出运算符，二目操作符 左侧ostream对象，右侧要打印的值，左侧对象为计算结果
//"hello world" 字符串字面值常量
//endl 操纵符。结束当前行，并将与设备关联的缓冲区中的内容刷到设备中。注意：及时刷新流，保证错误及时刷到设备
//std 标准库的命名空间。 命名空间：避免不经意的名字定义冲突，以及使用库中相同名字导致的冲突。
//:: 作用域操作符
```
5. 输入运算符>> 从给定的istream（左侧）读入数据，并存入给定对象（右侧）中，返回istream对象
6. 注释：单行注释， 界定符对注释（不能嵌套）
7. for循环：循环头（初始化语句; 循环条件; 表达式）+循环体
8. 读取不定量数据
```c++
while(std::cin >> value){
}
//使用istream作为循环条件时，检测流的状态做判定，当文件结束符或无效输入，会判假
//文件结束符 win：Ctrl+Z unix：Ctrl+D
```
9. 编译错误：语法错误，类型错误，声明错误，无法判断逻辑错误。
10. 文件重定向 `exec <infile >outfile`
11. 成员函数（方法）
```c++
item.isbn()
//类对象item 点运算符. 成员函数isbn 调用运算符()
```

## 第二章 变量和基本类型
1. 数据类型：数据的意义和可执行的操作

2. 基本数据类型：算数类型 空类型
算数类型：整型（整型，字符型，布尔型） 浮点型。最小尺寸以规定P30，可自定义更大尺寸（整型short 16/int 16 /long 32/long long 64）

3. 字符型char：
char 机器基本字符集大小 8
w_char_t 机器最大拓展字符集 16
char16_t 和 char32_t Unicode字符集 分别 16 32

4. 物质基本单位 比特bit
逻辑最小单位 字节byte
逻辑基本单位 字word

5. 单精度浮点 float 6位有效数字 通常32位
双精度浮点 double 10位有效数字 通常64位
拓展精度浮点 long double 10位有效数字 通常96或128位

6. 除布尔型和拓展字符型，带符号signed类型和无符号unsigned类型，正负值范围尽量平衡 -128～127
字符型分为三种：char ; signed char ; unsigned char，但表现形式只有signed char 和 unsigned char
- 取值非负 用unsigned 
- int不够 多用long long， long一般与int同大小
- 算数表达式尽量不用char bool 用的时候要表明符号类型
- 浮点数用double

7. 类型转换
- 非bool - bool：0 false 非0 true
- bool - 非bool： false 0 true 1
- 浮点 - 整型 取整
- 整型 - 浮点 小数0,整数可能溢出
- 赋超出范围值给无符号时，初始值对无符号类型表示数值总数取模后的余数
-  赋超出范围值给带符号时， 结果未定义
含有无符号类型的表达式
- 算数表达式中有无符号也有int时，int会被转为无符号数
- 无符号数减去一个数时，必须保证结果非负
- 无符号数不会小于0，在循环中特重要，可能导致死循环

8. 字面值常量
20 024 0x24
十进制默认带符号（但十进制字面值没有赋值，-代表取赋值），八/十六都可能，取能容纳的类型最小大小
short 无字面值类型
浮点型字面值 默认double
如果两个字符字面值之间仅有空格/缩进/换行符，则为一个整体
nullptr 指针字面值常量

9. 转义序列
不可打印字符和特殊含义字符不能被直接使用，需要转义序列
泛化转义序列: \x一个或多个十六进制数字，\ 一至三个八进制数字

10. 指定字面值类型
P37

11. 变量=对象，具名的、可供程序操作的存储空间

12. 变量初始化
列表初始化
```
int name1 = 0;
int name2 = {0};	//C++11
int name3{0};	//C++11
int name4(0);
```
以上四种都对，2/3这种初始化列表的形式可以在初始值存在丢失信息风险时，编译器报错
默认初始化：全局变量初始为0,函数成员变量不初始化，对象的默认初始化由对象自身决定

13. 变量声明和定义
```
extern int  i;	//声明
int i;	//定义
extern int i = 0;	//定义
```
变量可被多次声明但只能定义一次

14. 标识符
数字，字母，下划线，大小写敏感，下划线和字母开头

15. 作用域
大多数作用域用{}分割，名字自声明开始，结束语作用域末端。
全局作用域，块作用域
- 变量在使用时再定义
嵌套作用域 内层作用域可以访问和重新定义外层作用域变量
```
#include<iostream>
int iout = 55;
int main(){
	int iin = 2;
	std::cout << iout << "VS" << iin << std::endl;
	int iout = -78;
	std::cout << iout << "VS" << iin << std::endl;
	std::cout << ::iout << "VS" << iin << std::endl;//作用域操作符
	return 0;
} 
55VS2
-78VS2
55VS2
```

16. 复合类型
指针和左值引用（引用）
refer to引用：非对象而是为对象起另外一个名字，必须初始化—把引用类型和初始值绑定在一起，不能改绑。

```
int i = 1;
int &j = i;
int i = 1, &j = i;
int &j = 1;	//错误，必须对象
```
```
int i = 1;
int * j = &i;

int * j;
j = &i;

int i = 1;
int * j1 = &i;
int * j2 = j1;
```
point to指针：指针为对象。可以拷贝和赋值，在定义时初始化非必须。
取地址符&，不能定义指向引用的指针
指针的四种状态：
- 指向一个对象
- 指向紧邻对象所占空间的下一个位置
- 空指针
- 无效指针
解引用符 *
```
int i = 1;
int * p = &i;
cout << *p  << endl;
```
空指针
```
int * p1 = nullptr;
int * p2 = 0;
int * p3 = NULL;	//NULL 预处理变量@cstdlib 由预处理器控制
```
初始化所有指针对象，尽量再定义了对象以后再定义指向它的指针
void * 只可以存放任何对象的指针：1.和别的指针比较，2.作为函数的输入输出，3.赋值给另一个void指针
& * 修饰对内容
```
int* p
int *p
int * p
（int (*p)）	//*修饰p
```
指向指针的指针和指向指针的引用: 变量前从右向左阅读，离变量越近对变量的类型有最直接的影响
```
int i = 1;
int *ptri = &i;
int **ppi = &ptri;	//指向指针的指针
int *&ptoi = ptri;	//指向指针的引用
```

17. const限定符

const变量的值不被改变，但在工程中意义明显且方便修改。
const变量必须初始化，可以是任意复杂表达式。

const与初始化:
const限制了只能在const对象上执行不改变其内容的的操作，但对初始化const变量的的表达式没有const变量的规定。但是要保证初始化const变量的表达式的类型必须与const限定的变量本身的类型一致，否则编译器会产生临时量。

a. 
```
int ival = 1;
const int &refval = ival;	//合法
```

b. 
```
double dval = 3.14;
const int &refval = dval;
```

等价于：

c. 
```
double dval = 3.14;
int temp = dval;	//临时量
int &refval = temp;	//通过引用修改dval时不可能实现
```

对const的引用(对常量的引用)

```
const int &i1 = 55;	//正确
int &i2 = 55; 	//错误
```

const引用对const变量的引用，通过引用和变量本身都不能修改
const引用对一般变量的引用，通过引用不能改变，通过变量本身可以改变
对常量的引用只规定了对常量的引用不能通过该引用改变引用的变量的值。

```
int val1 = 55;
const int &r2val = val1;	//正确，但不能通过r2val修改val1的值
```

const对象的作用域
const对象在编译过程中将const变量替换成相应的值。
默认只在当前文件有效，可以多文件出现同名const变量；
如果要求跨文件使用，且为复杂表达式初始化，需要在所有的定义和声明前都加extern

```
extern const int val = fcn();
extern const int val;
```

18. const与指针

指向常量的指针

```
const int val1 = 1;
int *ptrval1 = &val1;	//错误，类型不一致
const int *ptrval2 = &val1;
&ptrval2 = 55;	//错误，val1是常量
```

指针的类型必须与指向的对象的类型一致，有两个例外，之一：指向常量的指针除指向常量外可以指向一个非常量对象。
- 存放常量对象的地址，只能使用指向常量的指针。
- 指向常量的指针只规定了指向常量的指针不能通过该指针改变指向的变量的值。

常量指针
- 常量指针必须初始化，指针的地址不能改变，但指向的非const变量可以改变。
- 指向对象的类型严格一致。

```
int err1 = 1;
int *const curErr = &err1;	//常量指针
const double val1 = 1.;
const double *const curval1 = &val1;	//指向常量对象的常量指针。
//从右向左读
```

**顶层const和底层const**
top-level const 表示指针本身是个常量，low-level const表示所指向的对象是个const，引用const一定为底层const。
顶层：我为常量 
底层：我的本尊是常量

```
const int *const p1 = nullptr;	//左侧const为底层，右侧const为顶层
```

底层const对数据操作的限制不能忽视。
底层const在初始化时可以用同类型的非const对象
用于声明引用的const都是底层const


19. constexpr和常量表达式

**常量表达式**  变量值不会改变且在编译过程过程中就能得到计算结果的表达式。是否为常量表达式由数据类型+初始值共同决定

```
const int sz = fcn();	//不是常量表达式，初始值在运行时才能获得
```

**constexpr类型** 编译器验证变量值是否为常量表达式，因此必须用常量表达式初始化。解决目标初始值为常量表达式但初始值并非常量表达式的情况。

```
constexpr int sz = fcn();	//fcn()必须为constexpr函数
```

- 变量为常量表达式，则声明constexpr类型，加constexpr关键字，表明为常量表达式。
- 定义constexpr函数，用该函数初始化constexpr变量。

**字面值类型** constexpr函数使用类型的限制，只能使用字面值类型（算术类型、引用、指针等，更多见7.5.6和19.3）
- 指针和引用类型在定义成constexpr时，初始值需限制为nullptr或0；或储存在固定地址的对象，例如全局变量和一些特殊变量（见6.1.1）
- **字面值** **字面值类型** 区别

指针与constexpr

```
const int *p = nullptr;	//指向整型常量的指针p 底层const
constexpr int *ptr = nullptr;	//指向int的指针常量ptr 顶层const
```

- constexpr指针是***常量指针，可以指向常量也可以指向非常量***

```
constexpr const int *p = nullptr;	//p为常量指针
constexpr int *ptr = nullptr;	//ptr为常量指针
```

20. 处理类型

**类型别名** 类型的另一个名字，使类型的意义更直观。
**别名声明**using关键字
typedef关键字

```
typedef double wages;	//typedef
using wages = double;	//using
using SI = Sales_item;	//using
```

```
typedef char *pstring;	//(char *)
const pstring cstr = 0;	//指向字符的常量指针
const char *cstr = 0;	//指向字符常量的指针
```

21. auto类型说明符

**auto类型说明符** 让编译器去分析表达式所属的类型。
- auto变量必须有初始值
- 编译器推断的auto类型有时会和初始值的类型并不完全一样，编译器会适当的改变结果类型使其更符合初始化规则。
引用初始化auto类型

```
int i = 0, &r = i;
auto a = r;	//a为整型
```

const初始化auto类型，忽略顶层const，保留底层const

```
const int ci = i, &cr = ci;
auto b = ci;	//b整型，忽略顶层const
auto c = cr;	//c整型，忽略顶层const
auto d = &i;	//d整型指针
auto e = &ci;	//指向整型常量的指针（对常量对象取地址是一种底层const）
const auto f = ci;	//f 顶层const类型
```

auto引用类型

```
auto &g = i;	//整型引用
auto &h = 43;	//错误，非常量不能引用字面值
const auto &j = 43;	//常量可以引用字面值
```

- auto 引用时顶层const特性还在，引用初始化auto时，忽略顶层const

一条auto定义语句中，定义多个变量，初始化的**基本数据类型**必须一致。

```
auto k = ci， &l = i;	//正确，k整型 l整型引用
auto &m = ci，*n = &ci;	//错误
auto &o = i， *p = &ci;	//正确
```

22. decltype类型指示符

decltype类型说明符 选择并返回表达式的数据类型，不计算表达式的值。

```
decltype(f()) sum = x;
decltype(i) s = y;
```

- decltype使用的表达式是个变量，则decltype返回该表达式的类型，包括顶层const和引用（注意初始化问题）。
- 引用作为对象的同义词出现，在decltype处的用法是个例外。

**decltype 和引用**
decltype使用的表达式不是一个变量，则返回表达式结果对应的类型。有特殊情况，会返回一个引用类型，见4.1.1。一般来说，这种情况意味着**该表达式的结果对象能作为一个赋值语句的左值**。

```
int i = 42, *p = &i, &r = i;
decltype(r + 0) b;	//正确，加法结果是int，b为未初始化的int
decltype(*p) c;	//错误 c为int &类型，必须初始化
```

- decltype(r)为引用类型，decltype(r+0)为int类型，想让引用类型变int时，可以使用此技巧
- decltype(×p) 解引用操作符/× 得到的是引用类型
- decltype()的类型与表达式的形式相关，特别是括号

```
decltype((i)) d;	//d为引用类型
decltype(i) e;	//e为int型
```

变量不加括号，结果就是变量的类型；变量加一层或多层时，编译器看作表达式，结果变量可以看做一种作为赋值语句左值的特殊表达式，会得到引用类型

23. 自定义数据结构

**结构体** struct 类名 类体{};
- 类体可以为空
- {}内为新的作用域，类内部名字必须唯一，可以与类外部名字重复
- 只有数据成员

```
struct dataset{
/*

*/
} INRIA，COCO;
struct dataset{
/*

*/
};	//;不能少
dataset INRIA， COCO;
```

结构体的**类内初始值**	//C++11
用来初始化数据成员，没有初始值的数据成员被默认初始化
初始化方式：{}、=，不能使用（）。 

24. 头文件与预处理

**头文件**

```
#include <iostream>
#include "conv_layer.h"
```

**预处理器** 继承自C语言
- **头文件保护符** 无视作用域规则

```
#define

#ifdef ....
#define ....
#endif

#ifndef ....
#define....
#endif
```

- 预处理变量唯一。通常的做法：基于头文件中类的名字构建头文件保护符的名字
- 预处理变量的名字全部大写，避免与其他实体命名冲突

ch2 end

## 第三章 字符串、向量和数组
### 3.1 命名空间的using声明
[**using**关键字](https://www.zhihu.com/question/26911239)
原则上多使用using命令`using std::cin`，尽量避免using编译命令`using namespace std;`
using声明形式

```
using namespace::name;
```

using原则：
- 每个名字都需要独立的using声明
- 头文件不应包含using声明

### 3.2 标准库类型 string
string 表示可变长的字符序列，属于std命名空间

```
#include<string>
using std::string
```

**string 定义和初始化**

```
string s1;	//默认初始化
string s2(s1);	//s2是s1的副本
string s3("sval");	//s3是"sval"的副本
string s4 = s1;	//等价与s2式
string s5 = "sval";	//等价与s3式
string s6(5, 'c');	
```

- 使用’=‘，拷贝初始化 s4 s5
- 不使用’=‘ 直接初始化 s1 s2 s3 s6
- 不是s6这种多值初始化，拷贝和直接通用，s6只能使用直接初始化，或者使用临时对象来实现拷贝初始化

```
string s7 = string(5, 'c')
```

**string对象常用操作**

```
os<<s;	//将s写入os流，返回os对象
is>>s;
getline(is, s);	//读一行字符串到s，返回is对象
s.empty();	//s是否为空
s.size();	//s字符个数
s[n];	//返回第n字符的引用
s1 + s2;	//字符串拼接
s1 = s2;	//字符串拷贝赋值
s1 == s2;	//比较
s1 != s2;
<, <=, >, >=;
```

**读写string对象**

```
string s;
cin >> s;
cout << s << endl;
```

输入：“       Hello World！        ”
输出：Hello
string对象会自动忽略开头的空白（空格符、换行符、制表符等），从第一个真正的字符开始读，到第一个空格结束

```
cin >> s1 >> s2;
cout << s1 << s2 << endl；
```

输入：“       Hello World！        ”
输出：HelloWorld！
连续输入，以空格分隔符
 **读取未知数量的string**

```
 string word;
 while(cin >> word){
	 cout << word << endl;
}
```

**getline读取一整行string**

```
decltye(cin) getline(cin, s);
```

- getline()以换行符为终止符，可以保留输入过程中的空白。
- cin包含换行符，s不包括换行符

**empty() size()**

```
bool s.empty();
```

```
string::size_type s.size();
```

- 大多数标准库都定义了几种配套的类型，体现标准库类型与机器无关。
- string::size_type 为无符号整型。**有关string，应避免使用int**

**string比较**
依照大小写敏感的字典顺序进行比较
- 长度不同，短的string跟长的string前一部分一致，短string对象小
- 两个string某些位置不同，首次不同的字符的大小为两个string的大小

**string赋值** 在标准库设计时易用性像内置类型看齐，因此大多数标准库类型支持赋值操作

**string +**
string + string		//允许
string + 字符串字面值	//允许, 字符串字面值不是string类型（兼容C，以及历史原因）
字符串字面值 + string	//不允许
字符串字面值 + 字符串字面值	//不允许
string + 字符串字面值 + 字符串字面值	//允许，等同于(string + 字符串字面值) + 字符串字面值

**处理string中的字符**

```
#include<cctype>	//cctype即C中的ctype,具体is...\to...函数见p82
```

建议使用C++版本的C标准头文件cname，避免C标准头文件name.h@p82


**范围for语句 处理string每一个字符**
范围for语句

```
for (循环变量: 序列表达式)	//循环变量用于访问序列的基础元素
	statement
```

```
string str("some string");
for (auto c: str)	//auto
	cout << c << endl;//没有{}?
```

范围for语句 改变字符串中的字符, 需要吧循环定义为引用类型

```
string str("some string");
for (auto &c: str)	//auto
	c = toupper(c);
```

**string下标(索引)**
访问string对象的单个指定字符: 1.下标, 2. 迭代器
下标运算符[]

```
s[string::size_type]
```

- 下标参数不可超出string对象, 不可访问空string对象, 因此在使用string对象时,要检查是否为空string
- 字符串不是常量, 下标返回的字符可以赋值

**使用下标方法迭代string**

```
for(decltype(s.size()) index =0; index != s.size() && !isspace(s[index])；++index)	//decltype(s.size()) , 分号
	s[index] = toupper(s[index]);
```

无论合适使用字符串的下标,需检查下标变量的合法性 - string::size_type无符号性质

### 3.3 标准库类型 vector

