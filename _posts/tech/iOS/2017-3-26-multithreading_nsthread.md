---
layout: post
title: 多线程编程（一）-- NSThread
category: 技术 
tags:
description:
---

苹果公司的 Cocoa 框架共支持三种多线程机制，分别为 NSThread、GCD（Grand Central Dispatch）、 NSOperation。

# NSThread

> [POSIX Threads](https://en.wikipedia.org/wiki/POSIX_Threads)，通常会叫做 pthreads，是一种独立于语言系统的执行模式。NSThread 是在 pthread 之上封装的类，在 GCD 与 NSOperation 出来之前，它作为 iOS 开发多线程编程唯一的解决方案。

### 创建线程

NSThread 有两种直接创建线程的方式：

第一个是实例方法--直接创建线程并且开始运行线程

```objective-c
NSThread* myThread = [[NSThread alloc] initWithTarget:self
                                          selector:@selector(myThreadMainMethod:)
                                            object:nil];
[myThread start];  // Actually create the thread
```

第二个是类方法--先创建线程对象，然后再运行线程操作，在运行线程操作前可以设置线程的优先级等线程信息，但这个 selector 只能有一个参数，而且不能有返回值。

```objective-c
[NSThread detachNewThreadSelector:@selector(myThreadMainMethod:) toTarget:self withObject:nil];
```

另外使用任意 NSObject 实例对象也可以创建线程

```objective-c
// NSObject 实例方法创建一个后台线程,这种方法与 NSThread 的 detachNewThreadSelector:toTarget:withObject: 方法类似
[self performSelector:@selector(dispatched) withObject:nil];
```

### 设置线程属性

##### 设置堆栈大小 （setStackSize）

创建线程时系统已经为它分配了一定的内存空间作为栈，栈用于管理栈帧(Stack frame，是一个为函数保留的区域，用来存储关于参数、局部变量和返回地址的信息。堆栈帧通常是在新的函数调用的时候创建，并在函数返回的时候销毁。

调用栈（Call stack）就是指存放某个程序的正在运行的函数的信息的栈。Call stack 由 stack frames 组成，每个 stack frame 对应于一个未完成运行的函数。

如果想改变线程栈的大小，必须要在创建线程之前设置。 也就是说，使用 NSThread 创建一个实例对象，必须在调用 start 方法之前使用 setStackSize: 方法设置它栈的大小。

举 🌰

```objective-c
- (void)🌰
{
   NSThread *thread = [[NSThread alloc] initWithTarget:self selector:@selector(threadWithStackSize) object:nil];
   // 如果不设置栈区内存大小限制，会有栈区内存溢出，初始为 512KB
   [thread setStackSize:2048000];
   [thread start];
}

- (void)threadWithStackSize
{
    // 因为没有设置指针 * ，所以当前数组为栈数组
    int array[1024000];
    array[1] = 100;
}
```

##### 配置线程本地变  (threadDictionary)

线程拥有一个字典（mutable），在线程的整个生命周期中，你可以使用该字典保存信息，NSThread 提供了 *threadDictionary* 方法获取该字典。

##### 设置线程优先级 (setThreadPriority 0~1)

任何新线程都有一个与之关联的默认优先级，内核调度算法在决定该运行哪个线程时，会把线程的优先级作为考量因素，较高优先级的线程会比较低优先级的线程具有更多的运行机会。较高优先级不保证你的线程具体执行的时间，只是相比较低优先级的线程，它更有可能被调度器选择执行而已。使用 NSThread 的 setThreadPriority: 方法设置线程优先级。

苹果建议使用线程默认的优先级，提高线程的优先级会增加低优先级线程饥饿的可能性。如果应用程序中包含高、低优先级线程交互的情况，饥饿的低优先级线程可能会阻塞其它线程，造成性能瓶颈。
