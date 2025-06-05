---
Type:
  - Page
aliases: 
tags: 
Status: 
modifiedDate: 星期四, 六月 5日 2025, 10:52:55 上午
---

## 定义

- 一种现代渲染架构,旨在将传统渲染管线中由 **CPU 完成的大量工作转移到 GPU 上执行**
- 其核心思想是让 GPU 尽可能地自主管理和处理渲染任务，从而**大幅减少 CPU 的开销，解决 Draw Call 瓶颈，并实现更高效、更复杂的场景渲染**。
- 如
    - 剔除
    - LoD 选择 [Nanite](Nanite.md)
    - 实例化
    - Draw Call **Indirect Draw**[^1]

### 核心思想

- 最小化 CPU 和 GPU 的交互
    - CPU 发送少量全局命令
    - 让 LoD 选择, 剔除等计算由 GPU 执行
- 利用 GPU 的并行能力
- 利用 GPU 的 Indirect Draw
    - 即让 GPU 发出 DrawCall, 填充 command buffer . 从而减少 CPU 与 GPU 的通信开销, 从而大量减少 DrawCall

## 原因

- 传统渲染管线 (CPU Driven)的问题
    - CPU 瓶颈, cpu 遍历每个物体, 进行大量计算, 如 LoD, 视锥剔除等, 发送大量 DrawCall
    - Draw 实际上是有开销的, 大量 DrawCall 使 GPU 等待 CPU, 导致 GPU 利用率低
    - 并行性低: CPU 是串行处理任务的

## 优势

> 这块内容好像牛头不对马嘴

- 减少了被绘制的三角形 (三角形包括顶点)
    - 更加精确的剔除对最终画面没有贡献的三角形
        - 原来是只要物体一部分出现在视锥体内部, 整个 mesh 的三角形就都要被绘制
        - 现在把 mesh 细分为多个 cluster, 减少了被绘制的三角形
    - 把模型的顶点数据进一步切分为更细粒度的**Cluster**（或者叫做**Meshlet**），让每个Cluster的粒度能够更好地适应Vertex Processing阶段的Cache大小，并以Cluster为单位进行各类剔除
- 模型数、三角形顶点数和面数的问题得到了极大的优化改善。
- 但高精度的模型、像素级别的小三角形给渲染管线带来了新的压力：**光栅化**和**重绘（overdraw）**的压力。

[^1]: CPU 只提交一个或少数几个间接绘制命令给 GPU。这些间接命令包含一个指向 GPU 缓冲区（通常由 Compute Shader 填充）的指针，该缓冲区中包含了所有需要渲染的对象的绘制参数（例如，每个对象的顶点/索引偏移、实例数量、绘制类型等）。
