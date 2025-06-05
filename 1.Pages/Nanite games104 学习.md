---
Type:
  - Reading
aliases: 
tags: 
Status: 
modifiedDate: 星期四, 六月 5日 2025, 10:36:14 上午
---

## 前置知识

- Nanite 的核心思想, 让 GPU 承担尽可能多的工作
	- [GPU Driven Pipeline](GPU%20Driven%20Pipeline.md)
	- 让 GPU 承担尽可能多的工作
		- Compute Shader 的作用
- 思想源头
	- Mesh Cluster Rendering
		- 刺客信条某一作
		- 把一个 Mesh 的三角形分为很多个固定大小的 cluster
		- culled by cluster bouding box
		- 流程 
		- 核心思想
			- 尽可能的减少绘制的 cluster, 也就是三角形
		- CPU 的工作
			- 非常粗糙的视锥体剔除
				- CPU quad tree culling
				- 一个 batch 处理, 把相同材质的物体尽可能放到一个 group 提交
			- 计算每个实例的 LoD
		- GPU 的工作
			- mesh->Chunk->cluster
				- 多个 cluster 组成一个 chunk
				- 多个 chunk 一个 mesh
			- 基于 cluster 的 bbx 进行剔除, 并输出 cluster list
			- 使用 compute shader 剔除背面三角形
			- 把所有可见三角形合并到一个 index buffer 中, 并 draw call
- 怎么进行 Occlusion culling 
	- 方法 1:
		- 启发式算法:最有可能被遮挡的物体
		- 深度重投影: 把上一帧的深度投影到当前帧
		- [HZB](Hi-Z.md)
		- 问题: 有 artifact, 但是可以接受?
	- 方法 2: 两 pass [[遮挡剔除]]
		- 用上一帧的 depth buffer 进行提前过滤, 并更新 zbuffer, 并渲染
		- 再用被剔除的物体重新进行深度测试, 并渲染
- 阴影的遮挡剔除
	- 与方法 2 类似, 重投影上一帧的 depth buffer ,并进行快速的遮挡剔除测试
	- 被相机剔除的 shadow caster 物体的阴影, 基本不可见, 所以在计算阴影贴图时可以剔除他们
- [Visibility Buffer](Visibility%20Buffer.md)
	- forward shading
	- [延迟渲染](延迟渲染.md)
		- 问题:
			- GBuffer 对带宽要求太大
			- 传统延迟渲染由于不确定的绘制顺序, 导致过多的 overdraw, 对 Gbuffer 重复的写入
	- VB
		- 第一个 pass 仅在 VB 中存储可见几何的 ID ,三角形 id, 重心坐标等
		- 第二个 pass 利用 vb 的信息, 采样,进行 shading
		- 怎么和延迟渲染结合
			- 新增一个 pass, 利用 vb 的信息, 采样 normal ,albedo 存储在 Gbuffer 中
			- 最后一个 pass, shading
- 什么是 Indirect Draw Call  [Indirect Draw Call \> ](Indirect%20Draw%20Call.md#^6c2620)

## 本体

### Overview

- 借鉴 [Virtual Texture](Virtual%20Texture.md)
- 目标是一个像素一个三角形
- 几何表示选择
	- 为什么不用体素 voxel?
		- 颠覆传统工作流
		- 体素存储效率低, 是三维的
		- 渲染效率和成熟度. 光栅管线很成熟,
		-  避免块状感
	- Subdivision surfaces ?
		- 只能细分, 不能粗糙
	- Map Based?
		- 难以控制复杂细节
	- Point Cloud
		- 大量的 overdraw
	- 所以只能三角形
- 核心思想
	- 根据屏幕大小确定几何, 即三角形大小

### Geometry Representation

- 根据视角的 Mesh LoD 选择与简化
	-  接缝处漏洞
		- 直接锁边?
			- 接缝处 mesh 精细度太高, 利用率低
		- Cluster Group
			- 多个 Cluster 形成 group
			- group 锁边, 再简化
				- 使用 QEM 指标进行简化
			- 简化后再分 Cluster
			- 简化后的 cluster 边缘, 和之前的 cluster 会变化, 从而去掉高频信息
			- 即使Group 边缘是锁边的, 但是不同层级的 Group 边缘是不同的
			- ![300](assets/Nanite%20games104%20学习-4.png)
			- 不同层级的 Cluster 对应关系如下图所示. 是一个 [DAG](DAG.md)
			- ![300](assets/Nanite%20games104%20学习-5.png)
			- Lod0 的 cluster 可能对应多个 Lod1 的 Cluster
			- **在渲染时, 相邻的 Cluster 的层级可能不同, 所以他们是可能重叠的**, 但只会渲染可见的片元

模型的简化和 BVH 的构建是离线的, LoD 选择是实时的

#### 运行时 LoD 选择

- LoD 的选择
    - 并行化, 让 compute shader 去做 (不是完全独立, 而是task Queue)
    - 设定误差 threadhold
        - 这里误差是指什么? 
            - 二次误差度量 (Quadric Error Metric, QEM): 最小化顶点到平面集合的距离平方和来合并顶点或边。
            - 感知误差 (Perceptual Error)
    - 逐 Group 检测, group 内存储了上界 thread hold
    - 如果 group 的误差上界>threadhold (剔除下面的节点)
    -  对于其内的所有 cluster
        - 逐 cluster, 如果 threadhold >某个 cluster 的误差 (剔除上面的节点)
            - 那如果在两者之间呢? 
        - 绘制
    - 简单理解 (不准确)并行遍历所有 cluster, 就是对于每个不同层级的 cluster ,只有其父节点误差>threadhold, 且其本身节点误差<=thread 会被渲染
        - `self error < threadhold < father error`
    - 还是得用 BVH 加速
        - 为每一层级的所有 Group 构建一个 BVH tree (这是空间结构吗? BVH 的作用是什么? #TODO ), 然后把这些 gruop 连到同一个根节点
          个人理解:
          可能就是为不同层级构建了一颗树, 根节点能根据误差, 快速选择最合适的子树, 这样就不用遍历其它的子树.
          对于每个子树, 其内部的节点可以说就是一个平坦的数组. 如果不是数组, 而是子树, 那么子树的作用是什么? #疑问
          ![](assets/Nanite%20games104%20学习-7.png)
        - 只判断那个误差和定下的 threadhold 相近的

#### 运行时 Cluster 剔除

- 再用 GPU 上的 JobSystem 加速 ***这个处理的是 cluster 剔除***
    - FIFO 队列
    - 每个线程既是生产者也是消费者
    - 每个线程总是队列中拿出一个节点进行判断
        - 如果一个节点 通过了剔除, 则将所有子节点加入队列, 
        - 没通过就丢弃当前节点
        - 结束线程工作
    - 就保证了各个线程之间的处理时长大致相同。
    - **技术细节不是很理解**

### Rendering

- Rasterization
    - 由于一个三角形对于一个像素
    - 而主流的光栅化是 tile based 的
    - 会有很多无效的计算 
    - 自己写一个 computeshader 进行光栅化
        - Nanite [Visibility Buffer](Visibility%20Buffer.md)
        - 一部分大三角形走传统光栅化
        - 另一部分小三角形走 Nanite 光栅化
    - 对于一些远距离物体, 可以用 Imposter 替代
- 材质
    - 最初版本, 每个材质都要进行一次全屏 pass
        - 但是材质动辄成千上万, 对应了不同的 shading code
    - 后来, 把屏幕分成很多 Tile,
        - 使用 compute shader 计算每个 Tile 内的材质范围 (材质对应深度, 即深度范围)
        - 若当前材质的Material ID处于Range内，则继续执行材质的PS；否则表示当前块内没有像素使用该材质，则整块可以剔除，此时只需将VS的顶点位置设置为NaN，GPU就会将对应的三角形剔除。
        - 这样虽然仍然每种材质需要执行一次全屏 pass, 但只有有当前材质的 Tile 进入了 pixel shader, 大大减少了 OverDraw
    - 借助硬件的**Early Z/Stencil完成了逐像素的材质ID剔除**
        - 只有当前像素的Material ID和待绘制的材质ID相同（Depth Test Pass) . Compare Function设置为Equal。
        - 且该像素为Nanite Mesh（Stencil Test Pass）
        - 才会真正执行PS

### Virtual Shadow Map

[[Virtual Shadow Map]]

### Streaming and Compression
