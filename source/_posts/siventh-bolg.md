---
title: 运算符重载
tags: C++
date: 2025-03-20
top_img: transparent
comments: false
---

# 运算符重载

<font color=red>**运算符重载的主要目的是：让运算符适用于自定义类型（类或结构体）**</font>

## 运算符重载概念

运算符重载是**具有特殊函数名的函数**，也具有返回值类型，函数名和参数列表
重载的运算符可以理解为带有特殊名称的函数，函数名是由关键字 operator 和其后要重载的运算符符号构成的。

### 重载的两种方法

1.类成员函数运算符重载（类内定义）
`return_type class_name::operator op(operand2) {}`
重载二元运算符时，成员运算符函数只需显式传递一个参数（即二元运算符的右操作数），而左操作数则是该类对象本身，通过`this`指针隐式传递。

重载一元运算符时，成员运算符函数没有参数，操作数是该类对象本身，通过`this`指针隐式传递

2.友元函数运算符重载（类外定义）
`return_type operator op(operand1, operand2) {}`

<font color=red>**运算符重载至少需要一个操作数是用户定义的类型**</font>

```cpp
MyClass operator+(MyClass a, int b);  // 合法（MyClass 是用户定义类型）
int operator+(int a, int b);          // 非法（两个操作数都是内置类型）
```

<font color=red>**运算符重载函数 ​​ 可以返回基本数据类型**</font>

### 不可以重载的运算符

`::`（作用域解析）
`.`（成员访问）
`.*`（通过成员指针的成员访问）
`?:`（三元条件）
`sizeof`：运算符（包括除 new，delete 外的关
键字运算符，如 alignof，typeid 等等）

**其他限制：**
• **不能创建新运算符**，例如 \*\*、<> 或 &|。

• 运算符 && 与 || 的重载**失去短路求值**。

• 重载的运算符 -> 必须要么返回裸指针，要么（按引用或值）返回同样重载了运算符 -> 的对象。

• 不可能更改运算符的**优先级、结合方向或操作数的数量**。

## 普通运算符重载

双目，单目运算符重载
例：

| 运算符名 | 类内定义                          | 类外定义                            |
| -------- | --------------------------------- | ----------------------------------- |
| 一元加   | T T::operator+() const;           | T operator+(const T& b);            |
| 加法     | T T::operator+(const T& b) const; | T operator+(const T& b,const T& c); |

部分情况下，需要返回左值对象，可使用 T& 作为返回值。例如 operator<< 和 operator>> 作为流插入和流提取的重载所返回的是 T&

## 自增/自减运算符重载

若为前缀自增运算符，直接重载（以++举例）：
`• return_type class_name::operator ++() {}`

```cpp
Integer& operator ++ () {
cerr<<"prefix is invoked"<<endl;
++x；
return *this;
}
```

• 若为后缀自增运算符，该函数有一个 int 类型的虚拟形参，这个形参在函数的主体中是不会被使用的，这只是一个约定，它告诉编译器递增运算符正在后缀模式下被重载：
`• return_type class_name::operator ++(int) {}`
<font color=red>这也是前缀和后缀的主要区分</font>

```cpp
Integer operator ++ (int){
cerr<<"suffix is invoked"<<endl;
return Integer(x++);
}
```

**要点：**

1. 内建前缀运算符与后缀运算符原型不一样
2. 前缀运算符，先增后用，一般返回增后的对象的引用（左值）
3. 后缀运算符，先用后增，先建立原始对象的副本再自增，一般返回副本对象（右值）

## 赋值运算符重载

| 运算符名 | 语法 | 类内定义                        | 类外定义                             |
| -------- | ---- | ------------------------------- | ------------------------------------ |
| 简单赋值 | a=b  | T& T::operator =（const T2& b); | N/A                                  |
| 加法赋值 | a+=b | T& T::operator +=(const T2& b); | T& operator +=（T& a ,const T2& b）; |

注意

·所有内建赋值运算符都返回`*this`，而大多数用户定义重载也会返回`*this`，从而能以与内建版本相同的方式使用用户定义运算符。然而，用户定义运算符重载中，返回类型可以是<font color=red>任意类型</font>包括`void`。

## 类型转换符重载

基本语法

```cpp
operator type() const {
    // 转换逻辑
    return value;
}
```

其中：
1.type 是要转换的目标类型 2.必须是成员函数 3.不能指定返回类型（因为返回类型就是 type） 4.通常应该是 const 的，因为它不应该修改对象的状态

```cpp
class MyInt {
    int value;
public:
    MyInt(int v) : value(v) {}
    // 转换为int的类型转换运算符
    operator int() const {
        return value;
    }
    // 转换为double的类型转换运算符
    operator double() const {
        return static_cast<double>(value);
    }
};

int main() {
    MyInt mi(42);
    int i = mi;      // 隐式调用operator int()
    double d = mi;   // 隐式调用operator double()
    // 显式转换
    int j = static_cast<int>(mi);
}
```

## 浅复制与深复制策略

**浅拷贝赋值**
• 由缺省的赋值运算符完成，会按成员声明顺序逐一调用成员的赋值运算
• 当成员是指针时，被赋值对象与赋值对象共用一个资源

**深拷贝赋值**
• 由用户定义的的赋值运算符完成。该函数不需要逐一调用非指针成员的赋
值运算（但建议按声明顺序逐一赋值）
• 当成员是指针时，需要释放已拥有资源，复制赋值对象对应的资源，并指
向该资源的副本，即被赋值对象拥有赋值对象资源的副本

**深拷贝策略**

```cpp
class IntArray {
int *a;
int n;
public:
IntArray(int n=1):n(n) {
a=new int[n];
for (int i=0;i<n;++i) a[i]=i;
}
~IntArray() {
cout << "release p=" << a <<endl;
delete[] a;
}
IntArray& operator = (const IntArray& other) {
if (this != &other) {
delete[] a; //这里a一定不为空
 n=other.n;
a=new int[n];
memcpy(a,other.a,sizeof(int)*n);
}
return *this;
}
void print() {
for (int i=0;i<n;++i) cout<<a[i]<<" ";
cout<<endl;
}

int main() {
IntArray a(4), b;
a.print();
b=a; // 调用深拷贝策略赋值
 b.print();
a.print(); //a,b point to ...
return 0;
}
};
```

## 下标运算符重载

**成员访问运算符**
C++规定，下标运算符[]必须以成员函数的形式进行重载。该重载函数在类中的声明格式如下：

```cpp
R& T::operator[](S b); (1)
const R& T::operator[](S b) const; (2)
```

(1)声明方式，[]不仅可以访问元素，还可以修改元素。
(2)声明方式，[]只能访问而不能修改元素。

**在实际开发中，我们应该同时提供以上两种形式，这样做是为了适应 const 对象，因为通过 const 对象只能调用 const 成员函数，如果不提供第二种形式，那么将无法问 const 对象的任何元素。**

例：

```cpp
class IntArray {
int *a;
int n;
public:
IntArray(int n=1):n(n) {…}
IntArray(const IntArray& other) {…}
~IntArray() {…}
int& operator[](int i) {
if (i>=0 && i < n) {
return a[i];
}
throw std::out_of_range("out of range");
}
const int& operator[](int i) const{
if (i>=0 && i < n) {
return a[i];
}
throw std::out_of_range("out of range");
}
void print() const {…
}
};
int main() {
IntArray a(4);
for (int i=0;i<4;i++) a[i]=i+1;
a.print();
//任务1：以下两个语句，b的哪些成员函数被调用
 const IntArray b=a;
cout << "b[0] = " << b[0] <<endl;
//任务2：print函数去除 const会如何？
 b.print();
//任务3:以下语句错误性质一样吗？
 //b[5]=10;
//a[5]=10;
return 0;
}
```

## 函数对象

### 函数调用

函数对象：函数调用 运算符为任何对象提供函数语义。
当一个类 T 定义了函数调用，则 T 类型的对象 a 就可以类似函数一样使用 a(…)。 T 也称为函数(callable)类。
函数对象**既有对象持有状态的特征，又有函数可调用特征。**

### 无状态

当定义的 operator ()函数 没有使用对象的成员时，函数对象调用函数，其表现如同普通函数调用

```cpp
class cmp {
public:
bool operator () (const int& a, const int& b)
{
return a<b;
}
};
int main() {
cmp f;
cout<<f(1,2)<<endl;
cout<<f(2,1)<<endl;
return 0;
}
```

### 有状态

当定义的 operator ()函数使用对象的成员时，函数对象调用函数，其形式如同普通函数调用一般，但其语义变为依赖对象本身内部状态。**同样的函数调用，结果却不同。**

```cpp
class GreaterThan {
int baseline;
public:
GreaterThan(int x):baseline(x) { }
bool operator () (const int& x) {
return x>baseline;
}
};
int main() {
GreaterThan g1(10), g2(20);
cout << g1(15) << endl;
cout << g2(15) << endl;
return 0;
}
```

## 类外定义运算符重载

例：

```cpp
struct Integer {
int x;
public:
Integer(int x=0):x(x) {}
Integer operator- (const Integer& Int) {
return Integer(x-Int.x);
}
Integer operator - () {
return Integer(-x);
}
void print() {
cout<<x<<endl;
}
};
Integer operator+(const Integer& lhs,const Integer&
rhs) {
return Integer(lhs.x+rhs.x);
}
int main() {
Integer a=3,b,c;
//任务1：解释两个赋值语句，哪些函数被调用
 b = a + 4;
c = 5 + a;
b.print();
c.print();
//任务2：将 struct 关键字改为 class
return 0;
}
```

## 友元与友元函数

### friend 关键字

顾名思义，友元函数就是给“朋友”看的函数，类内部声明 friend 普通函数，函数虽然不属于类，但却可以访问类的变量及函数（**包括私有的**）。**函数需要在类外实现**

```
友元函数（friend function）。
• friend return_type function_name(parameter_type_list);
```

### 其他类成员函数作为友元

类的成员函数也是函数的一种，所以其他类的成员函数也可以是友元函数！

一个类 A 可以将另一个类 B 声明为自己的友元，那么类 B 的所有成员函数就都可以访问类 A 对象的私有成员
`• friend class B; （在类 A 的内部）`

## 操作符重载必须是成员函数的

```cpp
a=b
a(b...)
a[b]
a->
```

## 重载匹配与隐式转换

### 对象隐式转换

**如果对象 T 存在构造函数 T(T1)，则 T1 类型对象（实参）可隐式转为 T 类型对象（形参）**

```cpp
class Integer {
int x;
public:
Integer(int x=0):x(x) {}
friend Integer operator+(const Integer &lhs,const Integer &rhs){
return lhs.x + rhs.x;
}
friend ostream& operator<<(ostream& o, const Integer &hs) {
o << hs.x;
return o;
}
int main() {
string s;
s = "Hello";
cout << s << endl;
Integer i1(3), i2;
i2 = 1.1 + i1;
cout << i2 << endl;
}
};
```

### 隐式转换-重载协议-const

如果重载的函数参数一样，可以通过转换到某个重载函数，编译会如何哪个版本的选择？
（1）类型直接匹配的优先选择；
（2）const 类型实参匹配 const 版本
（3）非 const 实参优先匹配非 const 版本。
没有则隐式转换为 const 版本匹配

```cpp
/* 隐式转换-重载协议-const */
…
class Integer {
int x;
public:
Integer(int x=0):x(x) {}
friend ostream& operator<<(ostream& o, const Integer &hs) {
o << "const " << hs.x;
return o;
}
friend ostream& operator<<(ostream& o, Integer &hs) {
o << "no_const " << hs.x ;
return o;
}
};
int main() {
Integer i1(1);
const Integer i2(2);
cout << i1 << "," << 3 << "," << i2 << endl;
}
```

### explicit

C++ 关键字 explicit，用于关闭这种自动类型转化的特性。

即被 explicit 关键字修饰的类构造函数，不能进行自动地隐式类型转换，只能显式地进行类型转换

### 隐式转换-nullptr

C++ 引入了 nullptr 表示空值指针字面量。
