---
Type:
  - Page
aliases:
  - vtable
tags: 
modifiedDate: 2025/06/13, 14:20:09
---

# C++虚表

## 定义

是实现**多态性 (Polymorphism)** 的核心机制，尤其是通过**虚函数 (virtual functions)** 实现运行时多态。

### 简单描述

   - **每个对象**都有自己的虚表
    - 如果子类没有覆写 (override) 父类的虚函数, 则虚表对应项存储父类虚函数的地址
    - 如果子类覆写了父类的虚函数, 则虚表存储子类虚函数的地址
    - 由此实现多态, 虚函数自动选择

## 存储位置

虚表通常放在程序的静态数据区的, [只读数据段](C++内存区域.md#静态数据区/全局数据区), 这个区域还存了常量, 比如字符串常量

### 为什么存在这里

- 只读数据段, 这个内存区域存储的是程序在运行时不会被修改的数据，例如字符串字面量、常量、以及像虚表这样的结构。
- 而虚表的结构在程序编译时就已经确定了。它是一个**静态的、只读的数组**，存储着类中每个虚函数的地址。程序运行时，这个虚表的内容是不应该被修改的。
    - 静态性
    - 只读性: 
    - 安全性: 防止意外修改
    - 效率: **每个类的虚表是共享的**[^1]

## 实现

![[assets/c000ed6b9781f8fb2992d3f9b9111f01.png]]
-  [[虚表存储大小]]
    - 一个派生类只有一个 vptr, 多继承会有基类数量个 vptr
[虚表的实现](虚表的实现.md)

[^1]: 虚表是静态的, 每个类的不同对象不会有不同的虚函数, 所以所有类的实例都会共享同一个虚表, 无需为每个对象单独存储虚表, 节省了内存
