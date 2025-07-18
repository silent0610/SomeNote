---
Type:
  - Page
aliases:
  - Endity Component System
  - 实体组件系统
tags: []
modifiedDate: 2025/06/18, 13:55:07
---

# ECS

[游戏引擎岗面经](游戏引擎岗面经.md)

## 简介

- 实体组件系统 (ECS) 是一种主要用于游戏开发和其他需要高性能的应用程序架构模式。
	- 遵循**组合优于继承**的原则，
	- 将**数据和行为分离**来提高灵活性和性能。

就像 Unity 的对象和组件

> 将实体、组件和系统分离，并优先考虑数据组织和高效处理。

> ECS 是[面向数据编程](DOP.md)在游戏开发（或类似领域）中的一个具体和成功的应用范例。

## 核心概念

- 实体 Endity
	- 没有任何逻辑或数据的纯粹标识符。可以将它们看作是对象的 ID。
- 组件 Component 
	- 轻量级的数据结构, 比如 Pos 是一个 Vec3. 不包含任何运行处理逻辑
- **系统 (Systems):** 
	- 包含处理组件数据的逻辑。系统查询具有特定组件的实体，并对这些组件的数据执行操作。
	- 如物理系统, 渲染系统等

## 关键实现

- 实体管理
	- 唯一 ID
	- 高效创建, 销毁和查找
	- 可以复用已销毁
- 组件存储
	- 相同类型组件存储在连续内存块中
		- 以 pos 的存储为例,即 `std::vector<vec3>Posses`
		- 每个组件类型都有其自身的数据结构
		- 实体和组件需要维护关联关系, 通常为每个实体维护一个指向其组件的索引或指针列表
- 系统管理
	- 一个系统对应**特定组件组合**
	- 需要注意系统的执行顺序
- 查询
	- 系统使用查询来高效地检索具有特定组件组合的实体。
	- 需要能够快速找到满足条件的实体集合
	- 可以使用索引或数据结构来加速查询过程。

## 为什么要 ECS

-  [数据局部性](数据局部性.md) 相同类型的组件是连续存储的, 使其拥有良好的数据局部性, 空间局部性
- 缓存友好, 因为良好的数据局部性, 增加了 cpu 缓存的命中率, 提高了任务执行速度
