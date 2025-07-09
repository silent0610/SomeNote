---
Type: 
aliases:
  - Phong 模型
tags: 
modifiedDate: 2025/06/23, 15:57:13
---

# Phong Shading

Phong模型是一种经验模型, 由三种反射光组成，分别是

1. 环境光（Ambient）：模拟间接光照，即光线经过周围环境表面多次反射后形成的，一般使用一个常量进行表示。
2. 漫反射光（Diffuse）：光线照射到一个着色点上时被均匀反射到各个方向上。取决于入射光线与表面法线之间的夹角和入射光线本身的强度。
3. 镜面反射光（Specular）：即高光，若物体表面很光滑，在入射光线的反射方向上，具有较强的反射光线。与观察角度有关。

## 实现

- 漫反射 即 [[Lambertian BRDF]]
- 高光, 使用反射向量和 viewdir 的夹角计算
![[assets/v2-e42efe5660e8aaddea68027b7c3990ad_r.jpg]]

[[Blinn-Phong]]
