---
Type:
aliases: 
tags: 
modifiedDate: 2025/06/12, 16:55:39
---

# Lambda 捕获初始化器

这允许创建使用任意表达式初始化的 lambda 捕获。捕获值的名称不需要与任何外部作用域中的变量相关，并且会在 lambda 体内部引入一个新的名称[^1]。初始化表达式在 lambda **创建时**[^2] 而不是 **调用时** 进行求值。

## 例子

### 初始化

```c++
int factory(int i) { return i * 10; }
auto f = [x = factory(2)] { return x; }; // 返回 20

auto generator = [x = 0] () mutable {
  // 如果没有 `mutable`，则无法编译，因为我们在每次调用时修改了 x
  return x++;
};
auto a = generator(); // == 0
auto b = generator(); // == 1
auto c = generator(); // == 2
```

### 移动初始化

可以将值 **移动**（或 **转发**）到 lambda 中，而不仅仅是通过拷贝或引用捕获，因此我们可以捕获那些只能通过值捕获的移动独占类型。注意在以下示例中，`task2` 的捕获列表中左侧的 `p` 是 lambda 体内部的一个新变量，而不是原始的 `p`。

```c++
auto p = std::make_unique<int>(1);

auto task1 = [=] { *p = 5; }; // 错误：std::unique_ptr 不能被拷贝
// vs.
auto task2 = [p = std::move(p)] { *p = 5; }; // OK：p 被移动构造到闭包对象中
// task2 创建后，原始的 p 已为空
```

### 不同的名字

`y` 这里相当于类的一个字段

```cpp
int x = 1;
auto f = [&r = x, y = x * 10]()mutable
    {
        ++r;
        std::cout << r << std::endl;
        ++y;
        return r + y;
    };
```

[^1]: 类的字段
[^2]: 即类的构造函数, 构造函数只执行一次
