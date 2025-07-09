---
Type: 
aliases:
  - Adapter
tags: 
modifiedDate: 2025/06/16, 17:51:46
---

# 适配器

## 定义

- 一种用于修饰容器、仿函数、迭代器接口的东西。包装一个已有的组件，并提供一个不同的、通常是更受限或行为更特殊的接口。
- 包装一个对象, 使其表现的像另一个东西

## 例子

顺序容器适配器：stack，queue，priority_queue
默认情况下，stack和queue是基于deque实现的；priority_queue是基于vector实现的。
