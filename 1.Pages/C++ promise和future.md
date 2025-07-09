---
Type: 
aliases: 
tags: 
modifiedDate: 2025/06/12, 20:32:17
---

# C++ Promise和future

## 定义

- **`std::promise`**: `std::promise` 对象可以看作是一个 **承诺**，它 **存储** 一个未来某个时刻才会产生的值。你可以把它想象成一个写入端，负责在将来的某个时间点设置一个值或一个异常。
    
- **`std::future`**: `std::future` 对象提供了一种 **访问** 异步操作结果的机制。你可以把它看作是一个读取端，它与一个 `std::promise` 相关联，并最终会获得 `promise` 所设定的值或异常。它允许你查询异步操作是否完成，并等待和获取其结果。

简单来说，`promise` 用来 **设置** 结果，`future` 用来 **获取** 结果。它们是 C++ 并发编程中用于同步和在线程间传递数据的重要工具。

## 例子

- 在 `future` 调用 `get()` 时才会阻塞, 期间可以执行其它任务

```cpp
#include <iostream>
#include <future>
#include <thread>
#include <chrono>

// 一个模拟耗时计算的函数
void calculate_value(std::promise<int> prom) {
    std::cout << "工作线程开始计算..." << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(2)); // 模拟耗时操作
    int result = 42;
    std::cout << "工作线程计算完成，设置结果。" << std::endl;
    prom.set_value(result); // 设置结果值
}

int main() {
    // 1. 创建一个 promise 对象
    std::promise<int> my_promise;

    // 2. 从 promise 获取 future 对象
    std::future<int> my_future = my_promise.get_future();

    // 3. 创建新线程，并将 promise 的所有权转移给它
    std::thread worker_thread(calculate_value, std::move(my_promise));

    std::cout << "主线程正在执行其他任务..." << std::endl;

    // 5. 在需要的时候，等待并获取 future 的结果
    std::cout << "主线程等待结果..." << std::endl;
    int result = my_future.get(); // get() 会阻塞直到 promise 设置了值

    std::cout << "主线程获取到的结果是: " << result << std::endl;

    // 等待工作线程结束
    worker_thread.join();

    return 0;
}
```
