---
Type:
  - Log
aliases: 
tags:
  - 渲染器实现细节
modifiedDate: 2025/06/25, 22:05:25
---

# SSAO硬渲染器内实现

一个完整的 pass

## CPU 端

- RT: 一张窗口大小[^1]的 RGBA, 32B
- Sampler:Nearest
- Buffers
    - CBuffer:
        - `总采样数`
        - 采样半径 (View Space)
        - Scale 蓝噪声纹理和 RT 大小的比值
    - Kernels Buffer 
        - `总采样数` 个 Kenel, 每个 kernel 都是一个采样位置
- Descriptor Set
    - Cbuffer
    - Camera Matrix
    - Depth
    - Normal
    - Noise (Blue Noise, 从网络下载)
    - kernels buffer
- Pipeline : 全屏 pass

## GPU 端

### Pixel Shader

- 使用噪声生成切线, 从而扰动在半球空间扰动采样点, 防止 Pattern
- 构造 TBN, 将采样点从切线空间转到模型空间

### Vertex Shader

- SV_Position
- 屏幕空间uv

## Blur

[交叉双边滤波](交叉双边滤波.md)

[^1]: 固定为 1280
