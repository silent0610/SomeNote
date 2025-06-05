---
Type: 
aliases:
  - Bidirectional Scattering Distribution Function
  - 双向散射分布函数
tags: 
modifiedDate: 星期四, 六月 5日 2025, 3:21:22 下午
---

## 定义

BSDF 是 **[BRDF](BRDF.md) 和 [BTDF](BTDF.md) 的广义化和超集**。它描述了光线在给定一个**入射方向** (ωi​) 时，有多少光线会从**另一个出射方向** (ωo​) 被**散射 (Scattered)** 出去，无论这种散射是反射（留在同一侧半球）还是透射（穿过表面到另一侧半球）。

**关系**：**BSDF = BRDF + BTDF**
- 当 ωo​ 位于与 ωi​ 相同的半球时，BSDF 的行为就是 BRDF。
- 当 ωo​ 位于与 ωi​ 相反的半球时，BSDF 的行为就是 BTDF。
