---
Type: 
aliases:
  - Bidirectional Scattering Distribution Function
  - 双向散射分布函数
tags: 
modifiedDate: 星期四, 六月 5日 2025, 7:34:17 晚上
---

# BSDF

## 定义

BSDF 是 **[BRDF](BRDF.md) 和 [BTDF](BTDF.md) 的广义化和超集**。它描述了光线在给定一个**入射方向** (ωi​) 时，有多少光线会从**另一个出射方向** (ωo​) 被**散射 (Scattered)** 出去，无论这种散射是反射（留在同一侧半球）还是透射（穿过表面到另一侧半球）。

**关系**：**BSDF = BRDF + BTDF**
- 当 ωo​ 位于与 ωi​ 相同的半球时，BSDF 的行为就是 BRDF。
- 当 ωo​ 位于与 ωi​ 相反的半球时，BSDF 的行为就是 BTDF。

## 单位

单位为 (BRDF 同样)

$$
sr^{-1}
$$

[^1]
从公式上推导
BSDF 的公式定义是

$$

f_{s} ( \omega_{i}, \omega_{o} )=\frac{d L_{o} ( \omega_{o} )} {d E_{i} ( \omega_{i} )}

$$

$L_o$ 即 Radiance, 单位为 $W/(m^2 \cdot sr)$
$E_i$ 是从入射方向 $ω_i​$ 照射到表面的 irradiance, 单位为 $W/(m^2)$

所以 BSDF 单位为

$$
\frac{W / ( m^{2} \cdot s r )} {W / m^{2}}=\frac{W} {m^{2} \cdot s r} \cdot\frac{m^{2}} {W}=\frac{1} {s r}=s r^{-1}
$$

[^1]: sr 是单位立体角, 总立体角是 4π , 来自于($\frac{A}{r^2}$)
