---
Type:
  - Page
aliases: 
tags: 
modifiedDate: 星期三, 六月 4日 2025, 9:51:30 晚上
---
- 一个 warp 内 有多个线程
- 一个 SM 内有一个处理器 (执行流水线)
- 一个完整的 GPU 包含多个 SM 流式多处理器 (Streaming Multiprocessor, SM，NVIDIA 术语) 
- 每个 SM/CU 都有自己的执行流水线（处理指令的硬件）。
- SM 之间并行
- 一个 SM 内的 Warp 间是串行的
