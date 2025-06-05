---
Type:
  - Page
aliases: 
tags: 
Status: 
modifiedDate: 星期三, 六月 4日 2025, 10:53:31 晚上
---
- 一种[局部着色模型](局部着色模型.md)
- 模拟次表面散射

公式

 $$
f (\mathbf{l}, \mathbf{v})=\frac{\rho_{\mathrm{ss}}}{\pi} \tag{9.11}
$$

> [!note] π
> 为什么要有这个π, 因为上面的**反照率 albedo**代表入射光反射出来的所有能量与原能量的比例, 而 BRDF 是两个方向之间的. 必须遵守能量守恒.
>  如果直接使用 albedo 在半球面进行积分 $\int(albedo)cos\theta dw$ ([渲染方程](渲染方程.md)), 会得到 π\*albedo>1, 显然能量不守恒, 所以要除π

之后为什么要乘π, 可能因为是镜面反射
