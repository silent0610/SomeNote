---
Type: 
aliases:
  - Reflective Shadow Maps
  - 反射阴影贴图
tags: 
modifiedDate: 星期三, 六月 4日 2025, 7:50:42 晚上
---
一种漫反射全局光照技术, 是一种 [Shadow Map](Shadow%20Map.md) 的扩展形式。 

## 假设

它假设场景中的表面都是漫反射的。

## 原理

- RSM是从光源的视角来进行渲染（类似shadow map）。存储有关可见表面的其他信息。[^1]
- 在进行最后着色的时候，RSM的纹素会被视为虚拟点光源，从而提供单次弹射的间接照明效果。
- 可以使用重要性驱动（importance-driven）的启发式方法，基于整个RSM来创建一些虚拟光源，并将其放置（splatted）在屏幕空间中。

## 优缺点

优点：流程简单，实现简单，可以直接利用shadow map的相关算法。很适合手电筒这类光源。

缺点：每个光源都需要生成一个shadow map。没有考虑间接光照的遮挡现象，存在漏光现象。如果创建的间接光源过少，会导致画面闪烁（间接光源的位置发生改变）。

[^1]: - Depth
    - Normal
    - Radiance
    
