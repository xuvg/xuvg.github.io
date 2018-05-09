---
title: "C++ Primer 第十六章 模板与泛型编程"
date: 2017-05-13 23:18:52
categories:
- C++
- C++11
- C++ primer
- 笔记
---

# 第十六章 模板与泛型编程
@(Coding)[C++, 笔记, C++ Primer]

## 16.1 定义模板
### 16.1.1 函数模板
**函数模板** **模板参数列表** **模板参数**
模板的定义：`template<typename T1,class T2>.... `
**实例化** **实例** 
非类型模板参数：`template<unsigned N,unsigned M>`
当一个模板被实例化时，非类型参数被一个用户提供的或编译器推断的值所代替。模板实参必须是常量表达式.
```
template<unsigned N, unsigned M>
int complate(const char (&p1)[N], const char (&p2)[M]){
	return strcmp(p1, p2);
}
```
一个非类型参数可以是一个整型,或者是一个指向对象或者函数类型的指针或(左值)引用. 绑定到非类型整型参数的实参必须是一个常量表达式.绑定到指针或引用非类型参数的参数必须具有静态的生存期.我们不能用一个普通的(非static)局部变量或动态对象作为指针或引用非类型模板参数的实参.指针参数也可以用nullptr或一个值为0的常量表达式来实例化.

**inline和constexpr的函数模板**
inline和constexpr放在模板参数列表之后, 返回类型之前.

两个重要原则：
- 模板中的函数参数是const的引用
- 函数体中的添加判断仅使用<比较运算, 使用`less<T>`更加,可保证类型无关和可移植性.

**模板编译**
编译器在实例化时生成模板. 函数模板和类模板成员函数的定义通常放在头文件中.

### 16.1.2 类模板
编译器不能为类模板推断模板参数类型.必须在模板名后的尖括号中提供额外信息(**显式模板实参**).

**定义类模板**
```
template <typename T> class Blob {};
```

**实例化类模板**
```
Blob<int>
```

类模板的名字不是类型名. 每个类模板的实例都是一个独立的不同的类.

**在模板作用域中引用模板类型**
如果一个类模板使用另一个模板,通常不使用实际类型(或值)的名字作为模板实参.而是通常将模板自己的参数作为被使用模板的参数.
```
template <typename T> class Blob {
	std::shared_ptr<std::vector<T>> data;   ///这样
	std::shared_ptr<std::vector<Blob<T>>> data;   ///而非这样
};
```

**类模板的成员函数**
定义在类模板之外的成员函数必须以`template`开头,后面接类模板参数列表
```
template <typename T>
void Blob<T>::check(...) {...}
```

**类模板的成员函数的实例化**
默认情况下,一个类模板的成员函数只有在程序用到它的时候才实例化.

**在类代码内简化模板类名的使用**
在类代码(包括类外定义成员函数)内可以不提供模板实参, 直接使用模板名.
```
template <typename T> class BlobPtr{
public:
	BlobPtr& operator++();   ///BlobPtr等同于BlobPtr<T>
};
```

**类模板和友元**
当一个类包含一个友元声明, 类和友元各自是否是模板是相互无关的.
- 类模板包含一个非模板友元, 友元可访问所有模板实例
- 友元自身是模板, 类可以授权给所有友元模板实例,也可以授权给特定实例.

**一对一友好关系**
```
template <typename> class BlobPtr;
template <typename> class Blob;
template <typename T>
	bool operator==(const Blob<T>&, const Blob<t>&);

template <typename T> class Blob{
///每个Blob实例将访问权限授予相同类型实例化的BlobPtr和相等运算符
	friend class BlobPtr<T>;
	friend bool operator==(const Blob<T>&, const Blob<t>&);
}
```

**通用和特定的模板友好关系**
```
template<typename T> class Pal;	///为何不是template <typename> class Pal;
template<typename T> class C2{
    friend class Pal<T>;//C2的每个实例都将实例化的Pal声明为友元，Pal的模板声明必须在作用域内
    template<typename X> friend class Pal2;//Pal2所有实例都是C2的每个实例的友元，无需前置声明
    friend class Pal3;//Pal3是非模板类，是C2所有实例的友元，无需前置声明
```

**令模板自己的参数类型成为友元 C++11**
```
template <typename Type> class Bar{
friend Type;
}
```

**模板类型别名**
```
typedef Blob<string> StrBlob;	///定义一个实例化的类的别名
```
```
template<typename T> using twin = pair<T, T>;
twin<int> win_loss;
```
```
template<typename T> using partNo = pair<T, unsigned>;	///固定模板参数
partNo<string> books;
```

**类模板的static成员**
类模板的static成员需定义为模板:
```
...
static std::size_t ctr;
};
template <typename T>
	size_t Foo<T>::ctr = 0;///定义并初始化

auto ii = Foo<int>::ctr;
```
