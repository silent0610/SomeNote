---
Type: 
aliases:
  - std::function
tags: 
modifiedDate: 2025/06/17, 13:46:00
---

# std_function

c++11新增了std::function、std::bind、lambda表达式等封装使函数调用更加方便。
- 可调用对象
    - **普通函数**
    - **函数指针**
    - [[Lambda 表达式]]
    - **函数对象（Functor）**，即重载了 `operator()` 的类的对象
    - 使用 `std::bind` 包装后的结果
    - 类的成员函数指针（通常需要和 `std::bind` 或 lambda 配合使用）

## 定义

`std::function` 是一个**通用的、多态的函数包装器**

## 作用

`std::function` 解决了什么问题？

在 C++11 之前，如果你想存储一个“可以被调用”的东西，会非常麻烦。

- 想存一个普通函数？你需要一个**函数指针**。
- 想存一个函数对象？你需要一个**该类的对象**。
- 想存一个有捕获的 Lambda？它有自己独特的、匿名的类型，你甚至无法写出它的类型名。

这意味着，如果你想创建一个可以存储**不同类型**回调函数的容器（比如 `std::vector`），几乎是不可能完成的任务。

**函数签名**指的是函数的**返回值类型**和**参数类型列表**。例如 `int(std::string, double)` 就是一个函数签名。

### 例子

声明一个 std:: function 对象，它可以包装任何“接受两个int，返回一个int”的可调用对象 
`std::function<int(int, int)> calculator;`
