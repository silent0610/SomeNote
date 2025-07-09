---
Type:
  - Page
aliases: 
tags: 
Status: 
modifiedDate: 2025/06/20, 17:37:11
---

# Cook-Torrance BRDF

Cook-Torrance 是一个[局部着色模型](局部着色模型.md)

## 核心公式

$$
f_r\left(\omega_i, \omega_o\right)=f_{\text {diffuse }}\left(\omega_i, \omega_o\right)+f_{\text {specular }}\left(\omega_i, \omega_o\right)
$$

### 漫反射项

使用 [Lambertian BRDF](Lambertian%20BRDF.md)
即

$$
f_{d i f f u s e}=\frac{\mathrm{A l b e d o}} {\pi}
$$

### 镜面反射项

$$
f_{s p e c u l a r}=\frac{D ( h ) \cdot F ( \omega_{i}, h ) \cdot G ( \omega_{i}, \omega_{o}, h )} {4 ( \mathbf{n} \cdot\omega_{i} ) ( \mathbf{n} \cdot\omega_{o} )}
$$

F 使用Schlick 菲涅尔近似
G2 使用 Schlick-Smith GGX 近似

$( \mathbf{n} \cdot\omega_{i} ) ( \mathbf{n} \cdot\omega_{o} )$ 负责根据光线和观察角度与表面法线的夹角进行正确的能量投影和衰减。

这个因子 `4` 是归一化常数, 精确来源涉及到对 BRDF 公式进行积分（在整个半球上）并确保其在物理上是能量守恒的。
- 能量守恒定律和赫姆霍兹互反性

#渲染器实现细节/PBR 
