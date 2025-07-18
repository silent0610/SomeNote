---
Type: 
aliases:
  - 野指针
  - Wild Pointer
tags: 
modifiedDate: 2025/06/17, 13:42:48
---

# C++野指针

## 定义

指向无效（未定义）内存地址的指针

## 原因

1. 指针变量声明时没有被初始化。当一个指针变量没有被初始化，它的值是不确定的，可能指向任意的内存地址。
2. 指针操作超越了变量的作用范围。当一个指针指向的变量超出了它的作用范围，该指针就成为了野指针。

## 解决

1. 在声明指针变量时，初始化为nullptr。这样可以避免指针变量的初始值是不确定的情况，可以将指针初始化为nullptr，即 `int *p = nullptr;`。
2. 在变量的作用域结束前释放指针所指向的内存，并将指针置为nullptr。当一个指针指向的变量超出了它的作用范围时，应该在变量的作用域结束前释放指针所指向的内存，并将指针置为nullptr
