---
Type:
aliases: 
tags: 
modifiedDate: 2025/06/12, 21:08:33
---

# C++Ranges

## 定义

-  [[C++20新特性]]引入
- 提供了一套用于处理元素序列（如容器、数组或任何可迭代序列）的现代化、高度可组合的组件和算法。其核心思想是将算法与数据解耦，并允许以一种声明式、函数式的风格对数据序列进行惰性求值和链式操作。

## 原因

### **为什么要引入 Ranges？(解决了什么痛点)**

在 C++20 之前，使用STL算法通常是这样的：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

    // 需求：筛选出所有偶数，将它们乘以2，然后打印出来
    std::vector<int> even_numbers;
    std::copy_if(numbers.begin(), numbers.end(), std::back_inserter(even_numbers),
                 [](int n) { return n % 2 == 0; });

    std::vector<int> doubled_numbers;
    std::transform(even_numbers.begin(), even_numbers.end(), std::back_inserter(doubled_numbers),
                   [](int n) { return n * 2; });

    for (int n : doubled_numbers) {
        std::cout << n << " ";
    }
    std::cout << std::endl;
}
```

- **冗长啰嗦**：需要重复书写 `.begin()` 和 `.end()`。
- **创建临时容器**：每一步操作（筛选、转换）都需要一个中间容器（如 `even_numbers`）来存储结果，这会带来不必要的内存分配和拷贝开销。
- **可读性差**：算法是独立的函数调用，很难一眼看出整个数据处理的流水线。你需要从内到外地去阅读和理解。
- **容易出错**：`begin` 和 `end` 迭代器必须配对正确，否则可能导致未定义行为。

## 组件

- **Range (范围)**：任何可以迭代的序列都是一个 Range。这可以是 `std::vector`、`std::list`，C风格数组，甚至是你自己定义的、只要支持 `begin()` 和 `end()` 的类型。
- **View (视图)**：这是 Ranges 的精髓所在。View 是对一个 Range 的**轻量级**引用或转换，它**不拥有**底层的数据。
    - **惰性求值,** 只有当你真正开始迭代并请求元素时，这些操作才会“按需”应用在每个元素上。
    - 可组合, 通过 `|` 符号连接在一起, 类似 shell 的管道流水线, 数据会依次经过每个阶段
- **Range Adaptor (范围适配器)**：这是一个可以接受一个 Range 并返回一个 View 的对象。例如，`std::views::filter` 是一个适配器，它接受一个 Range 并返回一个只包含满足特定条件元素的 View。`std::views::transform` 则返回一个对每个元素应用了某个操作的 View。

## 例子

```cpp
#include <iostream>
#include <vector>
#include <ranges> // 引入 ranges 头文件

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

    // 需求：筛选出所有偶数，将它们乘以2，然后打印出来

    // 使用管道符号 | 将适配器组合起来
    auto even_doubled = numbers
                      | std::views::filter([](int n) { return n % 2 == 0; })
                      | std::views::transform([](int n) { return n * 2; });

    // 此时，没有任何计算发生！even_doubled 只是一个描述了操作流程的轻量级视图。

    // 当我们开始迭代时，计算才会按需进行
    for (int n : even_doubled) {
        std::cout << n << " "; // 依次输出: 4 8 12 16 20
    }
    std::cout << std::endl;
}
```

**当我们开始迭代时，计算才会按需进行**
1. numbers 作为 range (数据源)
2. 所有 number, 经过一次 filter, 找到偶数
3. 只有是偶数的 number 进入第三阶段被 transform
