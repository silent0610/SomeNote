---
Type:
  - Page
aliases:
  - 间接绘制调用
tags: 
Status: 
modifiedDate: 2025/06/17, 15:18:09
---

# Indirect Draw Call

## 定义

- 可由 GPU 发出 Draw Call

### 核心思想

> 不再由 CPU 逐个地向 GPU 发送 Draw Call 的参数，而是将所有 Draw Call 的参数数据打包到一个缓冲区中，并存储在 GPU 可访问的内存中。然后，CPU 只发送一个非常少的命令（比如一个 `DrawIndirect` 命令）来告诉 GPU：“去那个缓冲区里读取绘制指令，然后执行它们。”

## 原因

传统: CPU 负责向 GPU 发出每一个渲染指令，我们称之为 **Draw Call**
一次 Draw Call 通常处理一个 Mesh, 指定 vertex, index, texture 等
当物体数量过多时
- **API 是有开销的**:   调用 drawcall 会有 cpu 开销
- **状态切换：** 在绘制不同物体时，CPU 需要频繁地设置 GPU 的状态（绑定顶点缓冲区、索引缓冲区、纹理、着色器等），这也增加了 CPU 的负担。

最终导致 CPU 瓶颈

## 细节

#TODO

## 举例

- **传统方式：** CPU 需要遍历所有石头，判断哪些可见，为每个可见的石头发送一个 Draw Call。如果 10000 颗石头有 5000 颗可见，CPU 需要发出 5000 个 Draw Call。
- **Indirect Draw Call 方式：** ^6c2620
    1. CPU 将所有石头的几何数据（顶点、索引）打包到一个大的 GPU 缓冲区中。
    2. GPU 上的一个计算着色器执行剔除操作，并判断每颗石头应该使用哪个 LoD。
    3. 计算着色器将可见石头的绘制参数（如石头类型、位置、索引偏移、顶点数量等）写入一个**间接参数缓冲区**。如果 5000 颗石头可见，这个缓冲区就包含 5000 个 Draw Call 的参数。
    4. CPU 只发送一个 `DrawIndexedIndirect` 命令，指向这个由 GPU 填充的缓冲区。
    5. GPU 接收到这一个命令后，自主地从缓冲区中读取 5000 颗石头的绘制参数，并完成渲染。

这样，CPU 的工作量从 5000 个 Draw Call 显著减少到了 1 个 Draw Call，极大地提高了效率。
