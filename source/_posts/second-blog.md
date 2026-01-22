---
title: 初试C++ 2.0
tags: C++
date: 2025-03-20
top_img: transparent
comments: false
---

# 初学 C++

---

## string 类与操作

### 初识类

**类是现实世界或抽象世界中概念，是关于同类事物定义的描述**

在标准库中有很多类，通过以下类可体验与 C 语言的不同
• < string >库中的`std::string` 类
• 输入/输出库中的 `std::ifstream, std::ofstream` 类

### string 类 与 C 风格字符串

C 风格字符串的缺点：

1.C 风格字符串是使用 null 字符 '\0' 终止的一维字符数组。因而字符串定义时，必须事先知道保留多大空间存储字符串

2.在字符串操作，如 strcat，时要保证目标字符串有足够的空间

3.使用 malloc 动态空间处理字符串，对一般程序员来说太复杂，非常容易出现
BUG

### 创建 string

```cpp
string s1;
string s2 = "c++";
string s3 = s2;
string s4 (5, 's');//s4 初始化为 5 个 s
```

### string 对象的运算

`string` 类实现了以下运算符的重载

1.联接运算：`operator+`，连接两个字符串或者一个字符串和一个字符

2.比较运算：`operator,<,<=,>,>=,<=>`（c++20 起），以字典序比较两个字符串(关系运算符)

3.下标运算：`operator[]`，访问指定字符，返回指定字符引用，即可作为左值

4.赋值运算：`operator=，+=`，右操作数可以是字符串，字符，C 字符串，字符数组

ps:字符串比较运算符`==,!=`的作用是：逐字符比较两个字符串内容

### string 对象的操作

#### 函数重载

C++ 则允许多个函数拥有相同的名字，只要它们的参数列表不同就可以，这就是函数的重载

```
以 string 对象的 find 函数为例：
• s.find(str) 在 s 中查找子串 str
• s.find("hello",3) 在s中从第4个位置开始查找子串"hello"
• s.find('H')在 s 中查找字符'H'
```

#### 基本操作

**`length()` 返回类型 `size_t` 的字符串的长度**

**`c_str()` 返回类型 `const char \*` 的 C 风格字符串。以 null 结束**
例：

```cpp
/* str-method-basic.cpp */
#include<iostream>
#include<string>
using namespace std;
int main() {
string s = "sysu-computer";
int len = s.length();
const char* cs = s.c_str();
cout << cs << " len is "
<< len << endl;
return 0;
}
```

#### 查找、替换

**`find(string|char\*|char s [,int pos=0])` 寻找首个等于给定字符序列的子串。搜索始于 pos，返回类型 size_t 的位置。rfind 从右边开始搜索**

**`replace(int pos, int count, string|char\*|char s)` 替换指定范围内容**
例：

```cpp
/* str-method-find.cpp */
#include<iostream>
#include<string>
using namespace std;
int main() {
string s = "sysu-computer";
int pos = s.find("computer");
s.replace(pos,8,"software");
cout << s << endl;
return 0;
}
```

#### 添加，插入，删除

`append(string)` 后附 string str

`append(int count, char ch)` 后 附 count 个字符 ch

`insert(int index, int count, char ch)` 在 index 位置插入 count 个 ch

`insert(int index, string|char\* s)` 在 index 位置插入 s

`erase(int index, int count)` 删 除 从 index 开始的 count 个字符

`clear()` 清空
注：clear() 仅清空字符串中的有效字符（即 size() 变为 0），但不会释放已分配的内存空间。这意味着 capacity()（预分配的内存容量）不会改变

例：

```cpp
/* str-method-insert.cpp */
#include<iostream>
#include<string>
using namespace std;
int main() {
string s = "ysu";
s.insert(0,1,'S').append(1,'-').append("Xcomputer");
s.erase(5,1);
cout << s << endl;
return 0;
}
```

#### 子串、比较、拷贝、交换

• `substr(int pos, int count)` 返回子串对
象

• `compare(string|char\* str)` 与 str 比较，
返回 1，0，-1 之一

• `compare(int pos, int count`,
string|char\* str) 字串与 str 比较，返回
1，0，-1 之一
compare 用于比较两个字符串的字典序关系

• `copy(string|char\* dest, int pos, int
count)` 将子串复制到目标对象

• `swap(string other)` 与 other 交换内容

例：

```cpp
/* str-method-substr.cpp */
#include<iostream>
#include<string>
using namespace std;
int main() {
string s = "Sysu-Computer";
cout << s.substr(0,4) << " ";
if (s.compare(5,8,"Computer")==0) {
cout << "OK!" << endl;
}
return 0;
}
```

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
int main() {
    int a = 5, b = 10;
    cout << "交换前：a=" << a << ", b=" << b << endl;
    swap(a, b); // 直接调用标准库的swap函数
    cout << "交换后：a=" << a << ", b=" << b << endl;
    return 0;
}
```

## 使用 C 标准库

C 标准库，即不仅可以用 g++ 编译 c 风格的 cpp 程序，也可以使用 C 库函数

## const & constexpr

const ：
• 限定为不可修改（只读）变量
• 限定作为常量或字面量

constexpr ：
声明编译时可以对函数或变量求值。
• 限定为常量表达式
• 限定为编译时可优化执行的函数

```cpp
constexpr int d() { return 40; }
```

constexpr 函数允许返回编译期常量。此处返回 40 是合法的常量表达式，且函数体符合 constexpr 的语法要求

**即凡是表达“只读”语义的场景都使用 const，表达“常量”语义的场景都使用 constexpr**

## auto，bool 关键字

### auto

auto 的原理就是根据初始化的表达式，在编译期间根据表达式类型自动推
导出对象的类型。

```cpp
auto x1 = 5; 等价于 int x1 = 5;
```

注意点： 1.用 auto 声明的变量必须初始化 2.函数和模板参数不能被声明为 auto 3.因为 auto 是一个占位符，并不是一个他自己的类型，因此不能用于类型转换或其
他一些操作，如 sizeof 和 typeid 4.定义在一个 auto 序列的变量必须始终推导成同一类型

```cpp
auto x1 = 5,x2 = 5.0,x3='r'; //这样写是不合法的，因为各个变量类型不同
```

### bool

bool - 是存放两个值 true 或 false 之一的类型。

#### 初始化

```cpp
bool flag = false;
bool flag = 1;
```

#### 隐式转换规则 ​

任何非零的整型值（无论是正数还是负数）在转换为布尔型时都会变为 true，而零值会转换为 false。因此，-5 作为非零整数，赋值给布尔变量时会得到 true

# 理论题

## typeid 操作符

### 1.​ 获取类型信息

```cpp
float f = 1.1f;
std::cout << typeid(f).name(); // 可能输出"float"（依赖编译器实现）
```

### 2.类型检查与比较

```cpp
Base* ptr = new Derived();
if (typeid(*ptr) == typeid(Derived)) { // 若Base是多态类（含虚函数）
// 执行派生类相关逻辑
}
```

## empty 函数

判断字符串是否为空的成员函数

```cpp
std::string s = "";
if (s.empty()) {
    std::cout << "字符串为空";
}
```

## find

当 find()方法找不到子串时返回：**`string::npos`**

string::find()的返回值类型是 size_type（无符号整型），而非简单的 intbool。当查找失败时，它不会返回-1 或 false，而是返回一个特殊常量 string::npos

## 范围 for 循环

范围 for 循环遍历字符串 str 中每个字符

```cpp
for(char c : str) {
    cout << c; }
```

## 双引号 " " 与 单引号 ' '

### 双引号 ""

双引号 ""：字符串字面量（String Literal）
用途：表示一个字符串（多个字符组成的序列）。

类型：`const char\*（C 风格字符串）或隐式转换为 std::string`

```cpp
const char* s1 = "Hello";   // C风格字符串
std::string s2 = "World";   // C++字符串对象
```

### 单引号 ''

单引号 ''：字符字面量（Character Literal）
用途：表示一个单个字符。

类型：`char。`
[========]

[========]

内存表示：仅存储单个字符的 ASCII 值。

```cpp
char c1 = 'A';        // 正确
char c2 = '\n';       // 正确（转义字符）
// char c3 = 'AB';    // 错误：单引号内只能有一个字符
```

注：**混淆二者会导致编译错误或逻辑错误。**

错误 1：单引号包裹多个字符

```cpp
char c = 'AB';  // 编译错误！单引号内只能有一个字符
```

错误 2：混淆字符和字符串比较

```cpp
std::string s = "A";
if (s == 'A') {  // 错误！'A'是char，s是std::string
    // ...
}
// 正确写法：if (s == "A") 或 if (s[0] == 'A')
```

错误 3：误用双引号初始化 char

```cpp
char c = "A";  // 错误！"A"是const char*，无法赋值给char
// 正确写法：char c = 'A';
```

## vector 容器

等待填坑
