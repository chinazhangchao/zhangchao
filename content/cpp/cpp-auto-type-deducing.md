---
title: "C++ auto 类型推导"
date: 2020-04-12T12:00:17+08:00
type: posts
categories: [C++]
keywords: [C++ auto 类型推导]
tags: [C++,类型推导]
---

C++ auto 类型推导规则与[模板类型推导]({{< ref "cpp-template-type-deducing.md" >}})类似，两者只有一点不一样。
前面说过，模板的声明和使用方式如下：
```cpp
template<typename T>
void f(ParamType param);

f(expr);
```
auto的使用方式如下：
```cpp
auto x = 27;
const auto cx = x;
const auto& rx = x;
```
在auto进行类型推导的时候，auto相当于模板里的T，变量类型修饰符相当于ParamType，等号右边的部分相当于expr。例如：
```cpp
auto x = 27;
// 进行x的类型推导，就如同下面的模板类型推导
template<typename T>
void func_for_x(T param);
func_for_x(27);
// 27是int型，根据上篇文章的推导规则，param是int型，因此x也是int型。

const auto& rx = x;
// 进行rx的类型推导，就如同下面的模板类型推导
template<typename T>
void func_for_rx(const T& param);

func_for_rx(x);
// x是int型,模板推导出的param类型是const int&，因此rx也是const int&。
```
auto类型推导规则根据类型修饰符也分三种情形，与[模板类型推导]({{< ref "cpp-template-type-deducing.md" >}})完全一致，对数组参数和函数参数的处理也完全一致，不再赘述。

开头说过，有一种情形auto与模板类型推导不一致。C++98初始化变量有两种方式：
```cpp
int x1 = 27;
int x2(27);
```
C++11增加了统一初始化语法：
```cpp
int x3 = { 27 };
int x4{ 27 };
```
这四种初始化方式效果都一样，把变量初始化为27。

但是如果把变量类型改成auto：
```cpp
auto x1 = 27;
auto x2(27);
auto x3 = { 27 };
auto x4{ 27 };
```
此时，前两个变量的类型还是int。后面两个变量的类型变成了`std::initializer_list<int>`。

这是auto推导规则的一个特殊的地方，**如果auto的变量初始化时使用了大括号，推导类型就是`std::initializer_list`。然而，模板推导时如果使用了大括号，则推导失败。**
```cpp
auto x = { 11, 23, 9 }; // x类型是std::initializer_list<int>

template<typename T>
void f(T param);

f({ 11, 23, 9 }); // 编译失败，无法推导出{ 11, 23, 9 }类型
```
如果把模板参数改为如下形式，则可推导成功：
```cpp
template<typename T>
void f(std::initializer_list<T> initList);

f({ 11, 23, 9 }); // 编译通过，initList类型是std::initializer_list<int>，T是int型
```
因此auto和模板类型推导的区别在于，**auto会把大括号初始化推导为std::initializer_list，模板不会。**

最后，还有一点需要注意的地方，C++14允许指定函数返回值为auto，C++14的lambda表达式也允许参数类型为auto。**这两种情况下的auto类型推导采用模板类型推导的规则。**

因此下面两段代码都编译不通过：
```cpp
auto createInitList() {
   return { 1, 2, 3 }; // 编译失败，无法推导{ 1, 2, 3 }类型
}
```
```cpp
std::vector<int> v;

auto resetV = [&v](const auto& newValue) { v = newValue; };

resetV({ 1, 2, 3 }); // 编译失败，无法推导{ 1, 2, 3 }类型
```

总结：
* auto类型推导与模板类型推导基本一致，除了auto把大括号初始化推导为std::initializer_list，模板不会。
* 函数返回值和lambda表达式参数中的auto采用模板类型推导的规则。
