---
title: 数据抽象与类
tags: C++
top_img: transparent
comments: false
---

# 数据抽象与类

## 抽象数据类型

1.抽象是从众多的事物中抽取出共同的、本质性的特征，而舍弃其非本质的特征的过程。

2.抽象的过程在于透过事物的现象获得它的本质，并用概念、原理、规律的形式描述它。

3.抽象的意义在于通过事物的本质，识别或区分事物

## 类的声明

### 类关键词

**class 或 struct 或 union **
用 class 定义的类，默认访问权限为`private`
用 struct 定义的结构体，默认访问权限为`public`

### 访问说明符

• `public` 公有，表示后续成员是对象使用者可访问

• `private` 私有，表示后续成员仅对象内部可访问

• `protected` 受保护

### 类成员

• 数据成员，按声明顺序存储，对齐规则同 C。
一般设计中，数据成员需要隐藏保护，需要通过函数成员访问

• 函数成员，公有成员可以被外部使用者访问；
私有成员仅内部访问；
const 函数成员不能修改数据成员

```cpp
类关键词 类标识符 {
  访问说明符：（可选）
  数据成员声明序列；
  函数成员声明序列；
}；
```

例：

```cpp
/* DATE.hpp*/
class DATE {
public:
void Set( int, int, int );
int getMonth() const;
void Increment();
private:
int month;
};
```

**<font color=red>成员函数可以定义在类定义内部，或者单独使用作用域解析运算符 :: 来定义</font>**
例：

```cpp
class DATE {
public:
void Set(int newYear, int newMonth, int newDay );
private:
};
void DATE::Set(int newYear, int newMonth, int newDay ) {
month = newMonth;
day = newDay;
year = newYear;
}
```

## 静态成员变量与非静态成员变量

| 特性       | 静态成员变量                | 非静态成员变量         |
| ---------- | --------------------------- | ---------------------- |
| 存储位置   | 全局数据区                  | 对象内存空间           |
| 生命周期   | 程序运行期间                | 随对象创建和销毁       |
| 共享性     | 所有对象共享同一份数据      | 每个对象有独立副本     |
| 初始化位置 | 类外（const static 可类内） | 构造函数或类内初始化器 |
| 访问方式   | 通过类名或对象访问          | 必须通过对象访问       |
| 线程安全   | 需额外同步机制（因共享）    | 通常线程安全（因独立） |

静态成员变量属于整个类。

## 静态成员函数和非静态成员函数

### 访问权限与作用范围

**静态成员函数**
​ 仅访问静态成员：只能操作类的静态成员变量和静态成员函数，无法直接访问非静态成员（因其不绑定任何对象实例）。
​ 无 this 指针：静态函数没有隐含的 this 参数，因此无法通过 this->访问实例成员。
**非静态成员函数**
​ 访问所有成员：可自由访问类的静态和非静态成员，包括调用其他非静态函数。
​ 隐含 this 指针：通过 this 指针隐式操作当前对象的数据

## 调用方式

​**静态成员函数**
​ 无需对象实例：可通过类名直接调用，如`ClassName::staticFunc()`，也可通过对象调用（不推荐）。
**非静态成员函数**
​ 必须依赖对象：必须通过对象实例调用，如`obj.nonStaticFunc()`，否则无法执行。

## 内存与生命周期

​**静态成员函数**
​ 全局存储：属于类本身，在程序启动时即存在，生命周期与程序一致。
​ 不增加对象大小：静态函数不占用对象内存空间，sizeof(对象)不包含其信息。
​**非静态成员函数**
​ 对象内存关联：每个对象实例拥有独立的非静态成员函数入口地址，但实际代码段共享同一份实现。
​ 随对象销毁失效：函数调用依赖于对象的生命周期。

```cpp
class MyClass {
public:
    static int staticVar;  // 静态成员变量
    int nonStaticVar;      // 非静态成员变量
    static void staticFunc() {
        staticVar = 10;    // ✔️ 可访问静态成员
        // nonStaticVar = 20; ❌ 无法访问非静态成员
    }
    void nonStaticFunc() {
        staticVar = 30;    // ✔️ 可访问静态成员
        nonStaticVar = 40; // ✔️ 可访问非静态成员
    }
};
int MyClass::staticVar = 0;
int main() {
    MyClass::staticFunc(); // 直接通过类名调用静态函数
    MyClass obj;
    obj.nonStaticFunc();  // 必须通过对象调用非静态函数
    return 0;
}
```
