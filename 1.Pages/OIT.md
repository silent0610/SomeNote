---
Type:
  - Page
aliases:
  - order-independent transparency
  - 顺序无关的透明度
tags: 
modifiedDate: 2025/06/19, 10:53:43
---

# OIT

## 定义

允许[半透明物体](半透明物体.md)以任意顺序进行绘制，而最终的颜色混合结果在视觉上仍然是正确或可接受的，无需在 CPU 或 GPU 上对物体或几何体进行预排序。
- 顺序无关意味着应用程序不需要对透明物体进行排序。

## 算法

- [点阵剔除半透明](点阵剔除半透明.md)
- [随机透明度](随机透明度.md)
- [深度剥离](深度剥离.md)
- [A-Buffer](A-Buffer.md) 
- [多层 alpha 混合](多层%20alpha%20混合.md)
