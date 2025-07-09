---
Type:
  - Page
aliases: 
tags: 
Status: 
modifiedDate: 2025/06/20, 17:58:43
---

# Lambertian BRDF

## 定义

- 一种[局部着色模型](局部着色模型.md)
- 模拟次表面散射
Lambertian BRDF是最简单的BRDF，它在半球方向上具有恒定值，即对于一条任意方向的入射光线，其反射光线在半球方向上均匀分布，将$f(\mathbf{l}, \mathbf{v})$项作为一个常数，可以计算得到定向半球反射率$R(\mathbf{l})$，即：

$$
R(\mathbf{l})=\pi f(\mathbf{l}, \mathbf{v})
$$

Lambertian BRDF的恒定反射率通常称为漫反射颜色或者反照率（albedo）$\rho$，物理正确的名称是次表面反照率（subsurface albedo）$\rho_{ss}$，因此可以获得Lambertian BRDF的表达式：
所以

 $$
f (\mathbf{l}, \mathbf{v})=\frac{\rho_{\mathrm{ss}}}{\pi} \tag{9.11}
$$

## 除π

> [!note] π
> 为什么要有这个π, 因为必须**遵守能量守恒.**
> 上面的**反照率 albedo**代表入射光反射出来的所有能量与原能量的比例, 而 BRDF 是两个方向之间的. 
>  如果直接使用 albedo 在半球面进行积分 $\int(albedo)cos\theta dw$ ([渲染方程](渲染方程.md)), 会得到 π\*albedo>1, 显然能量不守恒, 所以要除π

之后为什么要乘π, 可能因为是镜面反射
