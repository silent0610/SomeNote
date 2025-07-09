---
Type: 
aliases: 
tags:
  - TODO
modifiedDate: 2025/06/25, 22:12:42
Status:
---

# MVP 矩阵的推导

## Model

顺序是先乘 缩放 (剪切), 旋转, 平移

## View

**目标:** 把相机移到世界空间原点, 并使其看向-Z 方向
![[基底]]

### 回到 View 矩阵的推导

我们要做的就是把世界空间中的点在相机空间表示出来
这一般分两步, 

#### 第一步 , 平移

即把相机移到原点. 这个简单, 就是对相机的世界空间坐标取反即可

$$
\begin{aligned} \begin{array}{} \mathbf{M} &= \underbrace{ \left ( \begin{array}{} r_x & r_y & r_z & 0 \\ u_x & u_y & u_z & 0 \\ -v_x & -v_y & -v_z & 0 \\ 0 & 0 & 0 & 1 \\ \end{array} \right) }_{\mathsf{change\ of \ basis}} \underbrace{ \left ( \begin{array}{} 1 & 0 & 0 & -t_x \\ 0 & 1 & 0 & -t_y \\ 0 & 0 & 1 & -t_z \\ 0 & 0 & 0 & 1 \\ \end{array} \right) }_{\mathsf{translation}} \\[4mm] &=\left ( \begin{array}{} r_x & r_y & r_z & -\mathbf{t \cdot r} \\ u_x & u_y & u_z & -\mathbf{t \cdot u} \\ -v_x & -v_y & -v_z & -\mathbf{t \cdot v} \\ 0 & 0 & 0 & 1 \\ \end{array} \right) \end{array} \end{aligned}
$$

#### 第二步, 旋转

根据基底的知识, 我们要找到的就是两组基底怎么相互转换. 即 $M$ 是什么
那我们首先要找到两组基底
- 显然, D 为三条轴, 
    - 1,0,0
    - 0,1,0
    - 0,0,1 
- 那怎么计算 C 呢?
    - 我们首先计算观察向量 $\mathbf{v = (l-c) / \Vert l-c \Vert}$，即从相机位置指向目标位置的单位向量；
    - 然后计算指向右方的向量 $\mathbf{r = (v \times u^{\prime}) / \Vert v \times u^{\prime} \Vert}$；这里的 $u'$ 并不是相机实际的 up 向量, 而是 0,1,0
    - 然后 $\mathbf{u = r \times v}$
    - 这样就能得到 C 的基底 ruv 了 (和 x, y, z 对应)
- 所以 $M= C^{-1}D$, 即上面看到的形式

#### 为什么是-v?

- v 不是基向量, -v 才是. 
- 相机的背面, 也就是-v, 才是右手系的正方向
- 相机的前方 z 值是负的

## Projection

### 正交投影

相对简单, 先平移，再缩放成标准立方体
![[assets/818be2fb18aac3e9b53bb9a3abf4879c.png]] ![[assets/c9c3ca6aae0a88fd65f68d48b36cf27a.png]]
一般情况下, r=-l
t = -b
- **这里 0>near>far**
- glm 中 0 <near< far
- 对应位置乘-1 即可

### 透视投影

透视投影可分为两步
- 先把视锥体压缩成一个长方体
- 再对这个长方体进行正交投影得到裁剪空间

![[assets/20250619-2.png]]
- **这里 0<near<far**
- -1 - 1 的
- 右手坐标系。
- y 轴未翻转
- **是 GLM 的投影矩阵**
0-1的为，
![[assets/20250619-3.png]]
需要y翻转的话只需要取反 M\[1]\[1]
