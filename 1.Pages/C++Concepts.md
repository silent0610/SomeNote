---
Type:
aliases: 
tags: 
modifiedDate: 2025/06/12, 20:08:37
---

# C++Concepts

## 定义

- [[C++20新特性]]引入
- 概念的本质是一个**编译期的谓词（predicate）**。它是一个布尔值，用于**在开始实例化时**判断一个或多个模板参数是否满足一组预定义的约束。如果满足，谓词为 `true`，编译继续；如果不满足，谓词为 `false`，编译器立即给出一个清晰的错误报告。

## 原因

- 模板的编译错误信息基本不可读
- 编译器会“尽其所能”地去完成模板实例化，直到最深层次才发现 `p1 + p2` 这行代码无法编译, 最后返回不可读的错误信息
- 即: 我们无法在**模板的接口层面**告诉编译器和使用者：“我需要的类型 `T` 必须满足某些条件（比如必须可以相加）
- **过去**这种检查只能延迟到模板实例化时进行，导致了错误信息的滞后和模糊。
- 为了在**语言层面上**解决这个问题，让模板的约束变得清晰、可检查，从而提供人类可读的错误信息。

## 使用

### 定义

```cpp
#include <type_traits> // 用于 is_integral

// 定义一个名为 "Integral" 的概念
// 它要求类型 T 必须是一个整数类型
template <typename T>
concept Integral = std::is_integral_v<T>;

// 定义一个更复杂的概念 "Addable"
// 它要求类型 T 的对象 a 和 b，表达式 a + b 必须是合法的，
// 并且其结果必须能被转换成 std::ostream 可以输出的类型。
template <typename T>
concept Addable = requires(T a, T b) {
    // 这种 requires 块被称为 requires-expression
    { a + b } -> std::convertible_to<std::ostream&>; // 检查表达式语法和返回类型约束
};
```

有多种语法可以将概念应用到模板上，以约束模板参数。

### 使用

**【使用概念重写 `addAndPrint`】**

方法一：requires 子句 (Requires Clause)

这是最通用的方式。

C++

```
template <typename T>
    requires Addable<T> // 在模板参数列表后添加 requires 子句
void addAndPrint(T a, T b) {
    auto result = a + b;
    std::cout << result << std::endl;
}
```

方法二：约束模板参数 (Constrained Template Parameter)

这是最简洁、最推荐的方式。

C++

```
template <Addable T> //直接用概念名代替 typename/class
void addAndPrint(T a, T b) {
    // ... 函数体相同 ...
}
```

方法三：auto 占位符 (Abbreviated Function Template)

对于函数模板，如果参数类型直接使用了概念，可以用 auto 进一步简化。

C++

```
void addAndPrint(Addable auto a, Addable auto b) { // 用 "概念 auto" 作为参数类型
    // ... 函数体相同 ...
}
```

## 作用

- **极大地改善了编译错误信息**: 这是最直接、最重要的优势。错误信息从“天书”变成了人类可读的诊断报告，直接指明了类型不满足哪个约束条件。
- **更清晰的接口文档**: 概念本身就是一种文档。当使用者看到 `template <Invocable F>`，立刻就能明白模板参数 `F` 必须是一个可调用对象（如函数指针、Lambda 等），而无需去阅读冗长的文档或猜测。
- **更强的类型检查**: 约束在编译的最早阶段（重载决议阶段）就被检查，而不是等到模板实例化深处，这使得编译过程更稳健、更快速。
- **改进函数重载**: 我们可以基于概念来进行函数重载。编译器会根据传入的类型最匹配哪个概念，来选择最优的函数版本。
