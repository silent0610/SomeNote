---
Type: 
aliases:
  - std::move
tags: 
modifiedDate: 2025/06/17, 11:20:20
---

# std::move

**`std::move` 本身不执行任何“移动”操作，它也不移动任何东西。**

它的唯一作用是：**无条件地将一个表达式强制转换（cast）为“右值引用（rvalue reference）”类型**。

进行这种转换的唯一目的，是为了**触发并利用 C++的“移动语义（Move Semantics）”**，从而在特定场景下大幅提升性能。

当一个对象被转换为右值后，如果它被用于初始化或赋值给另一个同类型的对象，编译器会优先选择调用一个特殊的函数：

- **移动构造函数 (Move Constructor)**
- **移动赋值运算符 (Move Assignment Operator)**

而这两个函数所做的事情，才是真正的“移动”。

## 实现

```cpp
constexpr remove_reference_t<_Ty>&& move(_Ty&& _Arg) noexcept 
{
    return static_cast<remove_reference_t<_Ty>&&>(_Arg);
}
```

1. 通过万能引用,引用折叠，模板类型T可以与任意类型的实参相匹配，即T可以是左值，也可以是右值。
2. 通过remove_refrence，移除引用，得到具体的类型
3. 使用static_cast，将其转换为右值引用T&&。
