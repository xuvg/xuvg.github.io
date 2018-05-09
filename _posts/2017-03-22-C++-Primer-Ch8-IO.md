---
title: "C++ Primer 第八章 IO库"
date: 2017-03-22 16:19:57
categories:
- C++
- C++11
- C++ primer
- 笔记
---


# 第八章 IO库
@(Coding)[C++, 笔记, C++ Primer]

## 8.1 IO类
## 8.2 文件输入输出
`fstream`文件IO
1. `ifstream` 从文件读取数据
2. `ofstream'`向文件写入数据
3. `fstream` 读写文件

头文件fstream

操作:
`>>` `<<` `getline`
`fstream fstrm;`未绑定文件流
`fstream fstrm(s);`创建一个stream,默认mode打开名s的文件.s string或C风格字符串的指针. 构造函数都是explicit.
`fstream fstrm(s, mode);` 指定mode打开名s的文件
`fstrm.open(s);`打开s文件,绑定到fstrm
`fstrm.close();`关闭fstrm, 返回值void
`fstrm.is_open();`检查流关联文件是否打开,返回值bool

### 8.2.1 使用文件流对象
在使用基类型对象的地方,可以使用继承类型的对象代替.
例如,接受一个iostream类型的引用或者指针的函数,可以用一个对应的fstream或者sstream类型来调用.

**open/close成员函数
```
ifstream in(infile);
ofstream out;
out.open(ifile + ".copy");
if(out)		///调用open失败,failbit会被置位
			///调用open函数可能失败,在使用out前应检查调用open是否成功.
```
将文件流重新关联一个新文件
```
in.close();
in.open(infile + "2");
```
当一个fstream对象被销毁时,会自动调用close

### 8.2.2 文件模式
```
in 以读方式打开,仅ifstream和fstream对象可用
out 以写方式打开,仅ofstream和fstream对象可用,默认截断打开文件,需同时指定app模式或同时指定in模式
app 每次写操作前定位到文件末尾,前提自动默认为out模式(即使未显式定义out模式)
ate 打开文件后定位到文件末尾,可随意应用
trunc 截断文件,仅在out模式被设定时可用
binary 以二进制方式进行IO,可随意应用
```
ifstream的默认模式为in
ofstream的默认模式为out
fstream的默认模式为in和out

out模式会丢弃已有数据
每次open都会确认文件模式,默认为输出和截断

## 8.3 string流
头文件sstream: istringsream从string读入数据, ostringsream向string写入数据, stringsream两者皆可
特定的IO操作:
```
sstream strm;	///定义一个未绑定的stringstream对象
sstream strm(s);	///定义一个sstream对象,保存string s的一个拷贝.构造函数为explicit
strm.str();		///返回sstream所保存的string的拷贝
strm.str(s);	///将string s拷贝到strm中,返回值void
```

### 8.3.1 使用istringstream
```
struct PersonInfo{
string name;
vector<string> phone;
};

string line, word;
vector<PersonInfo> people;
while(getline(cin, line)){
PersonInfo info;
istringstream record(line);
record >> info.name;
while(record >> word)
	info.phones.push_back(word);
people.push_back(info);
}
```

### 8.3.2 使用ostringstream
```
for(const auto &entry : people){
	ostringstream formatted, badNums;
	for(const auto &nums : entry.phone){
		if(!valid(nums)){
			badNums << " " << format(nums);
		}else
			formatted << " " << fomat(nums);
	
	}
	if(badNum.str().empty())
		os << entry.name << " " << formatted.str() << endl;
	else
		cerr << "imput error" << entry.name << "invalid numbers(s)" << badNums.str() << endl;
}
```

