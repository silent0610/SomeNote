---
Type:
aliases: 
tags: 
modifiedDate: 2025/06/13, 16:29:33
---

# C++多继承对象内存布局

```cpp
class Base1 {
public:
    virtual void func1() { /*...*/ }
    virtual ~Base1() {}
    int b1_data;
};

class Base2 {
public:
    virtual void func2() { /*...*/ }
    virtual ~Base2() {}
    int b2_data;
};

class Derived : public Base1, public Base2 {
public:
    // 重写基类的虚函数
    void func1() override { /*...*/ }
    // 新增自己的虚函数
    virtual void func3() { /*...*/ }
    int d_data;
};
```

当创建一个 `Derived` 类的对象时，它的内存布局大致如下（在64位系统上）：
==经测试, 是正确的==, d_data 的偏移是 `32`
[[C++内存对齐]]

```
// Derived 对象的内存布局示意图

+---------------------------+  <-- Derived 对象的起始地址
|                           |
| --- Base1 subobject ---   |
|   vptr_1 (指向v-table_D_B1)|  (8 字节)
|   b1_data (int)           |  (4 字节)
|   (padding)               |  (4 字节，用于对齐)
|                           |
+---------------------------+  <-- Base2 subobject 的起始地址
|                           |
| --- Base2 subobject ---   |
|   vptr_2 (指向v-table_D_B2)|  (8 字节)
|   b2_data (int)           |  (4 字节)
|   (padding)               |  (4 字节，用于对齐)
|                           |
+---------------------------+
|                           |
| --- Derived members ---   |
|   d_data (int)            |  (4 字节)
|   (padding)               |  (4 字节)
|                           |
+---------------------------+
```

## 2. V-ptr 指向哪里？—— 多个虚函数表

这两个 v-ptr 指向的**不是**同一个虚函数表。编译器会为 `Derived` 类生成多个虚函数表（或者一个主表和多个副表），以对应不同的继承路径。

1. 主虚函数表 (Primary v-table): `vptr_1` 指向的这张表，是为 `Base1` 这条继承路径准备的。它包含了：
    - `Derived` 重写的 `Base1` 的虚函数（如 `&Derived::func1`）。
    - `Base1` 未被重写的虚函数（本例中没有）。
    - `Derived` 自己**新增的虚函数**（如 `&Derived::func3`）。
    - 指向 `type_info` 的指针等。
2. 次虚函数表 (Secondary v-table): `vptr_2` 指向的这张表，是为 `Base2` 这条继承路径准备的。它包含了：
    
    - `Derived` 重写的 `Base2` 的虚函数（本例中没有，所以会指向 `Base2::func2`）。
    - **这部分通常不包含派生类新增的虚函数**。
    - 它内部的函数指针可能不是直接指向函数，而是指向一小段叫**“Thunk”**的代码，这段代码的作用是调整 `this` 指针，因为 `Base2` 子对象在 `Derived` 对象内存中有一个偏移量。
    - 
