---
Type: 
aliases:
  - Reflective Shadow Maps
  - 反射阴影贴图
tags: 
modifiedDate: 2025/06/11, 13:05:41
---

# RSM

一种漫反射全局光照技术, 是一种 [Shadow Map](Shadow%20Map.md) 的扩展形式。 

## 假设

它假设场景中的表面都是漫反射的。

## 原理

- RSM是从光源的视角来进行渲染（类似shadow map）。存储有关可见表面的其他信息。[^1]
- 在进行最后着色的时候，RSM的纹素会被视为虚拟点光源，从而提供单次弹射的间接照明效果。
- 可以使用重要性驱动（importance-driven）的启发式方法，基于整个RSM来创建一些虚拟光源，并将其放置（splatted） 在屏幕空间中。

### 一些细节

- 使用Cone Tracing 分布样本点, 权重
- 针对屏幕上的像素, 会有极少部分像素由于法线, 距离等因素, 无法适用稀疏的采样, 所以针对这些像素, 单独做一次完整的 (数量更多?)采样

## 优缺点

Cool：
- 流程简单实现简单
- 光子注射
- Cone sampling in mipmap
- 在 低分辨率进行间接光照计算, 同时有误差检测
- 可以直接利用shadow map的相关算法。很适合手电筒这类光源。[^2]

缺点：每个光源都需要生成一个shadow map。没有考虑间接光照的遮挡现象，存在漏光现象。如果创建的间接光源过少，会导致画面闪烁（间接光源的位置发生改变）。

[^1]: - Depth
    - Normal
    - Radiance
    
[^2]: By Games104
