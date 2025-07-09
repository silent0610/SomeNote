---
Type: 
aliases:
  - atomic
tags: 
modifiedDate: 2025/06/18, 16:48:59
---

# C++原子操作

## 定义

`std::atomic` 是 [[C++11]] 提供的一套工具，用于在**多线程环境**下进行**无锁的原子操作**[^1]，以实现高效、安全的数据同步。它的主要作用就是**在不使用重量级互斥锁（`std::mutex`）的情况下，解决数据竞争问题**。

## 实现

`std::atomic` 通常利用底层CPU提供的特殊**原子指令**（如 `lock cmpxchg`）来实现，这比操作系统层面的互斥锁要**轻量和高效得多**。

## 例子

```cpp
#include <atomic>
#include <thread>
#include <vector>
#include <iostream>

// 使用 std::atomic 包装计数器，使其线程安全
std::atomic<int> atomic_counter(0);

// 普通的 int 类型，非线程安全
int regular_counter = 0;

void increment_counters() {
    for (int i = 0; i < 10000; ++i) {
        atomic_counter++;  // 这是一个单独的、不可分割的原子操作
        regular_counter++; // 这不是原子操作，会产生数据竞争
    }
}

int main() {
    std::vector<std::thread> threads;
    for (int i = 0; i < 10; ++i) {
        threads.push_back(std::thread(increment_counters));
    }

    for (auto& th : threads) {
        th.join();
    }

    std::cout << "原子计数器的最终值: " << atomic_counter << std::endl;
    std::cout << "普通计数器的最终值: " << regular_counter << std::endl;
}
```

**可能的输出：**

```
原子计数器的最终值: 100000
普通计数器的最终值: 54823 // 或其他某个不正确的、看起来随机的数字
```

[^1]: 指这个操作是**不可分割、不可中断**的。当一个线程在对原子变量进行操作时，其他线程无法看到它操作到一半的状态。
