---
Type: 
aliases:
  - Bounding Volume Hierarchy
  - 层次包围盒
tags: 
modifiedDate: 星期四, 六月 5日 2025, 8:45:51 晚上
---

# BVH

## 定义

光追或视锥剔除中常用的空间数据结构, 主要用于加速光线追踪中的求交测试

## 构造

通常通过 [Compute Shader](Compute%20Shader.md) 并行构造

### 自顶向下

从包含所有几何体的根节点开始，递归地将当前节点中的图元（Primitives，通常是三角形）分成两组，为每组创建一个子节点。这个过程重复进行，直到每个节点包含的图元数量达到预设的阈值（如 1 到 4 个），成为叶节点。

### 自底向上

从每个单独的图元（作为叶节点）开始。然后迭代地将最靠近或最适合合并的两个节点合并成一个父节点，并计算新的父节点的包围盒。这个过程重复进行，直到所有图元都被包含在一个单一的根节点中。

## 求交 (查询)

当一条光线射入场景进行求交测试时，BVH 会按以下方式加速查询：

1. **从根节点开始**：光线首先与 BVH 的**根节点 AABB** 进行求交测试。
2. **递归遍历**：
    - 如果光线**不与**当前节点的 AABB 相交，那么光线肯定也**不与**该节点包含的任何几何体相交。整个子树都可以被**跳过 (skip)**，无需进一步检查。
    - 如果光线**与**当前节点的 AABB 相交，那么光线可能与该节点包含的几何体相交。此时，光线会递归地进入该节点的**子节点**，并对子节点的 AABB 进行求交测试。
3. **精确求交**：
    - 当光线最终到达**叶节点**时，它会与叶节点中包含的**少量实际几何体**进行精确的求交测试，找到最近的交点。

## 分割

### 中点分割

### 表面积启发式分割

[[SAH]]
