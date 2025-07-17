# Java并发

# Java并发
## 基础
相关阅读：[Java 并发常见面试题总结（上） - JavaGuide -2022](https://javaguide.cn/java/concurrent/java-concurrent-questions-01.html)

### 什么是线程和进程?线程与进程的关系,区别及优缺点？⭐⭐⭐⭐
💡提示：可以从从 JVM 角度说进程和线程之间的关系

### 为什么要使用多线程呢?⭐⭐⭐
💡提示：从计算机角度来说主要是为了充分利用多核 CPU 的能力，从项目角度来说主要是为了提升系统的性能。

### 说说线程的生命周期和状态?⭐⭐⭐⭐
💡提示： 6 种状态（NEW、RUNNABLE、BLOCKED、WAITING、TIME_WAITING、TERMINATED）。

🌈拓展：在操作系统中层面线程有READY和RUNNING状态，而在 JVM 层面只能看到RUNNABLE状态。

### 什么是线程死锁?如何避免死锁?如何预防和避免线程死锁?⭐⭐⭐⭐
💡提示： 这里最好能够结合代码来聊，你要确保自己可以写出有死锁问题的代码。

🌈拓展：项目中遇到死锁问题是比较常见的，除了要搞懂上面这些死锁的基本概念之外，你还要知道线上项目遇到死锁问题该如何排查和解决。

## 乐观锁和悲观锁
相关阅读：[乐观锁和悲观锁详解 - JavaGuide -2022](https://javaguide.cn/java/concurrent/optimistic-lock-and-pessimistic-lock.html)

### 乐观锁和悲观锁的区别⭐⭐⭐⭐⭐
💡提示：乐观锁和悲观锁的最终目的都是为了保证线程安全，避免在并发场景下的资源竞争问题，但乐观锁不会真的去加锁。

### 如何实现乐观锁？⭐⭐⭐⭐
💡提示：乐观锁一般会使用版本号机制或 CAS 算法实现，CAS 算法相对来说更多一些，这里需要格外注意。

### CAS了解么？原理？⭐⭐⭐⭐⭐
💡提示：多地方都用到了CAS比如ConcurrentHashMap采用CAS和synchronized来保证并发安全，再比如java.util.concurrent.atomic包中的类通过volatile+CAS重试保证线程安全性。和面试官聊CAS的时候，你可以结合CAS的一些实际应用来说。

### 乐观锁存在哪些问题？⭐⭐⭐
💡提示： ABA 问题、循环时间长开销大、只能保证一个共享变量的原子操作

### 什么是 ABA 问题？ABA 问题怎么解决？⭐⭐⭐⭐
💡提示： 所谓 ABA 问题，就是一个值原来是 A，变成了 B，又变回了 A。这个时候使用 CAS 是检查不出变化的，但实际上却被更新了两次。ABA 问题的解决思路是在变量前面追加上版本号或者时间戳。从 JDK 1.5 开始，JDK 的 atomic 包里提供了一个类AtomicStampedReference类来解决 ABA 问题。

## JMM
相关阅读：[JMM（Java 内存模型）详解 - JavaGuide -2022](https://javaguide.cn/java/concurrent/jmm.html)

### 并发编程的三个重要特性⭐⭐⭐⭐⭐
💡提示： 原子性、可见性、有序性

### 什么是 JMM？为什么需要 JMM？⭐⭐⭐⭐⭐
💡提示：对于 Java 来说，你可以把 JMM 看作是 Java 定义的并发编程相关的一组规范，除了抽象了线程和主内存之间的关系之外，其还规定了从 Java 源代码到 CPU 可执行指令的这个转化过程要遵守哪些和并发相关的原则和规范，其主要目的是为了简化多线程编程，增强程序可移植性的。

相关阅读：[JMM（Java 内存模型）详解](https://javaguide.cn/java/concurrent/jmm.html)

### JMM 是如何抽象线程和主内存之间的关系？⭐⭐⭐⭐⭐
💡提示：Java 内存模型的抽象示意图如下：

![1732497954274-4a652a5d-ca74-48ed-98ef-881b61a5db9f.png](./img/6fjIVIykb8fuefZz/1732497954274-4a652a5d-ca74-48ed-98ef-881b61a5db9f-058392.png)

### Java 内存区域和 JMM 有何区别？⭐⭐⭐⭐
💡提示： Java 内存区域和内存模型是完全不一样的两个东西。

### happens-before 原则是什么？为什么需要 happens-before 原则？
💡提示：happens-before 原则的诞生是为了程序员和编译器、处理器之间的平衡。程序员追求的是易于理解和编程的强内存模型，遵守既定规则编码即可。编译器和处理器追求的是较少约束的弱内存模型，让它们尽己所能地去优化性能，让性能最大化。

## synchronized 和 volatile
### synchronized关键字⭐⭐⭐⭐⭐
💡提示：synchronized关键字几乎是面试必问，你需要搞懂下面这些synchronized关键字相关的问题：

+ synchronized关键字的作用，自己是怎么使用的。
+ synchronized关键字的底层原理（重点！！！）
+ JDK1.6 之后的synchronized关键字底层做了哪些优化。synchronized锁升级流程。
+ synchronized和ReentrantLock的区别。
+ synchronized和volatile的区别。

### volatile关键字⭐⭐⭐⭐⭐
💡提示：volatile关键字同样是一个重点！结合JMM（Java Memory Model，Java 内存模型）来回答就行了。

## ThreadLocal
### ThreadLocal类⭐⭐⭐⭐⭐
💡提示：关注ThreadLocal的底层原理、内存泄露问题以及自己是如何在项目中使用ThreadLocal关键字的。

## 线程池
相关阅读：[Java 线程池详解 - JavaGuide -2022](https://javaguide.cn/java/concurrent/java-thread-pool-summary.html)。

### 线程池⭐⭐⭐⭐⭐
💡提示：线程池有哪几种，各种线程池的优缺点，线程池的重要参数、线程池的执行流程、线程池的饱和策略、如何设置线程池的大小等等。

## AQS
相关阅读：[AQS 详解 - JavaGuide -2022](https://javaguide.cn/java/concurrent/aqs.html)。

### AQS 是什么？AQS 的原理是什么？⭐⭐⭐⭐⭐
💡提示： AQS 的全称为AbstractQueuedSynchronizer，翻译过来的意思就是抽象队列同步器。这个类在java.util.concurrent.locks包下面。AQS 核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制 AQS 是用CLH 队列锁实现的，即将暂时获取不到锁的线程加入到队列中。

### Semaphore 有什么用？Semaphore 的原理是什么？⭐⭐⭐⭐
💡提示：Semaphore是共享锁的一种实现，它默认构造 AQS 的state值为permits，你可以将permits的值理解为许可证的数量，只有拿到许可证的线程才能执行。

### CountDownLatch 有什么用？CountDownLatch 的原理是什么？用过 CountDownLatch 么？什么场景下用的？⭐⭐⭐⭐
💡提示：CountDownLatch允许count个线程阻塞在一个地方，直至所有线程的任务都执行完毕，一次性的。

### CyclicBarrier 有什么用？CyclicBarrier 的原理是什么？⭐⭐⭐
💡提示：CyclicBarrier和CountDownLatch非常类似，它也可以实现线程间的技术等待，但是它的功能比CountDownLatch更加复杂和强大。主要应用场景和CountDownLatch类似。



> 更新: 2024-01-03 01:43:07  
原文: [https://www.yuque.com/vip6688/neho4x/gnn7hrgerc6c3vit](https://www.yuque.com/vip6688/neho4x/gnn7hrgerc6c3vit)
>



> 更新: 2024-11-25 09:25:54  
> 原文: <https://www.yuque.com/neumx/laxg2e/69b8f64c03b6069084875670726e890b>