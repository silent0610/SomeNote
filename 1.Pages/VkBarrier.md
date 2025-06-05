---
Type:
  - Page
aliases: 
Status:
tags: 
modifiedDate: 星期一, 五月 26日 2025, 8:59:10 晚上
---

## 定义

`VkMemoryBarrier`、`VkBufferMemoryBarrier` 和 `VkImageMemoryBarrier` 统称为 `Barrier`，它们用于在**单个命令缓冲区内部** 或 **单个队列内部** 强制实现**管线阶段的执行顺序**和**内存访问的可见性**。它定义了一个“同步点”，**在该点之前特定管线阶段的写入操作必须完成，并且其结果对之后特定管线阶段的读取操作可见。**

## 一些细节

`Barrier` 依赖于 Vulkan 的**管线阶段 (Pipeline Stages)** 和**内存访问类型 (Access Types)** 概念。

- `srcStageMask` 和 `dstStageMask`：定义了同步发生在哪些管线阶段之间（例如，顶点着色器阶段完成写入，片段着色器阶段可以开始读取）。
- `srcAccessMask` 和 `dstAccessMask`：定义了在 Barrier 之前哪些内存写入必须完成，以及在 Barrier 之后哪些内存读取可以可见（例如，上一个绘制命令的颜色附件写入对下一个呈现命令的颜色附件读取可见）。
- `oldLayout` 和 `newLayout` (针对图像屏障)：用于进行图像布局转换，这是 `VkImageMemoryBarrier` 的一个独特且非常重要的功能。

## 应用

- **图像布局转换：** 这是 `Barrier` 最核心和最常用的功能。例如，在 deferred 中, 如果把几何 pass 的深度 attachment 就需要被转换为 `VK_IMAGE_LAYOUT_SHADER_READ_ONLY_OPTIMAL`, 以在后续 pass 中被采样
- **资源状态同步：** 当同一个资源（如缓冲区或图像）在不同的管线阶段或被不同的着色器访问时，需要 `Barrier` 来确保读写操作的正确顺序和内存可见性。例如，一个计算着色器写入一个缓冲区，随后一个顶点着色器需要读取这个缓冲区作为顶点数据。
- **计算着色器与图形管线的同步：** 如果计算着色器的输出（例如一个纹理）是图形管线某个阶段的输入，则需要 `Barrier` 来确保计算完成且数据已写入内存，以及所有权转移, 然后图形管线才能开始读取。
