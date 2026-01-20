---
title: 初试C++ 1.0
tags: C++
top_img: transparent
comments: false
---

# 初学 C++

---

## io 流--输入/输出操作符

### 标头< iostream >

•**endl**，输出 '\n' 并冲洗输出流。例如：cout << endl;

•**dec, hex, oct**，更改用于整数输入/输出的基数（进制）

•**left，right**， 设置填充字符的布置，即左对齐或右对齐

•**fixed，scientific**，更改用于浮点 I/O 的格式化

•**showpoint，noshowpoint**，控制浮点表示是否始终包含小数点

•**showpos，noshowpos**，控制是否将 + 号与非负数一同使用

### 标头< iomanip >

• setw(n)，更改下个输入/输出域的宽度，宽度为 n

• setprecision(n)，更改浮点精度

### 代码示例

```cpp
/* iostream-format.cpp */
#include<iostream>
#include<iomanip>
using namespace std;
int main() {
double myFloat=123.4578;
int myInt=5;
cout << fixed << showpoint << setprecision(3);
cout << setw(10) << left << "Float";
cout << setw(12) << right << myFloat << endl;
cout << setw(10) << left << "Int";
cout << setw(12) << right << myInt << endl;
return 0;
}
```

---

## 命名空间

C++命名空间提供了一种**避免名字冲突的方法**

• 在命名空间块内声明的符号被放入一个**具名的作用域中**，避免这些符号被误认为其他作用域中的同名符号。

• 多个命名空间块的名字<font color=aqua>**可以相同**,**这些块中的所有声明在该具名作用域声明**。</font>

• 例如：标准库中符号（类型、变量、常量、函数等）都在 std 名命空间块中声明。cout 在 std 名命空间块中声明，则 std::cout 则是该变量的有限定名

• **“::”是作用域解析运算符。**

### 声明与使用

#### 语法

**namespace 命名空间名 { 声明序列 }**

**• 声明具名命名空间**

<font color=blue>using namespace 命名空间名 ;</font>  
<font color=blue>using 命名空间名 :: 成员名 ;</font>

### 名命冲突示例

```cpp
/* ns-helloworld.cpp */
#include<iostream>
using namespace std;
int main() {
const char *cout = "hello world c++!";
std::cout << cout << endl;
return 0;
}
```

std::cout 是标准输出流对象**有限定名称**，避免了字符串变量 cout 的命名冲突

### 嵌套示例

```cpp
/* ns-embed.cpp */
#include<iostream>
using namespace std;
namespace sysu {
  namespace students {
    int collegeCount;
    void printColleges();
    }
}

void sysu::students::printColleges() {
cout << "Colleges " << collegeCount << endl;
}

int main() {
using namespace sysu::students;
collegeCount = 23;
printColleges();
return 0;
}
```

---

## 引用

### 引用的概念

• 声明具名的变量引用，即既存对象或函数的别名。

• 引用名**特点**：使用时类似变量，作为参数时能传引用

### 左值声明与示例

左值引用的**申明**：**type &别名 [= 左值表达式]**

```cpp
/* ref-definition.cpp */
#include<iostream>
using namespace std;
int main() {
int a = 1024;
int *p = &a; // p是指针; &a是a的地址
 int &x = a; // x是引用，它实际上与a是同一个变量
 cout << "a = " << a << endl;
cout << "x = " << x << endl;
cout << "*p = " << *p << endl;
x = 2000;
cout << "a = " << a << endl;
return 0;
}
```

### 注意

**引用不是对象**。它们可以不占用存储，尽管若需要分配存储以实现所需语义（例如，引用类型的非静态数

据成员通常会增加类的大小，量为存储内存地址所需），则由编译器具体实现决定。

**因为引用不是对象**，故

<font color=red>**• 不存在引用的数组**

**• 不存在指向引用的指针**

**• 不存在引用的引用**

**• C++不存在引用为空的概念，即引用必须被定义**</font>

### 指针与引用

• **相同**：可以使一个函数向调用者返回多个数值。

• **不同**：原理不同。引用作为函数参数，<font color=blue>**实参是变量本身，形参是实参变量的别名**</font>，使用变量时不需要解引用。而指针作函数参数，则实参是某变量的地址，使用这个地址访问对应的变量必须解引用。指针可以是 null 值，引用不可以是 null 值。

• 从返回值的角度看，**引用形参比利用指针方便**。

---

# 理论题

## 1

```cpp
下述语句的输出是：

cout << 1 + "20.24" << endl << 20.24;
```

**<font color=blue>1 + "20.24" </font>**

"20.24" 是字符串字面量，类型为 const char\*，指向字符 '2' 的地址。

1 + "20.24" 是对指针的算术运算。指针加 1 会移动 sizeof(char)（即 1 字节），因此指向字符串的第二个字符 '0'。

此时，指针指向的子字符串为 "0.24"，cout 会从该位置开始输出，直到遇到终止符 \0，故输出 0.24。

## 2

**<font color=blue>所有同名命名空间块的内容会被合并到同一命名空间</font>**

### 3

```cpp
以下代码的输出结果是什么？

cout << fixed << 3.14159 << endl;
​
3.141590
```

std::fixed 是 C++ 中的流操纵符，用于强制浮点数以固定小数格式输出，而非科学计数法。当未指定精度时，**默认保留 6 位小数**，不足的位数会自动补零

**<font color=red>若需控制小数点后的位数，需结合 setprecision(n)</font>**

例：

```cpp
cout << fixed << setprecision(2) << 3.14159 << endl;
```
