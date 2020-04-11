---
title: "C++模板类型推导"
date: 2020-04-11T20:00:17+08:00
type: posts
categories: [C++]
keywords: [C++模板类型推导]
tags: [C++,类型推导]
---

模板是C++的重要特性，是C++标准模板库的基础。模板可以根据数据类型自动生成代码，大大减少重复代码。模板实例化的时候编译器需要根据具体变量推导数据类型，模板推导出的类型很多时候是显而易见的，有些时候却不太明显，本文详细阐述一下C++模板的类型推导机制。
<!--more-->
在C++中声明一个模板函数的伪代码如下：
```cpp
template<typename T>
void f(ParamType param);
```

上面的`ParamType`与`T`不一定相同，比如：
```cpp
template<typename T>
void f(const T& param);
```
此时`ParamType`的类型是`const T&`。

调用模板函数方式如下：
```cpp
f(expr);
```
编译的时候，编译器通过表达式`expr`推导出两个类型`ParamType`与`T`。

模板的类型推导与`ParamType`密切相关，根据`ParamType`的类型可以分为三种情形：
1. **ParamType** 既不是指针也不是引用。
1. **ParamType** 是指针或引用，不是通用引用。
1. **ParamType** 是通用引用。

下面分别讨论一下三种情形：

### **ParamType** 既不是指针也不是引用
即`ParamType`与`T`相同，此时模板如下：
```cpp
template<typename T>
void f(T param);
```
这种情况下参数`param`的类型`T`可以理解为**值传递**（`param`会复制一份`expr`）时的类型。这意味着：
* 如果`expr`是一个引用，忽略引用部分。
* 如果`expr`带有`const`或`volatile`，忽略`const`、`volatile`。

举个例子：
```cpp
int x = 27;
const int cx = x;
const int& rx = x;
f(x);
f(cx);
f(rx);
```
上面的三种调用方式，`T`的类型都是`int`。

需要注意下面一种情况：
```cpp
const char* const ptr = "Fun with pointers";
f(ptr);
```

`ptr`是指向字符串常量的常量指针，此时进行**值传递**`T`的类型应为`const char*`。因为：
1. 开头的`const`修饰的是指向的对象，表示指向的字符串不可变，不可忽略，否则指向的类型就不对了。
2. `*`右边的`const`修饰的是指针，表示指针本身不可变，在**值传递**的情况下指针是被复制一份，该`const`没有意义。

### **ParamType** 是指针或引用，不是通用引用
这种情况下，推导步骤如下：
1. 如果`expr`是一个引用，忽略引用部分。
1. 将`expr`类型与`ParamType`进行模式匹配，先确定ParamType，再根据`ParamType`推导`T`。

举个例子：
```cpp
template<typename T>
void f(T& param);  // 参数是引用类型

int x = 27;
const int cx = x;
const int& rx = x;

f(x);  // x是int类型，ParamType类型是int&，所以T是int类型
f(cx); // cx是const int类型，ParamType类型是const int&，所以T是const int类型
f(rx); // rx是const int&类型，忽略引用部分，同cx，ParamType类型是const int&，所以T是const int类型
```

如果把参数类型改为`const T&`，相应的`T`的类型也会有一点变化：
```cpp
template<typename T>
void f(const T& param);

int x = 27;
const int cx = x;
const int& rx = x;

f(x);  // x是int类型，ParamType类型是const int&，所以T是int类型
f(cx); // cx是const int类型，ParamType类型是const int&，所以T是int类型
f(rx); // rx是const int&类型，忽略引用部分，同cx，ParamType类型是const int&，所以T是int类型
```

如果参数是指针类型，也类似：
```cpp
template<typename T>
void f(T* param);  // 参数是指针类型

int x = 27;
const int *px = &x;

f(&x); // &x是int*类型，ParamType类型是int*，所以T是int类型
f(px); // px是const int*类型，ParamType类型是const int*，所以T是const int类型
```

### **ParamType** 是通用引用
这种情况下，推导规则如下：
* 如果`expr`是左值，那么`T`和`ParamType`都是左值引用。这条规则很特殊：首先，这是唯一一种`T`被推导为引用类型的情形；其次，`ParamType`的声明形式是右值引用的语法，但是实际类型为左值引用。
* 如果`expr`是右值，推导规则与普通引用一致。

举例：
```cpp
template<typename T>
void f(T&& param); // 参数是通用引用

int x = 27;
const int cx = x;
const int& rx = x;

f(x);  // x 是左值，所以T和ParamType都是int&类型
f(cx); // cx 是左值，所以T和ParamType都是const int&类型
f(rx); // rx 是左值，所以T和ParamType都是const int&类型
f(27); // 27 是右值，ParamType是int&&类型，所以T是int类型
```

以上三种情形就是C++模板类型推导的全部规则了。下面简单补充说明一下C++里两种特殊的参数类型：数组参数和函数参数。

### 数组参数
在C语言和C++里，如下的两个函数声明是完全等价的：
```cpp
void myFunc(int param[]);
void myFunc(int* param);
```
即函数的**数组参数会被当成指针参数**。

如果要使用真正的数组参数，在C++中可以使用数组引用类型：
```cpp
void myFunc(int (&param)[5]); // 使用数组引用时必须指定数组大小，同定义数组一样
```
`int (&param)[5]`表示一个大小为5的整数数组引用类型。

了解数组参数的以上特点后，可以很容易的理解模板如何推导数组类型的参数。举两个例子：
```cpp
template<typename T>
void f(T param);

const char name[] = "J. P. Briggs";
f(name); // name是数组，param是值传递, 数组参数当成指针参数，因此param的类型是const char*
```

```cpp
template<typename T>
void f(T& param);

const char name[] = "J. P. Briggs";
f(name); // name是数组，param是引用类型，因此param的类型是const char(&)[13]
```

### 函数参数
除了数组参数，函数参数也会被当成函数指针。函数参数的类型推导规则跟数组参数完全一样。
```cpp
void someFunc(int, double);

template<typename T>
void f1(T param);

template<typename T>
void f2(T& param);

f1(someFunc); // param是值传递, 函数参数当成函数指针，因此param的类型是void (*)(int, double)
f2(someFunc); // param是引用类型，因此param的类型是void (&)(int, double)
```
实际使用中函数指针和函数引用基本没有区别。两者调用时都可以解引用或直接调用，唯一的区别是引用初始化时只能用函数名称，不能在前面加&。
```cpp
void (*pf)(int) = someFunc; // 也可以写成 void (*pf)(int) = &someFunc;
void (&rf)(int) = someFunc;

pf(8, 1.2); // 也可以写成 (*pf)(8, 1.2);
rf(8, 1.2); // 也可以写成 (*rf)(8, 1.2);
```

总结：
* ParamType 既不是指针也不是引用时，采用值传递模式，忽略表达式的引用部分、const、volatile。
* ParamType 是指针或引用，不是通用引用时，忽略引用部分，进行模式匹配，先确定ParamType，再推导T。
* ParamType 是通用引用时，左值特殊对待（T和ParamType都是左值引用）。
* 数组参数、函数参数非引用传递时当作指针，引用类型不会。
