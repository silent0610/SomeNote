---
Type:
aliases: 
tags: 
modifiedDate: 2025/06/12, 18:27:18
---

# C++ Auto和decltype

## Auto

对于复杂类型特别有用，可以提高可读性：

### 函数返回值推导

==[[C++14新特性]]引入==
- 函数也可以使用 `auto` 推导返回类型[^1]。
- **在 C++11 中，必须显式指定返回类型**，或者使用 `decltype`
//尾置返回
==需要注意所有 return 返回的类型必须相同==

```cpp
template <typename X, typename Y>
auto add(X x, Y y) -> decltype(x + y) { //尾置返回值
  return x + y;
}
```

## Decltype

可以返回 *表达式类型*
如果表达式包含 cv[^1] 限定符和引用，则会保留这些属性。`decltype` 的示例：

```cpp
int a = 1; // `a` 的声明类型为 `int`
decltype(a) b = a; // `decltype(a)` 是 `int`
const int& c = a; // `c` 的声明类型为 `const int&`
decltype(c) d = a; // `decltype(c)` 是 `const int&`
decltype(123) e = 123; // `decltype(123)` 是 `int`
int&& f = 1; // `f` 的声明类型为 `int&&`
decltype(f) g = 1; // `decltype(f)` 是 `int&&`
decltype((a)) h = g; // `decltype((a))` 是 int&
```

## Decltype (auto)

   - `decltype(auto)` 类型说明符也像 `auto` 一样推导类型。然而，它在推导返回类型时会保留引用和 cv 限定符，而 `auto` 则不会。

[^1]: const 和 volatile
