---
Type: 
aliases:
  - 动态漫反射全局光照
  - Dynamic Diffuse Global Illumination
tags:
  - 没看懂
  - TODO
modifiedDate: 2025/06/24, 12:48:25
---

# DDGI

是一种**基于[光照探针](Light%20Probe.md)的动态全局光照技术**，可以在**动态的场景**与光照中**实时的**生成漫反射的全局光照。
完整的DDGI算法依赖于实时光线追踪的支持。但如果只需要支持静态场景与动态光源的话，也可以将光追提前到预计算阶段从而能在低端设备上运行。
主要分为两个阶段：

1. 生成光照探针数据：每个probe都会存储该位置上所接收到的光照信息。（对空间中连续光场函数的采样）
2. 使用光照探针进行着色：查找该着色点周围的若干个probe，并将这些probe中存储的光照信息插值就可以得到着色点处的光照信息。（将采样得到的离散信号通过插值复原成原始的连续信号）

> 一种实时更新的光照探针（Irradiance Probe）技术

最好参见[动态漫反射全局光照（Dynamic Diffuse Global Illumination） - 知乎](https://zhuanlan.zhihu.com/p/404520592)

## 基本概念

**DDGI Probe**：每个probe中存储了该位置上的光照信息，包含三个关于方向的函数

1. E(w)：从w方向所接收到的irradiance信息
2. r(w)：w方向上，物体距离probe的距离 (半球方向均值)
3. $r^2(w)$：w方向上，物体距离probe距离的平方 (半球方向均值)

## DDGI数据结构

### DDGI Probe

- 使用[八面体映射](https://zhida.zhihu.com/search?content_id=178023513&content_type=Article&match_order=1&q=%E5%85%AB%E9%9D%A2%E4%BD%93%E6%98%A0%E5%B0%84&zhida_source=entity)
    - 八面体映射的效率高，分布更均匀，且误差更低。
- irradiance被编码为6x6的三维向量纹理，r(w)和r2(w)被一起编码进14x14的二维向量纹理（x分量存储 r(w) ，y分量存储 r2(w) ）。[^1]

### DDGI Volume

是一组DDGI Probe的集合
横轴为8x8=64个probe，纵轴为8个probe，每个probe占有8x8个像素，合计纹理大小为512x64 #没看懂 

## DDGI使用方法

### 基于实时光线追踪的DDGI Volume

Volume可以跟随摄像机的移动而移动，所以我们可以在摄像机上绑定多个大小不一的Volume进行级联，体积越小的Volume的精度越高。在渲染的时候对于近处的物体就可以采用高精度Volume进行渲染，而对于远处的物体则采用低精度的Volume进行渲染。

### 基于预计算的DDGI Volume

预计算场景的几何信息，并在运行阶段实时更新光照信息。由于几何信息是预计算的，所以我们不能去移动Volume的位置，所以无法将Volume绑定在摄像机上。

对于预计算方案来说，我们首先需要摆放一个大号的Volume来囊括整个场景，然后对于场景中的每一个房间都放置一个Volume。调节每个volume的权重。

## 使用DDGI Volume进行渲染

$$
\begin{aligned} L_{\text {diff }}\left(w_{o}\right) & =f_{\text {diff }}\left(w_{i}, w_{o}\right) \int_{\Omega} L\left(w_{i}\right) \cos \theta d w_{i} \\ & =\frac{\rho}{\pi} E\left(n_{i}\right)\end{aligned}
$$

DDGI只计算漫反射光照。E(ni)E(n_{i})E(ni​)可以通过周围8个probe中存储的E(ni)E(n_{i})E(ni​)来进行加权求和，在计算每个probe权重的时候，会使用三个权重系数（最后需要对权重进行归一化）：

1. 三线性插值系数：如果probe距离着色点较远，则降低该probe的权重
2. 方向系数：如果着色点法线与着色点到probe方向的夹角过大，则降低probe的权重
3. 切比雪夫系数：如果着色点到probe之间有较大的概率存在遮挡物，则降低probe的权重。

## 生成DDGI Volume

生成一个probe就是计算三项函数的积分，如果直接使用RTX硬件来追踪一次弹射的路径，噪声较大且只能获得一次间接光。DDGI中采用了另一种方法：

1. 从每个probe处发射若干条光线与场景相交，记录交点信息。
2. 将交点信息整合进一个类似G-buffer的贴图中，使用延迟渲染来计算交点处的直接光照；使用上一帧的DDGI Volume来计算交点的间接光照。最终的计算结果就是该光线的radiance。
3. 利用b中的计算结果，使用蒙特卡洛方法来更新probe。

### 发射光线 ：

从每个probe的位置出发，进行均匀球面采样（斐波那契螺旋），记录交点的世界空间position，normal，albedo，分别存储三张纹理中。

### 计算radiance ：

将上述步骤中的纹理作为G-buffer，送入延迟渲染管线中，进行一次延迟光照（直接光照）。

使用当前的DDGI Volume进行间接光照。将直接光照和间接光照的结果存入一个radiance贴图中。

### Probe混合

使用radiance贴图来更新probe中的irradiance，即 $E(w)=\frac{\sum L_{i} \cos \theta_{i}}{\sum \cos \theta_{i}}$

使用position贴图中来更新probe中的depth，即：

 $$
\begin{aligned} r(w) & =\frac{\sum d_{i} \cos ^{s} \theta_{i}}{\sum \cos ^{s} \theta_{i}} \\ r^{2}(w) & =\frac{\sum d_{i}^{2} \cos ^{s} \theta_{i}}{\sum \cos ^{s} \theta_{i}}\end{aligned}
$$

其中s是一个经验性的sharpness系数， 越大则偏离纹素方向的采样点占的比重就越小，一般取50。

### 时间超采样 ：

降低每帧的光线采样率，并复用历史帧的采样结果，从而获取更优的性能。在该情况下，每个probe只需要采样64根光线就可以拥有不错的结果。

在计算出当前帧的ProbeBlend的结果后，将其与上一帧的结果按某一比例进行插值即可，尽管我们这里仅仅是简单的将当前帧与上一帧进行混合，但上一帧的结果本身也包括了更早的数据。

[^1]: 所以一共是两张纹理, Cubemap 是 6 张
