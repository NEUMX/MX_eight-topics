# Java手撕题

## 多线程交替打印：打印内容为ABC循环
## 或者交替打印一段话或者多线程顺序打印 ABCD
## 或者多线程交替打印 0-100  
下面这些都是只打印一次的

### 方法1：使用 join
```java
public class PrintABCWithJoin {
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> System.out.print("A"));
        Thread t2 = new Thread(() -> System.out.print("B"));
        Thread t3 = new Thread(() -> System.out.print("C"));

        try {
            t1.start();
            t1.join(); // 等待 t1 完成
            t2.start();
            t2.join(); // 等待 t2 完成
            t3.start();
            t3.join(); // 等待 t3 完成
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

### 方法2：使用 wait/notify（<font style="color:#DF2A3F;">循环打印 ABC，次数自己决定</font>）
```java
public class PrintABCWithWaitNotify {
    private static final Object lock = new Object();
    private static int state = 0; // 控制打印顺序
    private static final int TOTAL_COUNT = 10; // 设定打印轮数

    public static void main(String[] args) {
        Thread t1 = new Thread(() -> print("A", 0));
        Thread t2 = new Thread(() -> print("B", 1));
        Thread t3 = new Thread(() -> print("C ", 2)); // 加空格让格式更清晰

        t1.start();
        t2.start();
        t3.start();
    }

    private static void print(String s, int target) {
        for (int i = 0; i < TOTAL_COUNT; i++) { // 让每个线程循环打印 TOTAL_COUNT 次
            synchronized (lock) {
                while (state % 3 != target) { // 不符合条件就等待
                    try {
                        lock.wait();
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                }
                System.out.print(s); // 打印字符
                state++; // 修改状态
                lock.notifyAll(); // 唤醒所有等待线程
            }
        }
    }
}

```

### 方法3：使用 CountDownLatch
```java
import java.util.concurrent.CountDownLatch; // 导入 CountDownLatch 类，用于线程同步

public class PrintABCWithCountDownLatch {
    public static void main(String[] args) {
        // 创建第一个门闩，初始计数为1，用于控制 A 和 B 的顺序
        CountDownLatch latch1 = new CountDownLatch(1);
        // 创建第二个门闩，初始计数为1，用于控制 B 和 C 的顺序
        CountDownLatch latch2 = new CountDownLatch(1);

        // 创建线程 t1，负责打印 "A"
        Thread t1 = new Thread(() -> {
            // 直接打印 "A"
            System.out.print("A");
            // 减少 latch1 的计数（从 1 变为 0），通知等待 latch1 的线程（t2）可以继续
            latch1.countDown();
        });

        // 创建线程 t2，负责打印 "B"
        Thread t2 = new Thread(() -> {
            try {
                // 等待 latch1 的计数变为 0（即等待 t1 完成打印 "A"）
                latch1.await();
                // 当 latch1 计数为 0 时，打印 "B"
                System.out.print("B");
                // 减少 latch2 的计数（从 1 变为 0），通知等待 latch2 的线程（t3）可以继续
                latch2.countDown();
            } catch (InterruptedException e) {
                // 处理线程中断异常
                e.printStackTrace();
            }
        });

        // 创建线程 t3，负责打印 "C"
        Thread t3 = new Thread(() -> {
            try {
                // 等待 latch2 的计数变为 0（即等待 t2 完成打印 "B"）
                latch2.await();
                // 当 latch2 计数为 0 时，打印 "C"
                System.out.print("C");
            } catch (InterruptedException e) {
                // 处理线程中断异常
                e.printStackTrace();
            }
        });

        // 启动所有线程
        t1.start();
        t2.start();
        t3.start();
    }
}
```

---

## 单例模式：懒汉，饿汉，双重校验锁
**答案：**

### 懒汉模式
```java
public class Singleton {
    // 私有构造方法，防止外部实例化
    private Singleton() {}

    // 定义静态实例变量，未初始化
    private static Singleton instance;

    // 提供静态方法获取实例，延迟加载
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton(); // 首次调用时创建实例
        }
        return instance;
    }
}
```

### 饿汉模式
```java
public class Singleton {
    // 私有构造方法，防止外部实例化
    private Singleton() {}

    // 类加载时立即创建实例
    private static Singleton instance = new Singleton();

    // 提供静态方法获取实例
    public static Singleton getInstance() {
        return instance;
    }
}
```

### 双重校验锁
```java
public class Singleton {
    // 私有构造方法，防止外部实例化
    private Singleton() {}

    // 使用volatile防止指令重排序，保证线程安全
    private static volatile Singleton instance;

    // 提供静态方法获取实例
    public static Singleton getInstance() {
        // 第一次检查，避免不必要的同步
        if (instance == null) {
            synchronized (Singleton.class) { // 加锁，保证线程安全
                // 第二次检查，确保只有一个实例被创建
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

**说明：** 展示了三种单例模式的实现方式，分别是延迟加载、立即加载和线程安全的双重校验锁。

---

## 生产者-消费者模型：例如一个厨子10s生产一个，一个客人4s消费一个
**答案：**

### 生产者-厨子
```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

// 生产者类：模拟厨子
class Producer implements Runnable {
    private BlockingQueue<Integer> queue; // 共享队列

    public Producer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(10000); // 每10秒生产一个餐品
                queue.add(1); // 将餐品放入队列
                System.out.println("厨师放入了一个餐品");
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}

// 消费者类：模拟顾客
class Consumer implements Runnable {
    private BlockingQueue<Integer> queue; // 共享队列

    public Consumer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(4000); // 每4秒消费一个餐品
                queue.take(); // 从队列中取出餐品
                System.out.println("顾客消费了1个餐品");
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}

// 主类：执行生产和消费
public class Main {
    public static void main(String[] args) {
        // 创建容量为100的阻塞队列
        BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(100);

        // 创建生产者和消费者对象
        Producer producer = new Producer(queue); 
        Consumer consumer = new Consumer(queue);

        // 创建生产者线程和消费者线程
        Thread threadA = new Thread(producer); 
        Thread threadB = new Thread(consumer);

        // 启动生产者和消费者线程
        threadA.start(); 
        threadB.start(); 
    }
}

```

**说明：** 使用BlockingQueue实现生产者-消费者模型，模拟厨子慢生产、顾客快消费的场景。

---

##  用java手撕实现多线程定时调用  
要使用Java实现多线程定时调用，可以利用`ScheduledExecutorService`，它提供了一个比`Timer`和`TimerTask`更强大和灵活的线程池任务调度功能。下面是一个例子，演示如何使用Java实现多线程定时任务调用。

### 示例代码：
```java
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

public class MultiThreadScheduledTask {

    public static void main(String[] args) {
        // 创建一个定时任务调度线程池
        ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(5); // 线程池大小为5

        // 创建一个定时任务
        Runnable task = new Runnable() {
            @Override
            public void run() {
                // 获取当前时间的格式化日期
                long currentTime = System.currentTimeMillis();
                SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
                String formattedDate = sdf.format(new Date(currentTime));

                // 输出线程名称和格式化后的时间
                System.out.println(Thread.currentThread().getName() + " - 执行定时任务: " + formattedDate);
            }
        };

        // 定义初始延迟时间为1秒，每3秒执行一次
        scheduler.scheduleAtFixedRate(task, 1, 3, TimeUnit.SECONDS);
    }
}

```

### 代码解析：
1. **创建线程池**：`ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(5);`
    - 这个线程池是用来执行定时任务的，线程池的大小为5，即最多可以同时有5个任务并发执行。
2. **创建定时任务**：`Runnable task = new Runnable() {...}`
    - 定时任务定义为一个`Runnable`接口的实现，在`run()`方法中执行任务的内容，这里是打印线程名称和当前时间。
3. **调度任务**：`scheduler.scheduleAtFixedRate(task, 1, 3, TimeUnit.SECONDS);`
    - `scheduleAtFixedRate`方法用于按照固定的时间间隔执行任务，参数包括：
        * `task`：要执行的任务。
        * `1`：初始延迟1秒执行任务。
        * `3`：每3秒执行一次任务。
        * `TimeUnit.SECONDS`：时间单位为秒。

## 动态代理（JDK动态代理）
**答案：**

##### 卖票接口
```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

// 卖票接口
interface SellTickets {
    void sell();
}

// 火车站类（目标类）
class TrainStation implements SellTickets {
    @Override
    public void sell() {
        System.out.println("火车站卖票");
    }
}

// 代理工厂类
class ProxyFactory {
    private TrainStation station = new TrainStation(); // 真实对象

    public SellTickets getProxyObject() {
        // 使用Proxy创建代理对象
        SellTickets sellTickets = (SellTickets) Proxy.newProxyInstance(
                station.getClass().getClassLoader(), // 类加载器
                station.getClass().getInterfaces(),  // 实现的接口
                new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        System.out.println("代理点收取一些服务费用(JDK动态代理方式)"); // 代理增强逻辑
                        Object result = method.invoke(station, args); // 调用真实对象的方法
                        return result;
                    }
                });
        return sellTickets;
    }
}

// 测试类
public class Client {
    public static void main(String[] args) {
        ProxyFactory factory = new ProxyFactory(); // 创建代理工厂
        SellTickets proxyObject = factory.getProxyObject(); // 获取代理对象
        proxyObject.sell(); // 调用代理方法
    }
}
```

##### **这个合并后的代码包含**：
1. **SellTickets 接口**：
    - 定义了卖票的抽象方法 **sell()**
2. **TrainStation 类**：
    - 实现了 **SellTickets 接口**
    - 提供了基本的卖票功能实现
3. **ProxyFactory 类**：
    - 持有真实对象 **TrainStation** 的引用
    - 使用 JDK 动态代理创建代理对象
    - 在代理逻辑中添加了服务费用的增强功能
4. **Client 测试类**：
    - 创建代理工厂
    - 获取代理对象
    - 测试代理方法

**说明：** 使用JDK动态代理增强火车站卖票功能，代理对象在调用时额外收取服务费。

---

## 
以上是从用户提供内容中提取的面试题及其答案，并为代码添加了详细的中文注释。未完成的题目仅列出问题，未提供实现代码。

## 多线程场景题：有5个人，在那赛跑，请你设计一个多线程的裁判程序给出他们赛跑的结果顺序，5个人的速度随机处理
**答案：**

```java
import java.util.Random;
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class CountDownLatchExample1 {
    // 用于记录完成顺序的计数器，volatile保证线程可见性
    public volatile static Integer num = 0;
    // 存储比赛结果的数组
    public static String[] res = new String[5];
    // 参赛人数
    private static final int threadCount = 5;

    public static void main(String[] args) throws InterruptedException {
        // 创建固定大小的线程池，线程数为5
        ExecutorService threadPool = Executors.newFixedThreadPool(5);
        // 使用CountDownLatch确保所有运动员跑完再输出结果
        final CountDownLatch countDownLatch = new CountDownLatch(threadCount);
        Random random = new Random(); // 随机数生成器，用于模拟跑步时间

        // 为每个运动员创建一个线程
        for (int i = 0; i < threadCount; i++) {
            final int threadnum = i; // 运动员编号
            threadPool.execute(() -> {
                try {
                    // 随机生成跑步时间（100到500毫秒之间）
                    int sleepTime = random.nextInt(401) + 100;
                    Thread.sleep(sleepTime); // 模拟跑步耗时
                    // 记录运动员完成时间和编号
                    res[num++] = "运动员" + (threadnum + 1) + "消耗的时间为" + sleepTime;
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                } finally {
                    countDownLatch.countDown(); // 运动员完成，计数器减1
                }
            });
        }

        countDownLatch.await(); // 等待所有运动员完成
        threadPool.shutdown(); // 关闭线程池
        // 输出比赛结果
        for (String re : res) {
            System.out.println(re);
        }
    }
}
```

**说明：** 使用线程池和CountDownLatch模拟赛跑，随机时间表示速度差异，结果按完成顺序记录。

---

## 手写线程池（实现一个简易线程池）
## 简易版本
<details class="lake-collapse"><summary id="ua51768e0"></summary><p id="u0a29d14f" class="ne-p"><span class="ne-text">以下是修改后的代码，其中打印信息已更改为中文，并且我已经在关键部分添加了详细注释：</span></p><pre data-language="java" id="xeosZ" class="ne-codeblock language-java"><code>import java.util.LinkedList;
import java.util.Queue;

// 线程池实现
public class MyThreadPool {
    private final int poolSize;  // 线程池大小
    private final WorkerThread[] threads;  // 线程池中的工作线程
    private final Queue&lt;Runnable&gt; taskQueue;  // 任务队列

    // 构造函数，初始化线程池大小，创建工作线程，初始化任务队列
    public MyThreadPool(int poolSize) {
        this.poolSize = poolSize;
        this.taskQueue = new LinkedList&lt;&gt;();
        this.threads = new WorkerThread[poolSize];

        // 初始化线程池，创建并启动工作线程
        for (int i = 0; i &lt; poolSize; i++) {
            threads[i] = new WorkerThread();
            threads[i].start();  // 启动工作线程
        }
    }

    // 提交任务到线程池
    public synchronized void submit(Runnable task) {
        taskQueue.offer(task);  // 将任务加入队列
        notify();  // 唤醒等待的工作线程
    }

    // 工作线程类
    private class WorkerThread extends Thread {
        @Override
        public void run() {
            while (true) {
                Runnable task = null;

                // 获取任务队列中的任务
                synchronized (MyThreadPool.this) {
                    // 如果队列为空，当前线程进入等待状态
                    while (taskQueue.isEmpty()) {
                        try {
                            MyThreadPool.this.wait();  // 当前线程等待
                        } catch (InterruptedException e) {
                            Thread.currentThread().interrupt();  // 恢复中断状态
                            return;
                        }
                    }
                    task = taskQueue.poll();  // 取出队列中的任务
                }

                // 执行任务
                if (task != null) {
                    task.run();  // 执行任务
                }
            }
        }
    }

    public static void main(String[] args) {
        // 创建一个线程池，包含5个线程
        MyThreadPool threadPool = new MyThreadPool(5);

        // 提交一些任务
        for (int i = 0; i &lt; 10; i++) {
            final int taskId = i;
            threadPool.submit(() -&gt; {
                // 打印执行任务的信息（中文）
                System.out.println(&quot;任务 &quot; + taskId + &quot; 正在由 &quot; + Thread.currentThread().getName() + &quot; 执行&quot;);
                try {
                    Thread.sleep(1000);  // 模拟任务执行时间
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();  // 任务执行被中断
                }
            });
        }
    }
}</code></pre><h3 id="62c61489"><span class="ne-text">代码解释：</span></h3><ol class="ne-ol"><li id="udc9895a9" data-lake-index-type="0"><strong><span class="ne-text">线程池管理</span></strong><span class="ne-text">：</span></li></ol><ul class="ne-list-wrap"><ul ne-level="1" class="ne-ul"><li id="u31ccd622" data-lake-index-type="0"><code class="ne-code"><span class="ne-text">MyThreadPool</span></code><span class="ne-text">类包含了线程池的核心功能，创建工作线程并管理任务队列。</span></li><li id="u43bbb30c" data-lake-index-type="0"><span class="ne-text">在构造函数中，我们初始化了线程池的大小，创建了工作线程，并启动了它们。</span></li></ul></ul><ol start="2" class="ne-ol"><li id="u8dfe94fa" data-lake-index-type="0"><strong><span class="ne-text">任务提交与调度</span></strong><span class="ne-text">：</span></li></ol><ul class="ne-list-wrap"><ul ne-level="1" class="ne-ul"><li id="ub6e682ed" data-lake-index-type="0"><code class="ne-code"><span class="ne-text">submit</span></code><span class="ne-text">方法用来将任务添加到任务队列中，并通过</span><code class="ne-code"><span class="ne-text">notify</span></code><span class="ne-text">唤醒等待中的线程执行任务。</span></li><li id="u0b5f1944" data-lake-index-type="0"><code class="ne-code"><span class="ne-text">taskQueue</span></code><span class="ne-text">是一个任务队列，使用</span><code class="ne-code"><span class="ne-text">LinkedList</span></code><span class="ne-text">实现，线程会从队列中取任务执行。</span></li></ul></ul><ol start="3" class="ne-ol"><li id="uc688ab85" data-lake-index-type="0"><strong><span class="ne-text">工作线程</span></strong><span class="ne-text">：</span></li></ol><ul class="ne-list-wrap"><ul ne-level="1" class="ne-ul"><li id="u65551d3e" data-lake-index-type="0"><code class="ne-code"><span class="ne-text">WorkerThread</span></code><span class="ne-text">是线程池中的工作线程类，它会一直运行，并在任务队列中等待任务。</span></li><li id="udafaf0ad" data-lake-index-type="0"><span class="ne-text">每个工作线程在队列为空时会调用</span><code class="ne-code"><span class="ne-text">wait()</span></code><span class="ne-text">进入等待状态，直到有新任务提交。</span></li></ul></ul><ol start="4" class="ne-ol"><li id="u7e0632c6" data-lake-index-type="0"><strong><span class="ne-text">主函数</span></strong><span class="ne-text">：</span></li></ol><ul class="ne-list-wrap"><ul ne-level="1" class="ne-ul"><li id="ub5193895" data-lake-index-type="0"><span class="ne-text">在主函数中，我们创建了一个包含5个线程的线程池，并提交了10个任务。每个任务模拟了1秒的执行时间，并打印执行的任务信息。</span></li></ul></ul><h3 id="4798728b"><span class="ne-text">中文打印信息：</span></h3><p id="ucc463a6e" class="ne-p"><span class="ne-text">任务输出部分已经修改为中文：</span></p><pre data-language="java" id="t32Oh" class="ne-codeblock language-java"><code>System.out.println(&quot;任务 &quot; + taskId + &quot; 正在由 &quot; + Thread.currentThread().getName() + &quot; 执行&quot;);</code></pre><h3 id="aea4621a"><span class="ne-text">扩展功能：</span></h3><ul class="ne-ul"><li id="ue8a3b6e5" data-lake-index-type="0"><span class="ne-text">这个线程池的实现很基础，仅包括线程池的创建、任务的提交和执行。实际应用中，线程池通常需要更复杂的功能，比如：任务拒绝策略、线程池的关闭、线程池的动态扩容等。</span></li></ul><p id="u81774745" class="ne-p"><span class="ne-text">如果你有其他的要求，或者希望对某些部分进行修改，随时告诉我！</span></p></details>
**答案：**  
实现一个简易线程池需要定义多个接口和类，以下是逐步实现：

##### 线程池基本功能接口
```java
public interface ThreadPool {
    // 提交任务到线程池
    void execute(Runnable runnable);

    // 关闭线程池
    void shutdown();

    // 获取线程池初始化时的线程大小
    int getInitSize();

    // 获取线程池最大线程数
    int getMaxSize();

    // 获取线程池核心线程数量
    int getCoreSize();

    // 获取活跃线程数量
    int getActiveCount();

    // 获取线程池缓存队列大小
    int getQueueSize();

    // 查看线程是否被销毁
    boolean isShutdown();
}
```

##### 拒绝策略接口
```java
@FunctionalInterface
public interface DenyPolicy {
    // 定义拒绝策略，当任务队列满时如何处理新任务
    void reject(Runnable runnable, ThreadPool threadPool);
}
```

##### 线程池工厂接口
```java
@FunctionalInterface
public interface ThreadFactory {
    // 创建线程的工厂方法
    Thread creatThread(Runnable runnable);
}
```

##### 任务队列接口
```java
public interface RunnableQueue {
    // 新任务提交到队列
    void offer(Runnable runnable);

    // 从队列中取出任务
    Runnable take();

    // 获取队列中任务数量
    int size();
}
```

##### 自定义异常
```java
public class RunnableDenyException extends RuntimeException {
    public RunnableDenyException(String msg) {
        super(msg); // 自定义异常信息
    }
}
```

##### 拒绝策略实现
```java
// 直接丢弃策略，不做任何通知
class DiscardDenyPolicy implements DenyPolicy {
    @Override
    public void reject(Runnable runnable, ThreadPool threadPool) {
        // 什么都不做，直接丢弃任务
    }
}

// 抛出异常通知策略
class AbortDenyPolicy implements DenyPolicy {
    @Override
    public void reject(Runnable runnable, ThreadPool threadPool) {
        throw new RunnableDenyException("这个线程:" + runnable + " 将会被丢弃");
    }
}

// 在提交者线程中运行任务策略
class RunnerDenyPolicy implements DenyPolicy {
    @Override
    public void reject(Runnable runnable, ThreadPool threadPool) {
        if (!threadPool.isShutdown()) {
            runnable.run(); // 直接在提交者线程中执行任务
        }
    }
}
```

##### 任务队列实现
```java
import java.util.LinkedList;

public class LinkedRunnableQueue implements RunnableQueue {
    private final int limit; // 任务队列的最大容量
    private final DenyPolicy denyPolicy; // 拒绝策略
    private final LinkedList<Runnable> runnableLinkedList; // 任务存储队列
    private final ThreadPool threadPool; // 关联的线程池

    public LinkedRunnableQueue(int limit, DenyPolicy denyPolicy, ThreadPool threadPool) {
        this.limit = limit;
        this.denyPolicy = denyPolicy;
        this.threadPool = threadPool;
        runnableLinkedList = new LinkedList<>();
    }

    @Override
    public void offer(Runnable runnable) {
        synchronized (runnableLinkedList) {
            // 如果队列已满，执行拒绝策略
            if (runnableLinkedList.size() >= limit) {
                denyPolicy.reject(runnable, threadPool);
            } else {
                // 添加任务到队列末尾，并唤醒等待的线程
                runnableLinkedList.addLast(runnable);
                runnableLinkedList.notifyAll();
            }
        }
    }

    @Override
    public Runnable take() {
        synchronized (runnableLinkedList) {
            // 如果队列为空，线程等待
            while (runnableLinkedList.isEmpty()) {
                try {
                    runnableLinkedList.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            // 返回并移除队列头部任务
            return runnableLinkedList.removeFirst();
        }
    }

    @Override
    public int size() {
        synchronized (runnableLinkedList) {
            return runnableLinkedList.size(); // 返回队列当前大小
        }
    }
}
```

##### 线程工厂实现
```java
import java.util.concurrent.atomic.AtomicInteger;

class DefaultThreadFactory implements ThreadFactory {
    private static final AtomicInteger GROUP_COUNTER = new AtomicInteger(1); // 线程组计数器
    private static final ThreadGroup group = new ThreadGroup("我的线程-" + GROUP_COUNTER.getAndDecrement()); // 线程组
    private static final AtomicInteger COUNTER = new AtomicInteger(0); // 线程计数器

    @Override
    public Thread creatThread(Runnable runnable) {
        // 创建并返回一个新线程，设置线程组和名称
        return new Thread(group, runnable, "线程池-" + COUNTER.getAndDecrement());
    }
}
```

##### 线程池实现
```java
import java.util.ArrayDeque;
import java.util.Queue;
import java.util.concurrent.TimeUnit;
import java.util.stream.IntStream;

public class BasicThreadPool extends Thread implements ThreadPool {
    private final int initSize; // 初始线程数
    private final int maxSize; // 最大线程数
    private final int coreSize; // 核心线程数
    private int activeCount; // 当前活跃线程数
    private final ThreadFactory threadFactory; // 线程工厂
    private final RunnableQueue runnableQueue; // 任务队列
    private volatile boolean isShutdown = false; // 线程池是否关闭
    private final Queue<ThreadTask> internalTasks = new ArrayDeque<>(); // 工作线程任务队列
    private final static DenyPolicy DEFAULT_DENY_POLICY = new DiscardDenyPolicy(); // 默认拒绝策略
    private final static ThreadFactory DEFAULT_THREAD_FACTORY = new DefaultThreadFactory(); // 默认线程工厂
    private final long keepAliveTime; // 线程存活时间
    private final TimeUnit timeUnit; // 时间单位

    // 默认构造函数
    public BasicThreadPool(int initSize, int maxSize, int coreSize, int queueSize) {
        this(initSize, maxSize, coreSize, DEFAULT_THREAD_FACTORY, queueSize, DEFAULT_DENY_POLICY, 2, TimeUnit.SECONDS);
    }

    // 完整构造函数
    public BasicThreadPool(int initSize, int maxSize, int coreSize, ThreadFactory threadFactory, int queueSize,
                           DenyPolicy denyPolicy, long keepAliveTime, TimeUnit timeUnit) {
        this.initSize = initSize;
        this.maxSize = maxSize;
        this.coreSize = coreSize;
        this.threadFactory = threadFactory;
        this.runnableQueue = new LinkedRunnableQueue(queueSize, denyPolicy, this);
        this.keepAliveTime = keepAliveTime;
        this.timeUnit = timeUnit;
        this.init(); // 初始化线程池
    }

    // 初始化线程池，创建初始线程
    private void init() {
        start(); // 启动线程池管理线程
        IntStream.range(0, initSize).forEach(i -> newThread()); // 创建初始线程
    }

    // 创建新线程
    private void newThread() {
        InternalTask internalTask = new InternalTask(runnableQueue); // 创建内部任务
        Thread thread = this.threadFactory.creatThread(internalTask); // 使用工厂创建线程
        ThreadTask threadTask = new ThreadTask(thread, internalTask); // 组合线程和任务
        internalTasks.offer(threadTask); // 添加到工作队列
        this.activeCount++; // 活跃线程数加1
        thread.start(); // 启动线程
    }

    // 移除线程
    private void removeThread() {
        ThreadTask threadTask = internalTasks.remove(); // 从工作队列移除任务
        threadTask.internalTask.stop(); // 停止任务
        this.activeCount--; // 活跃线程数减1
    }

    @Override
    public void execute(Runnable runnable) {
        if (this.isShutdown) {
            throw new IllegalStateException("这个线程池已经被销毁了");
        }
        this.runnableQueue.offer(runnable); // 提交任务到队列
    }

    @Override
    public void run() {
        // 线程池管理线程，动态调整线程数量
        while (!isShutdown && !isInterrupted()) {
            try {
                timeUnit.sleep(keepAliveTime); // 休眠一段时间
            } catch (InterruptedException e) {
                e.printStackTrace();
                isShutdown = true;
                break;
            }
            synchronized (this) {
                if (isShutdown) break;
                // 如果任务队列有任务且活跃线程少于核心线程，增加线程
                if (runnableQueue.size() > 0 && activeCount < coreSize) {
                    IntStream.range(initSize, coreSize).forEach(i -> newThread());
                    continue;
                }
                // 如果任务队列有任务且活跃线程少于最大线程，增加线程
                if (runnableQueue.size() > 0 && activeCount < maxSize) {
                    IntStream.range(coreSize, maxSize).forEach(i -> newThread());
                }
                // 如果任务队列为空且活跃线程多于核心线程，减少线程
                if (runnableQueue.size() == 0 && activeCount > coreSize) {
                    IntStream.range(coreSize, activeCount).forEach(i -> removeThread());
                }
            }
        }
    }

    @Override
    public void shutdown() {
        this.isShutdown = true; // 设置关闭标志
    }

    @Override
    public int getInitSize() {
        return this.initSize;
    }

    @Override
    public int getMaxSize() {
        return this.maxSize;
    }

    @Override
    public int getCoreSize() {
        return this.coreSize;
    }

    @Override
    public int getActiveCount() {
        return this.activeCount;
    }

    @Override
    public int getQueueSize() {
        return this.runnableQueue.size();
    }

    @Override
    public boolean isShutdown() {
        return this.isShutdown;
    }

    // 线程和任务的组合类
    private static class ThreadTask {
        Thread thread;
        InternalTask internalTask;

        public ThreadTask(Thread thread, InternalTask internalTask) {
            this.thread = thread;
            this.internalTask = internalTask;
        }
    }
}
```

##### 线程池内部任务类
```java
public class InternalTask implements Runnable {
    private final RunnableQueue runnableQueue; // 任务队列
    private volatile boolean running = true; // 运行状态

    public InternalTask(RunnableQueue runnableQueue) {
        this.runnableQueue = runnableQueue;
    }

    @Override
    public void run() {
        // 持续从队列中取出任务并执行
        while (running && !Thread.currentThread().isInterrupted()) {
            try {
                Runnable task = runnableQueue.take(); // 取出任务
                task.run(); // 执行任务
            } catch (Exception e) {
                running = false;
                break;
            }
        }
    }

    // 停止任务
    public void stop() {
        this.running = false;
    }
}
```

##### 测试类
```java
import java.util.concurrent.TimeUnit;

public class Main {
    public static void main(String[] args) {
        // 创建线程池，初始2个线程，最大6个，核心4个，队列容量100
        final ThreadPool threadPool = new BasicThreadPool(2, 6, 4, 100);
        // 提交20个任务
        for (int i = 0; i <= 20; i++) {
            threadPool.execute(() -> {
                try {
                    TimeUnit.SECONDS.sleep(1); // 模拟任务执行1秒
                    System.out.println(Thread.currentThread().getName() + "开始了");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            });
        }
    }
}
```

**说明：** 实现了一个简易线程池，包括任务队列管理、线程动态调整和拒绝策略。

---

## 手写一个HashMap，HashSet   
 MyHashMap  

```java
import java.util.LinkedList;

public class MyHashMap<K, V> {
    private static final int DEFAULT_CAPACITY = 16; // 默认容量
    private static final double LOAD_FACTOR = 0.75; // 负载因子

    private LinkedList<Node<K, V>>[] buckets; // 存储桶
    private int size; // 当前元素数量

    // 默认构造器
    public MyHashMap() {
        this(DEFAULT_CAPACITY);
    }

    // 指定容量的构造器
    public MyHashMap(int capacity) {
        // 使用安全的类型转换方式来避免未经检查的警告
        buckets = new LinkedList[capacity];
        for (int i = 0; i < capacity; i++) {
            buckets[i] = new LinkedList<>();
        }
        size = 0;
    }

    // 存储键值对
    public void put(K key, V value) {
        int index = getIndex(key); // 计算哈希值对应的桶索引
        LinkedList<Node<K, V>> bucket = buckets[index];

        // 检查是否已经存在该键
        for (Node<K, V> node : bucket) {
            if (node.key.equals(key)) {
                node.value = value; // 更新值
                return;
            }
        }

        // 如果不存在该键，则新增节点
        bucket.add(new Node<>(key, value));
        size++;

        // 检查是否需要扩容
        if ((double) size / buckets.length > LOAD_FACTOR) {
            resize();
        }
    }

    // 获取值
    public V get(K key) {
        int index = getIndex(key);
        LinkedList<Node<K, V>> bucket = buckets[index];

        for (Node<K, V> node : bucket) {
            if (node.key.equals(key)) {
                return node.value;
            }
        }
        return null; // 如果没有找到键，返回 null
    }

    // 删除键值对
    public void remove(K key) {
        int index = getIndex(key);
        LinkedList<Node<K, V>> bucket = buckets[index];

        for (Node<K, V> node : bucket) {
            if (node.key.equals(key)) {
                bucket.remove(node);
                size--;
                return;
            }
        }
    }

    // 获取当前元素数量
    public int size() {
        return size;
    }

    // 扩容方法
    private void resize() {
        LinkedList<Node<K, V>>[] oldBuckets = buckets;
        buckets = new LinkedList[oldBuckets.length * 2];
        for (int i = 0; i < buckets.length; i++) {
            buckets[i] = new LinkedList<>();
        }
        size = 0;

        // 将旧桶中的所有元素重新插入新桶
        for (LinkedList<Node<K, V>> bucket : oldBuckets) {
            for (Node<K, V> node : bucket) {
                put(node.key, node.value);
            }
        }
    }

    // 计算桶索引
    private int getIndex(K key) {
        return Math.abs(key.hashCode()) % buckets.length;
    }

    // 内部类：存储键值对的节点
    private static class Node<K, V> {
        K key;
        V value;

        Node(K key, V value) {
            this.key = key;
            this.value = value;
        }
    }
}

```

 MyHashSet  

```java
public class MyHashSet<E> {
    private static final Object PRESENT = new Object(); // 占位对象
    private final MyHashMap<E, Object> map;

    // 构造函数
    public MyHashSet() {
        map = new MyHashMap<>();
    }

    // 添加元素
    public boolean add(E e) {
        return map.put(e, PRESENT) == null; // 如果之前不存在该键，返回 true
    }

    // 删除元素
    public boolean remove(E e) {
        return map.get(e) != null && (map.remove(e) != null);
    }

    // 判断是否包含某个元素
    public boolean contains(E e) {
        return map.get(e) != null;
    }

    // 获取集合大小
    public int size() {
        return map.size();
    }

    // 清空集合
    public void clear() {
        map = new MyHashMap<>();
    }
}
```

## 未完成的题目（仅列出题目，未提供完整答案））
7. 有一个0-4的随机器rand4，如何实现0-6的随机器rand6，概率相同。拓展：rand X = func(rand Y)，实现func函数   
8. 及其逆天的一个阿里手撕，来自于@byebyeneu：写三个Spring接口，调用第一个接口的时候返回这个接口的累计调用次数，调用第二个接口的时候返回调用这个接口的累计p99，调用第三个接口的时候，如果这个接口这时的qps<10，返回success，如果这个接口这时qps>10，返回err

---



> 更新: 2025-05-10 15:36:44  
> 原文: <https://www.yuque.com/neumx/laxg2e/btvip1b8lpx5lq9u>