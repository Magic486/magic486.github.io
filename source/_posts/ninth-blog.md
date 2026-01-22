---
title: 继承和派生
tags: C++
date: 2025-03-20
top_img: transparent
comments: false
---

# 继承/派生的概念

当定义一个新的类 B 时，如果发现类 B 拥有某个已写好的类 A 的全部特点，此外还有类 A 没有的特点，那么就不必从头重写类 B，而是可以把类 A 作为一个“基类”（也称“父类”），把类 B 写为基类 A 的一个“派生类”（也称“子类”）。这样，就可以说从类 A “派生”出了类 B，也可以说类 B “继承”了类 A。

派生类是通过对基类进行扩充和修改得到的。基类的所有成员自动成为派生类的成员。

# 语法与访问控制

```cpp
class 派生类名：继承访问控制 基类类名{
成员访问控制：
 成员声明列表；
 ｝；
```

继承访问控制和成员访问控制均由保留 public、protected、private 来定义，缺省均为 private。
`private（私有的）`
`public (公有的）`
`protected（受保护的）`

| 基类中成员的访问控制 | 继承访问控制 | 派生类中继承成员的访问控制 |
| -------------------- | ------------ | -------------------------- |
| public               | public       | public                     |
| protected            |              | protected                  |
| private              |              | 不可访问                   |
| public               | protected    | protected                  |
| protected            |              | protected                  |
| private              |              | 不可访问                   |
| public               | private      | private                    |
| protected            |              | private                    |
| private              |              | 不可访问                   |

# 构造与析构顺序

# 派生与构造函数

# 派生与成员函数

# 改变访问控制

# 类型兼容性

# 类的类型转换

# 多重继承
