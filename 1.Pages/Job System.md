---
Type:
  - Page
aliases:
  - 任务系统
Status:
tags: 
modifiedDate: 星期一, 五月 26日 2025, 8:58:10 晚上
---
与[[线程池]]十分相似

## 定义

Job System 是一种轻量级的多线程任务调度框架，它将任务分解为多个小工作单元（Job），然后由多个工作线程（Worker Threads）从任务队列中提取执行，达到并行处理的目的。

## 核心组件

- Job
- Worker Thread
- Job Queue

## 一些细节

### 如何减少堵塞，如何在没有任务时挂起线程

- 使用  `std::condition_variable`
- 每个 worker thread 在**无任务时进入挂起状态（wait）**这里使用 lambda 函数简化;
- 当有新任务进入队列时，**主线程或任务提交线程调用 `notify_one` 或 `notify_all` 唤醒线程**；
