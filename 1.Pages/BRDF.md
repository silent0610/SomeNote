---
Type:
  - Page
aliases:
  - 双向反射分布函数
  - Bidirectional Reflectance Distribution Function
tags: 
Status:
modifiedDate: 星期三, 六月 4日 2025, 10:09:25 晚上
---

## 定义

BRDF 是一个数学函数，它描述了光线在一个表面上**如何被反射**。具体来说，它定义了当光线从一个方向入射到表面上某一点时，有多少能量会反射到另一个出射方向上。
漫反射, 镜面反射

## 物理基础

1. 能量守恒,出射的能量不能大于入射的能量
    1. 对于一个理想的 BRDF，在所有出射方向上对 BRDF 进行积分，结果不能超过 1。
2. Helmholtz 互异性, 即可逆, 即入射和出射方向互换，BRDF 值不变
3. 非负

## 与 [PBR](PBR.md) 的关系

PBR 的核心就是使用**基于物理的 BRDF 模型**来模拟光照

## 常见的 BRDF 模型

[Cook-Torrance BRDF](Cook-Torrance%20BRDF.md)
[Lambertian BRDF](Lambertian%20BRDF.md)
