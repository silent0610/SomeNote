---
Type: 
aliases: 
tags: 
modifiedDate: 2025/06/13, 20:52:37
---

# C++顶层Const和底层Const

## 顶层 Const

指针(引用) **本身不可变**
如
`int * const p`
`p` 这个指针保存的地址不可变

## 底层 Const

指 **指向对象不可变**
如
`const int *  p`
`p` 这个指针指向的对象的内容不可变
