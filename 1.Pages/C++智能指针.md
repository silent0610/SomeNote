---
Type:
  - Page
aliases: 
tags: 
modifiedDate: 星期六, 五月 31日 2025, 1:26:08 下午
---

## 定义

C++ 中用于管理动态分配内存（在堆上创建的对象）的类模板。
- 封装了原始指针
- 并在适当的时候自动释放所指向的内存
- 极大地帮助我们避免内存泄漏和悬挂指针等问题
- ==是 [RAII](RAII.md) 的重要实践==

由 C++11 引入

## 种类

[shared\_ptr](shared_ptr.md)
[unique\_ptr](unique_ptr.md)
[weak\_ptr](weak_ptr.md)
