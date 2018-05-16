---
title: "lambda&bind知识要点"
date: 2017-06-01 13:04:23
categories:
- C++
- C++11
- C++ primer
- 笔记
---

# lambda

lambda本质是一个匿名类, 也可以理解为内联函数.

当定义一个lambda时,编译器生成一个与lambda对应的新的类类型(未命名).

捕获上层作用域变量为类的数据成员, 在对象创建时被初始化.

lambda可作为函数的返回值.

```c++
#inlcude<funtional>
```



## 值捕获

值捕获发生在对象创建时, 值捕获的前提是变量可以被拷贝.

```c++
size_t v1 = 42;
auto f = [v1] { return v1; }; 	//42
v1 = 0; 	//0
auto j = f(); 	//42
```

## 引用捕获

引用捕获对不能拷贝的对象是必要的, 引用捕获的局部变量存在作用域限制, 在作为返回值时,不能使用引用捕获.

```c++
size_t v1 = 42;
auto f = [&v1] { return v1; }; 	//42
v1 = 0; 	//0
auto j = f(); 	//0
```

## 隐式捕获

编译器自动推断捕获变量, 以及捕获形式

- `[=]` : 自动推断为值捕获
- `[&]` : 自动推断为引用捕获

特别的, `[=, &m1, &m2, ...]` : 除指定变量为引用捕获, 其他自动推断为值捕获; `[&, m1, m2, ...]` :  除指定变量为值捕获, 其他自动推断为引用捕获.

## 可变lambda

对于捕获变量的值有改变需求, 需将lambda声明为`mutable`

```c++
size_t v1 = 42;
auto f = [v1] () mutable { return ++v1; };	//43
v1 = 0;		//0
auto j = f(); 	//43
```

## 尾置返回类型的使用

若lambda函数体只含有一个return语句, 可省略; 若还包含其他语句时, 需指定尾置返回类型, 不指定后置返回类型编译器默认返回void.



# 参数绑定

lambda表达式适用于短小的, 调用次数少的情形.

多次使用,或者实现复杂的需使用函数, 对于无捕获值的lambda改写简单, 否则需使用参数绑定.

`bind`接受一个可调用对象, 生成一个新的可调用函数

```c++
#include<functional>
```

##标准库bind函数 

`_n`占位符: `using std::placeholder::_1;` ,`using namespace std::placeholder`

调用bind函数,只需对`_n`依次赋实参,实参的类型从bind形参中对应的函数的形参对应顺序得出.

```c++
bool check_size(const string &, string::size_type);
auto check6 = bind(check_size, _1, 6);
auto checkn = bind(check_size, _1, _2);
```

利用bind实现参数重排

```c++
bool f(string &, string &);
auto g1 = bind(f, _1, _2);
auto g2 = bind(f, _2, _1);
g1("hello", "hi");
g2("hello", "hi");
```

## 标准库ref函数

`ref()`, `cref()`在绑定时对引用捕获方法的实现, 不能直接使用`&`

```c++
#include<functional>
for_each(words.begin(), words.end(), bind(print, ref(os), _1, ' '));
```



