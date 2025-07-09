---
Type:
  - Page
aliases:
  - std::shared_ptr
  - 共享指针
tags: 
modifiedDate: 2025/06/16, 19:33:02
---

# shared_ptr

## 定义

提供共享所有权 ，允许多个 `shared_ptr` 指向同一个对象。

## 实现细节

- 内部维护着一个控制块: 控制块的操作是原子操作 `atomic<int>*`
    - 强引用计数: 即多少个 `shared_ptr` 指向同一对象 
        - 当一个新的 `shared_ptr` 指向该对象时（例如通过拷贝构造或拷贝赋值另一个指向该对象的 `shared_ptr`），强引用计数会加 1。
        - 当一个 `shared_ptr` 被销毁（例如离开作用域）或者被重新赋值指向其他对象时，它之前指向对象的强引用计数会减 1。
    - 弱引用计数: 有多少个 [weak\_ptr](weak_ptr.md) 指向该对象

> 控制块只有当 `weak_count` 也降为 0 时才会被释放。

### 多线程安全性

> 需要注意的是 `shared_ptr` 管理对象的生命周期是线程安全的，但对象内部数据的并发访问不是。

个人理解就是: 一定可以获得对象, 以及得知对象什么时候被销毁. 但是如果要读写对象内部数据, 某个指针的修改不会同步反应到其它指针
需要使用临界区, 互斥量保护对象数据

### 代码

最基础版本, 不考虑 weak_ptr

```C++
template<typename T>
class SharedPtr
{
public:
  // 默认构造函数
  SharedPtr() :count_(new std::atomic<size_t>(0)), ptr_(nullptr) {}
  // 带参构造函数
  explicit SharedPtr(T* ptr) :count_(new std::atomic<size_t>(1)), ptr_(ptr) {}
  // 析构函数
  ~SharedPtr()
  {
    --(*count_);
    if (*count_ <= 0)
    {
      delete ptr_;
      delete count_;
      ptr_ = nullptr;
      count_=nullptr;
    }
  }
  // 拷贝构造函数
  SharedPtr(const SharedPtr& shared_ptr)
  {
    ptr_ = shared_ptr.ptr_;
    count_ = shared_ptr.count_;
    ++(*count_);
  }
  // 拷贝赋值运算符
  SharedPtr& operator=(const SharedPtr& shared_ptr)
  {
    ptr_ = new T(*shared_ptr.ptr_);
    count_ = shared_ptr.count_;
    ++(*count_);
    return (*this);
  }
  //另一种写法的拷贝赋值运算符
  SharedPtr& operator=(const SharedPtr& shared_ptr)
  {
    --(*count_);
    if(*count_==0)
    {
      delete ptr_;
      delete count_;
    }
    ptr_=shared_ptr.ptr_;
    count_=shared_ptr.count_;
    if(count_)
    {
      ++(*count_);
    }
    return (*this);
  }
  // 移动构造函数
  SharedPtr(const SharedPtr&& shared_ptr) noexcept
  {
    count_ = shared_ptr.count_;
    ptr_ = shared_ptr.ptr_;
    shared_ptr.count_ = shared_ptr.ptr_ = nullptr;
  }
  //移动赋值运算符
  SharedPtr& operator=(SharedPtr&& shared_ptr)
  {
    if (this != &shared_ptr)
    {
      count_ = shared_ptr.count_;
      ptr_ = shared_ptr.ptr_;
      shared_ptr.count_ = shared_ptr.ptr_ = nullptr;
    }
    return (*this);
  }

  T& operator*() { return  *ptr_; }
  T* operator->() { return  ptr_; }
  explicit operator bool() { return  ptr_ == nullptr; }
  T* get() { return ptr_; }

  size_t use_count() const { return *count_; }
  bool unique()const { return *count_ == 1; }
  void swap(SharedPtr& ptr) { std::swap(*this, ptr); }

private:
  std::atomic<size_t>* count_;
  T* ptr_;
};
```

## 作用

- 当一个智能指针在销毁时, 其强引用计数**降为 0** ，意味着不再有任何 `shared_ptr` 指向该对象。此时，会对该保存的指针进行 Delete**自动删除**，其占用的内存会被释放。
- **自动内存管理：** 显著减少内存泄漏的风险。
- **易于共享：** 方便地在代码的不同部分共享对象的所有权。

## 缺点

- [智能指针循环引用](智能指针循环引用.md)
