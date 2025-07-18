---
Type:
aliases: 
tags: 
modifiedDate: 2025/06/17, 20:28:09
---

# C++new

## New

在使用new分配内存的时候，通常会比请求的内存多一点，用于存储头部信息。

头部信息记录了分配的内存块的大小，delete在释放内存的时候，会先找到这些头部信息，然后再正确的释放整个块。

### 实现

- 首先调用operator new（）,从**堆**上分配足够大的原始内存空间
    - Operator new() 底层使用 malloc 实现
- 在这块内存上调用相应构造函数
    - 使用**placement new** 语义，在那块内存上构造对象。
- 返回该类型的指针

### 异常安全

- operator new()内存不足时抛出异常
- 如果在分配内存后，构造函数抛出了异常，`new` 会自动释放内存：
    - 其实现机制依赖编译器生成的清理代码（类似 try/catch + delete）。

### 类型

#### Plain new：

1. 就是普通的、常用的new
2. 在分配失败时，抛出bad_alloc

#### Nothrow new：

在分配失败时，不会抛出异常，而是会返nullptr

#### Placement new：

1. 在一块已经成功分配的内存上重新构造对象。仅仅调用了对象的构造函数，因此不用担心抛出异常(**对象构造函数可能抛出异常**)
2. 用于在一块较大的预分配内存上，构造不同类型的对象。
3. 对于对象数组，必须显式调用析构函数 `delete []`。

[[C++Delete]]
