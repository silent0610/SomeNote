---
Type: 
aliases:
  - dangling
  - 悬空指针
  - 垂悬指针
tags: 
modifiedDate: 2025/06/17, 13:44:28
---

# Dangling Pointer

## 定义

指向的内存空间已被释放或不再有效

## 解决

- 智能指针
- 要注意长生命周期的指针不能指向短生命周期的对象。
- 对于释放过内容的指针，立即将指针置为nullptr。
