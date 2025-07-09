---
Type: 
aliases:
  - Standard Template Library
  - 标准模板库
tags: 
modifiedDate: 2025/06/16, 15:04:28
---

# C++ STL

## 定义

STL，全称 **Standard Template Library（标准模板库）**，是 C++ 标准库的核心组成部分，它封装了大量常用的数据结构和算法。它不是指某一个具体的库文件，而是一整套**通用的、可复用的**组件，其代码都位于 C++ 的 `std::` 命名空间下。
STL 设计核心是**泛型编程**，通过模板实现高效且通用的数据结构和算法。
- 包括：
- 容器，算法，迭代器，仿函数，配接器(Adapters)和配置器，彼此可以组合套⽤。容器通过配置器取得数据存储空间，算法通过迭代器存取容器内容，仿函数可以协助算法完成不同的策略变化，配接器可以应⽤于容器、 仿函数和迭代器。
1. 容器：各种数据结构，例如vector，list，deque，set，map等。容器用于存储数据，容器属于类模板。
2. 算法：各种算法，例如sort，search。算法属于函数模板。
3. 迭代器：用于访问容器元素的统一接口, 提供了对容器对象的间接访问。
4. 函数对象(仿函数)[^1]：一种重载了operator()的类或者类模板，可以帮助算法实现不同的策略。
5. 适配器(Adapters)：一种用于修饰容器、仿函数、迭代器接口的东西。包装一个已有的组件，并提供一个不同的、通常是更受限或行为更特殊的接口。[^2]
6. 分配器(Allocater)：负责内存空间的管理和配置。

## 常见容器, 常见操作, 复杂度

- [[C++Vector|std::vector]]
- [[C++Map|std::map]], [[C++Set|std::set]], [[C++Multimap|std::multimap]], [[C++Multiset|std::multiset]]
- [[C++Unordered_Map|std::unordered_map]], [[C++Unordered_set|std::unordered_set]], [[C++Unordered_multimap|std::unordered_multimap]]，[[C++Unordered_multiset|std::unordered_multiset]]
    - 使用[[哈希表]]实现
    - 对于特殊类型,需要制定哈希函数
    - 插入、访问、删除操作都是O(1)，最坏情况下为O(n)（多个键值对存储在同一个桶里）
- [[C++List|std::list]]
- [[C++Deque|std::deque]]
- [[C++Stack|std::stack]]，[[C++priority_queue|std::priority_queue]]，[[C++Queue|std::queue]]

[^1]: 函数对象相比于普通函数指针的优势在于，它可以拥有自己的状态（成员变量）
[^2]: 如 `std::stack` (栈)：默认包装 `std::deque`,提供 `push`, `pop`, `top`
