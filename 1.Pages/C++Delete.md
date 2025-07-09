---
Type:
aliases: 
tags: 
modifiedDate: 2025/06/17, 20:13:37
---

# C++Delete

## Delete

### 实现

- 调用对象的析构函数 `obj->~MyClass();`
- 调用 `operator delete` 释放内存（默认用 `free()`）
