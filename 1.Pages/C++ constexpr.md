---
Type: 
aliases:
  - const expression
  - 常量表达式
tags: 
modifiedDate: 2025/06/18, 19:16:18
---

# C++ Constexpr

## 核心

将一些计算从**运行时（runtime）**提前到**编译时（compile-time）**，这带来了巨大的好处，包括更高的性能、更强的类型安全和更多的编译时编程能力。

## 和 Const 的区别

const 变量在初始化之前是未知的, 其值可以在运行时确定.
但是 constexpr **保证其值在编译时就必须是已知的**。它必须由一个真正的“常量表达式”来初始化。**在编译时可知**

## 作用

- **性能提升**：如果一个复杂的计算可以在编译时完成，那么在程序运行时，它就直接变成了一个预先算好的常量。这意味着**零运行时开销**。==将计算移到编译时==
- **定义真正的常量**：你可以用它来定义数组的大小、作为模板的非类型参数、用于 `enum` 的值、用于 `static_assert` 等，这些都要求值在编译时是确定的。

`constexpr` 还可以修饰类的构造函数，这意味着你可以在**编译时创建并初始化一个对象**

```cpp
class Point 
{ 
public: // C++11 constexpr 构造函数 
    constexpr Point(double x_val, double y_val) : x(x_val), y(y_val) {} 
    constexpr double get_x() const { return x; } 
    constexpr double get_y() const { return y; } 
private: 
    double x, y; 
};
// 在编译时创建一个 Point 对象 
constexpr Point p(9.4, 7.9); 
// 在编译时使用它的成员函数 
constexpr double p_x = p.get_x();
```

### 分支优化

可以在编译期确定哪条 if 分支, 另一条分支不被编译 [[C++17新特性]]

### Constexpr 函数

函数体不能包含汇编语句、goto语句、label、try块、静态变量、线程局部存储、没有初始化的普通变量，不能动态分配内存，不能有new delete等，不能虚函数。

#### Constexpr Lambda表达式

==[[C++17新特性]]==

## 限制

### c++11

- 函数体内部**只能有一句 `return` 语句**。
- 不能声明变量，不能有循环，不能有 `if/else`（但可以用三元运算符 `?:`）。

### c++14

相比于 11

- 函数体内可以包含多条 `return` 语句。
- 可以声明局部变量。
- 可以使用 `if/else`, `switch`, `for`, `while` 等控制流语句。
