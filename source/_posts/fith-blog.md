---
title: 数据抽象和类II
tags: C++
top_img: transparent
comments: false
---

# 数据抽象和类 II

## C 与 C++动态对象（变量）管理

堆（Heap）
• 共享的对象（变量）空间
• 由 stdlib 库管理

### C

必须 #include <stdlib.h>
• 申请空间，`void * malloc(size_t)`
• 释放空间，`void free(void *)`

```c
#include<stdio.h>
#include<stdlib.h>
typedef struct { int x; int y; } Point;
int main() {
// 分配变量或一维可变数组
Point *p1 = malloc(sizeof(Point) * 10);
// 分配二维数组(n,m是常数)
Point (*p2)[2][3] = malloc(sizeof(Point) * 6);
// 分配数组的数组
int n = 2, m = 3;
Point **p3 = malloc(sizeof(Point *) * n);
for (size_t i = 0; i < n; i++){
p3[i] = malloc(sizeof(Point) * m);
}
// do somesthing
free(p1);
free(p2);
// 必须先释放行数组
for (size_t i = 0; i < n; i++){
free(p3[i]);
}
free(p3);
return 0;
}
```

### C++ 动态对象（变量）与对象指针

<font color=red>**new 和 delete 运算符**</font>
**new**
• 分配空间
• 每个对象调用构造器
• 若内存分配失败，**抛出异常，不是 NULL**

```
指针=new 类型名；//动态创建一个对象
指针=new 类型名（初始化参数）；//动态创建一个对象并指定初始化参数
指针=new 类型名[数组长度]；//用于动态分配数组
```

**返回该类型的指针**
注：不是万能指针 `void *`

**delete []p**
• 为数组每个对象析构
• 释放空间

```
delete 变量名；//基本用法
delete []变量名；//用于释放数组
```

如果动态分配了一个数组，但是却用 delete p 的方式释放，没有用[]，则编译时
没有问题，运行时可能不会发生错误，但实际上会导致动态分配的数组没有被完
全释放。

用 delete 释放空间后，指针的值仍是原来指向的地址，但指针已无效（重复释放
将出错）

例：

```cpp
#include<iostream>
using namespace std;
void inc(int *p, int Length) {
for (int i = 0; i < Length; i++)
p[i]++;
}
int main() {
int *p;
int Length, i;
cout << "Enter the lenght you want: ";
cin >> Length;
p = new int[Length]{1,3,5};
inc(p, Length);
for (i = 0 ; i < Length; i++)
cout << *(p+i) << " ";
delete []p;
}
```

#### 对象指针访问成员

```cpp
int main() {
Text* str_ptr;
Text str("on stack 1");
str_ptr = &str;//取对象地址
str.Print(); //对象取成员
str_ptr->Print();//指针取成员
}
```

## =delete 和 =default

### 显式定义默认函数：=default

当我们声明有参构造函数时，编译器不会创建默认构造函数。

• =default 必须在函数申明后，让编译创建该构造函数

**• 特殊成员函数包括：**
➢ 默认构造函数:T()
➢ 析构函数:~T()
➢ 复制构造函数:T(const T&)
➢ 赋值运算:operator=(const T&)
➢ …

• 函数不能再自定义实现
例：

```cpp
#include <iostream>
using namespace std;
class A
{
public:
A(int x) {
cout << "This is a parameterized constructor";
}
A() = default;
};
int main(){
A a; //call A()
A x(1); //call A(int x)
cout<<endl;
return 0;
}
```

### 弃置函数：=delete

任何弃置函数的使用都是非良构的（程序无法编译）

删除的函数是隐式内联的，这一点非常重要，删除的函数定义必须是函数的**首次声明**

```cpp
例：
class A {
public:
A(int x): m(x) { }
// Delete the copy constructor
A(const A&) = delete;
// Delete the copy assignment operator
A& operator=(const A&) = delete;
int m;
};

以下方法是将函数声明为已删除的正确方法：
class C {
public:
C(C& a) = delete;
};
但是以下尝试声明删除函数的方法会产生错误：
// incorrect syntax of declaring a member function as deleted
class C {
public:
C();
};
// Error, the deleted definition of function C must be the first declaration of the function.
C::C() = delete;
```

## 对象成员初始化

#### 默认成员初始化器

它是成员声明中包含的花括号或等号初始化器

#### 成员初始化器列表

```cpp
struct Ray {
Ray (Vector3 origin, Vector3
direction):origin(origin),direction(direction) {};
Vector3 origin; //射线的原点。
Vector3 direction; //射线的方向。
};
```

**必须使用初始化器列表的时候:**

1.常量成员，因为常量只能初始化不能赋值，所以必须放在初始化器列表里面

2.引用类型，引用必须在定义的时候初始化，并且不能重新赋值，所以也要使用初始化器

3.没有无参构造函数的类成员，使用初始化器列表可以使用有参构造函数初始化

成员是按照他们在类中声明的顺序进行初始化的，而不是按照他们在初始化器
列表出现的顺序初始化的：

```cpp
class foo {
public:
int i ;int j ;
foo(int x):i(x), j(i){}; // ok, 先初始化i，后初始化j
};
```
