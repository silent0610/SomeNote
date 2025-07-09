---
Type: 
aliases:
  - std::forward
tags: 
modifiedDate: 2025/06/17, 13:14:53
---

# C++完美转发

## 定义

**在函数模板中，把接收到的参数，以“原封不动的方式”转发给另一个函数**，不改变其左值/右值属性、**底层const**性等。

## 实现

- 使用 `std::forward`
- 和**万能引用**
关键在于 `std::forward` 的作用。`std::forward` 是一个**条件转换**工具，它的行为完全取决于你提供给它的模板参数 `T`。

- 当 `T` 是一个左值引用类型（如 `int&`）时，它返回一个左值。
- 当 `T` 是一个非引用类型或右值引用类型（如 `int` 或 `int&&`）时，它返回一个右值。

>  表达式 `std::forward<int>(a)` 的结果是一个 **`int` 类型的右值 (rvalue)**。

### 原理

该函数原理为先用remove_reference函数去掉引用属性，再根据接收参数的类型选择转发左值的重载或转发右值的重载，在对应的函数内实行强制类型转换。

```cpp
//转发左值
template<typename _Tp>
constexpr _Tp&&
forward(typename std::remove_reference<_Tp>::type& __t) noexcept
{ 
    return static_cast<_Tp&&>(__t); 
}

//转发右值
template<typename _Tp>
constexpr _Tp&&
forward(typename std::remove_reference<_Tp>::type&& __t) noexcept
{
    static_assert(!std::is_lvalue_reference<_Tp>::value, "bad forward call");
    return static_cast<_Tp&&>(__t);
}
```
