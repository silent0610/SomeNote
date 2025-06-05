---
Type: 
aliases:
  - Ground Truth Ambient Occlusion
tags: 
modifiedDate: 星期三, 六月 4日 2025, 3:33:44 下午
---
- GTAO 在半圆切面上一次计算两个方向, [HBAO](HBAO.md) 在圆上计算一个方向, 这真的算差别吗😅
- 添加了 Mult Bounce 函数, 使用多项式拟合**环境光在物体表面的多次反射**
- 夹角是和 viewDir[^1] 的最大夹角, 而 英伟达的 HBAO 的实现是和 (0,0,1) 的夹角 #疑问 
- 两种形式的积分近似
    - 余弦权重
    - 均匀权重

[^1]: normalize(-viewPos)
