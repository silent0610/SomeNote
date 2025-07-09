---
Type: 
aliases: 
tags: 
modifiedDate: 2025/06/13, 16:32:31
---

# C++三法则

如果你为一个类显式地定义了以下三个特殊成员函数中的任何一个，那么你很可能需要将这三个都定义出来。
- **析构函数 (Destructor)**: `~MyClass()`
- **拷贝构造函数 (Copy Constructor)**: `MyClass(const MyClass& other)`
- **拷贝赋值运算符 (Copy Assignment Operator)**: `MyClass& operator=(const MyClass& other)`
