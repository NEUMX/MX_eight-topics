# 5、GC篇

# 5、GC篇
# <font style="color:#000000;">5.1 概述</font>
> + <font style="color:#000000;"> 常见的垃圾回收器算法有哪些，各有什么优劣？（网易） </font>
>

> + <font style="color:#000000;"> 常见的垃圾回收器算法有哪些，各有什么优劣？（阿里-天猫、UC） </font>
>

> + <font style="color:#000000;"> 有哪些垃圾回收方法，jdk8的垃圾收集器是什么？（搜狐、万达集团） </font>
>

> + <font style="color:#000000;"> G1原理。（亚信） </font>
>

> + <font style="color:#000000;"> 几种垃圾回收器（亚信） </font>
>

> + <font style="color:#000000;"> 垃圾回收器有哪些？都有哪些算法来实现？项目中用的垃圾回收器是什么？（平安） </font>
>

> + <font style="color:#000000;"> 垃圾回收器的基本原理是什么？垃圾回收器可以马上回收内存吗？有什么办法主动通知虚拟机进行垃圾回收？（平安） </font>
>

> + <font style="color:#000000;"> 你知道那些垃圾回收器（高德地图） </font>
>

> + <font style="color:#000000;"> 有些垃圾方法，8的垃圾收集器是什么。（新浪） </font>
>

> + <font style="color:#000000;"> GC 收集器有哪些？CMS 收集器与 G1 收集器的特点。 </font>
>

> + <font style="color:#000000;"> 请问吞吐量的优化和响应优先的垃圾收集器是如何选择的呢？（滴滴） </font>
>

> + <font style="color:#000000;"> 你知道哪几种垃圾收集器，各自的优缺点，重点讲下cms和G1，包括原理，流程，优缺点。（拼多多） </font>
>

> + <font style="color:#000000;"> CMS 收集器与 G1 收集器的特点。 (蚂蚁金服) </font>
>

> + <font style="color:#000000;"> G1回收器讲下回收过程 (蚂蚁金服) </font>
>

> + <font style="color:#000000;"> 你知道哪几种垃圾回收器，各自的优缺点，重点讲一下cms和g1，包括原理，流程，优缺点 (蚂蚁金服) </font>
>

> + <font style="color:#000000;"> CMS特点，垃圾回收算法有哪些？各自的优缺点，他们共同的缺点是什么？ (天猫) </font>
>

> + <font style="color:#000000;"> 讲一下CMS垃圾收集器垃圾回收的流程，以及CMS的缺点 (抖音) </font>
>

> + <font style="color:#000000;"> Java的垃圾回收器都有哪些，说下g1的应用场景，平时你是如何搭配使用垃圾回收器的 (滴滴) </font>
>

> + <font style="color:#000000;"> 说几个垃圾回收器，cms回收器有哪几个过程，停顿几次，会不会产生内存碎片。老年代产生内存碎片会有什么问题。 (小米) </font>
>

> + <font style="color:#000000;"> JVM有哪三种垃圾回收器？ (阿里) </font>
>

> + <font style="color:#000000;"> 吞吐量优先选择什么垃圾回收器？响应时间优先呢？ (阿里) </font>
>

> + <font style="color:#000000;"> 常见的垃圾回收器算法有哪些，各有什么优劣？ (字节跳动) </font>
>

> + <font style="color:#000000;"> CMS和G1了解么，CMS解决什么问题，说一下回收的过程。(字节跳动) </font>
>

> + <font style="color:#000000;"> CMS回收停顿了几次，为什么要停顿两次。(字节跳动) </font>
>

> + <font style="color:#000000;"> CMS过程是怎样的？内部使用什么算法做垃圾回收？ (美团) </font>
>

> + <font style="color:#000000;"> g1和cms区别,吞吐量优先和响应优先的垃圾收集器选择 (携程) </font>
>


## **1. JVM的垃圾回收（GC，Garbage Collection）机制（垃圾回收算法）**
<font style="color:#000000;">JVM的垃圾回收是一种自动内存管理机制，它负责在程序运行期间跟踪并回收堆内存中不再使用的对象所占用的空间。通过GC，程序员无需手动释放内存资源，从而降低了编程复杂性和出错的可能性。</font>

## **<font style="color:#000000;">2. GC是什么？为什么要有GC？</font>**
+ <font style="color:#000000;"> </font>**<font style="color:#000000;">GC是什么</font>**<font style="color:#000000;">： </font><font style="color:#000000;">GC是Java虚拟机的一项核心功能，用于监控和回收堆内存中的无用对象，防止内存泄漏和溢出。当一个对象不再被任何引用变量所指向时，就认为它是不可达的，即为垃圾对象，GC会识别这些垃圾对象并在适当的时候进行回收。 </font>
+ <font style="color:#000000;"> </font>**<font style="color:#000000;">为什么要有GC</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">避免了手动管理内存带来的复杂性和易错性。</font>
    - <font style="color:#000000;">自动回收不再使用的内存，提高了资源利用率，确保了程序能够持续稳定运行。</font>
    - <font style="color:#000000;">减轻了开发者的负担，让开发者可以更多地关注业务逻辑而非底层内存管理。</font>

## **<font style="color:#000000;">3. 什么情况下触发垃圾回收？</font>**
<font style="color:#000000;">垃圾回收的具体触发条件包括但不限于以下几种情况：</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">堆空间不足</font>**<font style="color:#000000;">：当应用程序新创建对象时，若堆内存空间不足，JVM将试图发起一次垃圾回收操作，以回收那些已经不再使用的对象占据的内存，以便为新的对象分配空间。 </font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">年轻代空间不足</font>**<font style="color:#000000;">：对于大多数垃圾收集器（如HotSpot VM的Serial、Parallel、CMS或G1），新生代（Young Generation）满时，会触发Minor GC（也称为Young GC）。 </font>
3. <font style="color:#000000;"> </font>**<font style="color:#000000;">老年代空间不足</font>**<font style="color:#000000;">：经过多次Minor GC后仍然存活的对象会被晋升到老年代，如果老年代空间不足，通常会触发Major GC或者Full GC，具体取决于使用的垃圾收集器策略。 </font>
4. <font style="color:#000000;"> </font>**<font style="color:#000000;">系统调用System.gc()</font>**<font style="color:#000000;">：虽然不推荐直接调用此方法来强制执行垃圾回收，但在某些特殊情况下，JVM可能会响应这个请求而进行垃圾回收。但请注意，这并不保证立即执行垃圾回收。 </font>
5. <font style="color:#000000;"> </font>**<font style="color:#000000;">满足特定条件的并发标记周期结束</font>**<font style="color:#000000;">：对于CMS、G1等并发收集器，在并发标记阶段结束后，根据统计分析结果，可能触发垃圾回收。 </font>
6. <font style="color:#000000;"> </font>**<font style="color:#000000;">元空间或Metaspace不足</font>**<font style="color:#000000;">：在Java 8及更高版本中，类元数据存储在元空间（Metaspace），当元空间不足时，也会触发相应的内存回收过程。 </font>



# <font style="color:#000000;">5.2 垃圾回收器</font>
> + <font style="color:#000000;"> 常见的垃圾回收器算法有哪些，各有什么优劣？（网易） </font>
>

> + <font style="color:#000000;"> 常见的垃圾回收器算法有哪些，各有什么优劣？（阿里-天猫、UC） </font>
>

> + <font style="color:#000000;"> 有哪些垃圾回收方法，jdk8的垃圾收集器是什么？（搜狐、万达集团） </font>
>

> + <font style="color:#000000;"> G1原理。（亚信） </font>
>

> + <font style="color:#000000;"> 几种垃圾回收器（亚信） </font>
>

> + <font style="color:#000000;"> 垃圾回收器有哪些？都有哪些算法来实现？项目中用的垃圾回收器是什么？（平安） </font>
>

> + <font style="color:#000000;"> 垃圾回收器的基本原理是什么？垃圾回收器可以马上回收内存吗？有什么办法主动通知虚拟机进行垃圾回收？（平安） </font>
>

> + <font style="color:#000000;"> 你知道那些垃圾回收器（高德地图） </font>
>

> + <font style="color:#000000;"> 有些垃圾方法，8的垃圾收集器是什么。（新浪） </font>
>

> + <font style="color:#000000;"> GC 收集器有哪些？CMS 收集器与 G1 收集器的特点。 </font>
>

> + <font style="color:#000000;"> 请问吞吐量的优化和响应优先的垃圾收集器是如何选择的呢？（滴滴） </font>
>

> + <font style="color:#000000;"> 你知道哪几种垃圾收集器，各自的优缺点，重点讲下cms和G1，包括原理，流程，优缺点。（拼多多） </font>
>

> + <font style="color:#000000;"> CMS 收集器与 G1 收集器的特点。 (蚂蚁金服) </font>
>

> + <font style="color:#000000;"> G1回收器讲下回收过程 (蚂蚁金服) </font>
>

> + <font style="color:#000000;"> 你知道哪几种垃圾回收器，各自的优缺点，重点讲一下cms和g1，包括原理，流程，优缺点 (蚂蚁金服) </font>
>

> + <font style="color:#000000;"> CMS特点，垃圾回收算法有哪些？各自的优缺点，他们共同的缺点是什么？ (天猫) </font>
>

> + <font style="color:#000000;"> 讲一下CMS垃圾收集器垃圾回收的流程，以及CMS的缺点 (抖音) </font>
>

> + <font style="color:#000000;"> Java的垃圾回收器都有哪些，说下g1的应用场景，平时你是如何搭配使用垃圾回收器的 (滴滴) </font>
>

> + <font style="color:#000000;"> 说几个垃圾回收器，cms回收器有哪几个过程，停顿几次，会不会产生内存碎片。老年代产生内存碎片会有什么问题。 (小米) </font>
>

> + <font style="color:#000000;"> JVM有哪三种垃圾回收器？ (阿里) </font>
>

> + <font style="color:#000000;"> 吞吐量优先选择什么垃圾回收器？响应时间优先呢？ (阿里) </font>
>

> + <font style="color:#000000;"> 常见的垃圾回收器算法有哪些，各有什么优劣？ (字节跳动) </font>
>

> + <font style="color:#000000;"> CMS和G1了解么，CMS解决什么问题，说一下回收的过程。(字节跳动) </font>
>

> + <font style="color:#000000;"> CMS回收停顿了几次，为什么要停顿两次。(字节跳动) </font>
>

> + <font style="color:#000000;"> CMS过程是怎样的？内部使用什么算法做垃圾回收？ (美团) </font>
>

> + <font style="color:#000000;"> g1和cms区别,吞吐量优先和响应优先的垃圾收集器选择 (携程) </font>
>


## **1. 常见的垃圾回收器算法及其优劣：**
1. <font style="color:#000000;"> </font>**标记-清除（Mark-Sweep）**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">算法描述：首先进行标记，识别出所有活动对象；然后清除未被标记的对象。</font>
    - <font style="color:#000000;">优点：实现简单。</font>
    - <font style="color:#000000;">缺点：会产生内存碎片，且清除过程效率不高。</font>
2. <font style="color:#000000;"> </font>**复制（Copying）**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">算法描述：将堆划分为两个大小相等的区域，每次只使用其中一个区域分配内存，当该区域填满时，将存活对象复制到另一个区域，并清理已使用的区域。</font>
    - <font style="color:#000000;">优点：不会有内存碎片问题，适用于新生代中的Eden区和Survivor区。</font>
    - <font style="color:#000000;">缺点：需要额外的空间，且对于大对象处理效率较低。</font>
3. <font style="color:#000000;">  </font>**标记-压缩（Mark-Compact）**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">算法描述：类似于标记-清除，但在清除后会对存活对象进行整理，移动至一起并消除碎片。</font>
    - <font style="color:#000000;">优点：避免了内存碎片的问题。</font>
    - <font style="color:#000000;">缺点：移动对象的过程较慢，导致STW时间较长。</font>
4. <font style="color:#000000;"> </font>**分代收集（Generational Collection）**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">算法描述：根据对象生命周期的不同，将堆内存划分为新生代、老年代等多个区域，不同区域采用不同的垃圾回收策略。</font>
    - <font style="color:#000000;">优点：利用大部分对象“朝生夕死”的特性，提高垃圾回收效率。</font>
    - <font style="color:#000000;">缺点：增加了系统的复杂性。</font>

## **2. **<font style="color:#000000;">JDK8及之后版本的主要垃圾收集器包括：</font>
+ **Serial/Serial Old**<font style="color:#000000;">：单线程收集器，适合客户端应用或小型服务器环境。</font>
+ **Parallel Scavenge/Parallel Old**<font style="color:#000000;">：多线程并行收集器，关注高吞吐量。</font>
+ **CMS（Concurrent Mark Sweep）**<font style="color:#000000;">：并发标记清除收集器，目标是减少停顿时间，适用于对响应时间敏感的应用场景，但可能导致浮动垃圾和内存碎片。</font>
+ **G1（Garbage First）**<font style="color:#000000;">：从JDK9开始成为默认的老年代收集器，支持分代收集和区域化收集，具备预测停顿时间和并行化处理能力，能够更好地适应大内存服务端应用。</font>

## **3. **<font style="color:#000000;">G1垃圾回收器原理：</font>
<font style="color:#000000;">G1将整个Java堆划分为多个大小相等的Region。在回收过程中，它会优先回收垃圾收益高的Region，即所谓的“Garbage First”。其回收流程包括年轻代回收（Young GC）、混合回收（Mixed GC）以及并发标记周期（Concurrent Marking）。通过整体上兼顾吞吐量和延迟的要求，实现了可预测的暂停时间模型。</font>

## **4. **<font style="color:#000000;">CMS收集器与G1收集器特点对比：</font>
+ <font style="color:#000000;"> CMS： </font>
    - <font style="color:#000000;">优势：低延迟，具有并发标记和并发清除阶段，尽可能减少STW的时间。</font>
    - <font style="color:#000000;">劣势：并发阶段占用较多CPU资源，容易产生浮动垃圾，且在老年代空间不足时可能导致“Concurrent Mode Failure”，另外，由于采用标记-清除算法，长时间运行后可能会出现内存碎片问题。</font>
+ <font style="color:#000000;"> G1： </font>
    - <font style="color:#000000;">优势：提供了更细粒度的内存分区管理，能预测停顿时间，对大内存友好，同时采用了部分并发标记-压缩算法，有效减少了内存碎片。</font>
    - <font style="color:#000000;">劣势：相较于CMS，G1的算法更为复杂，某些情况下性能可能不如CMS，尤其是对于小对象较多的情况。</font>

## **5. **<font style="color:#000000;">选择垃圾收集器的原则：</font>
+ <font style="color:#000000;">吞吐量优化优先：通常会选择Parallel Scavenge / Parallel Old收集器组合，或者在大型系统中考虑使用G1。</font>
+ <font style="color:#000000;">响应时间优先：对于延迟要求较高的应用，可以选择CMS或者直接使用G1收集器，它们都致力于缩短垃圾回收带来的程序暂停时间。</font>

<font style="color:#000000;">关于CMS和G1的具体回收过程、优缺点以及如何搭配使用垃圾收集器，可以根据实际情况结合上述信息进一步分析。</font>

# <font style="color:#000000;">5.3 垃圾回收算法</font>
> + <font style="color:#000000;"> 什么时候对象可以被收回？（阿里-闲鱼） </font>
>

> + <font style="color:#000000;"> 什么时候对象可以被收回？（拼多多） </font>
>

> + <font style="color:#000000;"> GC算法都有哪些？他们之间的区别是什么？（菜鸟） </font>
>

> + <font style="color:#000000;"> JVM的常用的GC算法（高得地图） </font>
>

> + <font style="color:#000000;"> JVM的垃圾回收为什么采用分代GC。跟语言有关系吗？（阿里-钉钉） </font>
>

> + <font style="color:#000000;"> 分代的意义说一下 （阿里-钉钉） </font>
>

> + <font style="color:#000000;"> 而全GC时间较长 分代GC可以大大降低GC时间而且也可以保证heap不会过快增长（墨迹天气） </font>
>

> + <font style="color:#000000;"> GC垃圾回收机制算法（数信互融科技发展有限公司） </font>
>

> + <font style="color:#000000;"> GC的算法，复制算法和标记清除的优缺点？（迪原创新） </font>
>

> + <font style="color:#000000;"> 常用的GC算法，如何确定哪些是要被清除的哪些是不能被清除（网易邮箱、美团） </font>
>

> + <font style="color:#000000;"> 垃圾回收机制的几种回收算法（亚信） </font>
>

> + <font style="color:#000000;"> GC算法都有哪些？他们之间的区别是什么？各自的适用场景？（B站） </font>
>

> + <font style="color:#000000;"> GC分代算法（花旗银行） </font>
>

> + <font style="color:#000000;"> 新生代的垃圾回收什么时候触发（花旗银行） </font>
>

> + <font style="color:#000000;"> 老年代的垃圾回收机制什么时候触发，自动触发的阈值是多少（花旗银行） </font>
>

> + <font style="color:#000000;"> GC 的两种判定方法：（360安全） </font>
>

> + <font style="color:#000000;"> GC 的三种收集方法：标记清除、标记整理、复制算法的原理与特点，分别用在什么地方，如果让你优化收集方法，有什么思路？（腾讯） </font>
>

> + <font style="color:#000000;"> 如何判断一个对象是否存活?（唯品会） </font>
>

> + <font style="color:#000000;"> Java 中垃圾收集的方法有哪些?（苏宁） </font>
>

> + <font style="color:#000000;"> 你是用什么方法判断对象是否死亡？（滴滴） </font>
>

> + <font style="color:#000000;"> 说一下GC算法，分代回收说下 (百度) </font>
>

> + <font style="color:#000000;"> 垃圾收集策略和算法 (百度) </font>
>

> + <font style="color:#000000;"> 说一下gc算法，分代回收说下 (百度) </font>
>

> + <font style="color:#000000;"> 说一下gc算法，分代回收说下 (百度) </font>
>

> + <font style="color:#000000;"> JVM有哪些回收算法，对应的收集器有哪些？ (蚂蚁金服) </font>
>

> + <font style="color:#000000;"> 如何判断一个对象是否存活？ (蚂蚁金服) </font>
>

> + <font style="color:#000000;"> JVM GC算法有哪些，目前的JDK版本采用什么回收算法 (蚂蚁金服) </font>
>

> + <font style="color:#000000;"> 垃圾回收算法的实现原理。 (京东) </font>
>

> + <font style="color:#000000;"> JVM场景问题， 标记清除多次后老年代产生内存碎片，引起full gc，接下来可能发生什么问题。 (小米) </font>
>

> + <font style="color:#000000;"> Java怎么进行垃圾回收的？什么对象会进老年代？ 垃圾回收算法有哪些？为什么新生代使用复制算法？ (京东) </font>
>

> + <font style="color:#000000;"> 讲一下JVM中如何判断对象的生死？ (京东) </font>
>

> + <font style="color:#000000;"> 如何选择合适的垃圾收集算法？ (阿里) </font>
>

> + <font style="color:#000000;"> 讲一讲垃圾回收算法。 (阿里) </font>
>

> + <font style="color:#000000;"> JVM有哪些回收算法，对应的收集器有哪些？ (拼多多) </font>
>

> + <font style="color:#000000;"> 讲讲你知道的垃圾回收算法 (字节跳动) </font>
>

> + <font style="color:#000000;"> Java对象的回收方式，回收算法。 (字节跳动) </font>
>

> + <font style="color:#000000;"> JVM垃圾收集算法与收集器有哪些？ (京东) </font>
>

> + <font style="color:#000000;"> JVM场景问题， 标记清除多次后老年代产生内存碎片，引起full gc，接下来可能发生什么问题？ (美团) </font>
>

> + <font style="color:#000000;"> 分代垃圾回收过程？ (美团) </font>
>

> + <font style="color:#000000;"> GC如何分代的？各代用什么算法回收？ (美团) </font>
>


## **<font style="color:#000000;">1. 对象可以被回收的条件：</font>**
<font style="color:#000000;">在Java中，一个对象只有当它不再被任何强引用所指向时才能被视为可回收。具体来说，通过可达性分析算法（根搜索算法）来判断对象是否存活，即从一系列GC Roots（如虚拟机栈中的局部变量表、方法区静态变量、常量引用等）出发，无法通过任何路径访问到的对象被认为是不可达的，也就是垃圾对象。</font>

## **<font style="color:#000000;">2. 常见的垃圾收集算法及其特点：</font>**
1. <font style="color:#000000;"> </font>**<font style="color:#000000;">标记-清除（Mark-Sweep）</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">原理：首先遍历所有对象并标记活动对象，然后清除未被标记的对象。</font>
    - <font style="color:#000000;">优缺点：实现简单，但会产生大量不连续的空间碎片，可能导致内存分配效率降低。</font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">复制（Copying）</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">原理：将堆内存分为两块，每次只使用其中一块进行对象分配，当这块区域满时，将存活的对象复制到另一块区域，并清理已使用的区域。</font>
    - <font style="color:#000000;">优缺点：不会产生碎片，但需要额外的一半空间，且对于大对象处理效率较低。</font>
3. <font style="color:#000000;"> </font>**<font style="color:#000000;">标记-整理（Mark-Compact）</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">原理：类似于标记-清除，但在清除后对存活对象进行整理，移动至内存的一端，从而消除碎片。</font>
    - <font style="color:#000000;">优点：解决了内存碎片问题，但需要移动对象，STW时间可能较长。</font>
4. <font style="color:#000000;"> </font>**<font style="color:#000000;">分代收集（Generational Collection）</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">原理：根据对象生命周期的不同，将堆划分为新生代和老年代。新生代采用复制算法（Eden和Survivor），老年代通常采用标记-压缩或标记-清除算法。</font>
    - <font style="color:#000000;">优点：利用了大部分对象“朝生夕死”的特性，提高了垃圾回收效率。</font>

## **<font style="color:#000000;">3. JVM的分代垃圾回收机制：</font>**
+ **<font style="color:#000000;">新生代</font>**<font style="color:#000000;">：主要针对新创建的对象，采用复制算法，当 Eden 区和 Survivor 区空间不足时触发 Minor GC。</font>
+ **<font style="color:#000000;">老年代</font>**<font style="color:#000000;">：经过多次Minor GC仍存活的对象会被晋升到老年代，当老年代空间不足时会触发Major GC或者Full GC。</font>

## **<font style="color:#000000;">4. 判定对象死亡的方法：</font>**
+ <font style="color:#000000;">引用计数法：每个对象都有一个引用计数器，当引用计数为0时，对象被认为可以回收。但由于循环引用等问题，Java并没有采用此方法。</font>
+ <font style="color:#000000;">可达性分析（Reachability Analysis）：从GC Roots开始，通过引用链遍历整个对象图，不可到达的对象将被标记为可回收。</font>

## **<font style="color:#000000;">5. 垃圾回收器与对应的算法：</font>**
+ <font style="color:#000000;">新生代：</font>**<font style="color:#000000;">Serial/ParNew（标记-复制）、Parallel Scavenge（标记-复制）。</font>**
+ <font style="color:#000000;">老年代：</font>**<font style="color:#000000;">Serial Old（标记-整理）、Parallel Old（标记-整理）、CMS（并发标记-清除）</font>**<font style="color:#000000;">。</font>
+ <font style="color:#000000;">G1垃圾收集器：在整体上采用标记-压缩算法，但在逻辑上分区进行处理，具有分代收集的特点。</font>

## **<font style="color:#000000;">6. JDK版本及当前主流回收算法：</font>**
+ <font style="color:#000000;">JDK8以前，使用Parallel Scavenge / Parallel Old或CMS作为默认的老年代收集器。</font>
+ <font style="color:#000000;">JDK9以后，G1成为默认的老年代收集器。</font>
+ <font style="color:#000000;">JDK11及以上版本引入ZGC和Shenandoah，它们提供了更低的暂停时间和对大内存更好的支持。</font>

## **<font style="color:#000000;">7. 选择合适的垃圾收集器原则：</font>**
<font style="color:#000000;">根据应用的需求，如果关注系统吞吐量，可以选择Parallel系列；如果关注延迟，减少停顿时间，可以选择CMS或G1；对于超大内存和极低停顿要求的应用，可以考虑ZGC或Shenandoah。</font>

## **<font style="color:#000000;">8. 关于老年代产生内存碎片的问题：</font>**
<font style="color:#000000;">标记清除多次后老年代产生内存碎片，导致Full GC频繁，这不仅会延长系统的停顿时间，还会影响后续的内存分配效率。解决方法是采用标记-整理算法（例如Serial Old、Parallel Old），或者使用G1收集器，它能更有效地管理内存空间，避免过大的内存碎片。若碎片问题严重，可能需要调整JVM参数、优化程序设计，或者更换适合的垃圾收集器。</font>

# <font style="color:#000000;">5.4 其 它</font>
> + <font style="color:#000000;"> GC回收的是哪部分的垃圾？（vivo） </font>
>

> + <font style="color:#000000;"> 什么是内存泄漏和什么是内存溢出（陌陌） </font>
>

> + <font style="color:#000000;"> Java存在内存泄漏吗，内存泄漏的场景有哪些，如何避免（百度） </font>
>

> + <font style="color:#000000;"> SafePoint 是什么（360安全） </font>
>

> + <font style="color:#000000;"> Java 中会存在内存泄漏吗，简述一下？（猎聘） </font>
>

> + <font style="color:#000000;"> 垃圾回收的优点和原理，并考虑 2 种回收机制？基本原理是什么？（瓜子） </font>
>

> + <font style="color:#000000;"> 什么是分布式垃圾回收（DGC）？它是如何工作的？（瓜子） </font>
>

> + <font style="color:#000000;"> 强引用、软引用、弱引用、虚引用的区别？（字节跳动） </font>
>

> + <font style="color:#000000;"> 垃圾回收的优点和原理。 (蚂蚁金服) </font>
>

> + <font style="color:#000000;"> 如何解决内存碎片的问题？ (阿里) </font>
>

> + <font style="color:#000000;"> 垃圾回收机制等 (支付宝) </font>
>

> + <font style="color:#000000;"> Java GC机制？GC Roots有哪些？ (拼多多) </font>
>

> + <font style="color:#000000;"> JVM怎样判断一个对象是否可回收，怎样的对象才能作为GC root (腾讯) </font>
>

> + <font style="color:#000000;"> 什么是Full GC？GC? major GC? stop the world (腾讯) </font>
>

> + <font style="color:#000000;"> System.gc()和RunTime.gc()会做什么事情？ (字节跳动) </font>
>

> + <font style="color:#000000;"> Java GC机制？GC Roots有哪些？ (字节跳动) </font>
>

> + <font style="color:#000000;"> 哪些部分可以作为GC Root？ (字节跳动) </font>
>

> + <font style="color:#000000;"> Java GC机制？GC Roots有哪些？ (抖音) </font>
>

> + <font style="color:#000000;"> Java GC机制？GC Roots有哪些？ (京东) </font>
>

> + <font style="color:#000000;"> GC是什么？为什么要有GC？ (美团) </font>
>

> + <font style="color:#000000;"> 简述Java垃圾回收机制 (美团) </font>
>

> + <font style="color:#000000;"> 如何判断一个对象是否存活？（或者GC对象的判定方法）  (美团) </font>
>

> + <font style="color:#000000;"> 垃圾回收的优点和原理。(美团) </font>
>

> + <font style="color:#000000;"> 垃圾回收器的基本原理是什么？垃圾回收器可以马上回收内存吗？(美团) </font>
>

> + <font style="color:#000000;"> 有什么办法主动通知虚拟机进行垃圾回收？  (美团) </font>
>

> + <font style="color:#000000;"> GC root如何确定，哪些对象可以作为GC Root? (美团) </font>
>

> + <font style="color:#000000;"> 你开发中使用过WeakHashMap吗？(京东) </font>
>


## **1. GC回收的是哪部分的垃圾？（vivo）**
<font style="color:#000000;">GC主要回收堆内存中不再使用的对象所占用的空间，这些对象就是垃圾。具体来说，它会回收那些无法通过可达性分析算法从GC Roots追溯到的对象。</font>

## **<font style="color:#000000;">2. 什么是内存泄漏和内存溢出（陌陌）</font>**
+ <font style="color:#000000;"> </font>**<font style="color:#000000;">内存泄漏（Memory Leak）</font>**<font style="color:#000000;">：程序在申请内存后，无法释放已不再使用的内存空间，导致这部分内存无法被再次使用。在Java中，虽然有垃圾回收机制，但如果存在长期引用（如静态集合类、线程局部变量等）持有了本该释放的对象，也会造成内存泄漏。 </font>
+ <font style="color:#000000;"> </font>**<font style="color:#000000;">内存溢出（Memory Overflow）</font>**<font style="color:#000000;">：程序运行过程中向系统申请的内存超过了系统能提供的最大值，导致系统无法分配更多的内存给程序，从而抛出OutOfMemoryError异常。例如，创建大量大对象或递归调用栈过深等情况可能导致内存溢出。 </font>

## **<font style="color:#000000;">3. Java存在内存泄漏吗，内存泄漏的场景有哪些，如何避免（百度）</font>**
<font style="color:#000000;">Java中确实存在内存泄漏的情况，常见场景包括：</font>

+ <font style="color:#000000;">静态集合类持有对象引用，使得对象无法被垃圾回收器回收。</font>
+ <font style="color:#000000;">线程资源未正确关闭或清理，导致相关资源长时间不被释放。</font>
+ <font style="color:#000000;">注册监听器后未及时注销，持续持有外部类的引用。</font>
+ <font style="color:#000000;">内部类对外部类的隐式引用等。</font>

<font style="color:#000000;">避免内存泄漏的方法：</font>

+ <font style="color:#000000;">使用完对象后设置引用为null，尤其是在循环引用的情况下。</font>
+ <font style="color:#000000;">及时关闭并清理不再使用的资源，如数据库连接、文件流等。</font>
+ <font style="color:#000000;">注意监听器注册与注销，确保资源得到及时释放。</font>
+ <font style="color:#000000;">合理设计数据结构和类关系，避免出现不必要的强引用链。</font>

## **<font style="color:#000000;">4. SafePoint 是什么（360安全）</font>**
<font style="color:#000000;">SafePoint是JVM中的一种特殊位置，在这个位置上执行垃圾回收不会对程序状态产生影响。当JVM需要进行垃圾回收或者进行其他操作时，会首先将所有线程暂停至最近的SafePoint，然后开始执行GC或者其他操作。在Java HotSpot虚拟机中，SafePoint通常出现在指令序列中的空闲区域，如方法调用返回前、循环跳转处、抛出异常的位置等。</font>

## **<font style="color:#000000;">5. Java 中会存在内存泄漏吗，简述一下？（猎聘）</font>**
<font style="color:#000000;">Java语言本身的设计具有自动垃圾回收机制，但在特定情况下仍可能出现内存泄漏。例如，如果一个对象虽然已经不再被应用程序使用，但由于某些原因仍然保持了对该对象的引用，那么垃圾回收器就无法回收它，这种现象即为Java内存泄漏。</font>



## **<font style="color:#000000;">6. 垃圾回收的优点和原理，并考虑2种回收机制；基本原理是什么？（瓜子）</font>**
<font style="color:#000000;">优点：</font>

+ <font style="color:#000000;">自动管理内存，减轻程序员负担。</font>
+ <font style="color:#000000;">避免因手动管理内存不当而导致的错误和性能问题。</font>
+ <font style="color:#000000;">提高系统的稳定性和可用性。</font>

<font style="color:#000000;">基本原理： </font><font style="color:#000000;">垃圾回收的基本原理是基于可达性分析算法，通过一系列被称为GC Roots的对象作为起点，遍历整个对象图，不可达的对象被视为垃圾。</font>

<font style="color:#000000;">两种常见的回收机制：</font>

+ **<font style="color:#000000;">标记-清除（Mark-Sweep）</font>**<font style="color:#000000;">：先标记所有活动对象，然后清除未被标记的对象。</font>
+ **<font style="color:#000000;">复制（Copying）</font>**<font style="color:#000000;">：将内存分为两部分，每次只使用其中一部分，当这部分满时，把存活的对象复制到另一部分，然后清空已使用的部分。</font>

## **<font style="color:#000000;">7. 什么是分布式垃圾回收（DGC），它是如何工作的？（瓜子）</font>**
<font style="color:#000000;">分布式垃圾回收主要用于分布式环境下的JVM实例，比如JVM集群。DGC可以跨越多个JVM实例进行垃圾回收，确保跨JVM引用的对象能够在适当的时候被正确回收。其工作原理通常涉及引用计数、跨进程引用跟踪以及远程对象的生命周期管理等技术，以协调各个JVM之间的垃圾回收行为。</font>

## **<font style="color:#000000;">8. 强引用、软引用、弱引用、虚引用的区别？（字节跳动）</font>**
+ **<font style="color:#000000;">强引用（Strong Reference）</font>**<font style="color:#000000;">：最普遍的引用类型，只要强引用存在，垃圾回收器就不会回收对象。即使发生OOM也不会回收这类对象。</font>
+ **<font style="color:#000000;">软引用（Soft Reference）</font>**<font style="color:#000000;">：当内存不足时，垃圾回收器会回收软引用指向的对象。主要用于实现内存敏感的高速缓存。</font>
+ **<font style="color:#000000;">弱引用（Weak Reference）</font>**<font style="color:#000000;">：无论内存是否充足，只要进行垃圾回收，就会回收弱引用指向的对象。主要用于防止内存泄漏，但不能保证对象始终存在。</font>
+ **<font style="color:#000000;">虚引用（Phantom Reference）</font>**<font style="color:#000000;">：又称幽灵引用，仅用于跟踪对象被垃圾回收的状态，无法通过虚引用访问对象，也无法阻止对象被回收。</font>

## <font style="color:#000000;">9. 垃圾回收的优点</font>
<font style="color:#000000;">垃圾回收的优点主要包括自动化内存管理、减少内存错误和提高系统稳定性。</font>

## <font style="color:#000000;">10. 如何解决内存碎片的问题？ (阿里) </font>
<font style="color:#000000;">解决内存碎片问题可以通过压缩（如标记-整理算法）、分代收集（年轻代采用复制算法减少老年代碎片）等方式。</font>

## <font style="color:#000000;">11. GC root如何确定，哪些对象可以作为GC Root? (美团) </font>
<font style="color:#000000;">GC Roots包括虚拟机栈中的局部变量表引用的对象、方法区常量池引用的对象、类静态属性引用的对象、正在执行的异常处理器引用的对象等。</font>

## <font style="color:#000000;">12. 什么是Full GC？GC? major GC? stop the world (腾讯) </font>
<font style="color:#000000;">Full GC是对整个堆内存（包括新生代和老年代）进行的垃圾回收，Major GC一般指对老年代的回收，Stop the World指的是在执行垃圾回收时，所有应用线程停止工作等待垃圾回收完成的过程。</font>

## <font style="color:#000000;">13. System.gc()和RunTime.gc()会做什么事情？ (字节跳动) </font>
<font style="color:#000000;">System.gc()和Runtime.gc()方法只是建议JVM执行垃圾回收，但并不保证立即进行垃圾回收。</font>

## <font style="color:#000000;">14. 你开发中使用过WeakHashMap吗？(京东) </font>
<font style="color:#000000;">WeakHashMap是一种特殊的Map实现，它允许键被垃圾回收器回收，当键不存在时对应的值也将被移除。</font>



> 更新: 2024-01-13 16:36:21  
原文: [https://www.yuque.com/vip6688/neho4x/itua6udze8dsmt9a](https://www.yuque.com/vip6688/neho4x/itua6udze8dsmt9a)
>



> 更新: 2024-11-25 09:18:38  
> 原文: <https://www.yuque.com/neumx/laxg2e/f71ed7c0589d1132ed28e82879b00031>