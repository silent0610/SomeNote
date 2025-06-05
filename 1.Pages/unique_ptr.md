---
Type:
  - Page
aliases:
  - std::unique_ptr
tags: 
modifiedDate: 星期六, 五月 31日 2025, 1:26:40 下午
---

## 定义

- 提供独占所有权，确保同一时间只有一个 `unique_ptr` 可以指向某个对象。

## 实现

- 保存一个裸指针成员
- **析构函数**：在其析构函数中，对内部存储的裸指针调用 `delete`（或自定义删除器）。
- **移动构造/赋值**：允许所有权从一个 `unique_ptr` 转移到另一个，原 `unique_ptr` 变为 `nullptr`。
- **禁用拷贝**：拷贝构造函数和拷贝赋值运算符被显式地标记为 `delete`，以防止多个 `unique_ptr` 共享同一资源。

## 使用注意

仍然可以让两个 unique_ptr 保有同一个指针, 运行时不会报错, 但是在析构时会报错

```cpp
int main()
{
    int *ptr = new int[10];
    for (int i = 0; i < 10; ++i)
    {
        ptr[i] = i;
    }
    std::unique_ptr<int> uptr{ptr};
    std::unique_ptr<int> uptr1{ptr};
    std::cout << *(uptr1.get() + 5);
    std::cout << *(uptr.get() + 5);
}
```

如这段代码, uptr 和 uptr1 都是可用的, 但是在 析构时, 由于它们对 ptr 重复执行了 delete 就会报错
