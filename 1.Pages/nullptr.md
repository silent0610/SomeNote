---
Type:
aliases: 
tags: 
modifiedDate: 2025/06/12, 14:29:03
---

# Nullptr

## 特点

- **类型安全**：`nullptr` 的类型是 `std::nullptr_t`。它**不是整数类型**。
- **明确的转换规则**：`std::nullptr_t` 可以被**隐式地转换成任意类型的指针**（如 `int*`, `char*`, `MyClass*` 等），但**不能被隐式地转换成整数类型**。

## 原因: **二义性**

在 C++11 之前，开发者通常使用 `NULL` 或者 `0` 来表示空指针。但这两种方式都存在一个非常严重的问题：**类型不明确，存在二义性**。

`NULL` 在 C++ 的头文件中，通常被定义为一个宏：

`#define NULL 0`

所以 `NULL` 和 `0` 本质上都是一个**整数**

在**函数重载**（Function Overloading）时会出现意想不到的错误。

```cpp
void processPointer(int* ptr) {
    std::cout << "Processing a pointer...\n";
}

void processInteger(int num) {
    std::cout << "Processing an integer...\n";
}
```

因为 `NULL` 就是 `0`，编译器看到了 `processPointer(0)`。对于 `0` 这个字面量，它既可以被看作是整数 `0`，也可以被看作是空指针。但是在函数重载决议中，**`int` 类型是比 `int*` 类型更完美的匹配**。
