---
Type:
  - Page
aliases: 
Status:
tags: 
modifiedDate: 星期一, 五月 26日 2025, 8:57:57 晚上
---
[Vulkan](Vulkan.md) 是一个显式、低级的图形 API 中. 正确地管理 GPU 和 CPU 之间的同步以及 GPU 内部操作的顺序和数据可见性是至关重要的。`Semaphore`、`Fence` 和 `Barrier` 就是实现这些同步的核心工具。

- [VkSemaphore](VkSemaphore.md)
	- GPU 内的同步
- [VkFence](VkFence.md)
	- GPU 和 CPU 之间同步
- [VkBarrier](VkBarrier.md)
	- GPU 内部的细粒度操作顺序和内存可见性同步，尤其是图像布局转换。
