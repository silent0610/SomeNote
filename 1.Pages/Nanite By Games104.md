---
Type:
  - Page
aliases:
  - Virtual Geometry
tags: 
Status: 
modifiedDate: 星期二, 六月 3日 2025, 9:16:29 晚上
---

## Nanite

### 前置

draw primitive -> draw scene
- Instance
	- Chunk 
		- Cluster
			- Triangle

#### 两阶段遮挡剔除

- 复用上一帧的深度缓冲，根据启发式算法得到, 当前帧最有可能看的见（最近）的几个物体, 并进行粗略的深度测试
- 得到 zbuffer
- 重新对第一步中被剔除的物体进行深度测试

这样做的好处在于，挑选出最有可能看见的几个物体有助于减少测试成本。  
比如眼前的屏幕，可能 1 个像素来来回回要写入十多次，现在先将最有可能的屏幕写入，后面被屏幕遮挡的其它物体就不太可能写入深度缓冲

#### 快速阴影遮挡剔除

把上一帧的阴影贴图的深度投影到这一帧

#### Visibility Buffer

[延迟渲染](延迟渲染.md) [[G-Buffer]]  
G-Buffer 大大加大了带宽消耗

- 就是先记录当前像素的可见的三角形的 ID
- 后面着色就直接从三角形 ID 倒查纹理等

### Overview

类似于 [Virtual Texture](Virtual%20Texture.md) 的思想

### Geometry Representation

- 排除
	- voxel 太贵
	- 细分表面的方法
	- Maps-based 
	- Point cloud **overdraw！**
- 最后选择用 triangle 表示几何
- 屏幕有多精细，就绘制多少三角形，尽量让像素和三角形一一对应
- 应用 cluster 
- 越近的 cluster LoD 级别越低，即越精细。Mipmap 和 LoD 就像金字塔，底层为 0，等级越高，越粗糙
- 但会产生缝隙问题
	- 做法一：锁住 cluster 的边缘
		- 这样做效率不高
		- 人眼能看出明显 cluster 差距
	- 做法二：cluster0 分 Group，只锁 Group 的边缘三角形。对 Group 内部简化。对得到的所有三角形再分 cluster，再分 Group。如此循环
		- 保证 LoD 切换时，Group边界不会固定
		- 就类似做了一个抖动 Jittering
		- ![](assets/Pasted%20image%2020250116204333.png)

---

- QEM 简化
- 如何选择 LoD
	- 并行
	- #没看懂
	- BVH 的作用
		- 根据距离预先过滤？
	- Job System

### Rendering

tile base rendering  
tile 和材质对应

### Virtual Shadow Map

[级联阴影](Cascade%20Shadow%20Map.md)
根据几何在视空间的大小确定其在阴影空间的大小  
思想类似于 virtual texture。  
只更新需要的  
#没看懂 

### Streaming and Compression
