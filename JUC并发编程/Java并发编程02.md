# Java并发编程02

# Java并发编程02
## <font style="color:#000000;">JMM(Java 内存模型)</font>
<font style="color:#000000;">JMM（Java 内存模型）相关的问题比较多，也比较重要，于是我单独抽了一篇文章来总结 JMM 相关的知识点和问题：</font>[JMM（Java 内存模型）详解](https://www.yuque.com/vip6688/neho4x/gtxnwutswuxo5tpm)

## <font style="color:#000000;">volatile 关键字</font>
### <font style="color:#000000;">2.1 如何保证变量的可见性？</font>
<font style="color:#000000;">在 Java 中，</font><font style="color:#000000;">volatile</font><font style="color:#000000;">关键字可以保证变量的可见性，如果我们将变量声明为</font>**<font style="color:#000000;">volatile</font>**<font style="color:#000000;">，这就指示 JVM，这个变量是共享且不稳定的，每次使用它都到主存中进行读取。</font>

![1732497475797-7ce57dd3-35d1-476e-b57f-449762eb1e27.png](./img/yl5kpkT8Yz9ysjOy/1732497475797-7ce57dd3-35d1-476e-b57f-449762eb1e27-907146.png)

<font style="color:#000000;">JMM(Java 内存模型)</font>

![1732497475878-adafc808-2552-4ef8-b9d4-22897dfc6292.png](./img/yl5kpkT8Yz9ysjOy/1732497475878-adafc808-2552-4ef8-b9d4-22897dfc6292-364883.png)

<font style="color:#000000;">JMM(Java 内存模型)强制在主存中进行读取</font>

<font style="color:#000000;">volatile</font><font style="color:#000000;">关键字其实并非是 Java 语言特有的，在 C 语言里也有，它最原始的意义就是禁用 CPU 缓存。如果我们将一个变量使用</font><font style="color:#000000;">volatile</font><font style="color:#000000;">修饰，这就指示 编译器，这个变量是共享且不稳定的，每次使用它都到主存中进行读取。</font>

<font style="color:#000000;">volatile</font><font style="color:#000000;">关键字能保证数据的可见性，但不能保证数据的原子性。</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字两者都能保证。</font>

### <font style="color:#000000;">2.2 如何禁止指令重排序？</font>
**<font style="color:#000000;">在 Java 中，</font>****<font style="color:#000000;">volatile</font>****<font style="color:#000000;">关键字除了可以保证变量的可见性，还有一个重要的作用就是防止 JVM 的指令重排序。</font>**<font style="color:#000000;">如果我们将变量声明为</font>**<font style="color:#000000;">volatile</font>**<font style="color:#000000;">，在对这个变量进行读写操作的时候，会通过插入特定的</font>**<font style="color:#000000;">内存屏障</font>**<font style="color:#000000;">的方式来禁止指令重排序。</font>

<font style="color:#000000;">在 Java 中，</font><font style="color:#000000;">Unsafe</font><font style="color:#000000;">类提供了三个开箱即用的内存屏障相关的方法，屏蔽了操作系统底层的差异：</font>

```java
public native void loadFence();
public native void storeFence();
public native void fullFence();
```

<font style="color:#000000;">理论上来说，你通过这个三个方法也可以实现和</font><font style="color:#000000;">volatile</font><font style="color:#000000;">禁止重排序一样的效果，只是会麻烦一些。</font>

<font style="color:#000000;">下面我以一个常见的面试题为例讲解一下</font><font style="color:#000000;">volatile</font><font style="color:#000000;">关键字禁止指令重排序的效果。</font>

<font style="color:#000000;">面试中面试官经常会说：“单例模式了解吗？来给我手写一下！给我解释一下双重检验锁方式实现单例模式的原理呗！”</font>

**<font style="color:#000000;">双重校验锁实现对象单例（线程安全）</font>**<font style="color:#000000;">：</font>

```java
public class Singleton {

    private volatile static Singleton uniqueInstance;

    private Singleton() {
    }

    public  static Singleton getUniqueInstance() {
       //先判断对象是否已经实例过，没有实例化过才进入加锁代码
        if (uniqueInstance == null) {
            //类对象加锁
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

<font style="color:#000000;">uniqueInstance</font><font style="color:#000000;">采用</font><font style="color:#000000;">volatile</font><font style="color:#000000;">关键字修饰也是很有必要的，</font><font style="color:#000000;">uniqueInstance = new Singleton();</font><font style="color:#000000;">这段代码其实是分为三步执行：</font>

1. **<font style="color:#000000;">为uniqueInstance分配内存空间</font>**
2. **<font style="color:#000000;">初始化uniqueInstance</font>**
3. **<font style="color:#000000;">将uniqueInstance指向分配的内存地址</font>**

<font style="color:#000000;">但是由于 JVM 具有指令重排的特性，执行顺序有可能变成 1->3->2。指令重排在单线程环境下不会出现问题，但是在多线程环境下会导致一个线程获得还没有初始化的实例。例如，线程 T1 执行了 1 和 3，此时 T2 调用</font><font style="color:#000000;">getUniqueInstance</font><font style="color:#000000;">() 后发现</font><font style="color:#000000;">uniqueInstance</font><font style="color:#000000;">不为空，因此返回</font><font style="color:#000000;">uniqueInstance</font><font style="color:#000000;">，但此时</font><font style="color:#000000;">uniqueInstance</font><font style="color:#000000;">还未被初始化。</font>

### <font style="color:#000000;">2.3 volatile 可以保证原子性么？</font>
**<font style="color:#000000;">volatile</font>************<font style="color:#000000;">关键字能保证变量的可见性，但不能保证对变量的操作是原子性的。</font>**

<font style="color:#000000;">我们通过下面的代码即可证明：</font>

```java

public class VolatoleAtomicityDemo {
    public volatile static int inc = 0;

    public void increase() {
        inc++;
    }

    public static void main(String[] args) throws InterruptedException {
        ExecutorService threadPool = Executors.newFixedThreadPool(5);
        VolatoleAtomicityDemo volatoleAtomicityDemo = new VolatoleAtomicityDemo();
        for (int i = 0; i < 5; i++) {
            threadPool.execute(() -> {
                for (int j = 0; j < 500; j++) {
                    volatoleAtomicityDemo.increase();
                }
            });
        }
        // 等待1.5秒，保证上面程序执行完成
        Thread.sleep(1500);
        System.out.println(inc);
        threadPool.shutdown();
    }
}
```

<font style="color:#000000;">正常情况下，运行上面的代码理应输出</font><font style="color:#000000;">2500</font><font style="color:#000000;">。但你真正运行了上面的代码之后，你会发现每次输出结果都小于</font><font style="color:#000000;">2500</font><font style="color:#000000;">。</font>

<font style="color:#000000;">为什么会出现这种情况呢？不是说好了，</font><font style="color:#000000;">volatile</font><font style="color:#000000;">可以保证变量的可见性嘛！</font>

<font style="color:#000000;">也就是说，如果</font><font style="color:#000000;">volatile</font><font style="color:#000000;">能保证</font><font style="color:#000000;">inc++</font><font style="color:#000000;">操作的原子性的话。每个线程中对</font><font style="color:#000000;">inc</font><font style="color:#000000;">变量自增完之后，其他线程可以立即看到修改后的值。5 个线程分别进行了 500 次操作，那么最终 inc 的值应该是 5*500=2500。</font>

<font style="color:#000000;">很多人会误认为自增操作inc++是原子性的，</font>**<font style="color:#000000;background-color:#d9eafc;">实际上，inc++其实是一个复合操作</font>**<font style="color:#000000;">，包括三步：</font>

1. <font style="color:#000000;">读取 inc 的值。</font>
2. <font style="color:#000000;">对 inc 加 1。</font>
3. <font style="color:#000000;">将 inc 的值写回内存。</font>

<font style="color:#000000;">volatile</font><font style="color:#000000;">是无法保证这三个操作是具有原子性的，有可能导致下面这种情况出现：</font>

1. <font style="color:#000000;">线程 1 对</font><font style="color:#000000;">inc</font><font style="color:#000000;">进行读取操作之后，还未对其进行修改。线程 2 又读取了</font><font style="color:#000000;">inc</font><font style="color:#000000;">的值并对其进行修改（+1），再将</font><font style="color:#000000;">inc</font><font style="color:#000000;">的值写回内存。</font>
2. <font style="color:#000000;">线程 2 操作完毕后，线程 1 对</font><font style="color:#000000;">inc</font><font style="color:#000000;">的值进行修改（+1），再将</font><font style="color:#000000;">inc</font><font style="color:#000000;">的值写回内存。</font>

<font style="color:#000000;">这也就导致两个线程分别对</font><font style="color:#000000;">inc</font><font style="color:#000000;">进行了一次自增操作后，</font><font style="color:#000000;">inc</font><font style="color:#000000;">实际上只增加了 1。</font>

<font style="color:#000000;">其实，如果想要保证上面的代码运行正确也非常简单，利用</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">、</font><font style="color:#000000;">Lock</font><font style="color:#000000;">或者</font><font style="color:#000000;">AtomicInteger</font><font style="color:#000000;">都可以。</font>

<font style="color:#000000;">使用</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">改进：</font>

```java
public synchronized void increase() {
    inc++;
}
```

<font style="color:#000000;">使用</font><font style="color:#000000;">AtomicInteger</font><font style="color:#000000;">改进：</font>

```java
public AtomicInteger inc = new AtomicInteger();

public void increase() {
    inc.getAndIncrement();
}
```

<font style="color:#000000;">使用</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">改进：</font>

```java
Lock lock = new ReentrantLock();
public void increase() {
    lock.lock();
    try {
        inc++;
    } finally {
        lock.unlock();
    }
}
```

## <font style="color:#000000;">乐观锁和悲观锁</font>
### <font style="color:#000000;">3.1 什么是悲观锁？</font>
<font style="color:#000000;">悲观锁总是假设最坏的情况，认为共享资源每次被访问的时候就会出现问题(比如共享数据被修改)，所以每次在获取资源操作的时候都会上锁，这样其他线程想拿到这个资源就会阻塞直到锁被上一个持有者释放。也就是说，</font>**<font style="color:#000000;background-color:#d9eafc;">共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程</font>**<font style="color:#000000;background-color:#d9eafc;">。</font>

<font style="color:#000000;">像 Java 中synchronized和ReentrantLock等</font><font style="color:#000000;background-color:#d9eafc;">独占锁就是悲观锁思想的实现</font><font style="color:#000000;">。</font>

```java
public void performSynchronisedTask() {
    synchronized (this) {
        // 需要同步的操作
    }
}

private Lock lock = new ReentrantLock();
lock.lock();
try {
    // 需要同步的操作
} finally {
    lock.unlock();
}
```

<font style="color:#000000;">高并发的场景下，激烈的</font>**<font style="color:#000000;background-color:#d9eafc;">锁竞争会造成线程阻塞</font>**<font style="color:#000000;">，大量阻塞线程会</font>**<font style="color:#000000;background-color:#d9eafc;">导致系统的上下文切换</font>**<font style="color:#000000;">，</font>**<font style="color:#000000;background-color:#d9eafc;">增加系统的性能开销</font>**<font style="color:#000000;">。并且，悲观锁还可能会存在死锁问题，影响代码的正常运行。</font>

### <font style="color:#000000;">3.2 什么是乐观锁？</font>
<font style="color:#000000;">乐观锁总是假设最好的情况，认为共享资源每次被访问的时候不会出现问题，线程可以不停地执行，无需加锁也无需等待，只是在</font>**<font style="color:#000000;background-color:#d9eafc;">提交修改的时候去验证对应的资源（也就是数据）是否被其它线程修改了</font>**<font style="color:#000000;">（具体方法可以使用</font>**<font style="color:#000000;background-color:#d9eafc;">版本号机制或 CAS 算法</font>**<font style="color:#000000;">）。</font>

<font style="color:#000000;">在 Java 中</font><font style="color:#000000;">java.util.concurrent.atomic</font><font style="color:#000000;">包下面的原子变量类（比如</font><font style="color:#000000;">AtomicInteger</font><font style="color:#000000;">、</font><font style="color:#000000;">LongAdder</font><font style="color:#000000;">）就是使用了乐观锁的一种实现方式</font>**<font style="color:#000000;">CAS</font>**<font style="color:#000000;">实现的。</font>

![1732497475962-43bb2c31-e23c-4bb8-8d5b-f0777f27d506.png](./img/yl5kpkT8Yz9ysjOy/1732497475962-43bb2c31-e23c-4bb8-8d5b-f0777f27d506-444037.png)

```java
// LongAdder 在高并发场景下会比 AtomicInteger 和 AtomicLong 的性能更好
// 代价就是会消耗更多的内存空间（空间换时间）
LongAdder sum = new LongAdder();
sum.increment();
```

<font style="color:#000000;">高并发的场景下，乐观锁相比悲观锁来说，不存在锁竞争造成线程阻塞，也不会有死锁的问题，在性能上往往会更胜一筹。但是，如果冲突频繁发生（</font>**<font style="color:#000000;background-color:#d9eafc;">写占比非常多的情况</font>**<font style="color:#000000;">），</font>**<font style="color:#000000;background-color:#d9eafc;">会频繁失败和重试，这样同样会非常影响性能，导致 CPU 飙升。</font>**

<font style="color:#000000;">不过，大量失败重试的问题也是可以解决的，像我们前面提到的</font><font style="color:#000000;">LongAdder</font><font style="color:#000000;">以空间换时间的方式就解决了这个问题。</font>

<font style="color:#000000;">理论上来说：</font>

+ <font style="color:#000000;">悲观锁通常多用于写比较多的情况（多写场景，竞争激烈），这样可以避免频繁失败和重试影响性能，悲观锁的开销是固定的。不过，如果乐观锁解决了频繁失败和重试这个问题的话（比如</font><font style="color:#000000;">LongAdder</font><font style="color:#000000;">），也是可以考虑使用乐观锁的，要视实际情况而定。</font>
+ <font style="color:#000000;">乐观锁通常多用于写比较少的情况（多读场景，竞争较少），这样可以避免频繁加锁影响性能。不过，乐观锁主要针对的对象是单个共享变量（参考java.util.concurrent.atomic    包下面的原子变量类）。</font>

### <font style="color:#000000;">3.3 如何实现乐观锁？</font>
<font style="color:#000000;">乐观锁一般会使用版本号机制或 CAS 算法实现，CAS 算法相对来说更多一些，这里需要格外注意。</font>

#### <font style="color:#000000;">3.3.1 版本号机制</font>
<font style="color:#000000;">一般是在数据表中加上一个数据版本号</font><font style="color:#000000;">version</font><font style="color:#000000;">字段，表示数据被修改的次数。当数据被修改时，</font><font style="color:#000000;">version</font><font style="color:#000000;">值会加一。当线程 A 要更新数据值时，在读取数据的同时也会读取</font><font style="color:#000000;">version</font><font style="color:#000000;">值，在提交更新时，若刚才读取到的 version 值为当前数据库中的</font><font style="color:#000000;">version</font><font style="color:#000000;">值相等时才更新，否则重试更新操作，直到更新成功。</font>

**<font style="color:#000000;">举一个简单的例子</font>**<font style="color:#000000;">：假设数据库中帐户信息表中有一个 version 字段，当前值为 1 ；而当前帐户余额字段（</font><font style="color:#000000;">balance</font><font style="color:#000000;">）为 $100 。</font>

1. <font style="color:#000000;">操作员 A 此时将其读出（</font><font style="color:#000000;">version</font><font style="color:#000000;">=1 ），并从其帐户余额中扣除 </font>$ 50（  $<font style="color:#000000;">100-$50 ）。</font>
2. <font style="color:#000000;">在操作员 A 操作的过程中，操作员 B 也读入此用户信息（</font><font style="color:#000000;">version</font><font style="color:#000000;">=1 ），并从其帐户余额中扣除 </font>$ 20 （  $<font style="color:#000000;">100-$20 ）。</font>
3. <font style="color:#000000;">操作员 A 完成了修改工作，将数据版本号（</font><font style="color:#000000;">version</font><font style="color:#000000;">=1 ），连同帐户扣除后余额（</font><font style="color:#000000;">balance</font><font style="color:#000000;">=$50 ），提交至数据库更新，此时由于提交数据版本等于数据库记录当前版本，数据被更新，数据库记录</font><font style="color:#000000;">version</font><font style="color:#000000;">更新为 2 。</font>
4. <font style="color:#000000;">操作员 B 完成了操作，也将版本号（</font><font style="color:#000000;">version</font><font style="color:#000000;">=1 ）试图向数据库提交数据（</font><font style="color:#000000;">balance</font><font style="color:#000000;">=$80 ），但此时比对数据库记录版本时发现，操作员 B 提交的数据版本号为 1 ，数据库记录当前版本也为 2 ，不满足 “ 提交版本必须等于当前版本才能执行更新 “ 的乐观锁策略，因此，操作员 B 的提交被驳回。</font>

<font style="color:#000000;">这样就避免了操作员 B 用基于</font><font style="color:#000000;">version</font><font style="color:#000000;">=1 的旧数据修改的结果覆盖操作员 A 的操作结果的可能。</font>

#### <font style="color:#000000;">3.3.2 CAS 算法</font>
<font style="color:#000000;">CAS 的全称是</font>**<font style="color:#000000;">Compare And Swap（比较与交换）</font>**<font style="color:#000000;">，用于实现乐观锁，被广泛应用于各大框架中。CAS 的思想很简单，就是用一个预期值和要更新的变量值进行比较，两值相等才会进行更新。</font>

<font style="color:#000000;">CAS 是一个原子操作，底层依赖于一条 CPU 的原子指令。</font>

**<font style="color:#000000;">原子操作</font>**<font style="color:#000000;">即最小不可拆分的操作，也就是说操作一旦开始，就不能被打断，直到操作完成。</font>

<font style="color:#000000;">CAS 涉及到三个操作数：</font>

+ **<font style="color:#000000;">V</font>**<font style="color:#000000;">：要更新的变量值(Var)</font>
+ **<font style="color:#000000;">E</font>**<font style="color:#000000;">：预期值(Expected)</font>
+ **<font style="color:#000000;">N</font>**<font style="color:#000000;">：拟写入的新值(New)</font>

<font style="color:#000000;">当且仅当 V 的值等于 E 时，CAS 通过原子方式用新值 N 来更新 V 的值。如果不等，说明已经有其它线程更新了 V，则当前线程放弃更新。</font>

**<font style="color:#000000;">举一个简单的例子</font>**<font style="color:#000000;">：线程 A 要修改变量 i 的值为 6，i 原值为 1（V = 1，E=1，N=6，假设不存在 ABA 问题）。</font>

1. <font style="color:#000000;">i 与 1 进行比较，如果相等， 则说明没被其他线程修改，可以被设置为 6 。</font>
2. <font style="color:#000000;">i 与 1 进行比较，如果不相等，则说明被其他线程修改，当前线程放弃更新，CAS 操作失败。</font>

<font style="color:#000000;">当多个线程同时使用 CAS 操作一个变量时，只有一个会胜出，并成功更新，其余均会失败，但失败的线程并不会被挂起，仅是被告知失败，并且允许再次尝试，当然也允许失败的线程放弃操作。</font>

**<font style="color:#000000;background-color:#d9eafc;">Java 语言并没有直接实现 CAS</font>**<font style="color:#000000;">，CAS 相关的实现是通过 C++ 内联汇编的形式实现的（JNI 调用）。因此， </font>**<font style="color:#000000;background-color:#d9eafc;">CAS 的具体实现和操作系统以及 CPU 都有关系</font>**<font style="color:#000000;">。</font>

<font style="color:#000000;">sun.misc</font><font style="color:#000000;">包下的</font><font style="color:#000000;">Unsafe</font><font style="color:#000000;">类提供了</font><font style="color:#000000;">compareAndSwapObject</font><font style="color:#000000;">、</font><font style="color:#000000;">compareAndSwapInt</font><font style="color:#000000;">、</font><font style="color:#000000;">compareAndSwapLong</font><font style="color:#000000;">方法来实现的对</font><font style="color:#000000;">Object</font><font style="color:#000000;">、</font><font style="color:#000000;">int</font><font style="color:#000000;">、</font><font style="color:#000000;">long</font><font style="color:#000000;">类型的 CAS 操作</font>

```java
/**
    *  CAS
  * @param o         包含要修改field的对象
  * @param offset    对象中某field的偏移量
  * @param expected  期望值
  * @param update    更新值
  * @return          true | false
  */
public final native boolean compareAndSwapObject(Object o, long offset,  Object expected, Object update);

public final native boolean compareAndSwapInt(Object o, long offset, int expected,int update);

public final native boolean compareAndSwapLong(Object o, long offset, long expected, long update);
```

<font style="color:#000000;">关于Unsafe类的详细介绍可以看这篇文章：</font>[Java 魔法类 Unsafe 详解](https://www.yuque.com/vip6688/neho4x/ezql2ci6pp8v68p5)

### <font style="color:#000000;">3.4 乐观锁存在哪些问题？</font>
<font style="color:#000000;">ABA 问题是乐观锁最常见的问题。</font>

#### <font style="color:#000000;">3.4.1 ABA 问题</font>
<font style="color:#000000;">如果一个变量 V 初次读取的时候是 A 值，并且在准备赋值的时候检查到它仍然是 A 值，那我们就能说明它的值没有被其他线程修改过了吗？很明显是不能的，因为在这段时间它的值可能被改为其他值，然后又改回 A，那 CAS 操作就会误认为它从来没有被修改过。这个问题被称为 CAS 操作的</font>**<font style="color:#000000;">"ABA"问题。</font>**

<font style="color:#000000;">ABA 问题的解决思路是在变量前面追加上</font>**<font style="color:#000000;">版本号或者时间戳</font>**<font style="color:#000000;">。JDK 1.5 以后的</font><font style="color:#000000;">AtomicStampedReference</font><font style="color:#000000;">类就是用来解决 ABA 问题的，其中的</font><font style="color:#000000;">compareAndSet()</font><font style="color:#000000;">方法就是首先检查当前引用是否等于预期引用，并且当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。</font>

```java
public boolean compareAndSet(V   expectedReference,
                             V   newReference,
                             int expectedStamp,
                             int newStamp) {
    Pair<V> current = pair;
    return
        expectedReference == current.reference &&
        expectedStamp == current.stamp &&
        ((newReference == current.reference &&
          newStamp == current.stamp) ||
         casPair(current, Pair.of(newReference, newStamp)));
}
```

#### <font style="color:#000000;">3.4.2 循环时间长开销大</font>
<font style="color:#000000;">CAS 经常会用到自旋操作来进行重试，也就是不成功就一直循环执行直到成功。如果长时间不成功，会给 CPU 带来非常大的执行开销。</font>

<font style="color:#000000;">如果 JVM 能支持处理器提供的 pause 指令那么效率会有一定的提升，pause 指令有两个作用：</font>

1. <font style="color:#000000;">可以延迟流水线执行指令，使 CPU 不会消耗过多的执行资源，延迟的时间取决于具体实现的版本，在一些处理器上延迟时间是零。</font>
2. <font style="color:#000000;">可以避免在退出循环的时候因内存顺序冲而引起 CPU 流水线被清空，从而提高 CPU 的执行效率。</font>

#### <font style="color:#000000;">3.4.3 只能保证一个共享变量的原子操作</font>
<font style="color:#000000;">CAS 只对单个共享变量有效，当操作涉及跨多个共享变量时 CAS 无效。但是从 JDK 1.5 开始，提供了</font><font style="color:#000000;">AtomicReference</font><font style="color:#000000;">类来保证引用对象之间的原子性，你可以把多个变量放在一个对象里来进行 CAS 操作.所以我们可以使用锁或者利用</font><font style="color:#000000;">AtomicReference</font><font style="color:#000000;">类把多个共享变量合并成一个共享变量来操作。</font>

## <font style="color:#000000;">synchronized 关键字</font>
### <font style="color:#000000;">4.1 synchronized 是什么？有什么用？</font>
<font style="color:#000000;">synchronized</font><font style="color:#000000;">是 Java 中的一个关键字，翻译成中文是同步的意思，主要解决的是多个线程之间访问资源的同步性，可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。</font>

<font style="color:#000000;">在 Java 早期版本中，</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">属于</font>**<font style="color:#000000;">重量级锁</font>**<font style="color:#000000;">，效率低下。这是因为监视器锁（monitor）是依赖于底层的操作系统的</font><font style="color:#000000;">Mutex Lock</font><font style="color:#000000;">来实现的，Java 的线程是映射到操作系统的原生线程之上的。如果要挂起或者唤醒一个线程，都需要操作系统帮忙完成，而操作系统实现线程之间的切换时需要从用户态转换到内核态，这个状态之间的转换需要相对比较长的时间，时间成本相对较高。</font>

<font style="color:#000000;">不过，在 Java 6 之后，</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">引入了大量的优化如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销，这些优化让</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">锁的效率提升了很多。因此，</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">还是可以在实际项目中使用的，像 JDK 源码、很多开源框架都大量使用了</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">。</font>

<font style="color:#000000;">关于偏向锁多补充一点：由于偏向锁增加了 JVM 的复杂性，同时也并没有为所有应用都带来性能提升。因此，在 JDK15 中，偏向锁被默认关闭（仍然可以使用</font><font style="color:#000000;">-XX:+UseBiasedLocking</font><font style="color:#000000;">启用偏向锁），在 JDK18 中，偏向锁已经被彻底废弃（无法通过命令行打开）。</font>

### <font style="color:#000000;">4.2 如何使用 synchronized？</font>
<font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字的使用方式主要有下面 3 种：</font>

1. <font style="color:#000000;">修饰实例方法</font>
2. <font style="color:#000000;">修饰静态方法</font>
3. <font style="color:#000000;">修饰代码块</font>

**<font style="color:#000000;">1、修饰实例方法</font>**<font style="color:#000000;">（锁当前对象实例）</font>

<font style="color:#000000;">给当前对象实例加锁，进入同步代码前要获得</font>**<font style="color:#000000;">当前对象实例的锁</font>**<font style="color:#000000;">。</font>

```java
synchronized void method() {
//业务代码
}
```

**<font style="color:#000000;">2、修饰静态方法</font>**<font style="color:#000000;">（锁当前类）</font>

<font style="color:#000000;">给当前类加锁，会作用于类的所有对象实例 ，进入同步代码前要获得</font>**<font style="color:#000000;">当前 class 的锁</font>**<font style="color:#000000;">。</font>

<font style="color:#000000;">这是因为静态成员不属于任何一个实例对象，归整个类所有，不依赖于类的特定实例，被类的所有实例共享。</font>

```java
synchronized static void method() {
    //业务代码
}
```

<font style="color:#000000;">静态</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">方法和非静态</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">方法之间的调用互斥么？不互斥！如果一个线程 A 调用一个实例对象的非静态</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">方法，而线程 B 需要调用这个实例对象所属类的静态</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">方法，是允许的，不会发生互斥现象，因为访问静态</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">方法占用的锁是当前类的锁，而访问非静态</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">方法占用的锁是当前实例对象锁。</font>

**<font style="color:#000000;">3、修饰代码块</font>**<font style="color:#000000;">（锁指定对象/类）</font>

<font style="color:#000000;">对括号里指定的对象/类加锁：</font>

+ <font style="color:#000000;">synchronized(object)</font><font style="color:#000000;">表示进入同步代码库前要获得</font>**<font style="color:#000000;">给定对象的锁</font>**<font style="color:#000000;">。</font>
+ <font style="color:#000000;">synchronized(类.class)</font><font style="color:#000000;">表示进入同步代码前要获得</font>**<font style="color:#000000;">给定 Class 的锁</font>**

```java
synchronized(this) {
    //业务代码
}
```

**<font style="color:#000000;">总结：</font>**

+ <font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字加到</font><font style="color:#000000;">static</font><font style="color:#000000;">静态方法和</font><font style="color:#000000;">synchronized(class)</font><font style="color:#000000;">代码块上都是是给 Class 类上锁；</font>
+ <font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字加到实例方法上是给对象实例上锁；</font>
+ <font style="color:#000000;">尽量不要使用</font><font style="color:#000000;">synchronized(String a)</font><font style="color:#000000;">因为 JVM 中，字符串常量池具有缓存功能。</font>

### <font style="color:#000000;">4.3 构造方法可以用 synchronized 修饰么？</font>
<font style="color:#000000;">先说结论：</font>**<font style="color:#000000;">构造方法不能使用 synchronized 关键字修饰。</font>**

<font style="color:#000000;">构造方法本身就属于线程安全的，不存在同步的构造方法一说。</font>

### <font style="color:#000000;">4.4 synchronized 底层原理了解吗？</font>
<font style="color:#000000;">synchronized 关键字底层原理属于 JVM 层面的东西。</font>

#### <font style="color:#000000;">4.4.1 synchronized 同步语句块的情况</font>
```java
public class SynchronizedDemo {
    public void method() {
        synchronized (this) {
            System.out.println("synchronized 代码块");
        }
    }
}
```

<font style="color:#000000;">通过 JDK 自带的</font><font style="color:#000000;">javap</font><font style="color:#000000;">命令查看</font><font style="color:#000000;">SynchronizedDemo</font><font style="color:#000000;">类的相关字节码信息：首先切换到类的对应目录执行</font><font style="color:#000000;">javac SynchronizedDemo.java</font><font style="color:#000000;">命令生成编译后的 .class 文件，然后执行</font><font style="color:#000000;">javap -c -s -v -l SynchronizedDemo.class</font><font style="color:#000000;">。</font>

![1732497476027-1f197070-ac4d-42e7-ae1c-f333f8169c9f.png](./img/yl5kpkT8Yz9ysjOy/1732497476027-1f197070-ac4d-42e7-ae1c-f333f8169c9f-786193.png)

<font style="color:#000000;">synchronized关键字原理</font>

<font style="color:#000000;">从上面我们可以看出：</font>**<font style="color:#000000;">synchronized</font>****<font style="color:#000000;">同步语句块的实现使用的是</font>****<font style="color:#000000;">monitorenter</font>****<font style="color:#000000;">和</font>****<font style="color:#000000;">monitorexit</font>****<font style="color:#000000;">指令，其中</font>****<font style="color:#000000;">monitorenter</font>****<font style="color:#000000;">指令指向同步代码块的开始位置，</font>****<font style="color:#000000;">monitorexit</font>************<font style="color:#000000;">指令则指明同步代码块的结束位置。</font>**

<font style="color:#000000;">上面的字节码中包含一个</font><font style="color:#000000;">monitorenter</font><font style="color:#000000;">指令以及两个</font><font style="color:#000000;">monitorexit</font><font style="color:#000000;">指令，这是为了保证锁在同步代码块代码正常执行以及出现异常的这两种情况下都能被正确释放。</font>

<font style="color:#000000;">当执行</font><font style="color:#000000;">monitorenter</font><font style="color:#000000;">指令时，线程试图获取锁也就是获取</font>**<font style="color:#000000;">对象监视器</font>************<font style="color:#000000;">monitor</font>**<font style="color:#000000;">的持有权。</font>

<font style="color:#000000;">在 Java 虚拟机(HotSpot)中，Monitor 是基于 C++实现的，由</font>[<font style="color:#000000;">ObjectMonitor</font>](https://github.com/openjdk-mirror/jdk7u-hotspot/blob/50bdefc3afe944ca74c3093e7448d6b889cd20d1/src/share/vm/runtime/objectMonitor.cpp)<font style="color:#000000;">实现的。每个对象中都内置了一个ObjectMonitor对象。另外，wait/notify等方法也依赖于monitor对象，这就是为什么只有在同步的块或者方法中才能调用wait/notify等方法，否则会抛出java.lang.IllegalMonitorStateException的异常的原因。</font>

<font style="color:#000000;">在执行</font><font style="color:#000000;">monitorenter</font><font style="color:#000000;">时，会尝试获取对象的锁，如果锁的计数器为 0 则表示锁可以被获取，获取后将锁计数器设为 1 也就是加 1。</font>

![1732497476108-bdd48dfa-617e-4c5d-8e1f-7d8bf1b614c7.png](./img/yl5kpkT8Yz9ysjOy/1732497476108-bdd48dfa-617e-4c5d-8e1f-7d8bf1b614c7-029964.png)

<font style="color:#000000;">执行 monitorenter 获取锁</font>

<font style="color:#000000;">对象锁的拥有者线程才可以执行monitorexit指令来释放锁。在执行monitorexit指令后，将锁计数器设为 0，表明锁被释放，其他线程可以尝试获取锁。</font>

![1732497476189-8a22b085-0c56-41b6-a7c0-d0667d06d9cf.png](./img/yl5kpkT8Yz9ysjOy/1732497476189-8a22b085-0c56-41b6-a7c0-d0667d06d9cf-846713.png)

<font style="color:#000000;">执行 monitorexit 释放锁</font>

<font style="color:#000000;">如果获取对象锁失败，那当前线程就要阻塞等待，直到锁被另外一个线程释放为止。</font>

#### <font style="color:#000000;">4.4.2 synchronized 修饰方法的的情况</font>
```java
public class SynchronizedDemo2 {
    public synchronized void method() {
        System.out.println("synchronized 方法");
    }
}
```

![1732497476259-cd730298-ced6-4e04-a842-80f4ba399c1e.png](./img/yl5kpkT8Yz9ysjOy/1732497476259-cd730298-ced6-4e04-a842-80f4ba399c1e-626450.png)

<font style="color:#000000;">synchronized关键字原理</font>

<font style="color:#000000;">synchronized</font><font style="color:#000000;">修饰的方法并没有</font><font style="color:#000000;">monitorenter</font><font style="color:#000000;">指令和</font><font style="color:#000000;">monitorexit</font><font style="color:#000000;">指令，取得代之的确实是</font><font style="color:#000000;">ACC_SYNCHRONIZED</font><font style="color:#000000;">标识，该标识指明了该方法是一个同步方法。JVM 通过该</font><font style="color:#000000;">ACC_SYNCHRONIZED</font><font style="color:#000000;">访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。</font>

<font style="color:#000000;">如果是实例方法，JVM 会尝试获取实例对象的锁。如果是静态方法，JVM 会尝试获取当前 class 的锁。</font>

#### <font style="color:#000000;">4.4.3 总结</font>
<font style="color:#000000;">synchronized</font><font style="color:#000000;">同步语句块的实现使用的是</font><font style="color:#000000;">monitorenter</font><font style="color:#000000;">和</font><font style="color:#000000;">monitorexit</font><font style="color:#000000;">指令，其中</font><font style="color:#000000;">monitorenter</font><font style="color:#000000;">指令指向同步代码块的开始位置，</font><font style="color:#000000;">monitorexit</font><font style="color:#000000;">指令则指明同步代码块的结束位置。</font>

<font style="color:#000000;">synchronized修饰的方法并没有monitorenter指令和monitorexit指令，取得代之的确是ACC_SYNCHRONIZED标识，该标识指明了该方法是一个同步方法。</font>

**<font style="color:#000000;">不过两者的本质都是对对象监视器 monitor 的获取。</font>**

<font style="color:#000000;">相关推荐：</font>[<font style="color:#000000;">Java 锁与线程的那些事 - 有赞技术团队</font>](https://tech.youzan.com/javasuo-yu-xian-cheng-de-na-xie-shi/)<font style="color:#000000;">。</font>

<font style="color:#000000;">进阶一下：学有余力的小伙伴可以抽时间详细研究一下对象监视器monitor。</font>

### <font style="color:#000000;">4.5 JDK1.6 之后的 synchronized 底层做了哪些优化？</font>
<font style="color:#000000;">JDK1.6 对锁的实现引入了大量的优化，如偏向锁、轻量级锁、自旋锁、适应性自旋锁、锁消除、锁粗化等技术来减少锁操作的开销。</font>

<font style="color:#000000;">锁主要存在四种状态，依次是：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态，他们会随着竞争的激烈而逐渐升级。注意锁可以升级不可降级，这种策略是为了提高获得锁和释放锁的效率。</font>

<font style="color:#000000;">关于这几种优化的详细信息可以查看下面这篇文章：</font>[synchronized锁优化](https://www.yuque.com/vip6688/neho4x/zkta2i99r6rlwmbz)

### <font style="color:#000000;">4.6 synchronized 和 volatile 有什么区别？</font>
<font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字和</font><font style="color:#000000;">volatile</font><font style="color:#000000;">关键字是两个互补的存在，而不是对立的存在！</font>

+ <font style="color:#000000;">volatile</font><font style="color:#000000;">关键字是线程同步的轻量级实现，所以</font><font style="color:#000000;">volatile</font><font style="color:#000000;">性能肯定比</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字要好 。但是</font><font style="color:#000000;">volatile</font><font style="color:#000000;">关键字只能用于变量而</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字可以修饰方法以及代码块 。</font>
+ <font style="color:#000000;">volatile</font><font style="color:#000000;">关键字能保证数据的可见性，但不能保证数据的原子性。</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字两者都能保证。</font>
+ <font style="color:#000000;">volatile</font><font style="color:#000000;">关键字主要用于解决变量在多个线程之间的可见性，而</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字解决的是多个线程之间访问资源的同步性。</font>

## <font style="color:#000000;">ReentrantLock</font>
### <font style="color:#000000;">5.1 ReentrantLock 是什么？</font>
<font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">实现了</font><font style="color:#000000;">Lock</font><font style="color:#000000;">接口，是一个可重入且独占式的锁，和</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字类似。不过，</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">更灵活、更强大，增加了轮询、超时、中断、公平锁和非公平锁等高级功能。</font>

```java
public class ReentrantLock implements Lock, java.io.Serializable {}
```

<font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">里面有一个内部类</font><font style="color:#000000;">Sync</font><font style="color:#000000;">，</font><font style="color:#000000;">Sync</font><font style="color:#000000;">继承 AQS（</font><font style="color:#000000;">AbstractQueuedSynchronizer</font><font style="color:#000000;">），添加锁和释放锁的大部分操作实际上都是在</font><font style="color:#000000;">Sync</font><font style="color:#000000;">中实现的。</font><font style="color:#000000;">Sync</font><font style="color:#000000;">有公平锁</font><font style="color:#000000;">FairSync</font><font style="color:#000000;">和非公平锁</font><font style="color:#000000;">NonfairSync</font><font style="color:#000000;">两个子类。</font>

![1732497476346-0999d27b-98ff-4626-a109-4edd72d91591.png](./img/yl5kpkT8Yz9ysjOy/1732497476346-0999d27b-98ff-4626-a109-4edd72d91591-262221.png)

<font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">默认使用非公平锁，也可以通过构造器来显式的指定使用公平锁。</font>

```java
// 传入一个 boolean 值，true 时为公平锁，false 时为非公平锁
public ReentrantLock(boolean fair) {
sync = fair ? new FairSync() : new NonfairSync();
}
```

<font style="color:#000000;">从上面的内容可以看出，ReentrantLock的底层就是由 AQS 来实现的。关于 AQS 的相关内容推荐阅读</font>[AQS 详解](https://www.yuque.com/vip6688/neho4x/ap0s309nq1cx4vab)

### <font style="color:#000000;">5.2 公平锁和非公平锁有什么区别？</font>
+ **<font style="color:#000000;">公平锁</font>**<font style="color:#000000;">: 锁被释放之后，先申请的线程先得到锁。性能较差一些，因为公平锁为了保证时间上的绝对顺序，上下文切换更频繁。</font>
+ **<font style="color:#000000;">非公平锁</font>**<font style="color:#000000;">：锁被释放之后，后申请的线程可能会先获取到锁，是随机或者按照其他优先级排序的。性能更好，但可能会导致某些线程永远无法获取到锁。</font>

### <font style="color:#000000;">5.3 synchronized 和 ReentrantLock 有什么区别？</font>
#### <font style="color:#000000;">5.3.1 两者都是可重入锁</font>
**<font style="color:#000000;">可重入锁</font>**<font style="color:#000000;">也叫递归锁，指的是线程可以再次获取自己的内部锁。比如一个线程获得了某个对象的锁，此时这个对象锁还没有释放，当其再次想要获取这个对象的锁的时候还是可以获取的，如果是不可重入锁的话，就会造成死锁。</font>

<font style="color:#000000;">JDK 提供的所有现成的</font><font style="color:#000000;">Lock</font><font style="color:#000000;">实现类，包括</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字锁都是可重入的。</font>

<font style="color:#000000;">在下面的代码中，</font><font style="color:#000000;">method1()</font><font style="color:#000000;">和</font><font style="color:#000000;">method2()</font><font style="color:#000000;">都被</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字修饰，</font><font style="color:#000000;">method1()</font><font style="color:#000000;">调用了</font><font style="color:#000000;">method2()</font><font style="color:#000000;">。</font>

```java
public class SynchronizedDemo {
    public synchronized void method1() {
        System.out.println("方法1");
        method2();
    }

    public synchronized void method2() {
        System.out.println("方法2");
    }
}
```

<font style="color:#000000;">由于</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">锁是可重入的，同一个线程在调用</font><font style="color:#000000;">method1()</font><font style="color:#000000;">时可以直接获得当前对象的锁，执行</font><font style="color:#000000;">method2()</font><font style="color:#000000;">的时候可以再次获取这个对象的锁，不会产生死锁问题。假如</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">是不可重入锁的话，由于该对象的锁已被当前线程所持有且无法释放，这就导致线程在执行</font><font style="color:#000000;">method2()</font><font style="color:#000000;">时获取锁失败，会出现死锁问题。</font>

#### <font style="color:#000000;">5.3.2 synchronized 依赖于 JVM 而 ReentrantLock 依赖于 API</font>
<font style="color:#000000;">synchronized</font><font style="color:#000000;">是依赖于 JVM 实现的，前面我们也讲到了 虚拟机团队在 JDK1.6 为</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字进行了很多优化，但是这些优化都是在虚拟机层面实现的，并没有直接暴露给我们。</font>

<font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">是 JDK 层面实现的（也就是 API 层面，需要 lock() 和 unlock() 方法配合 try/finally 语句块来完成），所以我们可以通过查看它的源代码，来看它是如何实现的。</font>

#### <font style="color:#000000;">5.3.3 ReentrantLock 比 synchronized 增加了一些高级功能</font>
<font style="color:#000000;">相比</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">，</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">增加了一些高级功能。主要来说主要有三点：</font>

+ **<font style="color:#000000;">等待可中断</font>**<font style="color:#000000;">:</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">提供了一种能够中断等待锁的线程的机制，通过</font><font style="color:#000000;">lock.lockInterruptibly()</font><font style="color:#000000;">来实现这个机制。也就是说正在等待的线程可以选择放弃等待，改为处理其他事情。</font>
+ **<font style="color:#000000;">可实现公平锁</font>**<font style="color:#000000;">:</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">可以指定是公平锁还是非公平锁。而</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">只能是非公平锁。所谓的公平锁就是先等待的线程先获得锁。</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">默认情况是非公平的，可以通过</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">类的</font><font style="color:#000000;">ReentrantLock(boolean fair)</font><font style="color:#000000;">构造方法来指定是否是公平的。</font>
+ **<font style="color:#000000;">可实现选择性通知（锁可以绑定多个条件）</font>**<font style="color:#000000;">:</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字与</font><font style="color:#000000;">wait()</font><font style="color:#000000;">和</font><font style="color:#000000;">notify()</font><font style="color:#000000;">/</font><font style="color:#000000;">notifyAll()</font><font style="color:#000000;">方法相结合可以实现等待/通知机制。</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">类当然也可以实现，但是需要借助于</font><font style="color:#000000;">Condition</font><font style="color:#000000;">接口与</font><font style="color:#000000;">newCondition()</font><font style="color:#000000;">方法。</font>

<font style="color:#000000;">如果你想使用上述功能，那么选择</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">是一个不错的选择。</font>

<font style="color:#000000;">关于</font><font style="color:#000000;">Condition</font><font style="color:#000000;">接口的补充：</font>

<font style="color:#000000;">Condition</font><font style="color:#000000;">是 JDK1.5 之后才有的，它具有很好的灵活性，比如可以实现多路通知功能也就是在一个</font><font style="color:#000000;">Lock</font><font style="color:#000000;">对象中可以创建多个</font><font style="color:#000000;">Condition</font><font style="color:#000000;">实例（即对象监视器），</font>**<font style="color:#000000;">线程对象可以注册在指定的</font>****<font style="color:#000000;">Condition</font>****<font style="color:#000000;">中，从而可以有选择性的进行线程通知，在调度线程上更加灵活。 在使用</font>****<font style="color:#000000;">notify()/notifyAll()</font>****<font style="color:#000000;">方法进行通知时，被通知的线程是由 JVM 选择的，用</font>****<font style="color:#000000;">ReentrantLock</font>****<font style="color:#000000;">类结合</font>****<font style="color:#000000;">Condition</font>****<font style="color:#000000;">实例可以实现“选择性通知”</font>**<font style="color:#000000;">，这个功能非常重要，而且是</font><font style="color:#000000;">Condition</font><font style="color:#000000;">接口默认提供的。而</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">关键字就相当于整个</font><font style="color:#000000;">Lock</font><font style="color:#000000;">对象中只有一个</font><font style="color:#000000;">Condition</font><font style="color:#000000;">实例，所有的线程都注册在它一个身上。如果执行</font><font style="color:#000000;">notifyAll()</font><font style="color:#000000;">方法的话就会通知所有处于等待状态的线程，这样会造成很大的效率问题。而</font><font style="color:#000000;">Condition</font><font style="color:#000000;">实例的</font><font style="color:#000000;">signalAll()</font><font style="color:#000000;">方法，只会唤醒注册在该</font><font style="color:#000000;">Condition</font><font style="color:#000000;">实例中的所有等待线程。</font>

### <font style="color:#000000;">5.4 可中断锁和不可中断锁有什么区别？</font>
+ **<font style="color:#000000;">可中断锁</font>**<font style="color:#000000;">：获取锁的过程中可以被中断，不需要一直等到获取锁之后 才能进行其他逻辑处理。</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">就属于是可中断锁。</font>
+ **<font style="color:#000000;">不可中断锁</font>**<font style="color:#000000;">：一旦线程申请了锁，就只能等到拿到锁以后才能进行其他的逻辑处理。</font><font style="color:#000000;">synchronized</font><font style="color:#000000;">就属于是不可中断锁。</font>

## <font style="color:#000000;">ReentrantReadWriteLock</font>
<font style="color:#000000;">ReentrantReadWriteLock</font><font style="color:#000000;">在实际项目中使用的并不多，面试中也问的比较少，简单了解即可。JDK 1.8 引入了性能更好的读写锁</font><font style="color:#000000;">StampedLock</font><font style="color:#000000;">。</font>

### <font style="color:#000000;">6.1 ReentrantReadWriteLock 是什么？</font>
<font style="color:#000000;">ReentrantReadWriteLock</font><font style="color:#000000;">实现了</font><font style="color:#000000;">ReadWriteLock</font><font style="color:#000000;">，是一个可重入的读写锁，既可以保证多个线程同时读的效率，同时又可以保证有写入操作时的线程安全。</font>

```java
public class ReentrantReadWriteLock
        implements ReadWriteLock, java.io.Serializable{
}
public interface ReadWriteLock {
    Lock readLock();
    Lock writeLock();
}
```

+ <font style="color:#000000;">一般锁进行并发控制的规则：读读互斥、读写互斥、写写互斥。</font>
+ <font style="color:#000000;">读写锁进行并发控制的规则：读读不互斥、读写互斥、写写互斥（只有读读不互斥）。</font>

<font style="color:#000000;">ReentrantReadWriteLock</font><font style="color:#000000;">其实是两把锁，一把是</font><font style="color:#000000;">WriteLock</font><font style="color:#000000;">(写锁)，一把是</font><font style="color:#000000;">ReadLock</font><font style="color:#000000;">（读锁） 。读锁是共享锁，写锁是独占锁。读锁可以被同时读，可以同时被多个线程持有，而写锁最多只能同时被一个线程持有。</font>

<font style="color:#000000;">和</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">一样，</font><font style="color:#000000;">ReentrantReadWriteLock</font><font style="color:#000000;">底层也是基于 AQS 实现的。</font>

![1732497476414-bd38cb57-f927-48fa-9663-c858320c5dc4.png](./img/yl5kpkT8Yz9ysjOy/1732497476414-bd38cb57-f927-48fa-9663-c858320c5dc4-028675.png)

<font style="color:#000000;">ReentrantReadWriteLock</font><font style="color:#000000;">也支持公平锁和非公平锁，默认使用非公平锁，可以通过构造器来显示的指定。</font>

```java
// 传入一个 boolean 值，true 时为公平锁，false 时为非公平锁
public ReentrantReadWriteLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
    readerLock = new ReadLock(this);
    writerLock = new WriteLock(this);
}
```

### <font style="color:#000000;">6.2 ReentrantReadWriteLock 适合什么场景？</font>
<font style="color:#000000;">由于</font><font style="color:#000000;">ReentrantReadWriteLock</font><font style="color:#000000;">既可以保证多个线程同时读的效率，同时又可以保证有写入操作时的线程安全。因此，在读多写少的情况下，使用</font><font style="color:#000000;">ReentrantReadWriteLock</font><font style="color:#000000;">能够明显提升系统性能。</font>

### <font style="color:#000000;">6.3 共享锁和独占锁有什么区别？</font>
+ **<font style="color:#000000;">共享锁</font>**<font style="color:#000000;">：一把锁可以被多个线程同时获得。</font>
+ **<font style="color:#000000;">独占锁</font>**<font style="color:#000000;">：一把锁只能被一个线程获得。</font>

### <font style="color:#000000;">6.4 线程持有读锁还能获取写锁吗？</font>
+ <font style="color:#000000;">在线程持有读锁的情况下，该线程不能取得写锁(因为获取写锁的时候，如果发现当前的读锁被占用，就马上获取失败，不管读锁是不是被当前线程持有)。</font>
+ <font style="color:#000000;">在线程持有写锁的情况下，该线程可以继续获取读锁（获取读锁时如果发现写锁被占用，只有写锁没有被当前线程占用的情况才会获取失败）。</font>

<font style="color:#000000;">读写锁的源码分析，推荐阅读</font>[<font style="color:#000000;">聊聊 Java 的几把 JVM 级锁 - 阿里巴巴中间件</font>](https://mp.weixin.qq.com/s/h3VIUyH9L0v14MrQJiiDbw)<font style="color:#000000;">这篇文章，写的很不错。</font>

### <font style="color:#000000;">6.5 读锁为什么不能升级为写锁？</font>
<font style="color:#000000;">写锁可以降级为读锁，但是读锁却不能升级为写锁。这是因为读锁升级为写锁会引起线程的争夺，毕竟写锁属于是独占锁，这样的话，会影响性能。</font>

<font style="color:#000000;">另外，还可能会有死锁问题发生。举个例子：假设两个线程的读锁都想升级写锁，则需要对方都释放自己锁，而双方都不释放，就会产生死锁。</font>

## <font style="color:#000000;">StampedLock</font>
<font style="color:#000000;">StampedLock</font><font style="color:#000000;">面试中问的比较少，不是很重要，简单了解即可。</font>

### <font style="color:#000000;">7.1 StampedLock 是什么？</font>
<font style="color:#000000;">StampedLock</font><font style="color:#000000;">是 JDK 1.8 引入的性能更好的读写锁，不可重入且不支持条件变量</font><font style="color:#000000;">Conditon</font><font style="color:#000000;">。</font>

<font style="color:#000000;">不同于一般的</font><font style="color:#000000;">Lock</font><font style="color:#000000;">类，</font><font style="color:#000000;">StampedLock</font><font style="color:#000000;">并不是直接实现</font><font style="color:#000000;">Lock</font><font style="color:#000000;">或</font><font style="color:#000000;">ReadWriteLock</font><font style="color:#000000;">接口，而是基于</font>**<font style="color:#000000;">CLH 锁</font>**<font style="color:#000000;">独立实现的（AQS 也是基于这玩意）。</font>

```java
public class StampedLock implements java.io.Serializable {
}
```

<font style="color:#000000;">StampedLock</font><font style="color:#000000;">提供了三种模式的读写控制模式：读锁、写锁和乐观读。</font>

+ **<font style="color:#000000;">写锁</font>**<font style="color:#000000;">：独占锁，一把锁只能被一个线程获得。当一个线程获取写锁后，其他请求读锁和写锁的线程必须等待。类似于</font><font style="color:#000000;">ReentrantReadWriteLock</font><font style="color:#000000;">的写锁，不过这里的写锁是不可重入的。</font>
+ **<font style="color:#000000;">读锁</font>**<font style="color:#000000;">（悲观读）：共享锁，没有线程获取写锁的情况下，多个线程可以同时持有读锁。如果己经有线程持有写锁，则其他线程请求获取该读锁会被阻塞。类似于</font><font style="color:#000000;">ReentrantReadWriteLock</font><font style="color:#000000;">的读锁，不过这里的读锁是不可重入的。</font>
+ **<font style="color:#000000;">乐观读</font>**<font style="color:#000000;">：允许多个线程获取乐观读以及读锁。同时允许一个写线程获取写锁。</font>

<font style="color:#000000;">另外，</font><font style="color:#000000;">StampedLock</font><font style="color:#000000;">还支持这三种锁在一定条件下进行相互转换 。</font>

```java
long tryConvertToWriteLock(long stamp){}
long tryConvertToReadLock(long stamp){}
long tryConvertToOptimisticRead(long stamp){}
```

<font style="color:#000000;">StampedLock</font><font style="color:#000000;">在获取锁的时候会返回一个 long 型的数据戳，该数据戳用于稍后的锁释放参数，如果返回的数据戳为 0 则表示锁获取失败。当前线程持有了锁再次获取锁还是会返回一个新的数据戳，这也是</font><font style="color:#000000;">StampedLock</font><font style="color:#000000;">不可重入的原因。</font>

```java
// 写锁
public long writeLock() {
    long s, next;  // bypass acquireWrite in fully unlocked case only
    return ((((s = state) & ABITS) == 0L &&
             U.compareAndSwapLong(this, STATE, s, next = s + WBIT)) ?
            next : acquireWrite(false, 0L));
}
// 读锁
public long readLock() {
    long s = state, next;  // bypass acquireRead on common uncontended case
    return ((whead == wtail && (s & ABITS) < RFULL &&
             U.compareAndSwapLong(this, STATE, s, next = s + RUNIT)) ?
            next : acquireRead(false, 0L));
}
// 乐观读
public long tryOptimisticRead() {
    long s;
    return (((s = state) & WBIT) == 0L) ? (s & SBITS) : 0L;
}
```

### <font style="color:#000000;">7.2 StampedLock 的性能为什么更好？</font>
<font style="color:#000000;">相比于传统读写锁多出来的乐观读是</font><font style="color:#000000;">StampedLock</font><font style="color:#000000;">比</font><font style="color:#000000;">ReadWriteLock</font><font style="color:#000000;">性能更好的关键原因。</font><font style="color:#000000;">StampedLock</font><font style="color:#000000;">的乐观读允许一个写线程获取写锁，所以不会导致所有写线程阻塞，也就是当读多写少的时候，写线程有机会获取写锁，减少了线程饥饿的问题，吞吐量大大提高。</font>

### <font style="color:#000000;">7.3 StampedLock 适合什么场景？</font>
<font style="color:#000000;">和</font><font style="color:#000000;">ReentrantReadWriteLock</font><font style="color:#000000;">一样，</font><font style="color:#000000;">StampedLock</font><font style="color:#000000;">同样适合读多写少的业务场景，可以作为</font><font style="color:#000000;">ReentrantReadWriteLock</font><font style="color:#000000;">的替代品，性能更好。</font>

<font style="color:#000000;">不过，需要注意的是</font><font style="color:#000000;">StampedLock</font><font style="color:#000000;">不可重入，不支持条件变量</font><font style="color:#000000;">Conditon</font><font style="color:#000000;">，对中断操作支持也不友好（使用不当容易导致 CPU 飙升）。如果你需要用到</font><font style="color:#000000;">ReentrantLock</font><font style="color:#000000;">的一些高级性能，就不太建议使用</font><font style="color:#000000;">StampedLock</font><font style="color:#000000;">了。</font>

<font style="color:#000000;">另外，StampedLock性能虽好，但使用起来相对比较麻烦，一旦使用不当，就会出现生产问题。强烈建议你在使用StampedLock之前，看看</font>[<font style="color:#000000;">StampedLock 官方文档中的案例</font>](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/StampedLock.html)<font style="color:#000000;">。</font>

### <font style="color:#000000;">7.4 StampedLock 的底层原理了解吗？</font>
<font style="color:#000000;">StampedLock</font><font style="color:#000000;">不是直接实现</font><font style="color:#000000;">Lock</font><font style="color:#000000;">或</font><font style="color:#000000;">ReadWriteLock</font><font style="color:#000000;">接口，而是基于</font>**<font style="color:#000000;">CLH 锁</font>**<font style="color:#000000;">实现的（AQS 也是基于这玩意），CLH 锁是对自旋锁的一种改良，是一种隐式的链表队列。</font><font style="color:#000000;">StampedLock</font><font style="color:#000000;">通过 CLH 队列进行线程的管理，通过同步状态值</font><font style="color:#000000;">state</font><font style="color:#000000;">来表示锁的状态和类型。</font>

<font style="color:#000000;">StampedLock</font><font style="color:#000000;">的原理和 AQS 原理比较类似，这里就不详细介绍了，感兴趣的可以看看下面这两篇文章：</font>

+ [AQS 详解](https://www.yuque.com/vip6688/neho4x/ap0s309nq1cx4vab)
+ [<font style="color:#000000;">StampedLock 底层原理分析</font>](https://segmentfault.com/a/1190000015808032)

<font style="color:#000000;">如果你只是准备面试的话，建议多花点精力搞懂 AQS 原理即可，</font><font style="color:#000000;">StampedLock</font><font style="color:#000000;">底层原理在面试中遇到的概率非常小。</font>

## <font style="color:#000000;">Atomic 原子类</font>
<font style="color:#000000;">Atomic 原子类部分的内容我单独写了一篇文章来总结：</font>[Atomic 原子类总结](https://www.yuque.com/vip6688/neho4x/redtpvha6bu6x6pf)

## <font style="color:#000000;">参考</font>
+ <font style="color:#000000;">《深入理解 Java 虚拟机》</font>
+ <font style="color:#000000;">《实战 Java 高并发程序设计》</font>
+ <font style="color:#000000;">Guide to the Volatile Keyword in Java - Baeldung：</font>[<font style="color:#000000;">https://www.baeldung.com/java-volatile</font>](https://www.baeldung.com/java-volatile)
+ <font style="color:#000000;">不可不说的 Java“锁”事 - 美团技术团队：</font>[<font style="color:#000000;">https://tech.meituan.com/2018/11/15/java-lock.html</font>](https://tech.meituan.com/2018/11/15/java-lock.html)
+ <font style="color:#000000;">在 ReadWriteLock 类中读锁为什么不能升级为写锁？：</font>[<font style="color:#000000;">https://cloud.tencent.com/developer/article/1176230</font>](https://cloud.tencent.com/developer/article/1176230)
+ <font style="color:#000000;">高性能解决线程饥饿的利器 StampedLock：</font>[<font style="color:#000000;">https://mp.weixin.qq.com/s/2Acujjr4BHIhlFsCLGwYSg</font>](https://mp.weixin.qq.com/s/2Acujjr4BHIhlFsCLGwYSg)
+ <font style="color:#000000;">理解 Java 中的 ThreadLocal - 技术小黑屋：</font>[<font style="color:#000000;">https://droidyue.com/blog/2016/03/13/learning-threadlocal-in-java/</font>](https://droidyue.com/blog/2016/03/13/learning-threadlocal-in-java/)
+ <font style="color:#000000;">ThreadLocal (Java Platform SE 8 ) - Oracle Help Center：</font>[<font style="color:#000000;">https://docs.oracle.com/javase/8/docs/api/java/lang/ThreadLocal.html</font>](https://docs.oracle.com/javase/8/docs/api/java/lang/ThreadLocal.html)





> 更新: 2024-01-19 01:23:45  
原文: [https://www.yuque.com/vip6688/neho4x/exbip1p0plhqkeax](https://www.yuque.com/vip6688/neho4x/exbip1p0plhqkeax)
>



> 更新: 2024-11-25 09:17:57  
> 原文: <https://www.yuque.com/neumx/laxg2e/6982e0d3318da67c93d1b9e32636db37>