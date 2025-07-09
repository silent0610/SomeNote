---
Type:
aliases: 
tags: 
modifiedDate: 2025/06/16, 09:33:11
---

# SDFDDGI 的探针定位

[[DepthSDF]] 的探针更新主体参考这个

## 步骤

- **获取原始探针位置：** 算法首先获取探针在统一网格中的原始位置 `pos` 。
- **查询 SDF 值：** 在 `pos` 处查询 SDF（符号距离场）的值 。
- **判断是否需要调整：** 如果查询到的 SDF 值**小于一个预设的阈值 `threshold1`**[^1]
    - **执行梯度下降：** 在这种情况下，算法会查询 SDF 的梯度，并使用**梯度下降法**来调整 `pos`，以获得一个**可接受的、远离原始位置附近的采样点** 。[^2]
- **稳定性检查：** 算法还会检查当前帧调整后的探针位置 `pos` 与上一帧的探针位置 `lastPos` 之间的距离。如果这个距离**超过了另一个阈值 `threshold2`**，那么上一帧的辐照度数据会被“拒绝”，并且会为该探针分配更多的光线来进行采样，以确保更稳定的结果 。
- **返回调整后的位置：** 最后，算法返回调整后的探针位置 `pos` 。

![[assets/20250609-1.png|300]]

```python
procedure UPDATEPROBEPOS
    1: lastPos ← position of probe in last frame  // 上一帧的探针位置
    2: pos ← position of probe in the uniform grid // 当前帧在统一网格中的探针初始位置

    3: if querySdf(pos) < threshold1 then  // 如果探针离物体太近（SDF值小于阈值1）
    4:     pos ← gradientDescent(SDF, pos) // 使用梯度下降法调整探针位置，使其远离物体
    5: end if

    6: if distance(pos, lastPos) > threshold2 then // 如果当前调整后的位置与上一帧的位置变化超过阈值2
    7:     markRejectHistory(probe) // 标记该探针的历史数据为“已拒绝”（需要分配更多光线更新）
    8: end if
    9: return pos // 返回调整后的探针位置
```

[^1]: 这意味着探针离物体太近，甚至可能在物体内部 。这种位置会对采样质量产生负面影响，并可能导致漏光
[^2]: ⭐ [[DepthSDF]] 可以用一个简单的神经网络
