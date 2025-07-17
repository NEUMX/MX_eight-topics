# Java常见线上问题总结

<font style="color:rgb(8,15,23);">绝⼤多数 Java 线上问题从表象来看通常可以归纳为 4 个⽅⾯：</font>**<font style="color:rgb(8,15,23);">CPU</font>**<font style="color:rgb(8,15,23);">、</font>**<font style="color:rgb(8,15,23);">内存</font>**<font style="color:rgb(8,15,23);">、</font>**<font style="color:rgb(8,15,23);">磁盘</font>**<font style="color:rgb(8,15,23);">、</font>**<font style="color:rgb(8,15,23);">⽹ </font>**

**<font style="color:rgb(8,15,23);">络</font>**<font style="color:rgb(8,15,23);">。⽐如，应⽤上线 </font>

<font style="color:rgb(8,15,23);">后突然 CPU 使⽤率 99%、内存泄漏、STW 时间过⻓，这些问题通常可以分为两⼤类： </font>

**<font style="color:rgb(8,15,23);">系统异常 </font>**<font style="color:rgb(8,15,23);">（CPU 占⽤率过⾼、磁盘使⽤率 100%、系统可⽤内存低等） </font>

**<font style="color:rgb(8,15,23);">业务异常 </font>**<font style="color:rgb(8,15,23);">（服务运⾏⼀段时间⾃动退出、服务间调⽤时间过⻓、多线程并发异常、死 </font>

## **<font style="color:rgb(8,15,23);">如何定位问题 </font>**
<font style="color:rgb(8,15,23);">解决问题的第⼀步是定位问题，因为只有定位到了问题产⽣的原因，才能准确的抉择出 </font>

<font style="color:rgb(8,15,23);">解决⽅案，排查 </font>

<font style="color:rgb(8,15,23);">⼿段⼀般包括以下⼏项，也可以将此理解为排查顺序： </font>

<font style="color:rgb(8,15,23);">1. 业务⽇志分析排查 </font>

<font style="color:rgb(8,15,23);">2. APM 分析排查 </font>

<font style="color:rgb(8,15,23);">3. 外部环境排查 </font>

<font style="color:rgb(8,15,23);">4. 应⽤服务排查 </font>

<font style="color:rgb(8,15,23);">5. 云⼚商或运营商问题排查 </font>

### **<font style="color:rgb(8,15,23);">业务⽇志分析排查 </font>**
<font style="color:rgb(8,15,23);">通常情况下，⼤部分错误信息都会在⽇志上有所体现，⽐如看看下⾯这段代码 </font>

![](./img/np2ER4LJeGkc1Uev/1725728425406-1898fa1a-1df3-4217-9989-65612d6525ed-180825.png)

<font style="color:rgb(8,15,23);">这段代码使⽤了 并发流 将数据加⼊到⼀个 List 中，再运⾏过程中，抛出了 </font>

<font style="color:rgb(8,15,23);">ArrayIndexOutOfBoundsException ，具体异常栈信息如下： </font>![](./img/np2ER4LJeGkc1Uev/1725728444414-3759a81b-45c4-4cee-a5e4-3616352baff3-414592.png)

<font style="color:rgb(8,15,23);">通过⽇志可以看到出错的位置在 TaskServiceImpl.java ⽂件的第 75⾏代码上，错误信 </font>

<font style="color:rgb(8,15,23);">息是 java.lang.ArrayIndexOutOfBoundsException:163 ，这就是问题定位，整体描述 </font>

<font style="color:rgb(8,15,23);">出来就是： 在TaskServiceImpl.java⽂件中第 75⾏代码上出现了数组越界异常，引起异常的原因是在调⽤ java.util.ArrayList#add()⽅法时因为并发请求导致没有动态扩展数组容量 。 </font>

<font style="color:rgb(8,15,23);">为什么我们能够直接描述出来是在调⽤ ArrayList#add() ⽅法时因为并发请求导致的 </font>

<font style="color:rgb(8,15,23);">没有动态扩展数组容量呢？因为我们结合 ArrayList 的内部实现原理及动态扩容的特性，可以知道在单线程的情况下，代码是串⾏执⾏，ArrayList 内的数组都会是先扩容再插⼊，所以不会出现数组越界的错误，既然单线程不会引起该错误，那⼀定就是多线程并发造成的了，解决⽅案就是给代码加锁或者由并发改串⾏，就是这么简单，这就是技能素养的体现。 </font>

<font style="color:rgb(8,15,23);">通过上⾯的分析，教育了我们：⼀定要在关键代码逻辑位置输出相关⽇志，尤其是在代 </font>

<font style="color:rgb(8,15,23);">码发⽣异常的时候，定要将⽇志输出到⽂件中，只有这样，才更利于我们的排查。 </font>

### **<font style="color:rgb(8,15,23);">APM 分析排查 </font>**
<font style="color:rgb(8,15,23);">APM，全称 Application Performance Management，应⽤性能管理，⽬的是通过各 </font>

<font style="color:rgb(8,15,23);">种探针采集数据， 收集关键指标，同时搭配数据呈现以实现对应⽤程序性能管理和故障管理的系统化解决⽅案。通过分布式链路调⽤跟踪系统，通过在系统请求中透传 trace-id，将所有相关⽇志进⾏聚合，然后⽇志统⼀采集和分析后，以图形化的形式展示给⼯程师们，⽽他们在排查问题的时候，可以简单粗暴且直观的调度出问题最根本的原因。 </font>

<font style="color:rgb(8,15,23);">业务⽇志较优于解决单体服务异常，但现在的应⽤通常使⽤的是分布式架构，⽽在分布式系统中，仅通过单服务节点的⽇志，很难将错误信息与请求链路关联在⼀起，也就是通过系统中某个服务的异常⽇志信息很难定位到问题的根本原因。并且我们还需要关注各服务执⾏过程中的耗时情况，及时的发现异常服务，并根据异常信息进⾏修复。要满⾜这种需求，仅通过分析单个服务的⽇志信息是不够的，此时则需要 APM 进⾏全链路分析，通过请求链路监控，实时的发现链路中相关服务的异常情况。</font>

![](./img/np2ER4LJeGkc1Uev/1725728658898-6f1c4709-5cd1-43ca-aa53-494db611986d-484993.png)

<font style="color:rgb(8,15,23);">⽐如我们使⽤Skywalking，进⼊到 Skywalking 的控制台查看的链路信息就是这样的： </font>

<font style="color:rgb(8,15,23);">⽬前市场上使⽤较多的链路跟踪⼯具有如下⼏个： </font>

<font style="color:rgb(8,15,23);">Apache Skywalking：</font>[<font style="color:rgb(10,108,255);">https://skywalking.apache.org/</font>](https://skywalking.apache.org/)<font style="color:rgb(10,108,255);"> </font>

<font style="color:rgb(8,15,23);">Pinpoint：</font>[<font style="color:rgb(10,108,255);">https://pinpoint.com/product/for-engineers</font>](https://pinpoint.com/product/for-engineers)<font style="color:rgb(10,108,255);"> </font>

<font style="color:rgb(8,15,23);">SpringCloud Zipkin： </font>

[<font style="color:rgb(10,108,255);">https://docs.spring.io/spring-cloud-sleuth/docs/current-SNAPSHOT/referenc</font>](https://docs.spring.io/spring-cloud-sleuth/docs/current-SNAPSHOT/referenc)<font style="color:rgb(10,108,255);"> </font><font style="color:rgb(8,15,23);">e/html/#sending-spans-to-zipkin </font>

### **<font style="color:rgb(8,15,23);">物理环境排查 </font>**
<font style="color:rgb(8,15,23);">物理环境是指⼯程所依附的物理环境，⽐如服务器、宿主机、容器等，细分为服务器负 </font>

<font style="color:rgb(8,15,23);">载、CPU、内存、磁盘、⽹络⼏个⽅⾯。 </font>

**<font style="color:rgb(8,15,23);">CPU 分析 </font>**

<font style="color:rgb(8,15,23);">排查 CPU 的⽬的主要是查看服务器 CPU 的使⽤率， 使⽤top 命令分析 CPU 使⽤情况 </font>

**<font style="color:rgb(8,15,23);">内存分析 </font>**

<font style="color:rgb(8,15,23);">使⽤free -m 命令查看内存使⽤情况 </font>

**<font style="color:rgb(8,15,23);">磁盘分析 </font>**

<font style="color:rgb(8,15,23);">使⽤df -h、iostat、lsof 等命令查看磁盘 IO 情况，找到读写异常的进程</font>

**<font style="color:rgb(8,15,23);">⽹络分析 </font>**

<font style="color:rgb(8,15,23);">使⽤dstat、vmstat 等命令查看⽹络流量、TCP 连接等情况，分析异常流量 </font>

### **<font style="color:rgb(8,15,23);">应⽤服务排查 </font>**
<font style="color:rgb(8,15,23);">应⽤排查，排查应⽤本身最有可能引发的问题，针对各种场景进⾏对应分析 </font>

**<font style="color:rgb(8,15,23);">CPU 分析 </font>**

<font style="color:rgb(8,15,23);">使⽤jstack 等命令进⾏JVM 分析 </font>

**<font style="color:rgb(8,15,23);">内存分析 </font>**

<font style="color:rgb(8,15,23);">使⽤jmap 等命令分析内存使⽤情况 </font>

### **<font style="color:rgb(8,15,23);">云⼚商或运营商问题排查 </font>**
<font style="color:rgb(8,15,23);">排查到了这⼀步的话，只需关注云⼚商或运营商官⽅公告即可。 </font>

<font style="color:rgb(8,15,23);">假设我们从业务⽇志、APM 分析中都没分析出问题所在，那么此时基本只能连接到⽣ </font>

<font style="color:rgb(8,15,23);">产环境中进⾏排查了。根据上⾯的外部排查⽅案，这⾥简单复习下常⽤的 Linux 分析命令以便从系统层⾯上分析。 </font>

## **<font style="color:rgb(8,15,23);">常⽤Linux 分析命令 </font>**
<font style="color:rgb(8,15,23);">针对外部环境排查，需要使⽤的常⽤Linux 分析命令包括 : top、free、df、lsof、dstat、 </font>

<font style="color:rgb(8,15,23);">vmstat 等，简单介绍如下： </font>

### **<font style="color:rgb(8,15,23);">CPU </font>**
<font style="color:rgb(8,15,23);">CPU 使⽤率是衡量系统繁忙程度的重要指标。但是 </font>**<font style="color:rgb(8,15,23);">CPU 使⽤率的安全阈值是相对的， </font>**

**<font style="color:rgb(8,15,23);">取决于你的系统的IO密集型还是计算密集型</font>**<font style="color:rgb(8,15,23);">。⼀般计算密集型应⽤CPU 使⽤率偏⾼load 偏低，IO 密集 型相反。top 命令是 Linux 下常⽤的 CPU 性能分析⼯具，能够实时显示系统中各个进程的资源占⽤状况，常⽤于服务端性能分析。 </font>

:::info
**top**

:::

![](./img/np2ER4LJeGkc1Uev/1725728850465-fa1567e7-039e-45b9-9ca6-145a4dde8ef1-192695.png)

<font style="color:rgb(8,15,23);">top 命令显示了各个进程 CPU 使⽤情况，⼀般 CPU 使⽤率从⾼到低排序展示输出。其中 </font>**<font style="color:rgb(8,15,23);">Load Average</font>**<font style="color:rgb(8,15,23);"> 显示最近 1 分钟、5 分钟和 15 分钟的系统平均负载，上图各值为 3.4、 3.31、3.46。 </font>

<font style="color:rgb(8,15,23);">我们⼀般会关注 CPU 使⽤率最⾼的进程，正常情况下就是我们的应⽤主进程。第 </font>

<font style="color:rgb(8,15,23);">七⾏以下：各进程的状态监控，参数说明： </font>

**<font style="color:rgb(8,15,23);">PID</font>**<font style="color:rgb(8,15,23);"> : 进程 id </font>

**<font style="color:rgb(8,15,23);">USER</font>**<font style="color:rgb(8,15,23);"> : 进程所有者的⽤户名 </font>

**<font style="color:rgb(8,15,23);">PR</font>**<font style="color:rgb(8,15,23);"> : 进程优先级 </font>

**<font style="color:rgb(8,15,23);">NI</font>**<font style="color:rgb(8,15,23);"> : nice 值。负值表示⾼优先级，正值表示低优先级 </font>

**<font style="color:rgb(8,15,23);">VIRT</font>**<font style="color:rgb(8,15,23);"> : 进程使⽤的虚拟内存总量，单位 kbSHR : 共享内存⼤⼩ </font>

**<font style="color:rgb(8,15,23);">%CPU</font>**<font style="color:rgb(8,15,23);"> : 上次更新到现在的 CPU 时间占⽤百分⽐ </font>

**<font style="color:rgb(8,15,23);">%MEM</font>**<font style="color:rgb(8,15,23);"> : 进程使⽤的物理内存百分⽐ </font>

**<font style="color:rgb(8,15,23);">TIME+</font>**<font style="color:rgb(8,15,23);"> : 进程使⽤的 CPU 时间总计，单位 1/100 秒 </font>

**<font style="color:rgb(8,15,23);">COMMAND</font>**<font style="color:rgb(8,15,23);"> : 命令名称、命令⾏ </font>

### **<font style="color:rgb(8,15,23);">内存 </font>**
<font style="color:rgb(8,15,23);">内存是排查线上问题的重要参考依据，free 是显示的当前内存的使⽤，-h 表示⼈类可 </font>

<font style="color:rgb(8,15,23);">读性 </font>

:::info
**<font style="color:rgb(8,15,23);">free -h </font>**

:::

![](./img/np2ER4LJeGkc1Uev/1725728973988-ab36fe9f-38e9-427f-9004-093837fb5835-898065.png)

<font style="color:rgb(8,15,23);">参数说明： </font>

**<font style="color:rgb(8,15,23);">total</font>**<font style="color:rgb(8,15,23);"> ：内存总数 </font>

**<font style="color:rgb(8,15,23);">used</font>**<font style="color:rgb(8,15,23);">：已经使⽤的内存数 </font>

**<font style="color:rgb(8,15,23);">free</font>**<font style="color:rgb(8,15,23);">：空闲的内存数 </font>

**<font style="color:rgb(8,15,23);">shared</font>**<font style="color:rgb(8,15,23);">：被共享使⽤的物理内存⼤⼩ </font>

**<font style="color:rgb(8,15,23);">buffers/buffer</font>**<font style="color:rgb(8,15,23);">：被 buffer 和 cache 使⽤的物理内存⼤⼩ </font>

**<font style="color:rgb(8,15,23);">available</font>**<font style="color:rgb(8,15,23);">： 还可以被应⽤程序使⽤的物理内存⼤⼩ </font>

### **<font style="color:rgb(8,15,23);">磁盘 </font>**
<font style="color:rgb(8,15,23);">磁盘满了很多时候会引发系统服务不可⽤等问题 </font>

:::info
**<font style="color:rgb(8,15,23);">df -h</font>**

:::

![](./img/np2ER4LJeGkc1Uev/1725729014293-9f35cce6-7a1c-4f53-9270-e5c7f58b44c6-263592.png)

### **<font style="color:rgb(8,15,23);">⽹络 </font>**
<font style="color:rgb(8,15,23);">dstat 是⼀个可以取代 vmstat，iostat，netstat 和 ifstat 这些命令的多功能产品。 </font>

:::info
**<font style="color:rgb(8,15,23);">dstat</font>**<font style="color:rgb(8,15,23);"> </font>

:::

![](./img/np2ER4LJeGkc1Uev/1725729043489-a4cd44b6-f689-4076-9e45-fa62190113b1-676070.png)

<font style="color:rgb(8,15,23);">默认情况下，dstat 每秒都会刷新数据。需要注意的是报告的第⼀⾏，由于 dstat 会通过上⼀次的报告来给出⼀个总结，所以第⼀次运⾏时是没有平均值和总值的相关数据。 </font>

<font style="color:rgb(8,15,23);">默认输出解释： </font>

**<font style="color:rgb(8,15,23);">CPU状态</font>**<font style="color:rgb(8,15,23);">：显示了⽤户，系统和空闲部分，便于分析 CPU 当前的使⽤状况 </font>

**<font style="color:rgb(8,15,23);">磁盘统计</font>**<font style="color:rgb(8,15,23);">：磁盘的读写操作，显示磁盘的读、写总数。 </font>

**<font style="color:rgb(8,15,23);">⽹络统计</font>**<font style="color:rgb(8,15,23);">：⽹络设备发送和接受的数据，显示了⽹络收、发数据总数。 </font>

**<font style="color:rgb(8,15,23);">分⻚统计</font>**<font style="color:rgb(8,15,23);">：系统的分⻚活动。 </font>

**<font style="color:rgb(8,15,23);">系统统计</font>**<font style="color:rgb(8,15,23);">：这⼀项显示的是中断（int）和上下⽂切换（csw）。 </font>

<font style="color:rgb(8,15,23);">到了这⼀步，如果 CPU、内存、磁盘、⽹络这四个地⽅都没有问题的话，那就进⼊了 </font>

<font style="color:rgb(8,15,23);">应⽤本身的问题排查阶段。下⼀步就该使⽤jvm 命令进⾏问题定位了，但很多研发可能由于⾃身⼯作经验不⾜、对 Java 内存模型理解不深、尚未掌握 Jvm 排查命令等原因对 Jvm 相关排查畏⼿畏脚，不够⾃信，进⽽影响排查进度。针对这种情况，本⽂推荐⼀款开源的 Java 诊断⼯具，对 Jvm 不熟的研发可以尝试学习使⽤下。相对 Jvm 命令来说简单多了 </font>

## **<font style="color:rgb(8,15,23);">Arthas 诊断命令 </font>**
<font style="color:rgb(8,15,23);">Arthas 是 Alibaba 开源的 Java 诊断⼯具，深受开发者喜爱。 </font>

<font style="color:rgb(8,15,23);">当你遇到以下类似问题⽽束⼿⽆策时，Arthas 可以帮助你解决 </font>

+ <font style="color:rgb(8,15,23);">这个类从哪个 jar 包加载的？为什么会报各种类相关的 Exception？ </font>
+ <font style="color:rgb(8,15,23);">我改的代码为什么没有执⾏到？难道是我没 commit？分⽀搞错了？ </font>
+ <font style="color:rgb(8,15,23);">遇到问题⽆法在线上 debug，难道只能通过加⽇志再重新发布吗？ </font>
+ <font style="color:rgb(8,15,23);">线上遇到某个⽤户的数据处理有问题，但线上同样⽆法 debug，线下⽆法重现！ </font>
+ <font style="color:rgb(8,15,23);">是否有⼀个全局视⻆来查看系统的运⾏状况？ </font>
+ <font style="color:rgb(8,15,23);">有什么办法可以监控到 JVM 的实时运⾏状态？ </font>
+ <font style="color:rgb(8,15,23);">怎么快速定位应⽤的热点，⽣成⽕焰图？ </font>

<font style="color:rgb(8,15,23);">Arthas⽀持 JDK 6+，⽀持 Linux、Mac、Winodws，采⽤命令⾏交互模式，同时提供 </font>

<font style="color:rgb(8,15,23);">丰富的 Tab ⾃动补全功能，进⼀步⽅便进⾏问题的定位和诊断。 </font>

### **<font style="color:rgb(8,15,23);">下载安装 </font>**
:::info
<font style="color:rgb(8,15,23);">curl -O </font>[<font style="color:rgb(8,15,23);">https://alibaba.github.io/arthas/arthas-boot.jar</font>](https://alibaba.github.io/arthas/arthas-boot.jar)<font style="color:rgb(8,15,23);"> </font>

:::

### **<font style="color:rgb(8,15,23);">启动 Arthas </font>**
:::info
<font style="color:rgb(8,15,23);">java -jar arthas-boot.jar</font>

:::

<font style="color:rgb(8,15,23);">arthas会列出已存在的 Java 进程，并提醒输入序号，键入回车，进入arthas 诊断界面。 </font>

![](./img/np2ER4LJeGkc1Uev/1725729252869-d619d7cf-556e-4262-af41-fed46dab3c6a-620744.png)

### **<font style="color:rgb(8,15,23);">开始诊断 	</font>**
<font style="color:rgb(8,15,23);">按照提醒，这里输入需要诊断的序号，输入回车，响应界面如下： </font>

![](./img/np2ER4LJeGkc1Uev/1725729232827-69795981-a5ab-45db-8e06-c19a2b3097e7-763947.png)

### **<font style="color:rgb(8,15,23);">查看 dashboard </font>**
<font style="color:rgb(8,15,23);">输入 dashboard【支持 Tab 补全命令】，输入回车，会展示当前系统的实时面板，按 </font>

<font style="color:rgb(8,15,23);">ctrl+c 可以中断执行。</font>

![](./img/np2ER4LJeGkc1Uev/1725729348863-80baf498-3ec4-4169-bc27-f6c676b3b252-372023.png)

<font style="color:rgb(8,15,23);">上图线上了当前系统对应的</font>**<font style="color:rgb(8,15,23);">线程 </font>**<font style="color:rgb(8,15,23);">、</font>**<font style="color:rgb(8,15,23);">内存（分代） </font>**<font style="color:rgb(8,15,23);">、GC 、Runtime 的各项信息。 </font>

<font style="color:rgb(8,15,23);">除了 dashboard 命令外， arthas 还有许多常用命令，这里挑一些做简单说明。 </font>

### **<font style="color:rgb(8,15,23);">arthas 常见命令介绍 </font>**
+ <font style="color:rgb(8,15,23);">jvm 查看当前 JVM 的信息 </font>
+ <font style="color:rgb(8,15,23);">thread 查看当前 JVM 的线程堆栈信息， -b 选项可以一键检测死锁 </font>
+ <font style="color:rgb(8,15,23);">trace 方法内部调用路径，并输出方法路径上的每个节点上耗时，服务间调用时间过长时使用 </font>
+ <font style="color:rgb(8,15,23);">stack 输出当前方法被调用的调用路径 </font>
+ <font style="color:rgb(8,15,23);">Jad 反编译指定已加载类的源码，反编译便于理解业务 </font>
+ <font style="color:rgb(8,15,23);">logger 查看和修改 logger，可以动态更新日志级别。 </font>

> <font style="color:rgb(10,108,255);">这里只列出常用命令，完整列表参考命令列表： </font>
>

> [<font style="color:rgb(10,108,255);">https://alibaba.github.io/arthas</font>](https://alibaba.github.io/arthas)
>


<font style="color:rgb(8,15,23);">arthas 使用上相对 JVM 命令简单很多，即便是工作年限不多的小伙伴学起来也很快。 </font>

<font style="color:rgb(8,15,23);">熟练使用 arthas 应该能诊断出大部分线上应用问题，但生产环境通常不允许擅自拷贝 jar 包，而且 arthas 会拖慢应用本身， 如果条件不允许，又该如何诊断呢？这边简单介绍下 jdk 自带的命令行工具。 	</font>

## **<font style="color:rgb(8,15,23);">JVM 问题定位命令 </font>**
<font style="color:rgb(8,15,23);">在 JDK 安装目录的 bin 目录下默认提供了很多有价值的命令行工具。每个小工具体积 </font>

<font style="color:rgb(8,15,23);">基本都比较小，因 为这些工具只是 jdk\lib\tools.jar 的简单封装。</font>

![](./img/np2ER4LJeGkc1Uev/1725729668049-9d5c9220-a286-44b4-a447-798af07a0503-660417.png)<font style="color:rgb(8,15,23);"> 其中，定位排查问题时最为常用命令包括： jps（进程）、jmap（内存）、jstack（线 </font>

<font style="color:rgb(8,15,23);">程）、jinfo（参 数）等。 </font>

+ <font style="color:rgb(8,15,23);">jps：查询当前机器所有 Java 进程信息 </font>
+ <font style="color:rgb(8,15,23);">jmap：输出某个 Java 进程内存情况 </font>
+ <font style="color:rgb(8,15,23);">jstack：打印某个 Java 线程的线程栈信息 </font>
+ <font style="color:rgb(8,15,23);">jinfo：用于查看 jvm 的配置参数 </font>

### **<font style="color:rgb(8,15,23);">jps </font>**
<font style="color:rgb(8,15,23);">jps 用于输出当前用户启动的所有进程 ID，当线上发现故障或者问题时，利用 jps 快 </font>

<font style="color:rgb(8,15,23);">速定位对应的 Java 进程 ID。 </font>

<font style="color:rgb(8,15,23);">参数解释： </font>

**<font style="color:rgb(8,15,23);">-m </font>**<font style="color:rgb(8,15,23);">：输出传入 main 方法的参数 </font>

**<font style="color:rgb(8,15,23);">-l</font>**<font style="color:rgb(8,15,23);">：输出完全的包名，应用主类名， jar 的完全路径名 </font>

:::info
<font style="color:rgb(8,15,23);">jps -m</font>

:::

![](./img/np2ER4LJeGkc1Uev/1725729765222-a910b659-f904-4d91-84f9-bf54e7e46720-723910.png)

<font style="color:rgb(8,15,23);">当然，我们也可以使用 Linux 提供的查询进程状态命令也能快速获取 Tomcat 服务的 </font>

<font style="color:rgb(8,15,23);">进程 id。比如： </font>

:::info
<font style="color:rgb(8,15,23);">ps -ef | grep tomcat </font>

:::

### **<font style="color:rgb(8,15,23);">jmap </font>**
<font style="color:rgb(8,15,23);">jmap（Java Memory Map）可以输出所有内存中对象的工具，甚至可以将 JVM 中的 </font>

<font style="color:rgb(8,15,23);">heap，以二进制输出成文本，使用方式如下： </font>

:::info
<font style="color:rgb(8,15,23);">jmap -heap pid 输出当前进程 JVM 堆内存新生代、老年代、持久代、 GC 算法等信息 </font>

:::

> **<font style="color:rgb(118,124,133);">注意： </font>**<font style="color:rgb(118,124,133);">pid 通过 jps 命令得知</font>
>


![](./img/np2ER4LJeGkc1Uev/1725729923101-daed5d9a-9f9d-4eb3-bdd4-55534246da01-920484.png)

**<font style="color:rgb(8,15,23);">jmap -histo</font>**<font style="color:rgb(8,15,23);">： </font>

:::info
<font style="color:rgb(8,15,23);">jmap -histo:live pid | head -n 20 输出当前进程内存中所有对象包含的大小</font>

:::

![](./img/np2ER4LJeGkc1Uev/1725730002091-efc63498-8c0d-425f-bdac-5ce6a205821b-092028.png)

<font style="color:rgb(8,15,23);">输出当前进程内存中所有对象实例数（instances）和大小（bytes），如果某个业务对 </font>

<font style="color:rgb(8,15,23);">象实例数和大小 存在异常情况，可能存在内存泄露或者业务设计方面存在不合理之处。 </font>

**<font style="color:rgb(8,15,23);">jmap -dump</font>**<font style="color:rgb(8,15,23);">： 		</font>

:::info
<font style="color:rgb(8,15,23);">jmap -dump:format=b,file=/apps/logs/gc/dump.hprof {pid} </font>

<font style="color:rgb(8,15,23);">以二进制形式输出当前内存的堆情况，便于导入 MAT 等工具进行分析情况 </font>

:::

<font style="color:rgb(8,15,23);">一般我们要求给 JVM 添加参数-XX:+Heap Dump On Out Of Memory Error OOM </font>

<font style="color:rgb(8,15,23);">确保应用发生 OOM 时 JVM 能够保存并 dump 出当前的内存镜像。当然如果你决定手动 dump 内存时， dump 操作占 据一定 CPU 时间片、内存资源、磁盘资源等，因此会带来 一定的负面影响。 </font>

<font style="color:rgb(8,15,23);">此外， dump 的文件可能比较大,一般我们可以考虑使用 zip 命令对文件进行压缩处 </font>

<font style="color:rgb(8,15,23);">理，这样在下载文件 时能减少带宽的开销。在下载 dump 文件完成之后，由于 dump </font>

<font style="color:rgb(8,15,23);">文件较大可将 dump 文件备份至制定 位置或者直接删除，以释放磁盘在这块的空间占 </font>

<font style="color:rgb(8,15,23);">用。 </font>

### **<font style="color:rgb(8,15,23);">jstack </font>**
<font style="color:rgb(8,15,23);">jstack 用于打印某个 Java 线程的线程栈信息，通常用以下三步分析： </font>

:::info
<font style="color:rgb(8,15,23);">printf '%x\n' tid 10 进制至 16 进制线程转换 </font>

<font style="color:rgb(8,15,23);">jstack pid | grep tid -C 30 --color </font>

<font style="color:rgb(8,15,23);">ps -mp 8278 -o THREAD,tid,time | head -n 40</font>

:::

<font style="color:rgb(8,15,23);">举个栗子，某 Java 进程 CPU 占用率高，我们想要定位到其中 CPU 占用率最高的线	程，如何定位？ </font>

<font style="color:rgb(8,15,23);">1、利用 top 命令可以查出占 CPU 最高的线程 pid </font>

:::info
<font style="color:rgb(8,15,23);">top -Hp pid </font>

:::

![](./img/np2ER4LJeGkc1Uev/1725730123423-60501e1c-8417-482e-bb64-9ac4d9ecc924-456381.png)

<font style="color:rgb(8,15,23);">2 、 占用率最高的线程 ID 为 22021，将其转换为 16 进制形式（因为 java native 线 </font>

<font style="color:rgb(8,15,23);">程以 16 进制形式输 出） </font>

:::info
<font style="color:rgb(8,15,23);">printf '%x\n' 22021 </font>

:::

![](./img/np2ER4LJeGkc1Uev/1725730139717-66b5fa55-c546-40eb-a2f0-316a02bcbf0e-692228.png)

<font style="color:rgb(8,15,23);">3 、利用 jstack 打印出 Java 线程调用栈信息 </font>

:::info
<font style="color:rgb(8,15,23);">jstack 21993 | grep '0x5605' -A 50 --color </font>

:::

![](./img/np2ER4LJeGkc1Uev/1725730197877-42ac1e77-0c8b-4852-b541-f597f70dec73-387894.png)

### **<font style="color:rgb(8,15,23);">jinfo </font>**
<font style="color:rgb(8,15,23);">jinfo 可以用来查看正在运行的 java 应用程序的扩展参数，包括 Java System 属性和 </font>

<font style="color:rgb(8,15,23);">JVM 命令行参数；也 可以动态的修改正在运行的 JVM 一些参数。 </font>

:::info
<font style="color:rgb(8,15,23);">jinfo pid</font>

:::

![](./img/np2ER4LJeGkc1Uev/1725730282024-6e95ec4c-7018-43ad-bf61-7da48ce02d1a-213453.png)

### **<font style="color:rgb(8,15,23);">jstat </font>**
<font style="color:rgb(8,15,23);">jstat 命令可以查看堆内存各部分的使用量，以及加载类的数量。 </font>

:::info
<font style="color:rgb(8,15,23);">jstat -gc pid </font>

:::

![](./img/np2ER4LJeGkc1Uev/1725730304828-a7e4df72-afd8-4e70-a6fe-9efa68d28160-804551.png)

### **<font style="color:rgb(8,15,23);">内存分析工具 MAT </font>**
<font style="color:rgb(8,15,23);">MAT（Memory Analyzer Tool），一个基于 Eclipse 的内存分析工具，是一个快速、 </font>

<font style="color:rgb(8,15,23);">功能丰富的 JAVA heap 分析工具，它可以帮助我们查找内存泄漏和减少内存消 耗。使用内存分析工具从众多的对象中进行 分析，快速的计算出在内存中对象的占用大小，看看是谁阻止了垃圾收集器的回收工作，并可以通过报表直观的查看到可能造成这种结果的对象。</font>

![](./img/np2ER4LJeGkc1Uev/1725730405435-239257d3-abcf-44ea-8d97-95edae80fee6-598174.png)

<font style="color:rgb(8,15,23);">右侧的饼图显示当前快照中最大的对象。单击工具栏上的柱状图，可以查看当前堆的类信息，包括类的 对象数量、浅堆（Shallow heap）、深堆（Retained Heap）。 </font>

**<font style="color:rgb(8,15,23);">浅堆</font>**<font style="color:rgb(8,15,23);">表示一个对象结构所占用内存的大小。 </font>**<font style="color:rgb(8,15,23);">深堆</font>**<font style="color:rgb(8,15,23);">表示一个对象被回收后，可以真实释 </font>

<font style="color:rgb(8,15,23);">放的内存大小。 </font>

+ <font style="color:rgb(8,15,23);">支配树（The Dominator Tree）： </font>

<font style="color:rgb(8,15,23);">列出了堆中最大的对象，第二层级的节点表示当被第一层级的节点所引用到的 </font>

<font style="color:rgb(8,15,23);">对象，当第一层级对 象被回收时，这些对象也将被回收。这个工具可以帮助 </font>

<font style="color:rgb(8,15,23);">我们定位对象间的引用情况，垃圾回收时候 的引用依赖关系 </font>

+ <font style="color:rgb(8,15,23);">Path to GC Roots </font>

<font style="color:rgb(8,15,23);">被 JVM 持有的对象，如当前运行的线程对象，被 systemclass loader 加载的 </font>

<font style="color:rgb(8,15,23);">对象被称为 GC Roots，从一个对象到 GC Roots 的引用链被称为 Path to GC </font>

<font style="color:rgb(8,15,23);">Roots， 通过分析 Path to GC Roots 可以找出 JAVA 的内存泄露问题，当 </font>

<font style="color:rgb(8,15,23);">程序不在访问该对象时仍存在到该对象的引用路径。</font>



> 更新: 2024-10-09 23:26:16  
原文: [https://www.yuque.com/vip6688/neho4x/ctm0s1hrbnrmbbqr](https://www.yuque.com/vip6688/neho4x/ctm0s1hrbnrmbbqr)
>



> 更新: 2024-11-25 09:41:19  
> 原文: <https://www.yuque.com/neumx/laxg2e/5787ab9a58a8375f97ed92aeffef46ae>