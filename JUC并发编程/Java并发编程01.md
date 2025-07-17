# Java并发编程01

# Java并发编程01
## <font style="color:#000000;">什么是线程和进程?</font>
### <font style="color:#000000;">1.1 何为进程?</font>
<font style="color:#000000;">进程是程序的一次执行过程，是系统运行程序的基本单位，因此进程是动态的。系统运行一个程序即是一个进程从创建，运行到消亡的过程。</font>

<font style="color:#000000;">在 Java 中，当我们启动 main 函数时其实就是启动了一个 JVM 的进程，而 main 函数所在的线程就是这个进程中的一个线程，也称主线程。</font>

<font style="color:#000000;">如下图所示，在 Windows 中通过查看任务管理器的方式，我们就可以清楚看到 Windows 当前运行的进程（</font><font style="color:#000000;">.exe</font><font style="color:#000000;">文件的运行）。</font>

![1732497473586-37ace30b-8621-43ef-9adb-9c54f0501070.png](./img/LdNyK5cE02eRr3_k/1732497473586-37ace30b-8621-43ef-9adb-9c54f0501070-676224.png)

<font style="color:#000000;">进程示例图片-Windows</font>

### <font style="color:#000000;">1.2 何为线程?</font>
<font style="color:#000000;">线程与进程相似，但线程是一个比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程共享进程的</font>**<font style="color:#000000;">堆</font>**<font style="color:#000000;">和</font>**<font style="color:#000000;">方法区</font>**<font style="color:#000000;">资源，但每个线程有自己的</font>**<font style="color:#000000;">程序计数器</font>**<font style="color:#000000;">、</font>**<font style="color:#000000;">虚拟机栈</font>**<font style="color:#000000;">和</font>**<font style="color:#000000;">本地方法栈</font>**<font style="color:#000000;">，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。</font>

<font style="color:#000000;">Java 程序天生就是多线程程序，我们可以通过 JMX 来看看一个普通的 Java 程序有哪些线程，代码如下。</font>

```java
public class MultiThread {
    public static void main(String[] args) {
        // 获取 Java 线程管理 MXBean
        ThreadMXBean threadMXBean = ManagementFactory.getThreadMXBean();
        // 不需要获取同步的 monitor 和 synchronizer 信息，仅获取线程和线程堆栈信息
        ThreadInfo[] threadInfos = threadMXBean.dumpAllThreads(false, false);
        // 遍历线程信息，仅打印线程 ID 和线程名称信息
        for (ThreadInfo threadInfo : threadInfos) {
            System.out.println("[" + threadInfo.getThreadId() + "] " + threadInfo.getThreadName());
        }
    }
}
```

<font style="color:#000000;">上述程序输出如下（输出内容可能不同，不用太纠结下面每个线程的作用，只用知道 main 线程执行 main 方法即可）：</font>

```java
[5] Attach Listener //添加事件
[4] Signal Dispatcher // 分发处理给 JVM 信号的线程
[3] Finalizer //调用对象 finalize 方法的线程
[2] Reference Handler //清除 reference 线程
[1] main //main 线程,程序入口
```

<font style="color:#000000;">从上面的输出内容可以看出：</font>**<font style="color:#000000;">一个 Java 程序的运行是 main 线程和多个其他线程同时运行</font>**<font style="color:#000000;">。</font>

## <font style="color:#000000;">Java 线程和操作系统的线程有啥区别？</font>
<font style="color:#000000;background-color:#d9eafc;">JDK 1.2 之前</font><font style="color:#000000;">，Java 线程是基于绿色线程（Green Threads）实现的，这是一种用户级线程（用户线程），也就是说 </font>**<font style="color:#000000;background-color:#d9eafc;">JVM 自己模拟了多线程的运行，而不依赖于操作系统</font>**<font style="color:#000000;">。由于绿色线程和原生线程比起来在使用时有一些限制（比如绿色线程不能直接使用操作系统提供的功能如异步 I/O、只能在一个内核线程上运行无法利用多核），在 </font>**<font style="color:#000000;background-color:#cef5f7;">JDK 1.2 及以后</font>**<font style="color:#000000;">，</font>**<font style="color:#000000;">Java 线程改为基于原生线程（Native Threads）实现</font>**<font style="color:#000000;">，也就是说 </font>**<font style="color:#000000;background-color:#cef5f7;">JVM 直接使用操作系统原生的内核级线程（内核线程）来实现 Java 线程</font>**<font style="color:#000000;">，由操作系统内核进行线程的调度和管理。</font>

<font style="color:#000000;">我们上面提到了用户线程和内核线程，考虑到很多读者不太了解二者的区别，这里简单介绍一下：</font>

+ <font style="color:#000000;">用户线程：由用户空间程序管理和调度的线程，运行在用户空间（</font>**<font style="color:#000000;">专门给应用程序使用</font>**<font style="color:#000000;">）。</font>
+ <font style="color:#000000;">内核线程：由操作系统内核管理和调度的线程，运行在内核空间（</font>**<font style="color:#000000;">只有内核程序可以访问</font>**<font style="color:#000000;">）。</font>

<font style="color:#000000;">顺便简单总结一下用户线程和内核线程的区别和特点：</font>**<font style="color:#000000;background-color:#d9eafc;">用户线程创建和切换成本低，但不可以利用多核</font>**<font style="color:#000000;">。</font>**<font style="color:#000000;background-color:#cef5f7;">内核态线程，创建和切换成本高，可以利用多核</font>**<font style="color:#000000;">。</font>

<font style="color:#000000;">一句话概括 Java 线程和操作系统线程的关系：</font>**<font style="color:#000000;">现在的 Java 线程的本质其实就是操作系统的线程</font>**<font style="color:#000000;">。</font>

<font style="color:#000000;">线程模型是用户线程和内核线程之间的关联方式，常见的线程模型有这三种：</font>

1. <font style="color:#000000;">一对一（一个用户线程对应一个内核线程）</font>
2. <font style="color:#000000;">多对一（多个用户线程映射到一个内核线程）</font>
3. <font style="color:#000000;">多对多（多个用户线程映射到多个内核线程）</font>

![1732497473706-893004b6-c7d0-4e58-96e0-9a6d2b7a410d.png](./img/LdNyK5cE02eRr3_k/1732497473706-893004b6-c7d0-4e58-96e0-9a6d2b7a410d-734567.png)

**<font style="color:#000000;">常见的三种线程模型</font>**

<font style="color:#000000;">在 Windows 和 Linux 等主流操作系统中，Java 线程采用的是一对一的线程模型，也就是一个 Java 线程对应一个系统内核线程。Solaris 系统是一个特例（Solaris 系统本身就支持多对多的线程模型），HotSpot VM 在 Solaris 上支持多对多和一对一。具体可以参考 R 大的回答:</font>[<font style="color:#000000;">JVM 中的线程模型是用户级的么？</font>](https://www.zhihu.com/question/23096638/answer/29617153)<font style="color:#000000;">。</font>

<font style="color:#000000;">虚拟线程在 JDK 21 顺利转正，关于虚拟线程、平台线程（也就是我们上面提到的 Java 线程）和内核线程三者的关系可以阅读我写的这篇文章：</font>[<font style="color:#000000;">Java 20 新特性概览</font>](https://javaguide.cn/java/new-features/java20.html)<font style="color:#000000;">。</font>

## <font style="color:#000000;">请简要描述线程与进程的关系,区别及优缺点？</font>
<font style="color:#000000;">从 JVM 角度说进程和线程之间的关系。</font>

### <font style="color:#000000;">3.1 图解进程和线程的关系</font>
<font style="color:#000000;">下图是 Java 内存区域，通过下图我们从 JVM 的角度来说一下线程和进程之间的关系。</font>

![1732497473795-6ab96892-f39f-4c47-af57-85e56215bb5f.png](./img/LdNyK5cE02eRr3_k/1732497473795-6ab96892-f39f-4c47-af57-85e56215bb5f-171337.png)

<font style="color:#000000;">Java 运行时数据区域（JDK1.8 之后）</font>

<font style="color:#000000;">从上图可以看出：一个进程中可以有多个线程，多个线程共享进程的</font>**<font style="color:#000000;">堆</font>**<font style="color:#000000;">和</font>**<font style="color:#000000;">方法区 (JDK1.8 之后的元空间)</font>****<font style="color:#000000;">资源，但是每个线程有自己的</font>****<font style="color:#000000;">程序计数器</font>**<font style="color:#000000;">、</font>**<font style="color:#000000;">虚拟机栈</font>**<font style="color:#000000;">和</font>**<font style="color:#000000;">本地方法栈</font>**<font style="color:#000000;">。</font>

**<font style="color:#000000;">总结：</font>************<font style="color:#000000;">线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。线程执行开销小，但不利于资源的管理和保护；而进程正相反。</font>**

<font style="color:#000000;">下面是该知识点的扩展内容！</font>

<font style="color:#000000;">下面来思考这样一个问题：为什么</font>**<font style="color:#000000;">程序计数器</font>**<font style="color:#000000;">、</font>**<font style="color:#000000;">虚拟机栈</font>**<font style="color:#000000;">和</font>**<font style="color:#000000;">本地方法栈</font>**<font style="color:#000000;">是线程私有的呢？为什么堆和方法区是线程共享的呢？</font>

### <font style="color:#000000;">3.2 程序计数器为什么是私有的?</font>
<font style="color:rgb(31, 35, 40);">每个线程都有独立的执行路径，程序计数器记录当前线程正在执行的Java方法的下一条指令地址。因为CPU时间片轮转给不同线程时，需要恢复到该线程上次离开的位置继续执行，所以每个线程都需要有一个独立的程序计数器。</font>

<font style="color:#000000;">程序计数器主要有下面两个作用：</font>

1. <font style="color:#000000;">字节码解释器通过改变程序计数器来依次读取指令，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。</font>
2. <font style="color:#000000;">在多线程的情况下，程序计数器用于记录当前线程执行的位置，从而当线程被切换回来的时候能够知道该线程上次运行到哪儿了。</font>

<font style="color:#000000;">需要注意的是，如果执行的是 native 方法，那么程序计数器记录的是 undefined 地址，只有执行的是 Java 代码时程序计数器记录的才是下一条指令的地址。</font>

<font style="color:#000000;">所以，</font><font style="color:#000000;background-color:#cef5f7;">程序计数器私有主要是为了</font>**<font style="color:#000000;background-color:#cef5f7;">线程切换后能恢复到正确的执行位置</font>**<font style="color:#000000;">。</font>

### <font style="color:#000000;">3.3 虚拟机栈和本地方法栈为什么是私有的?</font>
+ **<font style="color:#000000;">虚拟机栈：</font>**<font style="color:rgb(31, 35, 40);"> 虚拟机栈用于存储方法调用时产生的局部变量表、操作数栈、动态链接等信息。由于每个线程在运行过程中都会创建各自的栈帧来保存这些数据，因此每一个线程都必须拥有自己的栈空间以保证线程间的数据隔离，避免相互干扰。</font>
+ **<font style="color:#000000;">本地方法栈：</font>**<font style="color:rgb(31, 35, 40);">类似于虚拟机栈，本地方法栈主要用于支持native方法的执行。每个线程调用本地方法时同样会产生与之相关的状态信息，为了保持数据独立性和安全性，这部分区域也是线程私有的。</font>

<font style="color:#000000;">所以，</font><font style="color:#000000;background-color:#cef5f7;">为了</font>**<font style="color:#000000;background-color:#cef5f7;">保证线程中的局部变量不被别的线程访问到</font>**<font style="color:#000000;background-color:#cef5f7;">，虚拟机栈和本地方法栈是线程私有的</font><font style="color:#000000;">。</font>

### <font style="color:#000000;">3.4 一句话简单了解堆和方法区</font>
1. **<font style="color:rgb(31, 35, 40);">堆</font>**<font style="color:rgb(31, 35, 40);">（Heap）： 堆内存是所有对象实例化的地方，多个线程可能会访问同一个对象或对象集合。如果堆是线程私有的，那么对象就不能在多个线程间共享，这显然不符合多线程编程中资源共享的需求。另外，垃圾回收机制也需要全局地管理整个堆内存，确保所有线程都能够正确识别和处理不再使用的对象。</font>
2. **<font style="color:rgb(31, 35, 40);">方法区</font>**<font style="color:rgb(31, 35, 40);">（Method Area，或者称为元空间Metaspace）： 方法区主要存储已加载的类信息、常量池、静态变量以及即时编译器编译后的代码等数据。这些数据通常是全局性的，与特定线程无关，多个线程需要访问相同的类信息和静态变量，因此方法区也应该是线程共享的。此外，类的生命周期远长于方法调用，它们无需在线程切换时进行频繁的上下文切换和数据同步。</font>

<font style="color:#000000;">堆和方法区是所有线程共享的资源，其中</font>**<font style="color:#000000;background-color:#cef5f7;">堆是进程中最大的一块内存，主要用于存放新创建的对象</font>**<font style="color:#000000;"> (几乎所有对象都在这里分配内存)，</font>**<font style="color:#000000;background-color:#d9eafc;">方法区主要用于存放已被加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。</font>**

## <font style="color:#000000;">并发与并行的区别</font>
并发（Concurrency）与并行（Parallelism）是两个在多任务处理中相关的概念，它们的主要区别在于任务执行的方式和资源利用：

1. <font style="color:#000000;"> </font>**并发 (Concurrency)：**<font style="color:#000000;"> </font>
    - <font style="color:#000000;">并发是指多个任务在同一时间段内交替执行，虽然这些任务可能看起来像是同时进行，但实际上CPU在一个时间点上只会执行一个任务的一部分。</font>
    - <font style="color:#000000;">在单核处理器系统中，操作系统通过时间片轮转、中断等方式使得多个任务快速切换，给用户造成“同时”运行的假象。并发关注的是如何有效管理多个任务，使其能够共享同一计算资源，如处理器核心。</font>
    - <font style="color:#000000;">总结：</font><font style="color:#000000;background-color:#d9eafc;">两个及两个以上的作业在同一</font>**<font style="color:#000000;background-color:#d9eafc;">时间段</font>**<font style="color:#000000;background-color:#d9eafc;">内执行</font><font style="color:#000000;">。</font>
2. <font style="color:#000000;"> </font>**并行 (Parallelism)：**<font style="color:#000000;"> </font>
    - <font style="color:#000000;">并行则是指真正意义上的同时执行，即多个任务在同一时刻各自独立地在不同的计算资源上运行，例如在多核处理器或分布式系统中的多个节点上同时执行不同的任务。</font>
    - <font style="color:#000000;">并行强调的是充分利用硬件提供的多个处理单元来加速任务的完成，每个任务都能够在不被其他任务阻塞的情况下执行自己的操作。</font>
    - <font style="color:#000000;">总结：</font><font style="color:#000000;background-color:#cef5f7;">两个及两个以上的作业在同一</font>**<font style="color:#000000;background-color:#cef5f7;">时刻</font>**<font style="color:#000000;background-color:#cef5f7;">执行</font><font style="color:#000000;">。</font>

## <font style="color:#000000;">同步和异步的区别</font>
同步（Synchronous）和异步（Asynchronous）是计算机程序中用于描述执行模式或通信方式的两个重要概念，它们主要区别在于任务执行和响应等待的方式：

1. **同步 (Synchronous)：** 
    - 在同步操作中，调用一个方法或操作时，**<font style="background-color:#d9eafc;">调用方必须等待被调用方完成整个处理过程并返回结果后，才能继续执行后续的代码。</font>**
    - 例如，在单线程环境中，函数A调用函数B，直到函数B执行完毕并返回，函数A才会继续执行其后的语句。**<font style="background-color:#d9eafc;">同步方式简单直接，易于理解和控制，但可能会导致调用线程在等待期间被阻塞</font>**。
2. **异步 (Asynchronous)：** 
    - 在异步操作中，调用一个方法或操作时，**<font style="background-color:#d9eafc;">调用方无需等待被调用方完成，而是立即返回并继续执行后续的代码，同时被调用的方法会在后台独立执行。</font>**
    - 当被调用的操作完成后，**会通过回调函数、事件通知**、Future/Promise等机制**通知调用方结果已经准备就绪。**
    - 异步操作可以提高系统**响应速度**和**并发能力**，因为它不会阻塞调用线程，**特别适合于耗时较长的操作，如网络I/O、数据库查询等。**

举个实际例子：

+ 同步读取文件：当你使用某个方法读取文件时，直到文件内容全部加载到内存中，方法才返回，这段时间内其他代码无法执行。
+ 异步读取文件：当你发起一个文件读取请求后，方法立即返回，并不等待文件内容加载完成，而是在文件数据准备好之后通过回调函数或者其他机制告知你数据已可使用，此时你的主线程可以继续执行其他任务。

总结来说，同步与异步的核心差异在于是否**等待操作完成**以及**何时获取结果**。同步执行会导致调用线程等待，而异步执行则允许调用线程在等待结果的同时执行其他任务。

## <font style="color:#000000;">为什么要使用多线程?</font>
<font style="color:#000000;">先从总体上来说：</font>

+ **<font style="color:#000000;">从计算机底层来说：</font>**<font style="color:#000000;">线程可以比作是轻量级的进程，是程序执行的最小单位,线程间的切换和调度的成本远远小于进程。另外，多核 CPU 时代意味着多个线程可以同时运行，这减少了线程上下文切换的开销。</font>
+ **<font style="color:#000000;">从当代互联网发展趋势来说：</font>**<font style="color:#000000;">现在的系统动不动就要求百万级甚至千万级的并发量，而多线程并发编程正是开发高并发系统的基础，利用好多线程机制可以大大提高系统整体的并发能力以及性能。</font>

<font style="color:#000000;">再深入到计算机底层来探讨：</font>

+ **<font style="color:#000000;">单核时代</font>**<font style="color:#000000;">：在单核时代多线程主要是为了提高单进程利用 CPU 和 IO 系统的效率。 假设只运行了一个 Java 进程的情况，当我们请求 IO 的时候，如果 Java 进程中只有一个线程，此线程被 IO 阻塞则整个进程被阻塞。CPU 和 IO 设备只有一个在运行，那么可以简单地说系统整体效率只有 50%。当使用多线程的时候，一个线程被 IO 阻塞，其他线程还可以继续使用 CPU。从而提高了 Java 进程利用系统资源的整体效率。</font>
+ **<font style="color:#000000;">多核时代</font>**<font style="color:#000000;">: 多核时代多线程主要是为了提高进程利用多核 CPU 的能力。举个例子：假如我们要计算一个复杂的任务，我们只用一个线程的话，不论系统有几个 CPU 核心，都只会有一个 CPU 核心被利用到。而创建多个线程，这些线程可以被映射到底层多个 CPU 上执行，在任务中的多个线程没有资源竞争的情况下，任务执行的效率会有显著性的提高，约等于（单核时执行时间/CPU 核心数）。</font>

## <font style="color:#000000;">使用多线程可能带来什么问题?</font>
<font style="color:#000000;">并发编程的目的就是为了能提高程序的执行效率提高程序运行速度，但是并发编程并不总是能提高程序运行速度的，而且并发编程可能会遇到很多问题，比如：内存泄漏、死锁、线程不安全等等。</font>

## <font style="color:#000000;">如何理解线程安全和不安全？</font>
<font style="color:#000000;">线程安全和不安全是在多线程环境下对于同一份数据的访问是否能够保证其正确性和一致性的描述。</font>

+ <font style="color:#000000;">线程安全指的是在多线程环境下，对于同一份数据，不管有多少个线程同时访问，都能保证这份数据的正确性和一致性。</font>
+ <font style="color:#000000;">线程不安全则表示在多线程环境下，对于同一份数据，多个线程同时访问时可能会导致数据混乱、错误或者丢失。</font>

## <font style="color:#000000;">单核 CPU 上运行多个线程效率一定会高吗？</font>
<font style="color:#000000;">单核 CPU 同时运行多个线程的效率是否会高，取决于线程的类型和任务的性质。一般来说，有两种类型的线程：CPU 密集型和 IO 密集型。CPU 密集型的线程主要进行计算和逻辑处理，需要占用大量的 CPU 资源。IO 密集型的线程主要进行输入输出操作，如读写文件、网络通信等，需要等待 IO 设备的响应，而不占用太多的 CPU 资源。</font>

<font style="color:#000000;">在单核 CPU 上，同一时刻只能有一个线程在运行，其他线程需要等待 CPU 的时间片分配。如果线程是 CPU 密集型的，那么多个线程同时运行会导致频繁的线程切换，增加了系统的开销，降低了效率。如果线程是 IO 密集型的，那么多个线程同时运行可以利用 CPU 在等待 IO 时的空闲时间，提高了效率。</font>

<font style="color:#000000;">因此，对于单核 CPU 来说，如果任务是 CPU 密集型的，那么开很多线程会影响效率；如果任务是 IO 密集型的，那么开很多线程会提高效率。当然，这里的“很多”也要适度，不能超过系统能够承受的上限。</font>

## <font style="color:#000000;">说说线程的生命周期和状态?</font>
<font style="color:#000000;">Java 线程在运行的生命周期中的指定时刻只可能处于下面 6 种不同状态的其中一个状态：</font>

+ <font style="color:#000000;">NEW: 初始状态，线程被创建出来但没有被调用</font><font style="color:#000000;">start()</font><font style="color:#000000;">。</font>
+ <font style="color:#000000;">RUNNABLE: 运行状态，线程被调用了</font><font style="color:#000000;">start()</font><font style="color:#000000;">等待运行的状态。</font>
+ <font style="color:#000000;">BLOCKED：阻塞状态，需要等待锁释放。</font>
+ <font style="color:#000000;">WAITING：等待状态，表示该线程需要等待其他线程做出一些特定动作（通知或中断）。</font>
+ <font style="color:#000000;">TIME_WAITING：超时等待状态，可以在指定的时间后自行返回而不是像 WAITING 那样一直等待。</font>
+ <font style="color:#000000;">TERMINATED：终止状态，表示该线程已经运行完毕。</font>

<font style="color:#000000;">线程在生命周期中并不是固定处于某一个状态而是随着代码的执行在不同状态之间切换。</font>

<font style="color:#000000;">Java 线程状态变迁图(图源：</font>[<font style="color:#000000;">挑错 |《Java 并发编程的艺术》中关于线程状态的三处错误</font>](https://mp.weixin.qq.com/s/UOrXql_LhOD8dhTq_EPI0w)<font style="color:#000000;">)：</font>

![1732497473857-a35e2aab-19b0-4823-abe3-fefa797bc825.png](./img/LdNyK5cE02eRr3_k/1732497473857-a35e2aab-19b0-4823-abe3-fefa797bc825-411744.png)

<font style="color:#000000;">Java 线程状态变迁图</font>

<font style="color:#000000;">由上图可以看出：线程创建之后它将处于</font>**<font style="color:#000000;">NEW（新建）</font>**<font style="color:#000000;">状态，调用</font><font style="color:#000000;">start()</font><font style="color:#000000;">方法后开始运行，线程这时候处于</font>**<font style="color:#000000;">READY（可运行）</font>**<font style="color:#000000;">状态。可运行状态的线程获得了 CPU 时间片（timeslice）后就处于</font>**<font style="color:#000000;">RUNNING（运行）</font>**<font style="color:#000000;">状态。</font>

<font style="color:#000000;">在操作系统层面，线程有 READY 和 RUNNING 状态；而在 JVM 层面，只能看到 RUNNABLE 状态（图源：</font>[<font style="color:#000000;">HowToDoInJava</font>](https://howtodoinjava.com/)[<font style="color:#000000;">open in new window</font>](https://howtodoinjava.com/)<font style="color:#000000;">：</font>[<font style="color:#000000;">Java Thread Life Cycle and Thread States</font>](https://howtodoinjava.com/Java/multi-threading/Java-thread-life-cycle-and-thread-states/)[<font style="color:#000000;">open in new window</font>](https://howtodoinjava.com/Java/multi-threading/Java-thread-life-cycle-and-thread-states/)<font style="color:#000000;">），所以 Java 系统一般将这两个状态统称为</font>**<font style="color:#000000;">RUNNABLE（运行中）</font>**<font style="color:#000000;">状态 。</font>**<font style="color:#000000;">为什么 JVM 没有区分这两种状态呢？</font>**<font style="color:#000000;">（摘自：</font>[<font style="color:#000000;">Java 线程运行怎么有第六种状态？ - Dawell 的回答</font>](https://www.zhihu.com/question/56494969/answer/154053599)[<font style="color:#000000;">open in new window</font>](https://www.zhihu.com/question/56494969/answer/154053599)<font style="color:#000000;">） 现在的时分（time-sharing）多任务（multi-task）操作系统架构通常都是用所谓的“时间分片（time quantum or time slice）”方式进行抢占式（preemptive）轮转调度（round-robin 式）。这个时间分片通常是很小的，一个线程一次最多只能在 CPU 上运行比如 10-20ms 的时间（此时处于 running 状态），也即大概只有 0.01 秒这一量级，时间片用后就要被切换下来放入调度队列的末尾等待再次调度。（也即回到 ready 状态）。</font>**<font style="color:#000000;">线程切换的如此之快，区分这两种状态就没什么意义了。</font>**

![1732497473925-e21038c0-c179-4c3f-9cc4-cdb55bfb6b40.png](./img/LdNyK5cE02eRr3_k/1732497473925-e21038c0-c179-4c3f-9cc4-cdb55bfb6b40-720334.png)

<font style="color:#000000;">RUNNABLE-VS-RUNNING</font>

+ <font style="color:#000000;">当线程执行</font><font style="color:#000000;">wait()</font><font style="color:#000000;">方法之后，线程进入</font>**<font style="color:#000000;">WAITING（等待）</font>**<font style="color:#000000;">状态。进入等待状态的线程需要依靠其他线程的通知才能够返回到运行状态。</font>
+ **<font style="color:#000000;">TIMED_WAITING(超时等待)</font>**<font style="color:#000000;">状态相当于在等待状态的基础上增加了超时限制，比如通过</font><font style="color:#000000;">sleep（long millis）</font><font style="color:#000000;">方法或</font><font style="color:#000000;">wait（long millis）</font><font style="color:#000000;">方法可以将线程置于 TIMED_WAITING 状态。当超时时间结束后，线程将会返回到 RUNNABLE 状态。</font>
+ <font style="color:#000000;">当线程进入</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">方法/块或者调用</font><font style="color:#000000;">wait</font><font style="color:#000000;">后（被</font><font style="color:#000000;">notify</font><font style="color:#000000;">）重新进入</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">方法/块，但是锁被其它线程占有，这个时候线程就会进入</font>**<font style="color:#000000;">BLOCKED（阻塞）</font>**<font style="color:#000000;">状态。</font>
+ <font style="color:#000000;">线程在执行完了</font><font style="color:#000000;">run()</font><font style="color:#000000;">方法之后将会进入到</font>**<font style="color:#000000;">TERMINATED（终止）</font>**<font style="color:#000000;">状态。</font>

<font style="color:#000000;">相关阅读：</font>[<font style="color:#000000;">线程的几种状态你真的了解么？</font>](https://mp.weixin.qq.com/s/R5MrTsWvk9McFSQ7bS0W2w)<font style="color:#000000;">。</font>

## <font style="color:#000000;">什么是线程上下文切换?</font>
<font style="color:#000000;">线程在执行过程中会有自己的运行条件和状态（也称上下文），比如上文所说到过的程序计数器，栈信息等。当出现如下情况的时候，线程会从占用 CPU 状态中退出。</font>

+ **<font style="color:#000000;">主动让出 CPU，比如调用了sleep(),wait()等</font>**<font style="color:#000000;">。</font>
+ **<font style="color:#000000;">时间片用完，因为操作系统要防止一个线程或者进程长时间占用 CPU 导致其他线程或者进程饿死</font>**<font style="color:#000000;">。</font>
+ **<font style="color:#000000;">调用了阻塞类型的系统中断，比如请求 IO，线程被阻塞</font>**<font style="color:#000000;">。</font>
+ **<font style="color:#000000;">被终止或结束运行</font>**

<font style="color:#000000;">这其中前三种都会发生线程切换，线程切换意味着</font>**<font style="color:#000000;background-color:#d9eafc;">需要保存当前线程的上下文</font>**<font style="color:#000000;background-color:#d9eafc;">，</font>**<font style="color:#000000;background-color:#d9eafc;">留待线程下次占用 CPU 的时候恢复现场</font>**<font style="color:#000000;">。并加载下一个将要占用 CPU 的线程上下文。这就是所谓的</font>**<font style="color:#000000;">上下文切换</font>**<font style="color:#000000;">。</font>

<font style="color:#000000;">上下文切换是现代操作系统的基本功能，因其每次需要保存信息恢复信息，这将会占用 CPU，内存等系统资源进行处理，也就意味着效率会有一定损耗，如果频繁切换就会造成整体效率低下。</font>

## <font style="color:#000000;">什么是线程死锁?如何避免死锁?</font>
### <font style="color:#000000;">12.1 认识线程死锁</font>
<font style="color:#000000;">线程死锁描述的是这样一种情况：多个线程同时被阻塞，它们中的一个或者全部都在等待某个资源被释放。由于线程被无限期地阻塞，因此程序不可能正常终止。</font>

<font style="color:#000000;">如下图所示，线程 A 持有资源 2，线程 B 持有资源 1，他们同时都想申请对方的资源，所以这两个线程就会互相等待而进入死锁状态。</font>

![1732497474024-82396cca-deb0-4c8d-b25a-4842cef1bcd1.png](./img/LdNyK5cE02eRr3_k/1732497474024-82396cca-deb0-4c8d-b25a-4842cef1bcd1-240252.png)

<font style="color:#000000;">线程死锁示意图</font>

<font style="color:#000000;">下面通过一个例子来说明线程死锁,代码模拟了上图的死锁的情况 (代码来源于《并发编程之美》)：</font>

```java
public class DeadLockDemo {
    private static Object resource1 = new Object();//资源 1
    private static Object resource2 = new Object();//资源 2

    public static void main(String[] args) {
        new Thread(() -> {
            synchronized (resource1) {
                System.out.println(Thread.currentThread() + "get resource1");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource2");
                synchronized (resource2) {
                    System.out.println(Thread.currentThread() + "get resource2");
                }
            }
        }, "线程 1").start();

        new Thread(() -> {
            synchronized (resource2) {
                System.out.println(Thread.currentThread() + "get resource2");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource1");
                synchronized (resource1) {
                    System.out.println(Thread.currentThread() + "get resource1");
                }
            }
        }, "线程 2").start();
    }
}
```

<font style="color:#000000;">Output</font>

```java
Thread[线程 1,5,main]get resource1
Thread[线程 2,5,main]get resource2
Thread[线程 1,5,main]waiting get resource2
Thread[线程 2,5,main]waiting get resource1
```

<font style="color:#000000;">线程 A 通过</font><font style="color:#000000;">synchronized (resource1)</font><font style="color:#000000;">获得</font><font style="color:#000000;">resource1</font><font style="color:#000000;">的监视器锁，然后通过</font><font style="color:#000000;">Thread.sleep(1000);</font><font style="color:#000000;">让线程 A 休眠 1s 为的是让线程 B 得到执行然后获取到 resource2 的监视器锁。线程 A 和线程 B 休眠结束了都开始企图请求获取对方的资源，然后这两个线程就会陷入互相等待的状态，这也就产生了死锁。</font>

<font style="color:#000000;">上面的例子符合产生死锁的四个必要条件：</font>

1. <font style="color:#000000;">互斥条件：该资源任意一个时刻只由一个线程占用。</font>
2. <font style="color:#000000;">请求与保持条件：一个线程因请求资源而阻塞时，对已获得的资源保持不放。</font>
3. <font style="color:#000000;">不剥夺条件:线程已获得的资源在未使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。</font>
4. <font style="color:#000000;">循环等待条件:若干线程之间形成一种头尾相接的循环等待资源关系。</font>

### <font style="color:#000000;">12.2 如何预防和避免线程死锁?</font>
**<font style="color:#000000;">如何预防死锁？</font>**<font style="color:#000000;">破坏死锁的产生的必要条件即可：</font>

1. **<font style="color:#000000;">破坏请求与保持条件</font>**<font style="color:#000000;">：一次性申请所有的资源。</font>
2. **<font style="color:#000000;">破坏不剥夺条件</font>**<font style="color:#000000;">：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。</font>
3. **<font style="color:#000000;">破坏循环等待条件</font>**<font style="color:#000000;">：靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。</font>

**<font style="color:#000000;">如何避免死锁？</font>**

<font style="color:#000000;">避免死锁就是在资源分配时，借助于算法（比如银行家算法）对资源分配进行计算评估，使其进入安全状态。</font>

**<font style="color:#000000;">安全状态</font>**<font style="color:#000000;">指的是系统能够按照某种线程推进顺序（P1、P2、P3……Pn）来为每个线程分配所需资源，直到满足每个线程对资源的最大需求，使每个线程都可顺利完成。称</font><font style="color:#000000;"><P1、P2、P3.....Pn></font><font style="color:#000000;">序列为安全序列。</font>

<font style="color:#000000;">我们对线程 2 的代码修改成下面这样就不会产生死锁了。</font>

```java
new Thread(() -> {
    synchronized (resource1) {
        System.out.println(Thread.currentThread() + "get resource1");
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread() + "waiting get resource2");
        synchronized (resource2) {
            System.out.println(Thread.currentThread() + "get resource2");
        }
    }
}, "线程 2").start();
```

<font style="color:#000000;">输出：</font>

```java
Thread[线程 1,5,main]get resource1
Thread[线程 1,5,main]waiting get resource2
Thread[线程 1,5,main]get resource2
Thread[线程 2,5,main]get resource1
Thread[线程 2,5,main]waiting get resource2
Thread[线程 2,5,main]get resource2

Process finished with exit code 0
```

<font style="color:#000000;">我们分析一下上面的代码为什么避免了死锁的发生?</font>

<font style="color:#000000;">线程 1 首先获得到 resource1 的监视器锁,这时候线程 2 就获取不到了。然后线程 1 再去获取 resource2 的监视器锁，可以获取到。然后线程 1 释放了对 resource1、resource2 的监视器锁的占用，线程 2 获取到就可以执行了。这样就破坏了破坏循环等待条件，因此避免了死锁。</font>

## <font style="color:#000000;">sleep() 方法和 wait() 方法对比</font>
**<font style="color:#000000;">共同点</font>**<font style="color:#000000;">：两者都可以暂停线程的执行。</font>

**<font style="color:#000000;">区别</font>**<font style="color:#000000;">：</font>

+ **<font style="color:#000000;">sleep()</font>****<font style="color:#000000;">方法没有释放锁，而</font>****<font style="color:#000000;">wait()</font>************<font style="color:#000000;">方法释放了锁</font>**<font style="color:#000000;">。</font>
+ <font style="color:#000000;">wait()</font><font style="color:#000000;">通常被用于线程间交互/通信，</font><font style="color:#000000;">sleep()</font><font style="color:#000000;">通常被用于暂停执行。</font>
+ <font style="color:#000000;">wait()</font><font style="color:#000000;">方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的</font><font style="color:#000000;">notify()</font><font style="color:#000000;">或者</font><font style="color:#000000;">notifyAll()</font><font style="color:#000000;">方法。</font><font style="color:#000000;">sleep()</font><font style="color:#000000;">方法执行完成后，线程会自动苏醒，或者也可以使用</font><font style="color:#000000;">wait(long timeout)</font><font style="color:#000000;">超时后线程会自动苏醒。</font>
+ <font style="color:#000000;">sleep()</font><font style="color:#000000;">是</font><font style="color:#000000;">Thread</font><font style="color:#000000;">类的静态本地方法，</font><font style="color:#000000;">wait()</font><font style="color:#000000;">则是</font><font style="color:#000000;">Object</font><font style="color:#000000;">类的本地方法。为什么这样设计呢？下一个问题就会聊到。</font>

## <font style="color:#000000;">为什么 wait() 方法不定义在 Thread 中？</font>
<font style="color:#000000;">wait()</font><font style="color:#000000;">是让获得对象锁的线程实现等待，会自动释放当前线程占有的对象锁。每个对象（</font><font style="color:#000000;">Object</font><font style="color:#000000;">）都拥有对象锁，既然要释放当前线程占有的对象锁并让其进入 WAITING 状态，自然是要操作对应的对象（</font><font style="color:#000000;">Object</font><font style="color:#000000;">）而非当前的线程（</font><font style="color:#000000;">Thread</font><font style="color:#000000;">）。</font>

<font style="color:#000000;">类似的问题：</font>**<font style="color:#000000;">为什么</font>****<font style="color:#000000;">sleep()</font>****<font style="color:#000000;">方法定义在</font>****<font style="color:#000000;">Thread</font>****<font style="color:#000000;">中？</font>**

<font style="color:#000000;">因为</font><font style="color:#000000;">sleep()</font><font style="color:#000000;">是让当前线程暂停执行，不涉及到对象类，也不需要获得对象锁。</font>

## <font style="color:#000000;">可以直接调用 Thread 类的 run 方法吗？</font>
<font style="color:#000000;">这是另一个非常经典的 Java 多线程面试问题，而且在面试中会经常被问到。很简单，但是很多人都会答不上来！</font>

<font style="color:#000000;">new 一个</font><font style="color:#000000;">Thread</font><font style="color:#000000;">，线程进入了新建状态。调用</font><font style="color:#000000;">start()</font><font style="color:#000000;">方法，会启动一个线程并使线程进入了就绪状态，当分配到时间片后就可以开始运行了。</font><font style="color:#000000;">start()</font><font style="color:#000000;">会执行线程的相应准备工作，然后自动执行</font><font style="color:#000000;">run()</font><font style="color:#000000;">方法的内容，这是真正的多线程工作。 但是，直接执行</font><font style="color:#000000;">run()</font><font style="color:#000000;">方法，会把</font><font style="color:#000000;">run()</font><font style="color:#000000;">方法当成一个 main 线程下的普通方法去执行，并不会在某个线程中执行它，所以这并不是多线程工作。</font>

**<font style="color:#000000;">总结：调用</font>****<font style="color:#000000;">start()</font>****<font style="color:#000000;">方法方可启动线程并使线程进入就绪状态，直接执行</font>****<font style="color:#000000;">run()</font>****<font style="color:#000000;">方法的话不会以多线程的方式执行。</font>**







> 更新: 2024-01-17 22:54:18  
原文: [https://www.yuque.com/vip6688/neho4x/owc7xdd973pf0k86](https://www.yuque.com/vip6688/neho4x/owc7xdd973pf0k86)
>



> 更新: 2024-11-25 09:17:54  
> 原文: <https://www.yuque.com/neumx/laxg2e/76b2c00b492d9e0cde8e606019970a7d>