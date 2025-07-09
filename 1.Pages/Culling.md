---
Type: 
aliases:
  - 剔除
tags: 
modifiedDate: 2025/06/24, 13:51:36
---

# Culling

减少不必要的处理量, 无论是 CPU 还是 GPU, 判断哪些物体或哪些部分是不可见、不可达或无关紧要的，并将其从渲染流程中移除，从而节省大量的计算资源。
- 整个去除, 而不是 [[Clipping]] 裁剪

## 技术

见 RTR4 [Chapter 19 Acceleration Algorithms 加速算法](https://www.wolai.com/q4hLtwuXRJHtc8rytjmQQa)
- [[视锥体剔除]]
- [[遮挡剔除]]
    - earlyZ
    - HZB
    - Zprepass
    - 启发式算法与重投影
    - 复用上一帧深度信息预过滤
- [[背面剔除]]
    - 顶点着色之后
        - 在屏幕映射之后?
    - 光栅化之前
- [Nanite](Nanite.md)
    - cluster based culling
    - 两种遮挡剔除的前置方法
    - 动态选择 LOD
- [[TileBasedLighting]]
    - 剔除光照
- [[ClusterBasedLighting]]
    - 剔除光照
