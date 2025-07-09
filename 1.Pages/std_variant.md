---
Type: 
aliases:
  - std::variant
tags: 
modifiedDate: 2025/06/12, 18:57:38
---

# std::variant

| 对比项           | `std::variant`     | `union`（传统联合体）          |
| ------------- | ------------------ | ----------------------- |
| **标准引入版本**    | C++17              | C 语言时代就有                |
| **类型安全**      | ✔ 类型安全（记录当前类型）     | ❌ 不记录当前存储类型             |
| **支持非平凡类型**   | ✔（如 `std::string`） | ❌ 只能放 POD 类型（C++11 后扩展） |
| **构造 / 析构管理** | 自动调用构造、析构函数        | 手动管理或需 placement new    |
| **可读性与易用性**   | 高，可配合 `std::visit` | 低，容易误用                  |

```cpp
#include <variant>
#include <iostream>

std::variant<int, float, std::string> v;

v = 10;
v = 3.14f;
v = "hello";

std::visit([](auto&& val){
    std::cout << "当前值是: " << val << "\n";
}, v);

```
