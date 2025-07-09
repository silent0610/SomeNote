---
Type:
  - Jotting
aliases: 
tags: 
Status: 
modifiedDate: 2025/07/05, 18:03:46
---

# Nanite的Cluster剔除和LoD选择顺序疑问

首先, 他们肯定都有一个 BVH 结构
应该是这样吗
根节点, 存储整个模型的 包围盒
叶子结点, 存储每个 层级为 0 的 cluster 的包围盒
中间节点, 也是Cluster, 每个 Cluster 存储指针指向下一层级的所有与它相关的 Cluster
每个层级的所有 cluster 都能构成完整模型

个人理解
- 运行时
    - 将每个可见模型 (没被视锥剔除) 的根节点加入队列
    - 取队首节点
    - 判断该节点包围盒, 如果不被剔除 (视锥等)
        - 根据距离确定误差 threshold
        - 如果 threshold 满足, 结束, 当前 cluster 需要被渲染
        - 否则, 将所有子节点加入队列
- 但是 104 说 BVH 结构是这样的, 不同层级的节点构成一颗颗树, 统一连到根节点上,
- 然后先根据误差判断应该选哪棵子树
- 那这样一个模型被选择的 Cluster不都是一个 LoD 了吗? #疑问
- 每颗子树的中间节点又是什么?
 ![](assets/Nanite%20games104%20学习-7.png)
