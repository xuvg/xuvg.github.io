---
title: "C++ Primer 第六章 函数"
date: 2017-03-14 18:48:52
categories:
- C++
- C++11
- C++ primer
- 笔记
---

# 第六章 函数
@(Coding)[C++, 笔记, C++ Primer]
## 6.1
## 6.1.1
### 6.1.2
### 6.1.3
## 6.2
### .2.1
### .2.2
### 6.2.3
### 6.2.4 数组形参
以指针的方式给数字形参赋值(一般的,使用数组会将其转换为指针)
不能以值传递的形式给数组形参赋值，但可以写成数组形式(仍为指针)
```
void print(const int *)
void print(const int [])
void print(const int [10])
```
完全三者等价,[10]中10为期望大小,实际不一定.
特别的,数组形参可以传入同类型指针变量
```
int j = 1;
print(&j); //&j为int *
```
正确.

形参数组大小的安全规范:
- 使用标记指定数组长度,数组本身包含一个结束标识符(C风格字符串)
- 使用标准库begin和end函数规范: void print(const int *beg, const int *end)
- 显式传递一个表示数组大小的形参: void print(const int ia[], size_t size)

**数组引用形参**
```
void print(int (&arr)[10]);		///对维度10的数组的引用
void print(int &arr[10]);	///引用类型数组
```
[10] 维度是类型的一部分

**传递多维数组**
二维数组:指向数组的指针
```
void print(int (*matrix)[10], size_t rowsize);
void print(int matrix[][10], size_t rowsize);
void print(int **matrix, size_t rowsize, size_t colsize);		///?
int (*matrix)[10];		///指向维度10的数组的指针
int *matrix[10];	///指针数组
```

### 6.2.5 main: 处理命令行选项
```
int main(int argc, char argv[])
{...}
```
`prog -d -o ofile data0`
argc为5
```
argv[0] = prog;
argv[1] = -d;
argv[2] = -o;
argv[3] = ofile;
argv[4] = data0;
argv[5] = 0 ;
```
argv[0]保存保存程序名字,可选实参从argv[1]开始

### 6.2.6 含有可变参数的函数
用来处理无法提前向函数传递几个实参.
C++ 11提供两种方法:
- initializer_list 标准库类型, 处理相同实参类型
- 可变参数模版, 处理不同类型实参,见16.4
省略符形参 一般只用于与C函数交互的接口程序

**initializer_list**
initializer_list 一种标准库类型, 定义在initializer_list.h中, 主要成员函数:.size(), .begin(), .end().
initializer_list类似与vector 但initializer_list对象元素永远为常量值
initializer_list作为形参时,传入实参变量应采用花括号{}
如`{"func", expected, actual}`

**省略号形参**
varargv C标准库
省略号形参仅适用于C和C++通用的类型,特别的,*大部分*类类型会无法正确拷贝
```
void foo(pram_list, ...);		///部分省略号形参, pram_list正常类型检查, 省略号形参无需类型检查, 省略号形参前的逗号可以省略
void foo(...);		///完全省略号形参
```

## 6.3 返回类型和return语句


