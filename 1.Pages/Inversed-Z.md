---
Type:
  - Page
aliases:
  - 反转 Z 缓冲
  - Inverted Z-Buffer
tags: 
modifiedDate: 2025/06/19, 15:26:42
---

# Inversed-Z

传统的深度缓冲通常将 0.0 表示近裁剪面，1.0 表示远裁剪面

- 但是, 浮点数在接近 0 的精度要高于接近 1 的精度。
- 且, 绝大部分物体的深度集中在 1 附近[^1]
    
- 导致浮点数的精度利用率低, 且远处的物体的深度精度低. 进而导致 Z-fighting (深度冲突) 和其他视觉伪影。

反转 Z 缓冲将深度值存储为 1.0 到 0.0，其中 1.0 代表近裁剪面，0.0 代表远裁剪面。 即 $z'= 1-z$
从而让大部分物体的深度值接近 0, 充分利用 浮点数的精度特性

[^1]: 在透视投影下，深度值是非线性分布的。投影矩阵会把场景中的物体从摄像机空间转换到裁剪空间，然后深度值通常会被标准化到 [0,1] 范围。这个非线性变换的特性是：**大部分可区分的深度信息会被“压缩”到靠近远裁剪面的区域（即接近 1.0 的区域）**。近处的物体深度值变化很小，而远处的物体深度值变化很大。
