---
Type: 
aliases:
  - std::condition_variable
tags: 
modifiedDate: 2025/06/12, 15:55:39
---

# C++条件变量

## 定义

允许一个或多个线程等待某个特定**条件**的发生

## 使用

使用条件变量必须包含三个要素：
- **一个 `std::condition_variable` 实例**。
- **一个 `std::mutex`**：用于保护共享的条件状态。
- **一个共享的条件状态变量**（例如一个布尔值或一个队列的大小, 也可以用 lambda 函数封装）。

cv.wait (lock,...) 会原子地：
1. **解锁mutex** 
2. **让线程休眠**

当被唤醒时，它会：
1. **重新加锁mutex** 
2. **继续执行**

```cpp
#include <condition_variable>
#include <mutex>
#include <queue>
#include <thread>

std::mutex mtx_cv;
std::condition_variable cv;
std::queue<int> data_queue;
bool finished = false;

// 生产者线程
void producer() {
    for (int i = 0; i < 10; ++i) {
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
        {
            std::lock_guard<std::mutex> guard(mtx_cv);
            std::cout << "生产数据: " << i << std::endl;
            data_queue.push(i);
        } // 锁在这里释放
        
        // 通知一个等待的消费者线程
        cv.notify_one(); 
    }
    {
        std::lock_guard<std::mutex> guard(mtx_cv);
        finished = true;
    }
    cv.notify_all(); // 通知所有线程，生产结束
}

// 消费者线程
void consumer() {
    while (true) {
        std::unique_lock<std::mutex> lock(mtx_cv); // 使用 unique_lock

        // wait 会原子地：1. 解锁mutex 2. 让线程休眠
        // 当被唤醒时，它会：1. 重新加锁mutex 2. 继续执行
        // 第二个参数是一个 lambda，用于防止“虚假唤醒”（spurious wakeup）
        // 只有当队列不为空或生产结束时，才会停止等待
        cv.wait(lock, [] { return !data_queue.empty() || finished; });

        if (finished && data_queue.empty()) {
            break; // 结束条件
        }

        int data = data_queue.front();
        data_queue.pop();
        std::cout << "消费数据: " << data << std::endl;
    }
}
```
