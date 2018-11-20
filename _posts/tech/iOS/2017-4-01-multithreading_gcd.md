---
layout: post
title: 多线程编程（二）-- GCD
category: 技术
tags:
description:
---

苹果公司的 Cocoa 框架共支持三种多线程机制，分别为 NSThread、GCD（Grand Central Dispatch）、 NSOperation。

# GCD ( Grand Central Dispatch )

* 底层API
* 提供了一种新的方法来进行并发程序编写
* 事件控制系统
* 不是Cocoa 框架的一部分
* 开源

源码：https://opensource.apple.com/tarballs/libdispatch/

* Serial vs. Concurrent   串行 vs. 并发


* Synchronous vs. Asynchronous 同步 vs. 异步
* Deadlock 死锁
* Critical Section 临界区
* Race Condition 竞态条件
* Thread Safe 线程安全
* Priority Inversion 优先级反转
* Context Switch 上下文切换
* Concurrency vs Parallelism 并发与并行


# 常用 API

* 创建管理 Queue
* 提交 job
* Dispatch Group
* 管理 Dispatch Object
* Dispatch Block
* 信号量 Semaphore
* 队列屏障 Barrier
* Dispatch Source
* Queue Context 数据
* Dispatch I/O Channel
* Dispatch Data 对象

# 高级特性与新特性

# 源码剖析

# 参考资料

# 思考题

