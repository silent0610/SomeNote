---
Type: 
aliases: 
tags: 
modifiedDate: 2025/06/14, 15:18:25
---

# C++五法则

[[C++11]] 以后,因为引入了移动, 才出现

## 定义

如果你定义了“三法则”中的任何一个，那么你也应该考虑是否需要定义另外两个与移动相关的函数，从而凑齐五个。
- 析构函数
- 拷贝构造函数
- 拷贝赋值运算符
- **移动构造函数 (Move Constructor)**: `MyClass(MyClass&& other) noexcept`
- **移动赋值运算符 (Move Assignment Operator)**: `MyClass& operator=(MyClass&& other) noexcept`
