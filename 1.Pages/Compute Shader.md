---
Type:
aliases: 
tags: 
modifiedDate: 星期四, 六月 5日 2025, 11:22:36 上午
---

## 核心

旨在将 GPU 的**强大并行计算能力**用于**通用计算任务**，这些任务**不一定与传统的图形渲染管线直接相关**。

- **输入**：任意类型的缓冲区（Structured Buffers, Raw Buffers）、纹理（可读写）、常量缓冲（只读）。
- **输出**：任意类型的缓冲区和纹理（可读写）。

## 应用

- 图像处理
    - 模糊滤波降噪等
- 粒子模拟, 流体模拟
- [GPU Driven Pipeline](GPU%20Driven%20Pipeline.md) [Nanite](Nanite.md)
    - 剔除
    - LoD 计算
- 光追加速结构
- [SDF](SDF.md)
- 体素化
