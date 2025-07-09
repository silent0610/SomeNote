---
Type:
  - Page
aliases:
  - Schlick近似
tags: 
modifiedDate: 2025/06/20, 17:43:56
---

# Schlick菲涅尔近似

## 近似方程

$$
F(\mathbf{n}, \mathbf{l}) \approx F_0+\left(1-F_0\right)\left(1-(\mathbf{n} \cdot \mathbf{l})^{+}\right)^5
$$

菲涅尔反射率F，
实际上就是在白色和 $F_0$ 之间进行RGB插值，尽管它很简单，但是这个近似还是相当准确的。

## 粗糙度修正

$$
F(\mathbf{n}, \mathbf{l}) \approx F_0+\left(G-F_0\right)\left(1-(\mathbf{n} \cdot \mathbf{l})^{+}\right)^5
$$

其中

$$
G = max(1-Roughness,F_0)
$$

#渲染器实现细节/PBR

## 什么是 $F_0$, 怎么得到

- F0​ 指的是 **基础反射率 (Fresnel reflectance at normal incidence)**，也称为**0度入射角的菲涅尔反射率**
- 它代表了当光线**垂直（或近似垂直）入射到物体表面时**，有多少比例的光能会被表面反射出去。

通常可以使用折射率计算得到

### 不同材质的 F0

非金属材质的 F0 设置为>= 0.04
纯金属材质的 F0 设置为 albedo

- 如果 `Metallic = 0` (非金属)，则 `Base Color` 作为漫反射颜色，F0​ 固定为 `vec3(0.04)`。
- 如果 `Metallic = 1` (金属)，则 `Base Color` 作为 F0​ 的颜色，并且没有漫反射。
- 如果 `Metallic` 介于 0 和 1 之间，则进行混合。
即根据 `metallic` ,在 `0.04` 和 `baseColor` 间插值?
