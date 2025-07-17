# Java基础面试

# Java基础面试
## <font style="color:#000000;">1. equals 与 == 的区别 </font>
**<font style="color:#000000;">==</font>**

+ <font style="color:#000000;">如果是</font>**<font style="color:#000000;">基本类型</font>**<font style="color:#000000;">，==表示判断</font><font style="color:#000000;background-color:#fbde28;">它们值</font><font style="color:#000000;">是否相等;</font>
+ <font style="color:#000000;">如果是</font>**<font style="color:#000000;">引用对象</font>**<font style="color:#000000;">，==表示判断两个对象指向的</font><font style="color:#000000;background-color:#fbde28;">内存地址</font><font style="color:#000000;">是否相同。</font>

**<font style="color:#000000;">equals</font>**

+ <font style="color:#000000;">如果是</font>**<font style="color:#000000;">字符串</font>**<font style="color:#000000;">，表示判断</font><font style="color:#000000;background-color:#fbde28;">字符串内容</font><font style="color:#000000;">是否相同;</font>
+ <font style="color:#000000;">如果是 </font>**<font style="color:#000000;">object 对象的方法</font>**<font style="color:#000000;">，比较的也是</font><font style="color:#000000;background-color:#fbde28;">引用的内存地址值</font><font style="color:#000000;">;</font>
+ <font style="color:#000000;">如果自己的类</font>**<font style="color:#000000;">重写 equals 方法</font>**<font style="color:#000000;">，可以</font><font style="color:#000000;background-color:#fbde28;">自定义两个对象</font><font style="color:#000000;">是否相等。</font>

## <font style="color:#000000;">2. final, finally, finalize 的区别</font>
+ <font style="color:#000000;">final 用于修饰</font>**<font style="color:#000000;">属性</font>**<font style="color:#000000;">,</font>**<font style="color:#000000;">方法</font>**<font style="color:#000000;">和</font>**<font style="color:#000000;">类</font>**<font style="color:#000000;">, 分别表示属性</font><font style="color:#000000;background-color:#fbde28;">不能被重新赋值</font><font style="color:#000000;">, 方法</font><font style="color:#000000;background-color:#fbde28;">不可被覆盖</font><font style="color:#000000;">, 类 </font><font style="color:#000000;background-color:#fbde28;">不可被继承</font><font style="color:#000000;">.</font>
+ <font style="color:#000000;">finally 是</font><font style="color:#000000;background-color:#fbde28;">异常处理语句</font><font style="color:#000000;">结构的一部分，一般以 </font>**<font style="color:#000000;">try-catch-finally</font>************<font style="color:#000000;"> </font>**<font style="color:#000000;">出现，finally 代码块表示</font><font style="color:#000000;background-color:#fbde28;">总是被执行.</font>
+ <font style="color:#000000;">finalize 是 </font>**<font style="color:#000000;">Object 类</font>**<font style="color:#000000;">的一个方法，该方法一般由垃圾回收器来调用，当我们调用 </font>**<font style="color:#000000;">System.gc()</font>**<font style="color:#000000;"> 方法的时候，</font><font style="color:#000000;background-color:#fbde28;">由垃圾回收器调用 finalize()方法</font><font style="color:#000000;">，回收垃圾，JVM 并不保证此方法总被调用。但其执行“不稳定”，且有一定的性能问题，已经在 JDK 9 中被设置为弃用的方法了。</font>

## <font style="color:#000000;">3. 重载和重写的区别</font>
+ <font style="color:#000000;">重载表示同一个类中可以</font><font style="color:#000000;background-color:#fbde28;">有多个名称相同的方法</font><font style="color:#000000;">，但这些方法的</font><font style="color:#000000;background-color:#fbde28;">返回类型相同</font><font style="color:#000000;">，</font><font style="color:#000000;background-color:#fbde28;">参数列表各不相同</font><font style="color:#000000;">(即参数个数或类型不同)，JVM 调用方法是通过方法签名来判断到底要调用哪个方法的，而方法签名 = </font>**<font style="color:#000000;">方法名称</font>**<font style="color:#000000;"> + </font>**<font style="color:#000000;">参数类型</font>**<font style="color:#000000;"> + </font>**<font style="color:#000000;">参数个数</font>**<font style="color:#000000;">组成的一个唯一值，这个</font>**<font style="color:#000000;">唯一值</font>**<font style="color:#000000;">就是方法签名。</font>
+ <font style="color:#000000;">重写必须继承，重载不用。</font>
+ <font style="color:#000000;">重写表示子类中的方法与父类中的某个方法的</font>**<font style="color:#000000;">名称</font>**<font style="color:#000000;">、</font>**<font style="color:#000000;">参数类型</font>**<font style="color:#000000;">、</font>**<font style="color:#000000;">参数个数</font>**<font style="color:#000000;background-color:#fbde28;">完全相同</font><font style="color:#000000;">，通过子类实例对象调用这个方法时，将调用子类中的定义方法，这相当于把父类中定义的那个完全相同的</font><font style="color:#000000;background-color:#fbde28;">方法给覆盖了</font><font style="color:#000000;">，这是面向对象编程的</font>**<font style="color:#000000;">多态性</font>**<font style="color:#000000;">的一种表现。</font>
+ <font style="color:#000000;">重写的</font><font style="color:#000000;background-color:#fbde28;">方法修饰符大于等于</font><font style="color:#000000;">父类的方法，即访问权限只能比父类的更大，不能更小，而重载和修饰符无关。</font>
+ <font style="color:#000000;">重写覆盖的方法中，只能比父类</font><font style="color:#000000;background-color:#fbde28;">抛出更少的异常</font><font style="color:#000000;">，或者是抛出父类抛出的异常的子异常。</font>**<font style="color:#000000;">返回类型</font>**<font style="color:#000000;">不能比父类大。</font>

## <font style="color:#000000;">4. 两个对象的 hashCode()相同，则 equals()是否也一定为 true?</font>
<font style="color:#000000;">两个对象 equals 相等，则它们的 hashcode 必须相等，如果两个对象的 hashCode()相同，则 equals()不一定为 true。</font>

<font style="color:#000000;">hashCode </font><font style="color:#000000;">的常规协定:</font>

+ <font style="color:#000000;">在 Java 应用程序执行期间，在对同一对象多次调用 hashCode 方法时，必须一致地返回相同的整数，前提是将对象进行 equals 比较时所用的信息没有被修改。 从某一应用程序的一次执行到同一应用程序的另一次执行，该整数无需保持一致。</font>
+ <font style="color:#000000;">两个对象的 equals()相等，那么对这两个对象中的每个对象调用 hashCode 方法都必须生成相同的整数结果。</font>
+ <font style="color:#000000;">两个对象的 equals()不相等，那么对这两个对象中的任一对象上调用 hashCode 方法不要求一定生成不同的整数结果。但是，为不相等的对象生成不同整数结果可以提高哈希表的性能。</font>

## <font style="color:#000000;">5. 为什么重写 equals 时，必须重写 hashCode？</font>
<font style="color:#000000;">equals 和 hashCode 两个方法是用来</font><font style="color:#000000;background-color:#fbde28;">协同判断两个对象是否相等的</font><font style="color:#000000;">，采用这种方式的原因是可以</font><font style="color:#000000;background-color:#fbde28;">提高程序插入和查询的速度</font><font style="color:#000000;">，如果在重写 equals 时，不重写 hashCode，就会导致在某些场景下，例如将两个相等的自定义对象存储在 Set 集合时，就会出现程序执行的异常，为了保证程序的正常执行，所以我们就需要在重写 equals 时，也一并重写 hashCode 方法才行。</font>

<font style="color:#000000;">异常问题分析</font>

<font style="color:#000000;">比如在 Set 集合存储数据时，如果只重写了 equals 方法，那么默认情况下，Set 进行去重判断时，会</font><font style="color:#000000;background-color:#fbde28;">先判断两个对象的 hashCode 是否相同</font><font style="color:#000000;">，而此时因为没有重写 hashCode 方法，所以会</font><font style="color:#000000;background-color:#fbde28;">默认调用 Object 中的 hashCode 方法</font><font style="color:#000000;">，而 Object 中的 hashCode 方法会得到两个对象的哈希值，而两个对象因为</font><font style="color:#000000;background-color:#fbde28;">引用地址不同</font><font style="color:#000000;">，所以哈希的结果也是不同的，因此会直接返回 false，于是 Set 集合就插入了两个相等的对象。</font>

<font style="color:#000000;">但是，如果在重写 equals 方法时，也重写了 hashCode 方法，那么在执行判断时会去</font><font style="color:#000000;background-color:#fbde28;">执行重写的 hashCode 方法</font><font style="color:#000000;">，此时对比的是两个对象的</font><font style="color:#000000;background-color:#fbde28;">所有属性的 hashCode</font><font style="color:#000000;">，因为属性为基础数据类型或 String，所以哈希之后得到的 hashCode 值也是相同的，于是再去调用 equals 方法，发现两个对象确实是相等的，于是就返回 true 了，因此 Set 集合就不会存储两个相等的数据了，那么整个程序也能正常执行了。</font>

## <font style="color:#000000;">6. </font><font style="color:#000000;">抽象类和接口有什么区别?</font>
+ <font style="color:#000000;">抽象类要被子类</font>**<font style="color:#117cee;">继承</font>**<font style="color:#000000;">，接口要被子类</font>**<font style="color:#117cee;">实现</font>**<font style="color:#000000;">。</font>
+ <font style="color:#000000;">抽象类可以</font>**<font style="color:#117cee;">有构造方法</font>**<font style="color:#000000;">，接口中不能有构造方法。</font>
+ <font style="color:#000000;">抽象类中可以有</font>**<font style="color:#117cee;">普通成员变量</font>**<font style="color:#000000;">，接口中没有普通成员变量，它的变量只能是</font>**<font style="color:#117cee;">公共的静态的常量</font>**
+ <font style="color:#000000;">一个类可以实</font><font style="color:#000000;background-color:#fbde28;">现多个接口</font><font style="color:#000000;">，但是只能</font><font style="color:#000000;background-color:#fbde28;">继承一个父类</font><font style="color:#000000;">，这个父类可以是抽象类。</font>
+ <font style="color:#000000;">接口只能做方法声明，抽象类中可以作</font>**<font style="color:#117cee;">方法声明</font>**<font style="color:#000000;">，也可以做</font>**<font style="color:#117cee;">方法实现</font>**<font style="color:#000000;">。</font>
+ <font style="color:#000000;">抽象级别(从高到低):</font><font style="color:#000000;background-color:#fbde28;">接口>抽象类>实现类</font><font style="color:#000000;">。</font>
+ <font style="color:#000000;">抽象类主要是用来</font><font style="color:#000000;background-color:#fbde28;">抽象类别</font><font style="color:#000000;">，接口主要是用来</font><font style="color:#000000;background-color:#fbde28;">抽象方法功能。</font>
+ <font style="color:#000000;">抽象类的关键字是 </font>**<font style="color:#000000;">abstract</font>**<font style="color:#000000;">，接口的关键字是 </font>**<font style="color:#000000;">interface</font>**

## <font style="color:#000000;">7. BIO、NIO、AIO 有什么区别?</font>
<font style="color:#000000;">它们三者都是用来实现 IO（Input/Output）操作的，它们的区别如下。</font>

+ **<font style="color:#000000;">BIO</font>**<font style="color:#000000;"> 传统的 java.io 包，它是基于流模型实现的，交互的方式是</font><font style="color:#000000;background-color:#fbde28;">同步、阻塞方式</font><font style="color:#000000;">，也就是说在读入输入流或者输出流时，在读写动作完成之前，线程会一直阻塞在那里，它们之间的调用是</font><font style="color:#000000;background-color:#fbde28;">可靠的线性顺序</font><font style="color:#000000;">。它的优点就是代码比较简单、直观；缺点就是 IO 的效率和扩展性很低，</font><font style="color:#000000;background-color:#fbde28;">容易成为应用性能瓶颈</font><font style="color:#000000;">。</font>
+ **<font style="color:#000000;">NIO</font>**<font style="color:#000000;"> 是 Java 1.4 引入的 java.nio 包，提供了 Channel、Selector、Buffer 等新的抽象，可以构建</font><font style="color:#000000;background-color:#fbde28;">多路复用的、同步非阻塞 IO</font><font style="color:#000000;"> 程序，同时提供了更接近操作系统底层高性能的数据操作方式。</font>
+ **<font style="color:#000000;">AIO</font>**<font style="color:#000000;"> 是 Java 1.7 之后引入的包，是 NIO 的升级版本，提供了</font><font style="color:#000000;background-color:#fbde28;">异步非堵塞的 IO </font><font style="color:#000000;">操作方式，因此人们叫它 AIO（Asynchronous IO），</font><font style="color:#000000;background-color:#fbde28;">异步 IO 是基于事件和回调机制实现的</font><font style="color:#000000;">，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。</font>

<font style="color:#000000;">简单来说：BIO 就是传统 IO 包，产生的最早；NIO 是对 BIO 的改进提供了多路复用的同步非阻塞 IO，而 AIO 是 NIO 的升级，提供了异步非阻塞 IO。</font>

**<font style="color:#000000;">BIO 是一个连接一个线程。</font>**

**<font style="color:#000000;">NIO 是一个请求一个线程。</font>**

**<font style="color:#000000;">AIO 是一个有效请求 一个线程。</font>**<font style="color:#000000;"> </font>

+ <font style="color:#000000;">BIO:同步并阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请 求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造 成不必要的线程开销，当然可以通过线程池机制改善。</font>
+ <font style="color:#000000;">NIO:同步非阻塞，服务器实现模式为一个请求一个线程，即客户端发送的连 接请求都会注册到多路复用器上，多路复用器轮询到连接有 I/O 请求时才启动 一个线程进行处理。</font>
+ <font style="color:#000000;">AIO:异步非阻塞，服务器实现模式为一个有效请求一个线程，客户端的 IO 请 求都是由 OS 先完成了再通知服务器应用去启动线程进行处理。</font>

## <font style="color:#000000;">8. String，Stringbuffer，StringBuilder 的区别?</font>
**<font style="color:#000000;">String</font>**<font style="color:#000000;">:</font>

+ <font style="color:#000000;">String 类是一个不可变的类，一旦创建就不可以修改。</font>
+ <font style="color:#000000;">String 是 final 类，不能被继承</font>
+ <font style="color:#000000;">String 重写了 equals()方法和 hashCode()方法</font>

**<font style="color:#000000;">StringBuffer</font>**<font style="color:#000000;">:</font>

+ <font style="color:#000000;">继承自 AbstractStringBuilder，是可变类。</font>
+ <font style="color:#000000;">StringBuffer 使用 synchronized 来保证线程安全</font>
+ <font style="color:#000000;">可以通过 append 方法动态构造数据。</font>

**<font style="color:#000000;">StringBuilder</font>**<font style="color:#000000;">:</font>

+ <font style="color:#000000;">继承自 AbstractStringBuilder，是可变类。</font>
+ <font style="color:#000000;">StringBuilder 是非线程安全的。</font>
+ <font style="color:#000000;">执行效率比 StringBuffer 高。</font>

## <font style="color:#000000;">9. JAVA 中的几种基本数据类型是什么，各自占用多少字节呢?</font>
<font style="color:#000000;">在 Java 中，一共有 8 种基本类型（primitive type），其中有 4 种整型、2 种浮点类型、1 种用于表示 Unicode 编码的字符类型 char 和 1 种用于表示真假值的 boolean 类型。</font>

+ <font style="color:#000000;">4 种整型：</font>
    - <font style="color:#000000;">int </font><font style="color:#000000;">32位，4字节</font>
    - <font style="color:#000000;">short </font><font style="color:#000000;">16位，2字节</font>
    - <font style="color:#000000;">long 64位，8字节</font>
    - <font style="color:#000000;">byte 8位，1字节</font>
+ <font style="color:#000000;">2 种浮点类型：</font>
    - <font style="color:#000000;">float 32位，4字节</font>
    - <font style="color:#000000;">double 64位，8字节</font>
+ <font style="color:#000000;">字符类型：</font>
    - <font style="color:#000000;">char 16位，2字节</font>
+ <font style="color:#000000;">真假类型：</font>
    - <font style="color:#000000;">boolean </font>

## <font style="color:#000000;">10. Comparator 与 Comparable 有什么区别?</font>
+ <font style="color:#000000;">Comparable & Comparator 都是用来实现</font><font style="color:#000000;background-color:#fbde28;">集合中元素的比较、排序</font><font style="color:#000000;">的，只 是 Comparable 是在</font><font style="color:#000000;background-color:#fbde28;">集合内部定义的方法</font><font style="color:#000000;">实现的排序，Comparator 是在</font><font style="color:#000000;background-color:#fbde28;">集合外部实现</font><font style="color:#000000;">的排序，所以，如想实现排序，就需要在集合外定义 Comparator 接口的方法或在集合内实现 Comparable 接口的方法。</font>
+ <font style="color:#000000;">Comparator 位于包 </font>**<font style="color:#000000;">java.util</font>**<font style="color:#000000;"> 下，而 Comparable 位于包 </font>**<font style="color:#000000;">java.lang</font>**<font style="color:#000000;"> 下。</font>
+ <font style="color:#000000;">Comparable 是一个</font><font style="color:#000000;background-color:#fbde28;">对象本身就已经支持自比较所需要实现的接口</font><font style="color:#000000;">(如String、Integer 自己就可以完成比较大小操作，已经实现了 Comparable 接口) 自定义的类要在加入 list 容器中后能够排序，可以实现 Comparable 接口，在用 Collections 类的 sort 方法排序时，如果不指定 Comparator，那么 就以自然顺序排序， 这里的自然顺序就是实现 Comparable 接口设定的排序方式。</font>
+ <font style="color:#000000;">而 Comparator 是</font><font style="color:#000000;background-color:#fbde28;">一个专用的比较器</font><font style="color:#000000;">，当这个对象</font><font style="color:#000000;background-color:#fbde28;">不支持自比较或者自比较函数不能满足</font><font style="color:#000000;">你的要求时，你可以写一个比较器来完成两个对象之间大小的比较。</font>
+ <font style="color:#000000;">可以说一个是</font><font style="color:#000000;background-color:#fbde28;">自己完成比较</font><font style="color:#000000;">，一个是</font><font style="color:#000000;background-color:#fbde28;">外部程序实现比较</font><font style="color:#000000;">的差别而已。 用 Comparator 是策略模式(strategy design pattern)，就是不改变对象自身，而用一个策略对象(strategy object)来改变它的行为。 比如:你想对整数采用绝对值大小来排序，Integer 是不符合要求的，你不需要去修改 Integer 类(实际上你也不能这么做)去改变它的排序行为，只要使用一个实 现了 Comparator 接口的对象来实现控制它的排序就行了。</font>

<font style="color:#000000;">第一种：实体类自己实现Comparable接口比较</font>

```java
public class Student implements Comparable<Student> {
        private String name;
        private int age;

        @Override
        public int compareTo(Student o) {
            int flag = this.name.compareTo(o.name);
            if (flag == 0) {
                flag = this.age - o.age;
            }
            return flag;
        }
    }
    Collections.sort(students);
```

<font style="color:#000000;">第二种：借助比较器进行排序。</font>

```java
public class Student {
        private String name;
        private int age;
    }

    Collections.sort(students,(o1,o2) -> {
        int flag = o1.getName().compareTo(o2.getName());
        if (flag == 0) {
            flag = o1.getAge() - o2.getAge();
        }
        return flag;
    });
```

## **<font style="color:#000000;">11. String为什么设计成不可变的？（final修饰符）</font>**
<font style="color:#000000;">主要是从缓存、安全性、线程安全和性能等角度出发的。</font>

**<font style="color:#000000;">缓存</font>**

<font style="color:#000000;">字符串是使用最广泛的数据结构。</font><font style="color:#000000;background-color:#fbde28;">大量的字符串的创建是非常耗费资源的</font><font style="color:#000000;">，所以，Java提供了对字符串的缓存功能，可以大大的节省堆空间。</font>

<font style="color:#000000;">JVM中专门开辟了一部分空间来存储Java字符串，那就是</font><font style="color:#000000;background-color:#fbde28;">字符串池</font><font style="color:#000000;">。</font>

<font style="color:#000000;">通过字符串池，两个内容相同的字符串变量，</font><font style="color:#000000;background-color:#fbde28;">可以从池中指向同一个字符串对象</font><font style="color:#000000;">，从而节省了关键的内存资源。</font>

<font style="color:#000000;">对于这个例子，s和s2都表示"abcd"，所以他们会指向字符串池中的同一个字符串对象</font>

<font style="color:#000000;">但是，之所以可以这么做，主要是</font><font style="color:#000000;background-color:#fbde28;">因为字符串的不变性</font><font style="color:#000000;">。试想一下，如果字符串是可变的，我们一旦修改了s的内容，那必然导致s2的内容也被动的改变了，这显然不是我们想看到的。</font>

<font style="color:#000000;">当你在传参时不需要考虑谁会修改它的值；</font><font style="color:#000000;background-color:#fbde28;">如果是可变类的话，则有可能需要重新拷贝出来一个新值进行传参</font><font style="color:#000000;">，这样在性能上就会有一定的损失。</font>

**<font style="color:#000000;">安全</font>**

<font style="color:#000000;">不可变会自动使字符串成为线程安全的，</font><font style="color:#000000;background-color:#fbde28;">因为当从多个线程访问它们时，它们不会被更改。</font>

<font style="color:#000000;">因此，一般来说，不可变对象可以在同时运行的多个线程之间共享。它们也是线程安全的，因为如果线程更改了值，那么将在字符串池中</font><font style="color:#000000;background-color:#fbde28;">创建一个新的字符串，而不是修改相同的值</font><font style="color:#000000;">。因此，字符串对于多线程来说是安全的。</font><font style="color:#000000;">String </font><font style="color:#000000;">类中有很多调用底层的本地方法，调用了操作系统的 API，</font><font style="color:#000000;background-color:#fbde28;">如果方法可以重写，可能被植入恶意代码，破坏程序。</font>

**<font style="color:#000000;">hashcode缓存</font>**

<font style="color:#000000;">由于字符串对象被广泛地用作数据结构，它们也被广泛地用于哈希实现，如HashMap、HashTable、HashSet等。在对这些散列实现进行操作时，经常调用hashCode()方法。</font>

<font style="color:#000000;">不可变性保证了字符串的值不会改变。因此，hashCode()方法在String类中被重写，以方便缓存，这样在第一次hashCode()调用期间计算和缓存散列，并从那时起返回相同的值。</font>

**<font style="color:#000000;">性能</font>**

<font style="color:#000000;">前面提到了的字符串池、hashcode缓存等，都是提升性能的体现。</font>

<font style="color:#000000;">因为字符串不可变，所以可以用字符串池缓存，可以大大节省堆内存。而且还可以提前对hashcode进行缓存，更加高效</font>

<font style="color:#000000;">由于字符串是应用最广泛的数据结构，提高字符串的性能对提高整个应用程序的总体性能有相当大的影响。</font>

## <font style="color:#000000;">11. 说说 Java 中多态的实现原理</font>
+ <font style="color:#000000;">多态机制包括</font><font style="color:#000000;background-color:#fbde28;">静态多态</font><font style="color:#000000;">(编译时多态)和</font><font style="color:#000000;background-color:#fbde28;">动态多态</font><font style="color:#000000;">(运行时多态)</font>
+ <font style="color:#000000;">静态多态比如说</font><font style="color:#000000;background-color:#fbde28;">重载</font><font style="color:#000000;">，动态多态一般指在</font><font style="color:#000000;background-color:#fbde28;">运行时才能确定调用哪个方法</font><font style="color:#000000;">。</font>
+ <font style="color:#000000;">我们</font><font style="color:#000000;background-color:#fbde28;">通常所说的多态一般指运行时多态</font><font style="color:#000000;">，也就是编译时不确定究竟调用哪个具体方法，一直等到运行时才能确定。</font>
+ <font style="color:#000000;">多态实现方式</font><font style="color:#000000;background-color:#fbde28;">:子类继承父类(extends)和类实现接口(implements)</font>
+ <font style="color:#000000;background-color:#fbde28;">多态核心之处就在于对父类方法的改写或对接口方法的实现</font><font style="color:#000000;">，以取得在运行时不同的执行效果。</font>
+ <font style="color:#000000;">Java 里对象方法的调用是依靠类信息里的方法表实现的，对象方法引用调用和接口方法引用调用的大致思想是一样的。当调用对象的某个方法时，</font><font style="color:#000000;background-color:#fbde28;">JVM 查找该对象类的方法表以确定该方法的直接引用地址，有了地址后才真正调用该方法。</font>

## <font style="color:#000000;">12. Java 泛型和类型擦除</font>
<font style="color:#000000;">Java泛型（generics） 是JDK 5中引入的一个新特性，允许在定义类和接口的时候使用类型参数（type parameter）。</font><font style="color:#000000;background-color:#fbde28;">声明的类型参数在使用时用具体的类型来替换</font><font style="color:#000000;">。泛型最主要的应用是在JDK 5中的新集合类框架中。</font>

**<font style="color:#000000;">泛型的好处有两个：</font>**

+ **<font style="color:#000000;">方便</font>**<font style="color:#000000;">：可以</font><font style="color:#000000;background-color:#fbde28;">提高代码的复用性</font><font style="color:#000000;">。以List接口为例，我们可以将String、Integer等类型放入List中，如不用泛型，存放String类型要写一个List接口，存放Integer要写另外一个List接口，泛型可以很好的解决这个问题</font>
+ **<font style="color:#000000;">安全</font>**<font style="color:#000000;">：在泛型出之前，</font><font style="color:#000000;background-color:#fbde28;">通过Object实现的类型转换需要在运行时检查</font><font style="color:#000000;">，如果类型转换出错，程序直接GG，可能会带来毁灭性打击。而泛型的作用就是</font><font style="color:#000000;background-color:#fbde28;">在编译时做类型检查，这无疑增加程序的安全性</font>

**<font style="color:#000000;">泛型是如何实现的</font>**

<font style="color:#000000;">Java中的</font><font style="color:#000000;background-color:#fbde28;">泛型通过类型擦除的方式来实现</font><font style="color:#000000;">，通俗点理解，就是通过语法糖的形式，在.java->.class转换的阶段，</font><font style="color:#000000;background-color:#fbde28;">将List</font><font style="color:#000000;background-color:#fbde28;">擦除调转为List的手段</font><font style="color:#000000;">。换句话说，</font><font style="color:#000000;background-color:#fbde28;">Java的泛型只在编译期，Jvm是不会感知到泛型的。</font>

**<font style="color:#000000;">类型擦除</font>**

<font style="color:#000000;">类型擦除是Java在处理泛型的一种方式，如Java的编译器在编译以下代码时</font>

<font style="color:#000000;">在编译后的字节码文件中，会把泛型的信息擦除</font>

<font style="color:#000000;">也就是说，在代码中的Foo</font><font style="color:#000000;"> 和 Foo</font><font style="color:#000000;">使用的类，</font><font style="color:#000000;background-color:#fbde28;">经过编译后都是同一个类</font><font style="color:#000000;">。</font>

<font style="color:#000000;">所以说泛</font><font style="color:#000000;background-color:#fbde28;">型技术实际上是Java语言的一颗语法糖</font><font style="color:#000000;">，因为泛型经过编译器处理之后就被擦除了。</font>

<font style="color:#000000;">这种擦除的过程，被称之为——类型擦除。所以类型擦除指的是通过类型参数合并，将泛型类型实例关</font><font style="color:#000000;background-color:#fbde28;">联到同一份字节码上</font><font style="color:#000000;">。编译器只为泛型类型生成一份字节码，并将其实例关联到这份字节码上。类型擦除的关键在于从泛型类型中</font><font style="color:#000000;background-color:#fbde28;">清除类型参数的相关信息</font><font style="color:#000000;">，并且在必要的时候添加类</font><font style="color:#000000;background-color:#fbde28;">型检查和类型转换</font><font style="color:#000000;">的方法。</font>

<font style="color:#000000;">类型擦除可以简单的理解为将泛型java代码转换为普通java代码，只不过编译器更直接点，将泛型java代码直接转换成普通java字节码。</font>

**<font style="color:#000000;">类型擦除的缺点有哪些？</font>**

+ <font style="color:#000000;">泛型不可以重载</font>
+ <font style="color:#000000;">泛型异常类不可以多次catch</font>
+ <font style="color:#000000;">泛型类中的静态变量也只有一份，不会有多份</font>

## <font style="color:#000000;">13. int 和 Integer 有什么区别？</font>
<font style="color:#000000;">int 和 Integer的区别主要体现在以下几个方面：</font>

+ **<font style="color:#000000;">数据类型</font>**<font style="color:#000000;">不同：</font>
    - **<font style="color:#000000;">int</font>**<font style="color:#000000;"> 是</font><font style="color:#000000;background-color:#fbde28;">基础数据类型</font>
    - **<font style="color:#000000;">Integer</font>**<font style="color:#000000;"> 是</font><font style="color:#000000;background-color:#fbde28;">包装数据类型</font>
+ **<font style="color:#000000;">默认值</font>**<font style="color:#000000;">不同：</font>
    - **<font style="color:#000000;">int</font>**<font style="color:#000000;"> 的默认值是 </font><font style="color:#000000;background-color:#fbde28;">0</font>
    - **<font style="color:#000000;">Integer</font>**<font style="color:#000000;"> 的默认值是 </font><font style="color:#000000;background-color:#fbde28;">null</font>
+ <font style="color:#000000;">内存中</font>**<font style="color:#000000;">存储的方式</font>**<font style="color:#000000;">不同：</font>
    - **<font style="color:#000000;">int</font>**<font style="color:#000000;"> 在内存中直接存储的是</font><font style="color:#000000;background-color:#fbde28;">数据值</font>
    - **<font style="color:#000000;">Integer</font>**<font style="color:#000000;"> 实际存储的是</font><font style="color:#000000;background-color:#fbde28;">对象引用</font><font style="color:#000000;">，当 new 一个 Integer 时实际上是生成一个指针指向此对象</font>
+ **<font style="color:#000000;">实例化方式</font>**<font style="color:#000000;">不同：</font>
    - **<font style="color:#000000;">Integer</font>**<font style="color:#000000;"> </font><font style="color:#000000;background-color:#fbde28;">必须实例化</font><font style="color:#000000;">才可以使用</font>
    - **<font style="color:#000000;">int</font>**<font style="color:#000000;"> </font><font style="color:#000000;background-color:#fbde28;">不需要实例化</font>
+ **<font style="color:#000000;">变量的比较方式</font>**<font style="color:#000000;">不同：</font>
    - **<font style="color:#000000;">int</font>**<font style="color:#000000;"> 可以使用 </font><font style="color:#000000;background-color:#fbde28;">== 来比较</font><font style="color:#000000;">两个变量是否相等</font>
    - **<font style="color:#000000;">Integer</font>**<font style="color:#000000;"> 一定要使用 </font><font style="color:#000000;background-color:#fbde28;">equals 来比较</font><font style="color:#000000;">两个变量是否相等</font>
+ **<font style="color:#000000;">泛型使用</font>**<font style="color:#000000;">不同：</font>
    - **<font style="color:#000000;">Integer</font>**<font style="color:#000000;"> 能</font><font style="color:#000000;background-color:#fbde28;">用于泛型定义</font>
    - **<font style="color:#000000;">int</font>**<font style="color:#000000;"> 类型却不行</font>
+ <font style="color:#000000;">缓存机制:</font>
    - <font style="color:#000000;">为了节省内存和提高性能，Integer 类在内部</font><font style="color:#000000;background-color:#fbde28;">通过使用相同的对象引用实现缓存和重用</font><font style="color:#000000;">，Integer 类默认在-128 ~ 127 之间，可以通过 - XX:AutoBoxCacheMax 进行修改，且这种机制仅在自动装箱的时候有用，在使用构造器创建 Integer 对象时无用</font><font style="color:#000000;">。</font>

## <font style="color:#000000;">14. 说一下反射机制？</font>
<font style="color:#000000;">通过反射，程序可以在</font><font style="color:#000000;background-color:#fbde28;">运行时动态地获取类的信息</font><font style="color:#000000;">（如属性、方法、构造函数等），并能根据这些信息</font><font style="color:#000000;background-color:#fbde28;">创建对象、调用方法或修改属性值</font><font style="color:#000000;">。</font>

<font style="color:#000000;">Java 反射机制主要提供了以下功能，这些功能都位于 java.lang.reflect 包：</font>

+ <font style="color:#000000;">在运行时判断对象所属的类；</font>
+ <font style="color:#000000;">在运行时构造类的对象；</font>
+ <font style="color:#000000;">在运行时判断类所有的成员变量和方法；</font>
+ <font style="color:#000000;">在运行时调用对象的方法；</font>
+ <font style="color:#000000;">生成动态代理。</font>

## <font style="color:#000000;">15. 反射的使用场景</font>
+ <font style="color:#000000;">编程工具 IDEA 或 Eclipse 等，在写代码时会有代码（属性或方法名）提示，就是因为使用了反射</font>
+ <font style="color:#000000;">数据库连接池，也会使用反射调用不同类型的数据库驱动，代码如下所示：</font>

```java
// 使用反射加载数据库驱动类
Class<?> clazz = Class.forName(driverClassName);
            
// 将其实例化为Driver对象
Driver driver = (Driver) clazz.getDeclaredConstructor().newInstance();

// 注册到DriverManager
DriverManager.registerDriver(new DriverShim(driver));
```

+ <font style="color:#000000;">实现AOP（面向切面编程）中的方法拦截</font>

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

interface MyInterface {
    void doSomething();
}

class MyInterfaceImpl implements MyInterface {
    @Override
    public void doSomething() {
        System.out.println("Doing something...");
    }
}

public class AOPExample {
    public static void main(String[] args) {
        MyInterface target = new MyInterfaceImpl();

        InvocationHandler handler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("Before method call...");
                Object result = method.invoke(target, args);
                System.out.println("After method call...");
                return result;
            }
        };

        // 创建动态代理对象
        MyInterface proxyInstance = (MyInterface) Proxy.newProxyInstance(
                MyInterface.class.getClassLoader(),
                new Class[]{MyInterface.class},
                handler);

        // 调用方法，会触发invoke方法
        proxyInstance.doSomething();
    }
}
```

## <font style="color:#000000;">16. 如何使用反射？</font>
<font style="color:#000000;">在 Java 中，反射获取调用类可以通过 </font>**<font style="color:#117cee;">Class.forName()</font>**<font style="color:#000000;">，反射获取类实例要通过 </font>**<font style="color:#117cee;">newInstance()</font>**<font style="color:#000000;">，相当于 new 一个新对象，反射获取方法要通过 </font>**<font style="color:#117cee;">getMethod()</font>**<font style="color:#000000;">，获取到类方法之后使用 </font>**<font style="color:#117cee;">invoke()</font>**<font style="color:#000000;"> 对类方法进行调用。如果是类方法为私有方法的话，则需要通过 </font>**<font style="color:#117cee;">setAccessible(true)</font>**<font style="color:#000000;"> 来修改方法的访问限制，以上的这些操作就是反射的基本使用，具体调用如下。</font>

<font style="color:#000000;">① 反射调用静态方法</font>

```java
// 调用静态方法，不需要实例化对象 
Object result = method.invoke(null, "argument");
```

<font style="color:#000000;">② 反射调用公共方法</font>

```java
// 调用公共方法
Object result = method.invoke(instance, 42);
```

<font style="color:#000000;">③ 反射调用私有方法</font>

```java
// 获取私有方法（需要setAccessible(true)来允许访问）
Method privateMethod = clazz.getDeclaredMethod("privateMethod", String.class);
// 允许访问私有方法
privateMethod.setAccessible(true); 

// 调用私有方法
Object result = privateMethod.invoke(instance, "secret");
```

## <font style="color:#000000;">16. 反射有什么优缺点？</font>
<font style="color:#000000;">反射机制在编程中具有以下优点：</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">动态性与灵活性</font>**<font style="color:#000000;">：通过反射，程序可以在</font><font style="color:#000000;background-color:#fbde28;">运行时动态地获取类的信息</font><font style="color:#000000;">（如属性、方法、构造函数等），并能根据这些信息创建对象、调用方法或修改属性值。这使得代码能够在编译时不知道具体类型的情况下进行操作，</font><font style="color:#000000;background-color:#fbde28;">增强了软件的灵活性和可扩展性</font><font style="color:#000000;">。 </font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">通用框架设计</font>**<font style="color:#000000;">：反射是许多框架（如Spring、Hibernate等）的核心技术，它们利用反射来实现</font>**<font style="color:#117cee;">依赖注入</font>**<font style="color:#000000;">、</font>**<font style="color:#117cee;">AOP</font>**<font style="color:#000000;">、</font>**<font style="color:#117cee;">对象生命周期管理</font>**<font style="color:#000000;">等功能，大大简化了开发工作，</font><font style="color:#000000;background-color:#fbde28;">提高了代码的复用性和模块化程度</font><font style="color:#000000;">。 </font>
3. <font style="color:#000000;"> </font>**<font style="color:#000000;">序列化与反序列化</font>**<font style="color:#000000;">：在对象序列化过程中，反射可以用于读取对象的所有属性，</font><font style="color:#000000;background-color:#fbde28;">将其转换为字节流或其他格式的数据</font><font style="color:#000000;">；反之，在反序列化时，也能根据类信息动态生成对象，并</font><font style="color:#000000;background-color:#fbde28;">将数据填充到相应的属性中</font><font style="color:#000000;">。 </font>

<font style="color:#000000;">然而，反射也有其显著的缺点：</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">性能开销</font>**<font style="color:#000000;">：反射操作通常比直接的静态方法调用慢，因为它涉及到了</font><font style="color:#000000;background-color:#fbde28;">额外的类型检查、查找和安全控制</font><font style="color:#000000;">。频繁使用反射可能会导致性能下降。 </font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">安全性问题</font>**<font style="color:#000000;">：反射</font><font style="color:#000000;background-color:#fbde28;">允许访问私有成员和执行非公开API，</font><font style="color:#000000;">这可能导致封装原则被破坏，增加系统的不稳定性。</font><font style="color:#000000;background-color:#fbde28;">不受控的反射使用可能会暴露系统安全漏洞</font><font style="color:#000000;">，特别是在处理不可信任的数据或代码时。 </font>
3. <font style="color:#000000;"> </font>**<font style="color:#000000;">代码复杂度提高</font>**<font style="color:#000000;">：反射代码往往</font><font style="color:#000000;background-color:#fbde28;">较难理解和维护</font><font style="color:#000000;">，因为它</font><font style="color:#000000;background-color:#fbde28;">绕过了编译器的类型检查</font><font style="color:#000000;">，增加了潜在的</font><font style="color:#000000;background-color:#fbde28;">运行时错误风险</font><font style="color:#000000;">。同时，过度依赖反射可能会使原本清晰的逻辑变得模糊不清。 </font><font style="color:#000000;background-color:#fbde28;">可读性和可维护性变差。</font>

## <font style="color:#000000;">17. &和&&的区别</font>
在Java中，`&` 和 `&&` 都代表逻辑与运算符，但它们有以下主要区别：

1. **短路行为（Short-Circuit Evaluation）**： 
    - `&&` 是一个条件逻辑与运算符，它具有短路特性。如果第一个操作数（表达式）为 `false`，那么它不会计算第二个操作数，直接返回 `false`。这是因为<font style="background-color:#fbde28;">当两个表达式都必须为 </font>`<font style="background-color:#FBDE28;">true</font>`<font style="background-color:#fbde28;"> 时结果才为 </font>`<font style="background-color:#FBDE28;">true</font>`，既然第一个已经为 `false`，无论第二个如何，整体结果都将是 `false`，所以没有必要继续计算。
    - 而 `&` 运算符则不管第一个操作数的值是什么，都会计算两个操作数。
2. **使用场景不同**： 
    - `&&` 主要用于布尔表达式的逻辑判断，并利用其短路特性优化代码执行效率和避免不必要的计算。
    - `&` 在布尔上下文中可以用来代替 `&&`，但不具备短路特性；此外，`&` 还可以用作位运算符，<font style="background-color:#fbde28;">当作用于整型或字符型等数值类型时，它执行的是按位与操作</font>，即对两个操作数的二进制位进行逐位“与”运算。

举例说明：

```java
boolean a = true;
boolean b = someExpensiveComputation(); // 假设这是一个昂贵的操作

// 使用 &&
if (a && b) {
    // 如果 a 为 false，这里不会执行 b 的计算
}

// 使用 &
if (a & b) {
    // 即使 a 为 false，这里依然会执行 b 的计算
}

// 位运算示例
int x = 0b1010; // 二进制表示为1010（十进制10）
int y = 0b1100; // 二进制表示为1100（十进制12）

int result = x & y; // 按位与操作后，result将为0b1000（十进制8）
```

总结来说，在处理布尔逻辑时，通常更倾向于使用 `&&` 来确保程序性能，并防止可能的副作用。而当需要进行位操作时，则使用 `&`。

## <font style="color:rgb(63, 63, 63);">18. Java 中 IO 流分为几种?</font>
Java 中的 IO 流按照不同的分类标准可以分为多种类型。以下是主要的分类方式：

1. **按流向划分**： 
    - **<font style="color:#117cee;">输入流</font>**（Input Stream）：<font style="background-color:#fbde28;">从外部数据源读取数据到程序中</font>的流，例如 `FileInputStream`、`BufferedReader`。
    - **<font style="color:#117cee;">输出流</font>**（Output Stream）：<font style="background-color:#fbde28;">将数据从程序写入到外部目标</font>的流，例如 `FileOutputStream`、`BufferedWriter`。
2. **按处理单元划分**： 
    - **<font style="color:#117cee;">字节流</font>**（Byte Stream）：<font style="background-color:#fbde28;">以字节为单位进行读写操作</font>，主要用于处理二进制数据或非文本数据，如 `InputStream` 和 `OutputStream` 及其子类。
    - **<font style="color:#117cee;">字符流</font>**（Character Stream）：<font style="background-color:#fbde28;">以字符为单位进行读写操作</font>，主要用于处理文本数据，如 `Reader` 和 `Writer` 及其子类。
3. **按功能角色划分**： 
    - **<font style="color:#117cee;">节点流</font>**（Node Stream）：直接与数据源或者目标设备交互的流，比如直接操作文件的 `FileInputStream` 和 `FileWriter`。
    - **<font style="color:#117cee;">处理流</font>**（Wrapper Stream 或 Filter Stream）：对节点流进行包装和增强功能的流，提供缓冲、转换、过滤等功能，例如 `BufferedReader`、`BufferedWriter`、`DataInputStream`、`DataOutputStream`、`InputStreamReader`、`OutputStreamWriter` 等。
4. **特殊类型的流**： 
    - **<font style="color:#117cee;">对象流</font>**（Object Stream）：用于序列化和反序列化对象，即在IO流中保存和恢复对象的状态，如 `ObjectInputStream` 和 `ObjectOutputStream`。

## <font style="color:#000000;">19. 讲讲类的实例化顺序</font>
在Java中，类的实例化顺序遵循以下规则：

1. **父类静态成员初始化**： 
    - 在任何对象实例化之前，首先会执行所有<font style="background-color:#fbde28;">静态变量</font>和<font style="background-color:#fbde28;">静态块</font>（static block）的初始化。这些操作是<font style="background-color:#fbde28;">按类加载顺序进行的，与实例化无关</font>。如果父类中有静态数据成员或静态初始化块，它们会在子类之前被初始化。
2. **子类静态成员初始化**： 
    - 父类静态成员初始化完毕后，接着初始化<font style="background-color:#fbde28;">子类中的静态变量和静态块</font>。
3. **父类实例化过程**： 
    - 当创建子类实例时，首先调用父类的构造函数（<font style="background-color:#fbde28;">默认构造函数或通过</font>`<font style="background-color:#FBDE28;">super()</font>`<font style="background-color:#fbde28;">显式调用的构造函数</font>）。按照从父类到子类的顺序，先执行父类构造函数的主体代码。
4. **子类实例化过程**： 
    - 父类构造函数执行完毕后，开始执行子类自己的构造函数。子<font style="background-color:#fbde28;">类构造函数的第一行通常是一个隐式的</font>`<font style="background-color:#FBDE28;">super()</font>`<font style="background-color:#fbde28;">调用（除非显式指定了其他父类构造函数）</font>，用于完成父类实例的初始化。
    - 接下来执行子类构造函数的剩余部分，对子类特有的属性进行初始化。

总结起来，完整的实例化顺序如下：

1. 父类静态变量和静态初始化块初始化
2. 子类静态变量和静态初始化块初始化
3. 调用父类构造函数
4. 调用子类构造函数

注意：静态成员与类相关联，而不是与对象关联，所以无论创建多少个对象，静态成员都只会初始化一次。而构造函数则是每次创建对象实例时都会调用的。

## 20. <font style="color:#000000;">Java 创建对象有几种方式</font>
Java中创建对象的主要方式有以下几种：

1. **使用 **`**new**`** 关键字**： 
    - 这是最常见的创建对象的方式，通过<font style="background-color:#fbde28;">调用类的构造函数来初始化对象</font>。

```java
ClassName obj = new ClassName([arguments]);
```

其中 `ClassName` 是要创建的对象类名，`[arguments]` 是传递给构造函数的参数（如果有）。 

2. **使用反射 API (**`**Class.newInstance()**`** 方法)**： 
    - 如果类有一个默认（无参）构造器，可以<font style="background-color:#fbde28;">通过 </font>`<font style="background-color:#FBDE28;">Class</font>`<font style="background-color:#fbde28;"> 类的 </font>`<font style="background-color:#FBDE28;">newInstance()</font>`<font style="background-color:#fbde28;"> 方法创建对象</font>。

```java
Class<?> clazz = Class.forName("com.example.ClassName");
Object obj = clazz.newInstance();
```

3. **使用反射 API (Constructor 类的 newInstance() 方法)**： 
    - 可以更灵活地通过反射获取指定构造函数并调用其 `newInstance()` 方法来创建对象，这样可以使用带参数的构造函数。

```java
Constructor<ClassName> constructor = ClassName.class.getConstructor(ParameterTypes...);
ClassName obj = constructor.newInstance(Arguments...);
```

4. **克隆对象 (clone() 方法)**： 
    - 如果<font style="background-color:#fbde28;">一个类实现了 </font>`<font style="background-color:#FBDE28;">Cloneable</font>`<font style="background-color:#fbde28;"> 接口，并且重写了 </font>`<font style="background-color:#FBDE28;">Object</font>`<font style="background-color:#fbde28;"> 类的 </font>`<font style="background-color:#FBDE28;">clone()</font>`<font style="background-color:#fbde28;"> 方法</font>，那么可以通过调用该方法复制现有对象来创建新对象。

```java
ClassName original = ...;
ClassName clonedObj = (ClassName) original.clone();
```

5. **反序列化**： 
    - 当从序列化的数据流中恢复对象时，可以<font style="background-color:#fbde28;">利用 </font>`<font style="background-color:#FBDE28;">ObjectInputStream</font>`<font style="background-color:#fbde28;"> 的 </font>`<font style="background-color:#FBDE28;">readObject()</font>` 方法来创建对象。

```java
ObjectInputStream in = new ObjectInputStream(new FileInputStream("file.ser"));
ClassName obj = (ClassName) in.readObject();
```

6. **匿名内部类**： 
    - 虽然不是直接创建某个类的对象，但在声明和实例化时一起创建一个新的、未命名的子类对象。

```java
Runnable task = new Runnable() {
    @Override
    public void run() {
        // 任务实现代码
    }
};
```

7. **Lambda 表达式（Java 8 及以上版本）**： 
    - 对于实现了函数式接口的类，可以通过lambda表达式来创建对象。

```java
Supplier<ClassName> supplier = () -> new ClassName();
ClassName obj = supplier.get();
```

总结起来，在Java中创建对象最常见的是通过 `new` 关键字，而其他方式如反射、克隆、反序列化等在特定场景下也会被使用。在Java语言中，异常处理机制是通过使用一系列关键字和语句结构来实现的，主要包括 `try`、`catch`、`finally`、`throw` 和 `throws`。下面详细说明它们的作用以及如何使用：

## 21. <font style="color:rgb(63, 63, 63);">Java 语言是如何处理异常的，关键字 throws、throw、try、catch、finally 怎么使用?</font>
1. **try**： 
    - 用法：将可能出现异常的代码块放在 `try` 关键字后的<font style="background-color:#fbde28;">大括号 </font>`<font style="background-color:#FBDE28;">{}</font>`<font style="background-color:#fbde28;"> 中</font>。

```java
try {
    // 可能抛出异常的代码
}
```

当在 `try` 块中的代码执行时，如果发生了异常，则控制流会立即<font style="background-color:#fbde28;">跳转到相应的 </font>`<font style="background-color:#FBDE28;">catch</font>`<font style="background-color:#fbde28;"> 块</font>。 

2. **catch**： 
    - 用法：紧跟在 `try` 后面，用于捕获并处理在 `try` 块中抛出的特定类型的异常。

```java
catch (ExceptionType1 e) {
    // 处理 ExceptionType1 异常的代码
}
catch (ExceptionType2 e) {
    // 处理 ExceptionType2 异常的代码
}
```

捕获异常时，<font style="background-color:#fbde28;">可以针对不同类型的异常分别编写多个 </font>`<font style="background-color:#FBDE28;">catch</font>`<font style="background-color:#fbde28;"> 子句</font>，并按照从上至下的顺序检查异常类型是否匹配，遇到第一个匹配的异常类型就执行该 `catch` 块中的代码。 

3. **finally**： 
    - 用法：无论 `try` 块中是否发生异常，`finally` 子句都会被执行。<font style="background-color:#fbde28;">通常用来进行资源清理（如关闭文件、数据库连接等）。</font>

```java
try {
    // ...
} 
catch (Exception e) {
    // ...
} 
finally {
    // 清理资源或确保某些操作总是被执行的代码
}
```

<font style="background-color:#fbde28;">即使在 </font>`<font style="background-color:#FBDE28;">catch</font>`<font style="background-color:#fbde28;"> 子句中调用了 </font>`<font style="background-color:#FBDE28;">return</font>`<font style="background-color:#fbde28;"> </font>或者在 `try` 或 `catch` 中发生了未捕获的异常，`<font style="background-color:#FBDE28;">finally</font>`<font style="background-color:#fbde28;"> 子句也会执行。 </font>

4. **throw**： 
    - 用法：<font style="background-color:#fbde28;">在方法内部手动抛出一个异常对象</font>。当程序检测到某个条件不满足或者需要报告错误时，可以创建一个新的异常实例并通过 `throw` 关键字将其抛出。

```java
if (someConditionIsFalse) {
    throw new IllegalArgumentException("Invalid argument");
}
```

抛出异常后，当前方法的执行会被立即中断，控制权转移到能够捕获这个异常的最近的 `catch` 块。 

5. **throws**： 
    - 用法：<font style="background-color:#fbde28;">出现在方法声明的后面，用于指出该方法可能会抛出的异常类型列表</font>。当一个方法无法处理异常，需要将异常传递给它的调用者时，就需要在方法签名中声明可能抛出的异常。

```java
public void someMethod() throws IOException, SQLException {
    // 方法体内的代码可能会导致 IOException 或 SQLException
}
```

调用这样的方法时，要么在其内部使用 `<font style="background-color:#FBDE28;">try-catch</font>`<font style="background-color:#fbde28;"> 来处理这些异常</font>，要么在调用者的方法声明中也加上 `<font style="background-color:#FBDE28;">throws</font>`<font style="background-color:#fbde28;"> 关键字继续向上层抛出</font>。 

总结起来，在Java中，通过结合使用这些关键字和语句结构，可以有效地对运行时出现的异常进行捕获、处理和传递。在编写健壮的代码时，合理的异常处理策略至关重要。

## 22. <font style="color:#000000;">谈谈 Java 的异常层次结构</font>
<font style="color:#000000;">Java 的异常层次结构以 </font>`<font style="color:#000000;">java.lang.Throwable</font>`<font style="color:#000000;"> 类为根节点，它有</font><font style="color:#000000;background-color:#fbde28;">两个直接子类：</font>`<font style="color:#000000;background-color:#FBDE28;">Error</font>`<font style="color:#000000;background-color:#fbde28;"> 和 </font>`<font style="color:#000000;background-color:#FBDE28;">Exception</font>`<font style="color:#000000;">。这两个子类构成了 Java 异常处理体系的基础。</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">Throwable</font>**<font style="color:#000000;"> </font>
    - `<font style="color:#000000;">java.lang.Throwable</font>`<font style="color:#000000;"> 是所有错误和异常的基类，它包含了错误信息的基本描述，如堆栈跟踪（stack trace）等。</font><font style="color:#000000;background-color:#fbde28;">所有的异常和错误都继承自 Throwable 类，使得它们都可以被抛出（throw）和捕获（catch）。</font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">Error</font>**<font style="color:#000000;"> </font>
    - `<font style="color:#000000;">java.lang.Error</font>`<font style="color:#000000;"> 表示严重的系统级错误或者资源耗尽错误，</font><font style="color:#000000;background-color:#fbde28;">这些错误通常表明 JVM 无法在不中断应用程序的情况下继续执行</font><font style="color:#000000;">。例如： </font>
        * `<font style="color:#000000;">java.lang.VirtualMachineError</font>`<font style="color:#000000;">：虚拟机错误，包括 OutOfMemoryError（</font><font style="color:#000000;background-color:#fbde28;">内存溢出</font><font style="color:#000000;">）、StackOverflowError（</font><font style="color:#000000;background-color:#fbde28;">栈溢出</font><font style="color:#000000;">）等。</font>
        * `<font style="color:#000000;">java.lang.LinkageError</font>`<font style="color:#000000;">：链接错误，如类加载时遇到的问题。</font>
        * `<font style="color:#000000;">java.lang.InternalError</font>`<font style="color:#000000;"> 和 </font>`<font style="color:#000000;">java.lang.OutOfMemoryError</font>`<font style="color:#000000;"> 等其他类型的错误。</font>

<font style="color:#000000;">开发者一般不会去尝试捕获并处理 Error 类型的异常，因为这些错误往往表示程序已经发生了严重问题，而且</font><font style="color:#000000;background-color:#fbde28;">大部分情况下是无法通过编程方式恢复的</font><font style="color:#000000;">。 </font>

3. <font style="color:#000000;"> </font>**<font style="color:#000000;">Exception</font>**<font style="color:#000000;"> </font>
    - `<font style="color:#000000;">java.lang.Exception</font>`<font style="color:#000000;"> 是更广泛的异常类型，</font><font style="color:#000000;background-color:#fbde28;">用于描述程序运行时可以预期并可能需要处理的条件或状态</font><font style="color:#000000;">。根据是否需要在方法签名中声明，Exception 又分为两种主要类型： </font>
        * **<font style="color:#000000;">Checked Exception</font>**<font style="color:#000000;">：编译器要求必须显式处理（捕获或声明抛出）的异常，比如 </font>`<font style="color:#000000;">java.io.IOException</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">java.sql.SQLException</font>`<font style="color:#000000;"> 等。如果一个方法可能会抛出检查异常，那么要么在方法内部捕获并处理这个异常，要么在方法签名中使用 </font>`<font style="color:#000000;">throws</font>`<font style="color:#000000;"> 关键字声明会抛出此异常。</font>
        * **<font style="color:#000000;">Unchecked Exception</font>**<font style="color:#000000;">（也称为运行时异常）：不需要在方法签名中声明，但一旦发生，如果不捕获，则会在调用栈上层查找合适的处理器来处理。典型的运行时异常包括继承自 </font>`<font style="color:#000000;">java.lang.RuntimeException</font>`<font style="color:#000000;"> 的那些异常，如 </font>`<font style="color:#000000;">NullPointerException</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">IllegalArgumentException</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">ArrayIndexOutOfBoundsException</font>`<font style="color:#000000;"> 等。</font>

<font style="color:#000000;">在实际开发中，良好的异常处理策略应该关注如何有效地捕获、记录和传递异常信息，以及如何使代码具备更好的健壮性和可读性。对于检查型异常，开发者应考虑在何种上下文中捕获并处理最为合适；而对于运行时异常，应当尽量避免在代码逻辑中产生，并且在设计API时明确哪些操作可能导致这类异常。</font>

## 23. <font style="color:#000000;">静态内部类与非静态内部类有什么区别</font>
1. <font style="color:#000000;"> </font>**<font style="color:#117cee;">静态声明</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">静态内部类（Static Nested Class）使用了 </font>`<font style="color:#000000;">static</font>`<font style="color:#000000;"> 关键字修饰，而普通非静态内部类没有。</font>
2. <font style="color:#000000;"> </font>**<font style="color:#117cee;">依赖外部类实例</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">非静态内部类（也称为嵌套类或成员内部类）的实例与外部类的实例之间存在关联关系。创建非静态内部类对象时必须先有一个外部类对象，即</font><font style="color:#000000;background-color:#fbde28;">非静态内部类的实例是绑定到外部类的一个实例上的。</font>

```java
OuterClass outer = new OuterClass();
InnerClass inner = outer.new InnerClass();
```

```plain
- <font style="color:#000000;">而静态内部类则不需要依赖于外部类的对象即可创建其实例。</font>
```

```java
StaticNestedClass nested = new StaticNestedClass();
```

3. <font style="color:#000000;"> </font>**<font style="color:#117cee;">访问权限</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;"> </font><font style="color:#000000;background-color:#fbde28;">非静态内部类可以直接访问外部类的所有成员</font><font style="color:#000000;">（包括私有成员），因为它持有对外部类实例的隐式引用。 </font>
    - <font style="color:#000000;"> </font><font style="color:#000000;background-color:#fbde28;">静态内部类只能访问外部类的静态成员</font><font style="color:#000000;">，不能直接访问外部类的实例成员，除非通过一个外部类实例作为参数传递给它。 </font>
4. <font style="color:#000000;"> </font>**<font style="color:#117cee;">静态成员</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;background-color:#fbde28;">静态内部类可以拥有静态变量和静态方法</font><font style="color:#000000;">，而非静态内部类则不可以有静态成员。</font>
5. <font style="color:#000000;"> </font>**<font style="color:#117cee;">加载时机</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;background-color:#fbde28;">静态内部类会在外部类被加载时一起加载</font><font style="color:#000000;">，即使不创建外部类对象也能访问静态内部类及其静态成员。</font>
    - <font style="color:#000000;">非静态内部类的加载并不一定伴随着外部类的加载，</font><font style="color:#000000;background-color:#fbde28;">只有当创建非静态内部类的实例或者调用相关方法时才会加载非静态内部类。</font>
6. <font style="color:#000000;"> </font>**<font style="color:#117cee;">作用域</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">静态内部类主要用于组织代码、封装一些与外部类相关的独立逻辑，并且它的</font><font style="color:#000000;background-color:#fbde28;">可见性和生命周期并不依赖于外部类的实例。</font>
    - <font style="color:#000000;">非静态内部类更常用于实现某种形式的封装或关联，比如适配器模式中的包装类，或者是实现某个接口的具体策略等，它可以</font><font style="color:#000000;background-color:#fbde28;">更紧密地与外部类的行为结合在一起</font><font style="color:#000000;">。</font>

## <font style="color:#000000;">24. String s 与 new String 与有什么区别</font>
<font style="color:#000000;">在Java中，</font>`<font style="color:#000000;">String s</font>`<font style="color:#000000;"> 和 </font>`<font style="color:#000000;">new String()</font>`<font style="color:#000000;"> 两种方式都可以用来创建字符串对象，但它们之间存在一些关键区别：</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">使用字面量或常量池初始化</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">当你使用如下形式声明并初始化字符串时：</font>

```java
String s = "Hello";
```

<font style="color:#000000;"> 这里JVM首先会</font><font style="color:#000000;background-color:#fbde28;">检查字符串常量池</font><font style="color:#000000;">（String Pool）是否存在</font><font style="color:#000000;background-color:#fbde28;">已经存在的相同内容的字符串</font><font style="color:#000000;">。如果不存在，则会在字符串常量池中创建一个新字符串对象，</font><font style="color:#000000;background-color:#fbde28;">并将引用返回给变量</font>`<font style="color:#000000;background-color:#FBDE28;">s</font>`<font style="color:#000000;">。如果已经存在相同的字符串，则直接返回该字符串的引用。</font>

2. <font style="color:#000000;"> </font>**<font style="color:#000000;">通过构造函数创建新的对象实例</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">当你使用 </font>`<font style="color:#000000;">new</font>`<font style="color:#000000;"> 关键字来创建一个新的 </font>`<font style="color:#000000;">String</font>`<font style="color:#000000;"> 对象时：</font>

```java
String s = new String("Hello");
```

<font style="color:#000000;"> 这时候，无论字符串常量池中是否已有相同内容的字符串，</font><font style="color:#000000;background-color:#fbde28;">都会在堆内存中新创建一个 </font>`<font style="color:#000000;background-color:#FBDE28;">String</font>`<font style="color:#000000;background-color:#fbde28;"> 对象。</font><font style="color:#000000;">同时，如果字符串字面量 </font>`<font style="color:#000000;">"Hello"</font>`<font style="color:#000000;"> 在此之前尚未存在于常量池中，那么它会被添加到常量池中。因此，</font><font style="color:#000000;background-color:#fbde28;">实际上这里创建了两个对象：一个是常量池中的字符串对象，另一个是堆内存中的新对象</font><font style="color:#000000;">。</font><font style="color:#000000;background-color:#fbde28;">变量</font>`<font style="color:#000000;background-color:#FBDE28;">s</font>`<font style="color:#000000;background-color:#fbde28;">指向的是堆内存中的新对象。</font>

<font style="color:#000000;">总结来说：</font>

+ <font style="color:#000000;">直接赋值字面量的方式更倾向于</font><font style="color:#000000;background-color:#fbde28;">重用已有的字符串对象，从而节约内存。</font>
+ <font style="color:#000000;">使用 </font>`<font style="color:#000000;">new String()</font>`<font style="color:#000000;"> 的方式总是会在</font><font style="color:#000000;background-color:#fbde28;">堆内存中创建一个新的对象</font><font style="color:#000000;">，即使常量池中已经有相同内容的字符串。这在某些情况下可能</font><font style="color:#000000;background-color:#fbde28;">导致不必要的内存消耗</font><font style="color:#000000;">，尤其是在处理大量重复字符串时。</font>

<font style="color:#000000;">另外，当需要对字符串进行修改操作或者需要确保有独立的内存空间存储特定字符串时，可能会使用 </font>`<font style="color:#000000;">new String()</font>`<font style="color:#000000;"> 来显式创建新的字符串对象。但是，由于 </font>`<font style="color:#000000;">String</font>`<font style="color:#000000;"> 类型在Java中是不可变的，所以对字符串的任何修改操作（如替换字符、拼接等）实际上都会生成新的 </font>`<font style="color:#000000;">String</font>`<font style="color:#000000;"> 对象，而不会改变原对象的内容。</font>

## <font style="color:#000000;">25. 反射中，Class.forName 和 ClassLoader 的区别</font>
<font style="color:#000000;">在Java反射中，</font>`<font style="color:#000000;">Class.forName()</font>`<font style="color:#000000;"> 和 </font>`<font style="color:#000000;">ClassLoader</font>`<font style="color:#000000;"> 类加载类的方式有所不同，具体区别如下：</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">Class.forName() 方法</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">这是一个静态方法，位于 </font>`<font style="color:#000000;">java.lang.Class</font>`<font style="color:#000000;"> 类中。</font>
    - <font style="color:#000000;">通过调用 </font>`<font style="color:#000000;">Class.forName(className)</font>`<font style="color:#000000;"> 可以</font><font style="color:#000000;background-color:#fbde28;">根据全限定类名动态加载并初始化类</font><font style="color:#000000;">。如果类定义了静态初始化块（static initialization block），那么这些块会被执行。</font>
    - <font style="color:#000000;">当不指定类加载器时，它会使用当前线程的上下文类加载器（context class loader）来加载类；若指定了特定的类加载器，则使用该加载器加载类。</font>
    - <font style="color:#000000;">如果找不到指定名称的类或者类加载过程中出现异常，如 </font>`<font style="color:#000000;">ClassNotFoundException</font>`<font style="color:#000000;">，将会抛出异常。</font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">ClassLoader 类</font>**<font style="color:#000000;">： </font>
    - `<font style="color:#000000;background-color:#FBDE28;">ClassLoader</font>`<font style="color:#000000;background-color:#fbde28;"> 是一个抽象类，所有类加载器都继承自它</font><font style="color:#000000;">，负责将类文件中的二进制数据转换为 JVM 可识别和操作的运行时类结构。</font>
    - <font style="color:#000000;">开发者可以通过实现 </font>`<font style="color:#000000;">ClassLoader</font>`<font style="color:#000000;"> 的子类来自定义类加载策略，比如从网络、数据库或其他非标准来源加载类。</font>
    - <font style="color:#000000;">使用 </font>`<font style="color:#000000;">ClassLoader</font>`<font style="color:#000000;"> 加载类通常需要调用其 </font>`<font style="color:#000000;">loadClass()</font>`<font style="color:#000000;"> 方法，例如：</font>`<font style="color:#000000;">ClassLoader.getSystemClassLoader().loadClass(className)</font>`<font style="color:#000000;"> 或自定义加载器的 </font>`<font style="color:#000000;">loadClass()</font>`<font style="color:#000000;"> 方法。</font>
    - <font style="color:#000000;">相对于 </font>`<font style="color:#000000;">Class.forName()</font>`<font style="color:#000000;">，直接使用 </font>`<font style="color:#000000;">ClassLoader</font>`<font style="color:#000000;"> 提供了更多的灵活性，可以控制类加载的具体过程，并且可以选择不执行类的初始化。</font>

<font style="color:#000000;">总结起来，</font>`<font style="color:#000000;">Class.forName()</font>`<font style="color:#000000;"> 更像是一个方便的方法，</font><font style="color:#000000;background-color:#fbde28;">用于快速加载和初始化类</font><font style="color:#000000;">，而 </font>`<font style="color:#000000;">ClassLoader</font>`<font style="color:#000000;"> 则提供了</font><font style="color:#000000;background-color:#fbde28;">更底层的机制来管理类的加载过程</font><font style="color:#000000;">，尤其是在复杂的类加载场景下更为常用。此外，</font>`<font style="color:#000000;">Class.forName()</font>`<font style="color:#000000;"> </font><font style="color:#000000;background-color:#fbde28;">默认会初始化类</font><font style="color:#000000;">，而直接使用 </font>`<font style="color:#000000;">ClassLoader.loadClass()</font>`<font style="color:#000000;"> 方法加载类</font><font style="color:#000000;background-color:#fbde28;">不会自动触发类的初始化。</font>

## <font style="color:#000000;">26. JDK 动态代理与 cglib 实现的区别</font>
<font style="color:#000000;">JDK动态代理与CGLIB库实现的动态代理在Java中都是为了创建一个代理对象，以实现代理模式，但在实现原理和适用场景上有显著区别：</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">实现原理</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;"> </font>**<font style="color:#000000;">JDK动态代理</font>**<font style="color:#000000;">：</font><font style="color:#000000;background-color:#fbde28;">基于Java反射API实现。它要求被代理类必须实现至少一个接口</font><font style="color:#000000;">。代理类由</font>`<font style="color:#000000;">java.lang.reflect.Proxy</font>`<font style="color:#000000;">类通过字节码技术在运行时动态生成，并且这个代理类会实现目标类所实现的所有接口。代理类的方法会在调用时转发到InvocationHandler接口实现类中定义的invoke方法，该方法可以在此处理对原方法的增强逻辑。 </font>
    - <font style="color:#000000;"> </font>**<font style="color:#000000;">CGLIB代理</font>**<font style="color:#000000;">：</font><font style="color:#000000;background-color:#fbde28;">基于继承机制实现。它不需要被代理类实现任何接口，而是通过生成被代理类的子类来完成代理功能</font><font style="color:#000000;">。CGLIB使用ASM字节码操作框架，在运行期对字节码进行修改或动态生成新的类。因此，即使目标类没有接口，也可以为它创建代理。 </font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">适用场景</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;"> </font>**<font style="color:#000000;">JDK动态代理</font>**<font style="color:#000000;">：适用于已有接口的设计良好的面向接口编程的场景，特别是当希望提供统一的代理逻辑（如AOP中的切面逻辑）时。 </font>
    - <font style="color:#000000;"> </font>**<font style="color:#000000;">CGLIB代理</font>**<font style="color:#000000;">：适用于无法或者不愿意为被代理类添加接口的情况，例如对于遗留代码的代理、或是需要对类行为进行更深度定制的情况。由于是通过继承方式扩展，所以</font><font style="color:#000000;background-color:#fbde28;">CGLIB代理无法代理final类以及final方法。 </font>
3. <font style="color:#000000;"> </font>**<font style="color:#000000;">性能差异</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">通常情况下，JDK动态代理由于其仅限于接口代理，结构相对简单，因此在</font><font style="color:#000000;background-color:#fbde28;">性能上可能会优于CGLIB代理。</font>
    - <font style="color:#000000;">CGLIB代理生成的是子类，</font><font style="color:#000000;background-color:#fbde28;">需要创建额外的类文件</font><font style="color:#000000;">，如果频繁创建和销毁代理对象，可能会影响性能。然而，由于</font><font style="color:#000000;background-color:#fbde28;">现代JVM的优化，实际差距可能并不明显</font><font style="color:#000000;">，尤其是在持续存在的长期运行的应用中。</font>
4. <font style="color:#000000;"> </font>**<font style="color:#000000;">Spring框架集成</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">Spring AOP支持这两种代理方式，</font><font style="color:#000000;background-color:#fbde28;">并根据目标类是否有接口自动选择使用哪种方式</font><font style="color:#000000;">。默认情况下，如果目标类有接口，则使用JDK动态代理；若无接口，则采用CGLIB代理。</font>

<font style="color:#000000;">综上所述，开发者应根据具体需求和项目架构选择合适的代理实现方式。</font>

## <font style="color:#000000;">27. 深拷贝和浅拷贝区别</font>
深拷贝（Deep Copy）和浅拷贝（Shallow Copy）是面向对象编程中<font style="background-color:#fbde28;">复制对象时的两种不同策略</font>，它们的区别在于对对象内容复制的深度：

1. <font style="color:#000000;"> </font>**浅拷贝**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">当进行浅拷贝时，</font><font style="color:#000000;background-color:#fbde28;">新创建的对象与原对象共享相同的引用</font><font style="color:#000000;">。也就是说，如果一个对象包含其他对象的引用，那么在拷贝过程中，这些引用也会被复制到新对象中，而不是复制这些引用所指向的对象本身。</font>
    - <font style="color:#000000;">因此，对于基本数据类型字段（如int、char等），浅拷贝会创建新的副本；而对于引用类型的字段（如数组、类实例等），浅拷贝只是将这些引用复制了一份，所以</font><font style="color:#000000;background-color:#fbde28;">新旧对象的引用类型字段指向的是同一块内存区域。</font>
    - <font style="color:#000000;">如果</font><font style="color:#000000;background-color:#fbde28;">修改了原始对象或其引用类型字段的内容</font><font style="color:#000000;">，那么这个变化</font><font style="color:#000000;background-color:#fbde28;">会影响到通过浅拷贝得到的新对象</font><font style="color:#000000;">。</font>
2. <font style="color:#000000;"> </font>**深拷贝**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">在进行深拷贝时，</font><font style="color:#000000;background-color:#fbde28;">不仅复制对象本身的值，还递归地复制对象所引用的所有对象</font><font style="color:#000000;">，直到所有的可到达对象都被复制为止。</font>
    - <font style="color:#000000;">深拷贝会为所有嵌套的对象</font><font style="color:#000000;background-color:#fbde28;">生成全新的独立实体</font><font style="color:#000000;">，并且完全复制所有的属性，包括引用类型的属性。这意味着即使引用类型的字段，在深拷贝后也会</font><font style="color:#000000;background-color:#fbde28;">有独立的内存空间。</font>
    - <font style="color:#000000;">修改原始对象或其引用类型字段的内容不会影响到通过深拷贝得到的新对象。</font>

<font style="color:#000000;">举个例子来说明：</font>

```java
class Person {
    String name;
    Address address; // 假设Address是一个引用类型

    // 构造函数和其他方法...
}

Person original = new Person("Alice", new Address("123 Street"));

// 浅拷贝
Person shallowCopy = new Person(original.name, original.address);
// 修改original的address属性
original.address.setStreet("456 Avenue");

// 此时shallowCopy.address也被改变了，因为它们指向同一个Address对象

// 深拷贝
Person deepCopy = original.deepCopy(); // 假设有一个deepCopy方法实现深拷贝
// 修改original的address属性
original.address.setStreet("789 Boulevard");

// 此时deepCopy.address保持不变，因为它指向的是一个新的Address对象
```



<font style="color:#000000;">总结来说，浅拷贝仅仅复制了对象的一层</font><font style="color:#000000;background-color:#fbde28;">引用关系</font><font style="color:#000000;">，而深拷贝则确保了复制出的对象以及它内部包含的所有对象都是独立于原对象的新实例。</font>

## <font style="color:#000000;">28. JDK 和 JRE 有什么区别?</font>
+ <font style="color:#000000;">JDK:Java Development Kit 的简称，</font><font style="color:#000000;background-color:#fbde28;">Java 开发工具包</font><font style="color:#000000;">，提供了 Java 的开发环 境和运行环境。</font>
+ <font style="color:#000000;">JRE:Java Runtime Environment 的简称，</font><font style="color:#000000;background-color:#fbde28;">Java 运行环境</font><font style="color:#000000;">，为 Java 的运行提 供了所需环境。</font>

## <font style="color:#000000;">29. String 类的常用方法都有那些呢?</font>
<font style="color:#000000;"> indexOf():返回指定字符的索引。</font>

<font style="color:#000000;"> </font><font style="color:#000000;">charAt()</font><font style="color:#000000;">:返回指定索引处的字符。</font>

<font style="color:#000000;"> </font><font style="color:#000000;">replace()</font><font style="color:#000000;">:字符串替换。</font>

<font style="color:#000000;"> </font><font style="color:#000000;">trim()</font><font style="color:#000000;">:去除字符串两端空白。</font>

<font style="color:#000000;"> split():分割字符串，</font><font style="color:#000000;background-color:#fbde28;">返回一个分割后的字符串数组</font><font style="color:#000000;">。</font>

<font style="color:#000000;"> </font><font style="color:#000000;">getBytes()</font><font style="color:#000000;">:返回字符串的 </font><font style="color:#000000;">byte </font><font style="color:#000000;">类型数组。</font>

<font style="color:#000000;"> </font><font style="color:#000000;">length()</font><font style="color:#000000;">:返回字符串长度。</font>

<font style="color:#000000;"> </font><font style="color:#000000;">toLowerCase()</font><font style="color:#000000;">:将字符串转成小写字母。</font>

<font style="color:#000000;"> </font><font style="color:#000000;">toUpperCase()</font><font style="color:#000000;">:将字符串转成大写字符。</font>

<font style="color:#000000;"> </font><font style="color:#000000;">substring()</font><font style="color:#000000;">:截取字符串。</font>

<font style="color:#000000;"> equals():字符串比较。</font>

## <font style="color:#000000;">30. 谈谈自定义注解的场景及实现</font>
<font style="color:#000000;">自定义注解在Java编程中主要用于提供元数据（metadata），</font><font style="color:#000000;background-color:#fbde28;">为程序元素（如类、方法、字段等）增加额外的信息</font><font style="color:#000000;">，这些信息可以在编译时或运行时被处理。常见的使用场景包括但不限于：</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">框架集成与扩展</font>**<font style="color:#000000;">：许多框架（如Spring、Hibernate、JPA等）通过自定义注解来简化配置，如</font>`<font style="color:#000000;">@Component</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">@Service</font>`<font style="color:#000000;">用于组件扫描和依赖注入；</font>`<font style="color:#000000;">@Transactional</font>`<font style="color:#000000;">用于声明式事务管理；</font>`<font style="color:#000000;">@Entity</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">@Table</font>`<font style="color:#000000;">等用于数据库映射。 </font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">验证与校验</font>**<font style="color:#000000;">：例如使用JSR-303/JSR-349提供的标准注解（如</font>`<font style="color:#000000;">@NotNull</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">@Size</font>`<font style="color:#000000;">等）或者自定义注解进行参数校验，确保输入的有效性和安全性。 </font>
3. <font style="color:#000000;"> </font>**<font style="color:#000000;">日志记录与跟踪</font>**<font style="color:#000000;">：可以创建自定义注解，结合AOP（面向切面编程）实现对特定方法的调用情况进行日志记录或性能监控。 </font>
4. <font style="color:#000000;"> </font>**<font style="color:#000000;">API文档生成</font>**<font style="color:#000000;">：像Swagger这样的工具可以通过解析方法上的自定义注解（如</font>`<font style="color:#000000;">@Api</font>`<font style="color:#000000;">, </font>`<font style="color:#000000;">@ApiOperation</font>`<font style="color:#000000;">等）自动生成API文档。 </font>
5. <font style="color:#000000;"> </font>**<font style="color:#000000;">代码分析与重构辅助</font>**<font style="color:#000000;">：通过注解标注待优化或待重构的代码块，便于后续的自动化工具识别和处理。 </font>

<font style="color:#000000;">实现自定义注解的基本步骤如下：</font>

```java
// 定义注解
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME) // 注解保留策略，这里指定为运行时可见
@Target(ElementType.METHOD) // 注解的目标类型，这里是方法级别
public @interface MyCustomAnnotation {
    String value() default ""; // 注解的一个属性，可以有默认值
    int status(); // 另一个注解属性，无默认值，使用时必须赋值
}

// 使用注解
public class MyClass {
    @MyCustomAnnotation(value = "This is a custom annotation", status = 200)
    public void myMethod() {
        // 方法体...
    }
}

// 解析注解（通常在运行时通过反射实现）
public class AnnotationProcessor {
    public static void processAnnotations(Object obj) {
        for (Method method : obj.getClass().getDeclaredMethods()) {
            MyCustomAnnotation annotation = method.getAnnotation(MyCustomAnnotation.class);
            if (annotation != null) {
                System.out.println("Value: " + annotation.value());
                System.out.println("Status: " + annotation.status());
                // 根据注解信息执行相应的操作...
            }
        }
    }
}
```

<font style="color:#000000;">在这个例子中，我们首先定义了一个名为</font>`<font style="color:#000000;">MyCustomAnnotation</font>`<font style="color:#000000;">的注解，并指定了其保留策略为运行时可见，以及应用的目标类型是方法。然后，在类</font>`<font style="color:#000000;">MyClass</font>`<font style="color:#000000;">的方法上使用了这个注解。最后，通过反射获取并解析了这个注解的属性值，并可以根据注解信息进行相关处理。</font>

## <font style="color:#000000;">31. 说说你熟悉的设计模式有哪些?</font>
[工作中常用到6种设计模式](https://www.yuque.com/vip6688/nei2as/good3d5x6ggdscev)

## <font style="color:#000000;">32. 什么是值传递和引用传递?</font>
<font style="color:#000000;">在编程语言中，值传递和引用传递是两种参数传递方式，它们决定了函数（或方法）调用时如何处理实参（caller提供的参数）和形参（callee接收的参数）之间的关系。</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">值传递</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">在值传递机制中，当一个函数接收到一个参数时，它会</font><font style="color:#000000;background-color:#fbde28;">创建该参数的一个副本</font><font style="color:#000000;">，并将这个副本传入函数内部。因此，在函数内部对参数</font><font style="color:#000000;background-color:#fbde28;">所做的任何修改不会影响到原始实参的值</font><font style="color:#000000;">。</font>
    - <font style="color:#000000;">对于</font><font style="color:#000000;background-color:#fbde28;">基本数据类型</font><font style="color:#000000;">（如整数、浮点数、布尔值等），通常默认采用值传递的方式，因为它们本身就是不可变的。</font>
    - <font style="color:#000000;">例如，在Java、C++、JavaScript等语言中，</font><font style="color:#000000;background-color:#fbde28;">普通变量作为参数传递时，默认使用的是值传递</font><font style="color:#000000;">。</font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">引用传递</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">引用传递意味着函数接收的是</font><font style="color:#000000;background-color:#fbde28;">实参引用的一个副本</font><font style="color:#000000;">，也就是说，它指向了同一个内存地址。因此，在函数内部对参数的</font><font style="color:#000000;background-color:#fbde28;">修改会影响到外部实参的值</font><font style="color:#000000;">。</font>
    - <font style="color:#000000;">对于引用类型（比如对象、数组等），某些语言（如C++中的指针、引用，或者Java中的对象引用）在传递时可以</font><font style="color:#000000;background-color:#fbde28;">实现类似引用传递的效果</font><font style="color:#000000;">，尽管严格来说，</font><font style="color:#000000;background-color:#fbde28;">Java并没有真正的引用传递，而是通过对象引用的复制来实现类似效果。</font>
    - <font style="color:#000000;">在C#中，可以通过</font>`<font style="color:#000000;">ref</font>`<font style="color:#000000;">或</font>`<font style="color:#000000;">out</font>`<font style="color:#000000;">关键字实现引用传递；在Python中，所有变量都是引用传递，但请注意，由于Python中的可变与不可变类型的存在，即使对于引用传递，也不总是直接改变原对象内容，对于不可变类型（如整型、字符串、元组），实际上是改变了新对象并让引用指向新对象。</font>

<font style="color:#000000;">举例说明：</font>

+ <font style="color:#000000;">值传递示例（以Java为例）：</font>

```java
void changeValue(int num) {
    num = 10;
}

int originalNum = 5;
changeValue(originalNum);
// 此时originalNum仍然是5，因为num是原始值的一个副本
```

+ <font style="color:#000000;">类似引用传递示例（以Java为例）：</font>

```java
class Box {
    int value;
}

void changeBox(Box box) {
    box.value = 10;
}

Box originalBox = new Box();
originalBox.value = 5;
changeBox(originalBox);
// 此时originalBox.value被修改为10，因为box引用的是原始对象的内存地址
```

## 33. <font style="color:#000000;">可以在 static 环境中访问非 static 变量吗?</font>
<font style="color:#000000;">当类被 Java 虚拟机载入的时候，会对 static 变量进行初始化。</font>

<font style="color:#000000;">因为静态的成员属于类， 随着类的加载而加载到静态方法区内存，</font>

<font style="color:#000000;background-color:#fbde28;">当类加载时，此时不一定有实例创建，没有实例，就不可以访问非静态的成员。</font>

<font style="color:#000000;background-color:#fbde28;">类的加载先于实例的创建</font><font style="color:#000000;">，因此静态环境中，不可以访问非静态!</font>

## <font style="color:#000000;">34. </font><font style="color:#000000;">Java 支持多继承么,为什么?</font>
<font style="color:#000000;background-color:#fbde28;">Java不支持类的多继承</font><font style="color:#000000;">，即一个类不能直接继承多个父类。这是因为在多继承中可能出现菱形问题（Diamond Problem），即当一个子类同时继承了</font><font style="color:#000000;background-color:#fbde28;">两个具有相同方法签名的父类</font><font style="color:#000000;">时，编译器无法确定应该调用哪个父类的方法，这会导致二义性。</font>

<font style="color:#000000;">例如，在C++中：</font>

```cpp
class A {
public:
    void method() { cout << "From A"; }
};

class B : public A {
public:
    void method() { cout << "From B"; }
};

class C : public A {
public:
    void method() { cout << "From C"; }
};

// 不允许在Java中这样定义，但在C++中可以
class D : public B, public C {
public:
    // 如果在这里调用method()，应该执行A、B还是C的版本？
};
```

<font style="color:#000000;">为了解决这个问题并保持语言设计的简单性和一致性，Java采用了</font><font style="color:#000000;background-color:#fbde28;">单一继承和接口多重实现</font><font style="color:#000000;">的方式来提供类似的功能：</font>

+ <font style="color:#000000;">Java中的类只能单继承自一个父类，通过这种方式避免了菱形问题。</font>
+ <font style="color:#000000;">为了实现类似多继承的效果，Java引入了接口（Interface）。一个类可以实现多个接口，并且接口中只包含抽象方法和常量，所以不会出现与多继承相关的二义性问题。</font>

<font style="color:#000000;">例如：</font>

```java
interface IA {
    void method();
}

interface IB {
    void method();
}

class A implements IA {
    @Override
    public void method() { System.out.println("From A"); }
}

class B implements IB {
    @Override
    public void method() { System.out.println("From B"); }
}

class D extends SomeOtherClass implements IA, IB {
    @Override
    public void method() { 
        // 这里需要明确实现IA或IB接口中的method()
        // 或者根据需求调用super.method()来区分
    }
}
```

<font style="color:#000000;">在上述示例中，类D虽然实现了两个接口，但每个接口的方法都需要在D中明确地覆盖和实现，因此不会出现多继承时的二义性问题。</font>

## <font style="color:#000000;">35. 构造器是否可被重写?</font>
<font style="color:#000000;">构造器是不能被继承的，因为每个类的类名都不相同，而构造器名称与类名相同，所以谈不上继承。 又由于构造器不能被继承，所以相应的就不能被重写了。</font>

## 36. <font style="color:#000000;">用最有效率的方法计算 2 乘以 8?</font>
<font style="color:#000000;">理论上可以利用左移运算符（<<）来代替乘法操作。因为二进制下乘以2相当于向左移动一位。但请注意，这种方法仅适用于乘数是2的幂次的情况。</font>

<font style="color:#000000;">对于计算 2 * 8，等价于将 1 （即2的1次方）向左移动3位（因为8等于2的3次方），所以可以用以下Java代码实现：</font>

```java
public class Main {
    public static void main(String[] args) {
        int result = 1 << 3; // 等同于 2 * (2^3)
        System.out.println("The result is: " + result);
    }
}
```

<font style="color:#000000;">这段代码同样会输出 "The result is: 16"。虽然这种做法在特定场景下可能有其优势，但对于一般乘法操作而言，并不比直接使用乘法运算符更有效率或直观。</font>

## <font style="color:#000000;">37. char 型变量中能不能存</font><font style="color:#000000;">储</font><font style="color:#000000;">一个中文汉字，为什么?</font>
<font style="color:#000000;">在Java中，char型变量可以存储一个中文汉字。这是因为Java中的字符类型（char）是采用</font><font style="color:#000000;background-color:#fbde28;">Unicode编码</font><font style="color:#000000;">的，</font><font style="color:#000000;background-color:#fbde28;">每个char占用2个字节（16位）</font><font style="color:#000000;">，而Unicode编码可以表示包括中文在内的各种国际字符集中的字符。</font>

<font style="color:#000000;">Unicode编码为每一个字符分配了一个唯一的码点（code point），对于基本的多文种平面（BMP，Basic Multilingual Plane），它使用了从U+0000到U+FFFF的码点范围，其中包含了大部分常用字符，包括</font><font style="color:#000000;background-color:#fbde28;">所有基本的汉字字符。由于这个范围内的字符都可以用一个16位的Unicode码来表示</font><font style="color:#000000;">，因此可以被存放在Java的一个char变量中。</font>

<font style="color:#000000;">需要注意的是，如果要表示超出BMP范围的字符，例如一些扩展字符或特殊符号，那么需要两个</font>`<font style="color:#000000;">char</font>`<font style="color:#000000;">类型的值组成一个代理对（surrogate pair）来存储，这种情况在Java中通常会使用</font>`<font style="color:#000000;">Character</font>`<font style="color:#000000;">类或者字符串处理方法来处理。但对于基本的汉字字符来说，单个char类型足以容纳。</font>

## <font style="color:#000000;">38. 如何实现对象克隆?</font>
+ <font style="color:#000000;">实现 Cloneable 接口，重写 clone() 方法。</font>
+ <font style="color:#000000;">Object 的 clone() 方法是浅拷贝，即如果类中属性有自定义引用类型，只拷贝引用，不拷贝引用指向的对象。</font>
+ <font style="color:#000000;">对象的属性的 Class 也实现 Cloneable 接口，在克隆对象时也手动克隆属性，完成深拷贝</font>
+ <font style="color:#000000;">结合序列化(JDK java.io.Serializable 接口、JSON 格式、XML 格式等)，完成深拷贝</font>

<font style="color:#000000;">注意：对于包含复杂数据结构（如嵌套对象、集合、映射等）的对象，实现深拷贝可能更加复杂，因为需要递归地对所有引用类型字段进行深拷贝处理。另外，如果某个引用类型的字段没有实现克隆功能，那么这个字段依然会导致浅拷贝问题。因此，在实际开发中，往往需要根据具体情况设计合理的克隆策略。</font>

## <font style="color:#000000;">39. object 中定义了哪些方法?</font>
+ <font style="color:#000000;">getClass(); 获取类结构信息</font>
+ <font style="color:#000000;">hashCode() 获取哈希码</font>
+ <font style="color:#000000;">equals(Object) 默认比较对象的地址值是否相等，子类可以重写比较规则</font>
+ <font style="color:#000000;">clone() 用于对象克隆</font>
+ <font style="color:#000000;">toString() 把对象转变成字符串</font>
+ <font style="color:#000000;">notify() 多线程中唤醒功能</font>
+ <font style="color:#000000;">notifyAll() 多线程中唤醒所有等待线程的功能</font>
+ <font style="color:#000000;">wait() 让持有对象锁的线程进入等待</font>
+ <font style="color:#000000;">wait(long timeout) 让持有对象锁的线程进入等待，设置超时毫秒数时间</font>
+ <font style="color:#000000;">wait(long timeout, int nanos) 让持有对象锁的线程进入等待，设置超时纳秒数时间</font>
+ <font style="color:#000000;">finalize() 垃圾回收前执行的方法</font>

## <font style="color:#000000;">40. hashCode 的作用是什么?</font>
+ <font style="color:#000000;">hashCode 的存在主要是用于</font><font style="color:#000000;background-color:#fbde28;">查找的快捷性</font><font style="color:#000000;">，如 Hashtable，HashMap 等， hashCode 是用来在散列存储结构中</font><font style="color:#000000;background-color:#fbde28;">确定对象的存储地址的</font><font style="color:#000000;">;</font>
+ <font style="color:#000000;">如果两个对象相同，就是适用于 equals(java.lang.Object) 方法，那么这两个对象的 hashCode 一定要相同;</font>
+ <font style="color:#000000;">如果对象的 equals 方法被重写，那么对象的 hashCode 也尽量重写，并且产生 hashCode 使用的对象，</font><font style="color:#000000;background-color:#fbde28;">一定要和 equals 方法中使用的一致</font><font style="color:#000000;">，否则就会违反上面提到的第 2 点;</font>
+ <font style="color:#000000;">两个对象的 hashCode 相同，并不一定表示两个对象就相同，也就是不一定适用于equals(java.lang.Object) 方法，只能够说明这两个对象在散列存储结构中.</font>

## <font style="color:#000000;">41. for-each 与常规 for 循环的效率对比</font>
在J<font style="color:#000000;">ava中，</font>`<font style="color:#000000;">for-each</font>`<font style="color:#000000;">循环（也称为增强型for循环）和常规的索引式</font>`<font style="color:#000000;">for</font>`<font style="color:#000000;">循环在效率上的差异主要取决于它们遍历的数据结构以及实现细节。</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">ArrayList</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">对于实现了</font>`<font style="color:#000000;">List</font>`<font style="color:#000000;">接口且内部使用数组存储元素的集合类如</font>`<font style="color:#000000;">ArrayList</font>`<font style="color:#000000;">，在进行迭代时，两种循环方式的效率相近。</font>`<font style="color:#000000;">for-each</font>`<font style="color:#000000;">循环在编译后实际上也是通过迭代器进行访问的，但在大部分情况下，由于避免了显式的索引操作，代码更为简洁，不易出错。</font>
    - <font style="color:#000000;">当仅需读取数据而不做其他额外操作时，两者性能差距不大，甚至在某些场景下，</font>`<font style="color:#000000;">for-each</font>`<font style="color:#000000;">可能因为更少的变量操作而稍微快一点。</font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">LinkedList</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">对于</font>`<font style="color:#000000;">LinkedList</font>`<font style="color:#000000;">这类链表结构的集合类，由于其内部结构不支持随机访问，所以使用普通</font>`<font style="color:#000000;">for</font>`<font style="color:#000000;">循环基于索引访问时，需要频繁地从一个节点移动到下一个节点，这会导致大量的指针跳跃操作，效率较低。</font>
    - <font style="color:#000000;">相反，</font>`<font style="color:#000000;">for-each</font>`<font style="color:#000000;">循环在这种情况下通常会更快，因为它底层使用的是迭代器，迭代器已经为链表优化了遍历过程，无需每次都计算索引位置。</font>
3. <font style="color:#000000;"> </font>**<font style="color:#000000;">固定大小数组</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">对于基本类型的固定大小数组，普通的</font>`<font style="color:#000000;">for</font>`<font style="color:#000000;">循环可以通过索引直接访问内存中的连续区域，理论上可以比</font>`<font style="color:#000000;">for-each</font>`<font style="color:#000000;">循环更快一些，尤其是在大量迭代的情况下。不过，这种差距通常很小，除非是在极高的性能要求场合才有可能明显感受到。</font>

<font style="color:#000000;">综上所述，在实际开发中，选择哪种循环方式往往更多地依赖于代码的可读性和维护性，而不是微小的性能差距。对于大多数应用而言，</font>`<font style="color:#000000;">for-each</font>`<font style="color:#000000;">循环因其简洁和不易出错的特性而受到推荐。当然，在对性能有严格要求的场景下，根据具体的数据结构特点选择合适的循环方式是必要的。</font>

## <font style="color:#000000;">42. 写出几种单例模式实现，懒汉模式和饿汉模式区别？</font>
[单例模式的七种写法](https://www.yuque.com/vip6688/nei2as/pttm4baxccia3qma)

## <font style="color:#000000;">43. 请列出 5 个运行时异常？</font>
<font style="color:#000000;">以下是Java中5个常见的运行时异常（它们都是</font>`<font style="color:#000000;">java.lang.RuntimeException</font>`<font style="color:#000000;">的子类）：</font>

1. <font style="color:#000000;"> </font>**<font style="color:#117cee;">NullPointerException</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">当应用程序试图访问一个空对象引用的方法或字段时抛出。例如，调用 </font>`<font style="color:#000000;">null</font>`<font style="color:#000000;"> 对象的任何方法都会引发此异常。</font>
2. <font style="color:#000000;"> </font>**<font style="color:#117cee;">ArrayIndexOutOfBoundsException</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">当程序试图访问数组的一个不存在的索引时抛出。例如，如果尝试读取或写入超出数组长度范围的位置。</font>
3. <font style="color:#000000;"> </font>**<font style="color:#117cee;">ClassCastException</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">在试图将某个对象强制转换为其非实例化的类或接口时抛出。例如，当你把一个 </font>`<font style="color:#000000;">Object</font>`<font style="color:#000000;"> 强制转换为它实际类型以外的其他类型时。</font>
4. <font style="color:#000000;"> </font>**<font style="color:#117cee;">IllegalArgumentException</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">当向方法传递非法或不合适的参数时抛出。例如，如果你提供的参数违反了方法约定的限制条件。</font>
5. <font style="color:#000000;"> </font>**<font style="color:#117cee;">NumberFormatException</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">当在解析字符串为数值类型（如Integer、Float等）时，字符串内容无法被正确解析为相应类型的数字格式时抛出。例如，</font>`<font style="color:#000000;">Integer.parseInt("abc")</font>`<font style="color:#000000;"> 就会抛出该异常。</font>



<font style="color:#000000;">此外，还有许多其他的运行时异常，比如 </font>`<font style="color:#000000;">ArithmeticException</font>`<font style="color:#000000;">（数学运算异常，如除数为零）、</font>`<font style="color:#000000;">IllegalStateException</font>`<font style="color:#000000;">（当对象处于不适当的状态下进行操作时抛出）等。</font>

## <font style="color:#000000;">44. 2 个不相等的对象有可能具有相同的 hashcode 吗?</font>
<font style="color:#000000;">hashCode 的常规协定:</font>

+ <font style="color:#000000;">在 Java 应用程序执行期间，在对同一对象多次调用 hashCode 方法时，必须一 致地返回相同的整数，前提是将对象进行 equals 比较时所用的信息没有被修改。 从某一应用程序的一次执行到同一应用程序的另一次执行，该整数无需保持一致。</font>
+ <font style="color:#000000;">两个对象的 equals()相等，那么对这两个对象中的每个对象调用 hashCode 方法 都必须生成相同的整数果。</font>
+ <font style="color:#000000;">两个对象的 equals()不相等，那么对这两个对象中的任一对象上调用 hashCode 方法不要求一定生成不同的整数结果。但是，为不相等的对象生成不同整数结果可 以提高哈希表的性能</font>

## <font style="color:#000000;">45. 访问修饰符 public,private,protected,以及 default 的区别?</font>
<font style="color:#000000;">以下是一个表格，详细列出了Java中四种访问修饰符（public、private、protected和default）的区别：</font>

| 访问修饰符 | 同一类中 | 同一包内其他类 | 不同包的子类 | 不同包的非子类 |
| :---: | :---: | :---: | :---: | :---: |
| <font style="color:#000000;">public</font> | <font style="color:#000000;">可见</font> | <font style="color:#000000;">可见</font> | <font style="color:#000000;">可见</font> | <font style="color:#000000;">可见</font> |
| <font style="color:#000000;">protected</font> | <font style="color:#000000;">可见</font> | <font style="color:#000000;">可见</font> | <font style="color:#000000;">可见</font> | <font style="color:#000000;">不可见</font> |
| <font style="color:#000000;">(default)</font> | <font style="color:#000000;">可见</font> | <font style="color:#000000;">可见</font> | <font style="color:#000000;">不可见</font> | <font style="color:#000000;">不可见</font> |
| <font style="color:#000000;">private</font> | <font style="color:#000000;">可见</font> | <font style="color:#000000;">不可见</font> | <font style="color:#000000;">不可见</font> | <font style="color:#000000;">不可见</font> |




+ <font style="color:#000000;"> </font>**<font style="color:#000000;">public</font>**<font style="color:#000000;">：</font><font style="color:#000000;background-color:#fbde28;">具有最大的访问权限</font><font style="color:#000000;">。声明为public的类、方法或成员变量可以在任何地方被访问。 </font>
+ <font style="color:#000000;"> </font>**<font style="color:#000000;">protected</font>**<font style="color:#000000;">：</font><font style="color:#000000;background-color:#fbde28;">受保护的访问权限</font><font style="color:#000000;">。在同一个包内的任何类都可以访问protected修饰的内容；此外，不同包的子类也可以访问其父类中protected的成员。但直接在不同的包且不是子类的情况下无法访问。 </font>
+ <font style="color:#000000;"> </font>**<font style="color:#000000;">(default)</font>**<font style="color:#000000;"> 或称无修饰符：</font><font style="color:#000000;background-color:#fbde28;">默认访问权限</font><font style="color:#000000;">。在同一包内的任何类都可以访问，默认权限不适用于接口中的成员。对于没有指定访问修饰符的类、方法或字段，它们仅对同一包内的其他类可见。 </font>
+ <font style="color:#000000;"> </font>**<font style="color:#000000;">private</font>**<font style="color:#000000;">：</font><font style="color:#000000;background-color:#fbde28;">私有访问权限</font><font style="color:#000000;">。声明为private的类成员只能在其所属的类内部访问，任何其他类（包括同一个包内和不同包内的类）都无法访问。注意，类不能声明为private，只有类的内部成员可以是private。 </font>

## <font style="color:#000000;">46. 谈谈 final 在 java 中的作用?</font>
+ <font style="color:#000000;">final 修饰的类叫最终类，该类不能被继承。</font>
+ <font style="color:#000000;">final 修饰的方法不能被重写。</font>
+ <font style="color:#000000;">final 修饰的变量叫常量，常量必须初始化，初始化之后值就不能被修改。</font>

## 47. <font style="color:rgb(63, 63, 63);">java 中的 Math.round(-1.5) 等于多少呢?</font>
<font style="color:#000000;">JDK 中的 java.lang.Math 类:</font>

+ <font style="color:#000000;">round() :</font><font style="color:#000000;background-color:#fbde28;">返回四舍五入</font><font style="color:#000000;">，负 .5 小数返回较大整数，如 -1.5 返回 -1。</font>
+ <font style="color:#000000;">ceil() :返回</font><font style="color:#000000;background-color:#fbde28;">小数所在两整数间的较大值</font><font style="color:#000000;">，如 -1.5 返回 -1.0。</font>
+ <font style="color:#000000;">floor() :返回</font><font style="color:#000000;background-color:#fbde28;">小数所在两整数间的较小值</font><font style="color:#000000;">，如 -1.5 返回 -2.0。</font>

## <font style="color:#000000;">48. </font><font style="color:#000000;">String 属于基础的数据类型吗?</font>
<font style="color:#000000;">String 不属于基础类型，</font>

<font style="color:#000000;">基础类型有 8 种: byte、boolean、char、short、int、float、long、double，</font>

<font style="color:#000000;">而 String 属于引用数据类型。</font>

## <font style="color:#000000;">49. </font><font style="color:#000000;">如何将字符串反转呢?</font>
+ <font style="color:#000000;">使用 StringBuilder 或 StringBuffer 的 </font>**<font style="color:#117cee;">reverse</font>**<font style="color:#000000;"> 方法，本质都调用了它们的父类AbstractStringBuilder 的 reverse 方法实现。(JDK1.8)</font>
+ <font style="color:#000000;">使用 chatAt 函数，倒过来输出</font>

## <font style="color:#000000;">50. </font><font style="color:#000000;">在自己的代码中，如果创建一个 java.lang.String 类， 这个类是否可以被类加载器加载?为什么?</font>
<font style="color:#000000;">不可以。因为 JDK 处于安全性的考虑，基于双亲委派模型，优先加载 JDK 的 String 类，如果 java.lang.String 已经加载，便不会再次被加载。</font>

## <font style="color:#000000;">51. </font><font style="color:#000000;">谈谈你对 java.lang.Object 对象中 hashCode 和 equals 方法的理解。在什么场景下需要重新实现这两个方法。</font>
<font style="color:#000000;">在Java中，</font>`<font style="color:#000000;">java.lang.Object</font>`<font style="color:#000000;">是所有类的超类，它定义了两个关键的方法：</font>`<font style="color:#000000;">hashCode()</font>`<font style="color:#000000;"> 和 </font>`<font style="color:#000000;">equals()</font>`<font style="color:#000000;">。这两个方法对于对象的身份识别和集合操作至关重要。</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">hashCode()</font>**<font style="color:#000000;">： </font>
    - `<font style="color:#000000;">hashCode()</font>`<font style="color:#000000;">方法返回一个整数，这个整数通常是</font><font style="color:#000000;background-color:#fbde28;">对象的某种内部地址或者其内容的某种哈希表示</font><font style="color:#000000;">。</font><font style="color:#000000;background-color:#fbde28;">默认实现基于对象的内存地址计算</font><font style="color:#000000;">，这意味着每个对象（除非重写hashCode()）都有不同的哈希码。</font>
    - <font style="color:#000000;">哈希码主要用于支持哈希表（如HashMap、HashSet等）的数据结构，它们</font><font style="color:#000000;background-color:#fbde28;">通过哈希码来快速定位元素存储的位置</font><font style="color:#000000;">，从而实现高效查找、添加和删除操作。</font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">equals()</font>**<font style="color:#000000;">： </font>
    - `<font style="color:#000000;">equals()</font>`<font style="color:#000000;">方法</font><font style="color:#000000;background-color:#fbde28;">用于判断两个对象是否相等</font><font style="color:#000000;">。默认情况下，</font><font style="color:#000000;background-color:#fbde28;">Object类中的equals()方法会比较两个对象的引用是否指向同一个对象实例</font><font style="color:#000000;">，即只有当两个引用指向内存中的同一位置时才返回true。</font>
    - <font style="color:#000000;">在实际应用中，我们经常需要根据对象的</font><font style="color:#000000;background-color:#fbde28;">实际内容（而非引用）来判断</font><font style="color:#000000;">它们是否相等，因此常常需要</font><font style="color:#000000;background-color:#fbde28;">对equals()方法进行重写</font><font style="color:#000000;">。</font>

**<font style="color:#000000;">重新实现hashCode()和equals()的场景：</font>**

+ <font style="color:#000000;"> </font>**<font style="color:#000000;">集合类操作</font>**<font style="color:#000000;">：当你将</font><font style="color:#000000;background-color:#fbde28;">自定义类的对象</font><font style="color:#000000;">放入到HashSet、HashMap或HashTable等</font><font style="color:#000000;background-color:#fbde28;">集合中</font><font style="color:#000000;">，并且希望根据对象的内容（属性值）来进行查找、删除和唯一性判断时，</font><font style="color:#000000;background-color:#fbde28;">必须同时重写equals()和hashCode()方法。 </font>
    - <font style="color:#000000;">重写equals()是为了</font><font style="color:#000000;background-color:#fbde28;">正确地判断两个对象在业务逻辑上是否相等</font><font style="color:#000000;">。</font>
    - <font style="color:#000000;">同时，重写hashCode()要</font><font style="color:#000000;background-color:#fbde28;">确保“相等”的对象具有相同的哈希码</font><font style="color:#000000;">，即如果</font>`<font style="color:#000000;">a.equals(b)</font>`<font style="color:#000000;">为true，则</font>`<font style="color:#000000;">a.hashCode() == b.hashCode()</font>`<font style="color:#000000;">也应为true。这被称为</font><font style="color:#000000;background-color:#fbde28;">哈希码一致性约定</font><font style="color:#000000;">，以保证哈希表正常工作。</font>
+ <font style="color:#000000;"> </font>**<font style="color:#000000;">数据持久化与缓存</font>**<font style="color:#000000;">：在数据库操作、序列化、缓存系统等场合下，也需要</font><font style="color:#000000;background-color:#fbde28;">根据对象的实际内容来决定对象的唯一性</font><font style="color:#000000;">，这时通常需要重写equals()和hashCode()方法。 </font>

<font style="color:#000000;">总之，当类的实例应该根据其</font><font style="color:#000000;background-color:#fbde28;">实例变量的值</font><font style="color:#000000;">而不是其</font><font style="color:#000000;background-color:#fbde28;">内存地址</font><font style="color:#000000;">来确定相等性时，就需要重新实现</font>`<font style="color:#000000;">equals()</font>`<font style="color:#000000;">和</font>`<font style="color:#000000;">hashCode()</font>`<font style="color:#000000;">方法，以</font><font style="color:#000000;background-color:#fbde28;">满足集合操作的一致性和性能要求</font><font style="color:#000000;">。同时，为了保持良好的编程实践，这两个方法应该一起被重写，并且保持行为一致。</font>

## <font style="color:#000000;">52. 什么是序列化，怎么序列化，反序列呢?</font>
+ <font style="color:#000000;">序列化:把 Java 对象转换为字节序列的过程</font>
+ <font style="color:#000000;">反序列:把字节序列恢复为 Java 对象的过程</font>

## <font style="color:#000000;">53. java8 的新特性</font>
+ **<font style="color:#117cee;">Lambda</font>**<font style="color:#000000;">表达式:Lambda 允许把函数作为一个方法的参数</font>
+ **<font style="color:#117cee;">Stream</font>**<font style="color:#000000;"> API :新添加的 Stream API(java.util.stream) 把真正的函数式编程风格引入到 Java 中</font>
+ **<font style="color:#117cee;">方法引用</font>**<font style="color:#000000;">:方法引用提供了非常有用的语法，可以直接引用已有 Java 类或对象(实例)的方法或构造器。</font>
+ **<font style="color:#117cee;">默认方法</font>**<font style="color:#000000;">:默认方法就是一个在接口里面有了一个实现的方法。</font>
+ **<font style="color:#117cee;">Optional</font>**<font style="color:#000000;"> 类 :Optional 类已经成为 Java 8 类库的一部分，用来解决空指针异常。</font>
+ **<font style="color:#117cee;">DateTime</font>**<font style="color:#000000;"> API :加强对日期与时间的处理。</font>

## <font style="color:#000000;">54. break 和 continue 有什么区别?</font>
+ <font style="color:#000000;">break 可以使流程跳出 switch 语句体，也可以在循环结构终止本层循环体，从而 提前结束本层循环。</font>
+ <font style="color:#000000;">continue 的作用是跳过本次循环体中余下尚未执行的语句，立即进行下一次的循 环条件判定，可以理解为仅结束本次循环</font>

## <font style="color:#000000;">55. </font><font style="color:#000000;">匿名内部类是什么?如何访问在其外面定义的变量呢?</font>
<font style="color:#000000;">匿名内部类是Java中一种特殊的类，</font><font style="color:#000000;background-color:#fbde28;">它没有名字，并且在定义时就实例化</font><font style="color:#000000;">。通常情况下，匿名内部类用于</font><font style="color:#000000;background-color:#fbde28;">实现接口或者继承一个抽象类</font><font style="color:#000000;">，以提供某种功能的即时实现。这种类常用于</font><font style="color:#000000;background-color:#fbde28;">简化代码</font><font style="color:#000000;">，尤其是当仅需要使用一次该类实例的时候。</font>

<font style="color:#000000;">匿名内部类的定义语法如下：</font>

```java
new SuperClassOrInterface() {
    // 类体部分，包括变量声明、方法重写等
};
```

<font style="color:#000000;">访问在其外面定义的变量，分为两种情况：</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">访问外部类中的成员变量</font>**<font style="color:#000000;">： </font><font style="color:#000000;">匿名内部类可以</font><font style="color:#000000;background-color:#fbde28;">直接访问外部类中的静态变量和实例变量</font><font style="color:#000000;">（包括final修饰的实例变量），无论这些变量是否为final。例如： </font>

```java
public class OuterClass {
    private int outerValue;

    void someMethod() {
        // 创建并实例化一个匿名内部类
        new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("Outer value: " + outerValue);
            }
        };
    }
}
```

<font style="color:#000000;">在这个例子中，匿名内部类通过 </font>`<font style="color:#000000;">outerValue</font>`<font style="color:#000000;"> 访问了外部类 </font>`<font style="color:#000000;">OuterClass</font>`<font style="color:#000000;"> 的实例变量。 </font>

2. <font style="color:#000000;"> </font>**<font style="color:#000000;">访问局部变量</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">若要访问外部方法中的局部变量，则</font><font style="color:#000000;background-color:#fbde28;">该局部变量必须是final或事实上的final</font><font style="color:#000000;">（即虽然没有显式声明为final，但在使用它的匿名内部类之前，没有改变过其值）。</font>
    - <font style="color:#000000;">Java编译器会为这样的局部变量创建一个隐含的final副本，匿名内部类实际上是通过这个副本来访问局部变量的。</font>

<font style="color:#000000;">示例： </font>

```java
public class Example {
    public void process() {
        final String localVariable = "Hello";  // 必须为final

        Thread thread = new Thread(new Runnable() {  // 匿名内部类实现Runnable接口
            @Override
            public void run() {
                System.out.println(localVariable);  // 访问外部方法的局部变量
            }
        });

        thread.start();
    }
}
```

<font style="color:#000000;">在这个例子中，匿名内部类实现了 </font>`<font style="color:#000000;">Runnable</font>`<font style="color:#000000;"> 接口，并在 </font>`<font style="color:#000000;">run()</font>`<font style="color:#000000;"> 方法中访问了外部 </font>`<font style="color:#000000;">process()</font>`<font style="color:#000000;"> 方法中的 </font>`<font style="color:#000000;">localVariable</font>`<font style="color:#000000;"> 局部变量。由于 </font>`<font style="color:#000000;">localVariable</font>`<font style="color:#000000;"> 是final的，所以可以被匿名内部类访问。</font>

## <font style="color:#000000;">56. String s = "Hello";s = s + " world!";这两行代 码执行后，原始的 String 对象中的内容是否会改变?</font>
<font style="color:#000000;">没有。因为 String 被设计成不可变(immutable)类，所以它的所有对象都是 </font><font style="color:#000000;background-color:#fbde28;">不可变对象</font><font style="color:#000000;">。</font>

## <font style="color:#000000;">57. </font><font style="color:#000000;">String s="a"+"b"+"c"+"d";创建了几个对象?</font>
在<font style="color:#000000;">Java中，当你使用字符串字面量（String literals）通过 "+" 运算符连接时，编译器会进行优化，因此对于表达式 </font>`<font style="color:#000000;">String s = "a" + "b" + "c" + "d";</font>`<font style="color:#000000;"> 在</font><font style="color:#000000;background-color:#fbde28;">编译期间会合并这些字面量</font><font style="color:#000000;">成为一个单独的字符串常量。这意味着在运行时，实际上只创建了一个 </font>`<font style="color:#000000;">String</font>`<font style="color:#000000;"> 对象。所以，在这个特定的例子中，只创建了一个 </font>`<font style="color:#000000;">String</font>`<font style="color:#000000;"> 对象。</font>

## <font style="color:#000000;">58. </font><font style="color:#000000;">try-catch-finally-return 执行顺序</font>
<font style="color:#000000;">在Java中，</font>`<font style="color:#000000;">try-catch-finally-return</font>`<font style="color:#000000;"> 结构的执行顺序是这样的：</font>

1. <font style="color:#000000;"> </font>**<font style="color:#000000;">try块</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">首先执行 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 块中的代码。</font>
    - <font style="color:#000000;">如果 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 块中的代码没有抛出异常，则直接跳到第5步（finally块）。</font>
2. <font style="color:#000000;"> </font>**<font style="color:#000000;">catch块</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">如果 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 块中的代码</font><font style="color:#000000;background-color:#fbde28;">抛出了一个与 </font>`<font style="color:#000000;background-color:#FBDE28;">catch</font>`<font style="color:#000000;background-color:#fbde28;"> 子句匹配的异常</font><font style="color:#000000;">，则立即执行相应的 </font>`<font style="color:#000000;">catch</font>`<font style="color:#000000;"> 块中的代码。</font>
    - <font style="color:#000000;">如果有多个 </font>`<font style="color:#000000;">catch</font>`<font style="color:#000000;"> 子句，会按照从上到下的顺序检查每个子句，找到第一个能处理当前抛出异常类型的 </font>`<font style="color:#000000;">catch</font>`<font style="color:#000000;"> 子句执行。</font>
3. <font style="color:#000000;"> </font>**<font style="color:#000000;">finally块</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">不论 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 块或对应的 </font>`<font style="color:#000000;">catch</font>`<font style="color:#000000;"> 块是否抛出或捕获了异常，</font><font style="color:#000000;background-color:#fbde28;">都会执行 </font>`<font style="color:#000000;background-color:#FBDE28;">finally</font>`<font style="color:#000000;background-color:#fbde28;"> 块中的代码</font><font style="color:#000000;">。即使在 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 或 </font>`<font style="color:#000000;">catch</font>`<font style="color:#000000;"> 中使用了 </font>`<font style="color:#000000;">return</font>`<font style="color:#000000;"> 语句，也会在方法返回前执行 </font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块。</font>
4. <font style="color:#000000;"> </font>**<font style="color:#000000;">return语句</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">如果 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 或 </font>`<font style="color:#000000;">catch</font>`<font style="color:#000000;"> 块中有 </font>`<font style="color:#000000;">return</font>`<font style="color:#000000;"> 语句，并且在执行到 </font>`<font style="color:#000000;">return</font>`<font style="color:#000000;"> 之前没有发生任何异常，那么会在</font><font style="color:#000000;background-color:#fbde28;">返回之前执行 </font>`<font style="color:#000000;background-color:#FBDE28;">finally</font>`<font style="color:#000000;background-color:#fbde28;"> 块的代码</font><font style="color:#000000;">。</font>
    - <font style="color:#000000;">如果 </font>`<font style="color:#000000;">return</font>`<font style="color:#000000;"> 语句出现在 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 或 </font>`<font style="color:#000000;">catch</font>`<font style="color:#000000;"> 块中，并且其后还有 </font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块，那么方法返回的结果将会是 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 或 </font>`<font style="color:#000000;">catch</font>`<font style="color:#000000;"> 块中 </font>`<font style="color:#000000;">return</font>`<font style="color:#000000;"> 语句指定的值，但 </font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块仍会被执行。</font>
    - 这段话的意思是：
    1. <font style="color:#000000;">当 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 或 </font>`<font style="color:#000000;">catch</font>`<font style="color:#000000;"> 块中有 </font>`<font style="color:#000000;">return</font>`<font style="color:#000000;"> 语句，且在执行到该 </font>`<font style="color:#000000;">return</font>`<font style="color:#000000;"> 语句之前没有发生任何异常时，程序会在返回之前先执行 </font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块中的代码。这意味着无论是否发生异常，</font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块都会被执行。</font>

<font style="color:#000000;">例如：</font>

```java
int method() {
    try {
        // 某些操作...
        return 10;  // 在这里返回一个值
    } finally {
        System.out.println("Finally block is executed before the method returns.");
    }
}
```

<font style="color:#000000;">在这个例子中，即使 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 块中直接返回了一个值，也会</font><font style="color:#000000;background-color:#fbde28;">先打印出</font><font style="color:#000000;"> "Finally block is executed before the method returns."，然后</font><font style="color:#000000;background-color:#fbde28;">再将10作为方法的结果返回</font><font style="color:#000000;">给调用者。</font>

```plain
2. <font style="color:#000000;">如果 </font>`<font style="color:#000000;">return</font>`<font style="color:#000000;"> 语句出现在 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 或 </font>`<font style="color:#000000;">catch</font>`<font style="color:#000000;"> 块中，并且后面还有 </font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块，那么不论 </font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块内部如何操作，方法最终返回给调用者的值将是 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 或 </font>`<font style="color:#000000;">catch</font>`<font style="color:#000000;"> 块中 </font>`<font style="color:#000000;background-color:#FBDE28;">return</font>`<font style="color:#000000;background-color:#FBDE28;"> 语句指定的值。</font><font style="color:#000000;">而 </font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块虽然会被执行，但它不能改变已决定返回的那个值（除非通过间接方式，比如修改引用类型的对象状态）。</font>
```

<font style="color:#000000;">例如：</font>

```java
int method() {
    int result = 0;
    try {
        result = 10;
        return result;
    } finally {
        result = 20;  // 修改了局部变量result的值，但这不会影响已经决定返回的10
    }
}
```

<font style="color:#000000;">在此例中，尽管在 </font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块中改变了局部变量 </font>`<font style="color:#000000;">result</font>`<font style="color:#000000;"> 的值为20，但因为 </font>`<font style="color:#000000;">try</font>`<font style="color:#000000;"> 块中已经决定了要返回10，所以</font><font style="color:#000000;background-color:#fbde28;">最终方法会返回10给调用者</font><font style="color:#000000;">。而 </font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块内的改动对已经决定的</font><font style="color:#000000;background-color:#fbde28;">返回值没有直接影响。</font>



<font style="color:#000000;">具体流程图示如下：</font>

```java
try {
    // 执行 try 块代码
    if (无异常) {
        goto step 5;
    } else if (有异常) {
        goto step 2;
    }
} catch (ExceptionType1 e) {
    // 处理 ExceptionType1 异常
    goto step 5;
} catch (ExceptionType2 e) {
    // 处理 ExceptionType2 异常
    goto step 5;
} finally {
    // 不管是否有异常，始终执行 finally 块
    step 5: 
    // 如果前面的 try 或 catch 包含 return 语句，在执行 return 之前执行 finally 块
    }
```

<font style="color:#000000;">注意：如果在 </font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块中修改了要返回的变量值，这将不会影响已经决定要返回的原始值。但在 Java 8 及以上版本中，可以使用 </font>`<font style="color:#000000;">try-with-resources</font>`<font style="color:#000000;"> 语句，其中的 </font>`<font style="color:#000000;">finally</font>`<font style="color:#000000;"> 块可能会对资源清理后的返回结果产生影响。</font>

## 59. <font style="color:#000000;">Java 7 新的 try-with-resources 语句，平时有使用吗?</font>
<font style="color:#000000;">在Java 7及更高版本中，try-with-resources语句是一种非常实用且推荐使用的资源管理机制。在处理需要关闭的资源（如文件、数据库连接、网络套接字等）时，它可以确保资源在使用完毕后能够被正确、及时地关闭。</font>

<font style="color:#000000;">传统的做法是在finally块中手动关闭资源，但这种方式容易出错或遗漏。而try-with-resources通过自动关闭特性简化了这一过程：</font>

```java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        // 处理文件内容...
    }
} catch (IOException e) {
    // 处理异常
}
```

<font style="color:#000000;">在这个例子中，</font>`<font style="color:#000000;">BufferedReader</font>`<font style="color:#000000;"> 实现了 </font>`<font style="color:#000000;">AutoCloseable</font>`<font style="color:#000000;"> 接口，因此它可以在 try-with-resources 结构结束时自动调用其 </font>`<font style="color:#000000;">close()</font>`<font style="color:#000000;"> 方法来释放资源。即使在读取过程中抛出了异常，也会确保资源最终会被关闭。这种语法既简洁又安全，降低了资源泄漏的风险。</font>

## <font style="color:#000000;">60. 简述一下面向对象的”六原则一法则”。</font>
+ **<font style="color:#117cee;">单一职责原则</font>**<font style="color:#000000;">:一个类只做它该做的事情。</font>
+ **<font style="color:#117cee;">开闭原则</font>**<font style="color:#000000;">:软件实体应当对扩展开放，对修改关闭。</font>
+ **<font style="color:#117cee;">依赖倒转原则</font>**<font style="color:#000000;">:面向接口编程。</font>
+ **<font style="color:#117cee;">接口隔离原则</font>**<font style="color:#000000;">:接口要小而专，绝不能大而全。</font>
+ **<font style="color:#117cee;">合成聚合复用原则</font>**<font style="color:#000000;">:优先使用聚合或合成关系复用代码。</font>
+ **<font style="color:#117cee;">迪米特法则</font>**<font style="color:#000000;">:迪米特法则又叫最少知识原则，一个对象应当对其他对象有尽可能少的了解。</font>

## <font style="color:#000000;">61. 数组有没有 length()方法?String 有没有 length() 方法?</font>
<font style="color:#000000;">在Java中：</font>

+ <font style="color:#000000;">数组没有直接的 </font>`<font style="color:#000000;">length()</font>`<font style="color:#000000;"> 方法，但是数组对象有一个</font><font style="color:#000000;background-color:#fbde28;">公共字段（property）叫做 </font>`<font style="color:#000000;background-color:#FBDE28;">length</font>`<font style="color:#000000;">。例如，如果你有一个名为 </font>`<font style="color:#000000;">array</font>`<font style="color:#000000;"> 的数组，你可以通过 </font>`<font style="color:#000000;">array.length</font>`<font style="color:#000000;"> 来获取它的长度（元素个数）。</font>

```java
int[] myArray = new int[5];
System.out.println(myArray.length);  // 输出：5
```

+ <font style="color:#000000;">字符串类（String）确实有一个名为 </font>`<font style="color:#000000;">length()</font>`<font style="color:#000000;"> 的方法，它返回字符串中的字符数量。</font>

```java
String myString = "Hello, World!";
System.out.println(myString.length());  // 输出：13
```

## <font style="color:#000000;">62. switch 是否能作用在 byte 上，是否能作用在 long 上，是否能作用在 String 上?</font>
<font style="color:#000000;">在Java中，</font>`<font style="color:#000000;">switch</font>`<font style="color:#000000;">语句的行为随版本变化而有所改变：</font>

+ <font style="color:#000000;"> </font>**<font style="color:#000000;">对于byte类型</font>**<font style="color:#000000;">：是的，从Java早期版本开始，</font>`<font style="color:#000000;">switch</font>`<font style="color:#000000;">就可以作用于</font>`<font style="color:#000000;">byte</font>`<font style="color:#000000;">类型。这是因为</font>`<font style="color:#000000;">byte</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">short</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">char</font>`<font style="color:#000000;">和</font>`<font style="color:#000000;">int</font>`<font style="color:#000000;">都是兼容的，并且它们</font><font style="color:#000000;background-color:#fbde28;">可以隐式转换为</font>`<font style="color:#000000;background-color:#FBDE28;">int</font>`<font style="color:#000000;">。 </font>
+ <font style="color:#000000;"> </font>**<font style="color:#000000;">对于long类型</font>**<font style="color:#000000;">：在Java 7及以前的版本中，</font>`<font style="color:#000000;">switch</font>`<font style="color:#000000;">不能直接作用于</font>`<font style="color:#000000;">long</font>`<font style="color:#000000;">类型。这是因为在这些版本中，</font>`<font style="color:#000000;">switch</font>`<font style="color:#000000;">只支持</font>`<font style="color:#000000;">byte</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">short</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">char</font>`<font style="color:#000000;">和</font>`<font style="color:#000000;">int</font>`<font style="color:#000000;">类型作为其表达式的值。不过，从</font><font style="color:#000000;background-color:#fbde28;">Java 1.8（JDK 1.8）开始，</font>`<font style="color:#000000;background-color:#FBDE28;">switch</font>`<font style="color:#000000;background-color:#fbde28;">语句增加了对</font>`<font style="color:#000000;background-color:#FBDE28;">long</font>`<font style="color:#000000;background-color:#fbde28;">类型的支持</font><font style="color:#000000;">。 </font>
+ <font style="color:#000000;"> </font>**<font style="color:#000000;">对于String类型</font>**<font style="color:#000000;">：在Java 7之前，</font>`<font style="color:#000000;">switch</font>`<font style="color:#000000;">不支持</font>`<font style="color:#000000;">String</font>`<font style="color:#000000;">类型。但是</font><font style="color:#000000;background-color:#fbde28;">自Java 7起，引入了新的特性，允许</font>`<font style="color:#000000;background-color:#FBDE28;">switch</font>`<font style="color:#000000;background-color:#fbde28;">语句使用</font>`<font style="color:#000000;background-color:#FBDE28;">String</font>`<font style="color:#000000;background-color:#fbde28;">类型的变量作为表达式</font><font style="color:#000000;">。这意味着你可以根据字符串内容来执行不同的分支逻辑。 </font>

<font style="color:#000000;">总结起来：</font>

+ <font style="color:#000000;">Java 1.8及以后版本：</font>`<font style="color:#000000;">switch</font>`<font style="color:#000000;">能作用于</font>`<font style="color:#000000;">byte</font>`<font style="color:#000000;">和</font>`<font style="color:#000000;">long</font>`<font style="color:#000000;">类型。</font>
+ <font style="color:#000000;">自Java 7开始：</font>`<font style="color:#000000;">switch</font>`<font style="color:#000000;">能作用于</font>`<font style="color:#000000;">String</font>`<font style="color:#000000;">类型。</font>

<font style="color:#000000;">但在实际编程中，建议尽量避免使用</font>`<font style="color:#000000;">switch(String)</font>`<font style="color:#000000;">，因为它的效率通常不如基于对象或映射表的方式高，尤其是在处理大量字符串时。</font>

## <font style="color:#000000;">63. </font><font style="color:rgb(63, 63, 63);">String s = new String("jay");创建了几个字符串对象?</font>
<font style="color:#000000;">在Java中，执行 </font>`<font style="color:#000000;">String s = new String("jay");</font>`<font style="color:#000000;"> 代码会创建两个字符串对象。</font>

1. <font style="color:#000000;">首先，在编译时，字面量 "jay" 会被存储到</font><font style="color:#000000;background-color:#fbde28;"> Java 字符串常量池</font><font style="color:#000000;">（String Pool）中，创建一个字符串对象。</font>
2. <font style="color:#000000;">然后，在运行时，</font><font style="color:#000000;background-color:#fbde28;">通过 </font>`<font style="color:#000000;background-color:#FBDE28;">new String("jay")</font>`<font style="color:#000000;background-color:#fbde28;"> 创建了一个新的字符串对象</font><font style="color:#000000;">，</font><font style="color:#000000;background-color:#fbde28;">并将其引用赋值给变量 </font>`<font style="color:#000000;background-color:#FBDE28;">s</font>`<font style="color:#000000;">。这个新对象的内容与字符串常量池中的 "jay" 相同，但它们是</font><font style="color:#000000;background-color:#fbde28;">不同的对象实例。</font>

<font style="color:#000000;">所以总共创建了两个字符串对象。一个是存储在字符串常量池中的对象，另一个是在堆内存中通过 </font>`<font style="color:#000000;">new</font>`<font style="color:#000000;"> 关键字创建的对象。</font>

## <font style="color:#000000;">64. this 和 super 关键字的作用 </font>
<font style="color:#000000;">this:</font>

+ <font style="color:#000000;">对象内部指代自身的引用</font>
+ <font style="color:#000000;">解决成员变量和局部变量同名问题</font>
+ <font style="color:#000000;">可以调用成员变量，不能调用局部变量</font>
+ <font style="color:#000000;">可以调用成员方法</font>
+ <font style="color:#000000;">在普通方法中可以省略 this</font>
+ <font style="color:#000000;">在静态方法当中不允许出现 this 关键字 </font>

<font style="color:#000000;">super:</font>

+ <font style="color:#000000;">调用父类的成员或者方法  </font>
+ <font style="color:#000000;">调用父类的构造函数</font>

## <font style="color:#000000;">65. 我们能将 int 强制转换为 byte 类型的变量吗?如果该 值大于 byte 类型的范围，将会出现什么现象?</font>
<font style="color:#000000;">在Java中，可以将 </font>`<font style="color:#000000;">int</font>`<font style="color:#000000;"> 类型的变量强制转换为 </font>`<font style="color:#000000;">byte</font>`<font style="color:#000000;"> 类型，但是当你尝试将一个超出 </font>`<font style="color:#000000;">byte</font>`<font style="color:#000000;"> 范围（-128到127）的 </font>`<font style="color:#000000;">int</font>`<font style="color:#000000;"> 值赋给 </font>`<font style="color:#000000;">byte</font>`<font style="color:#000000;"> 变量时，</font><font style="color:#000000;background-color:#fbde28;">将会发生数值溢出（numeric overflow）。</font>

<font style="color:#000000;">Java在进行这种隐式或显式的类型转换时，</font><font style="color:#000000;background-color:#fbde28;">会自动截断超出目标类型范围的部分。</font><font style="color:#000000;">对于正数，它会截断高阶位；对于负数，由于Java的有符号整数表示方式，高位被截断后可能会导致更小的负数。</font>

<font style="color:#000000;">例如：</font>

```java
int intValue = 200;
byte byteValue = (byte)intValue; // 此时，尽管intValue超过了byte的最大值，但Java会执行强制类型转换

System.out.println(byteValue); // 输出结果将是 -56，因为200对byte类型来说是溢出的，实际存储的是200模256的结果，即-56（按补码计算）
```

<font style="color:#000000;">总之，在进行此类转换时，请确保你知道可能发生的数值溢出，并根据需要采取适当的错误检查措施以防止数据丢失或不正确的结果。</font>

## 66. <font style="color:#000000;">float f=3.4;正确吗?</font>
<font style="color:#000000;">在Java中，声明并初始化一个 </font>`<font style="color:#000000;">float</font>`<font style="color:#000000;"> 类型变量时，如果直接赋值给浮点数，并且该数值后面</font><font style="color:#000000;background-color:#fbde28;">没有显式指定 </font>`<font style="color:#000000;background-color:#FBDE28;">f</font>`<font style="color:#000000;background-color:#fbde28;"> 或 </font>`<font style="color:#000000;background-color:#FBDE28;">F</font>`<font style="color:#000000;background-color:#fbde28;"> 作为后缀来表示</font><font style="color:#000000;">这是一个 </font>`<font style="color:#000000;">float</font>`<font style="color:#000000;"> 类型的数字，</font><font style="color:#000000;background-color:#fbde28;">编译器会报错</font><font style="color:#000000;">，因为默认情况下小数点表示的是 </font>`<font style="color:#000000;">double</font>`<font style="color:#000000;"> 类型。</font>

<font style="color:#000000;">因此，以下语句在Java中是</font>**<font style="color:#000000;">错误</font>**<font style="color:#000000;">的：</font>

```java
float f = 3.4; // 错误，因为3.4默认被解释为double类型
```

<font style="color:#000000;">为了正确声明一个 </font>`<font style="color:#000000;">float</font>`<font style="color:#000000;"> 类型变量并初始化为 </font>`<font style="color:#000000;">3.4</font>`<font style="color:#000000;">，你需要在数值后面添加 </font>`<font style="color:#000000;">f</font>`<font style="color:#000000;"> 或 </font>`<font style="color:#000000;">F</font>`<font style="color:#000000;"> 后缀：</font>

```java
float f = 3.4f; // 正确，通过后缀f明确表明这是float类型
```

<font style="color:#000000;">现在，这条语句会声明并初始化一个 </font>`<font style="color:#000000;">float</font>`<font style="color:#000000;"> 类型变量 </font>`<font style="color:#000000;">f</font>`<font style="color:#000000;">，其值为 </font>`<font style="color:#000000;">3.4</font>`<font style="color:#000000;">。</font>

## 67. <font style="color:#000000;">接口可否继承接口?抽象类是否可实现接口?抽象类是否可继承实体类?</font>
<font style="color:#000000;">接口可以继承接口，抽象类可以实现接口，并且抽象类也可以继承实体类（即非抽象类）。具体说明如下：</font>

1. <font style="color:#000000;"> </font>**<font style="color:#117cee;">接口继承接口</font>**<font style="color:#000000;">： </font><font style="color:#000000;">在Java中，一个接口是可以继承另一个或多个接口的。通过这种方式，</font><font style="color:#000000;background-color:#fbde28;">子接口将继承父接口的所有方法声明</font><font style="color:#000000;">。例如： </font>

```java
interface InterfaceA {
    void methodA();
}

interface InterfaceB extends InterfaceA {
    void methodB();
}
```

2. <font style="color:#000000;"> </font>**<font style="color:#117cee;">抽象类实现接口</font>**<font style="color:#000000;">： </font><font style="color:#000000;">抽象类不仅可以实现一个或多个接口，而且由于抽象类中可以包含抽象方法和具体方法，因此它能够提供</font><font style="color:#000000;background-color:#fbde28;">接口方法的具体实现</font><font style="color:#000000;">或者</font><font style="color:#000000;background-color:#fbde28;">新的抽象方法</font><font style="color:#000000;">。例如： </font>

```java
interface InterfaceC {
    void methodC();
}

abstract class AbstractClass implements InterfaceC {
    @Override
    public abstract void methodC();

    // 或者提供方法的具体实现
    public void anotherMethod() {
        // 具体实现代码...
    }
}
```

3. <font style="color:#000000;"> </font>**<font style="color:#117cee;">抽象类继承实体类</font>**<font style="color:#000000;">： </font><font style="color:#000000;">抽象类同样可以继承自实体类（非抽象类），并添加额外的抽象方法或者覆盖实体类中的方法。但是要注意，如果实体类</font><font style="color:#000000;background-color:#fbde28;">没有默认构造函数</font><font style="color:#000000;">（无参数构造器），那么抽象类</font><font style="color:#000000;background-color:#fbde28;">无法直接实例化</font><font style="color:#000000;">，同时在子类中需要调用超类构造函数时可能会受限。示例： </font>

```java
class EntityClass {
    public EntityClass(int value) {
        // 构造函数
    }

    // 其他实体类成员
}

abstract class AbstractChildClass extends EntityClass {
    public AbstractChildClass(int value) {
        super(value);
    }

    // 抽象方法或其他方法
}
```

<font style="color:#000000;">总结来说，在Java中，接口与接口之间可以多继承，抽象类既可以实现接口也可以继承实体类，但都遵循Java类和接口的基本规则和限制。</font>

## <font style="color:#000000;">68. Reader 和 InputStream 区别?</font>
`<font style="color:#000000;">java.io.Reader</font>`<font style="color:#000000;"> 和 </font>`<font style="color:#000000;">java.io.InputStream</font>`<font style="color:#000000;"> 都是 Java 中处理输入数据流的抽象类，但它们之间存在显著的区别：</font>

1. **处理的数据类型**<font style="color:#000000;">：</font>
    - `<font style="color:#000000;">InputStream</font>`<font style="color:#000000;"> 是字节输入流，它处理的是原始字节序列，适合读取二进制数据或者需要以字节为单位进行操作的数据，例如从文件、网络等读取未经解码的原始数据。</font>
    - `<font style="color:#000000;">Reader</font>`<font style="color:#000000;"> 是字符输入流，它处理的是经过解码的字符序列，主要用于文本数据的读取。在读取文本时，它会将字节流转换成与平台无关的 Unicode 字符。</font>
2. **编码和解码**<font style="color:#000000;">：</font>
    - `<font style="color:#000000;">InputStream</font>`<font style="color:#000000;"> 不负责字符编码或解码，它直接处理原始字节数据。</font>
    - `<font style="color:#000000;">Reader</font>`<font style="color:#000000;"> 通常在内部使用某种字符集（如 UTF-8 或 ISO-8859-1）来解码字节流，并提供字符流给上层应用。</font>
3. **方法和操作**<font style="color:#000000;">：</font>
    - `<font style="color:#000000;">InputStream</font>`<font style="color:#000000;"> 提供的方法主要是基于字节的操作，如 </font>`<font style="color:#000000;">read()</font>`<font style="color:#000000;"> 方法返回的是一个字节。</font>
    - `<font style="color:#000000;">Reader</font>`<font style="color:#000000;"> 提供的方法基于字符，如 </font>`<font style="color:#000000;">read()</font>`<font style="color:#000000;"> 方法返回的是一个 Unicode 字符（通常是 </font>`<font style="color:#000000;">int</font>`<font style="color:#000000;"> 类型，范围在 0 到 65535 之间，代表一个 Unicode 码点）。</font>
4. **常见子类**<font style="color:#000000;">：</font>
    - <font style="color:#000000;">常见的 </font>`<font style="color:#000000;">InputStream</font>`<font style="color:#000000;"> 子类有 </font>`<font style="color:#000000;">FileInputStream</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">BufferedInputStream</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">ByteArrayInputStream</font>`<font style="color:#000000;"> 等。</font>
    - <font style="color:#000000;">常见的 </font>`<font style="color:#000000;">Reader</font>`<font style="color:#000000;"> 子类有 </font>`<font style="color:#000000;">FileReader</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">BufferedReader</font>`<font style="color:#000000;">、</font>`<font style="color:#000000;">CharArrayReader</font>`<font style="color:#000000;"> 等。</font>

<font style="color:#000000;">当需要读取文本文件时，通常会选择 </font>`<font style="color:#000000;">Reader</font>`<font style="color:#000000;"> 类及其子类，因为它们能够自动处理字符编码问题，保证读取到正确的文本内容；而处理二进制数据或原始字节流时，则选择 </font>`<font style="color:#000000;">InputStream</font>`<font style="color:#000000;"> 及其子类。如果需要从字节流转换到字符流，可以使用 </font>`<font style="color:#000000;">InputStreamReader</font>`<font style="color:#000000;"> 类作为桥梁，该类构造函数接受一个 </font>`<font style="color:#000000;">InputStream</font>`<font style="color:#000000;"> 对象并指定字符集，从而创建一个能够读取解码后字符的 </font>`<font style="color:#000000;">Reader</font>`<font style="color:#000000;">。</font>

## <font style="color:#000000;">69. 列举出 JAVA 中 6 个比较常用的包</font>
+ <font style="color:#000000;">以下是Java中六个比较常用的包：</font>
1. <font style="color:#000000;"> </font>**<font style="color:#117cee;">java.lang</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">这是</font><font style="color:#000000;background-color:#fbde28;">Java语言的核心包，系统自动导入</font><font style="color:#000000;">，包含所有基本数据类型、String类、Math类、System类、Object类等核心API，以及异常类（如NullPointerException、ClassNotFoundException等）、线程相关的接口和类（如Runnable、Thread）。</font>
2. <font style="color:#000000;"> </font>**<font style="color:#117cee;">java.util</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;background-color:#fbde28;">提供了大量工具类和集合框架</font><font style="color:#000000;">，包括List、Set、Map等各种容器接口及其实现类（ArrayList、LinkedList、HashSet、HashMap等），日期/时间处理类（Date、Calendar、LocalDate、LocalTime、LocalDateTime等），随机数生成器（Random），以及其他实用工具类（如Arrays、Collections、Formatter、Scanner等）。</font>
3. <font style="color:#000000;"> </font>**<font style="color:#117cee;">java.io</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;background-color:#fbde28;">提供了与文件、网络流及内存流操作相关的类</font><font style="color:#000000;">，用于进行输入输出操作，如FileInputStream、FileOutputStream、BufferedReader、PrintWriter、DataInputStream、ObjectInputStream/ObjectOutputStream等。</font>
4. <font style="color:#000000;"> </font>**<font style="color:#117cee;">java.net</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;background-color:#fbde28;">包含网络编程相关类</font><font style="color:#000000;">，比如URL、URLConnection、Socket、ServerSocket等，用于客户端和服务端的网络通信。</font>
5. <font style="color:#000000;"> </font>**<font style="color:#117cee;">java.sql</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;background-color:#fbde28;">提供了访问数据库的API，包含了与JDBC（Java Database Connectivity）相关的接口和类</font><font style="color:#000000;">，如Connection、Statement、PreparedStatement、ResultSet等。</font>
6. <font style="color:#117cee;"> </font>**<font style="color:#117cee;">javax.servlet</font>**<font style="color:#000000;"> 和 </font>**<font style="color:#117cee;">javax.servlet.http</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;background-color:#fbde28;">Web应用开发中的重要包，提供了Servlet API</font><font style="color:#000000;">，包含HttpServlet、ServletRequest、ServletResponse等类，用于创建Web服务器端的应用程序组件。</font>
7. <font style="color:#117cee;"> </font>**<font style="color:#117cee;">java.nio</font>**<font style="color:#000000;"> 和 </font>**<font style="color:#117cee;">java.nio.channels</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;background-color:#fbde28;">用于非阻塞（Non-blocking）I/O操作，提供了一种更高效的I/O模型</font><font style="color:#000000;">。其中包含了ByteBuffers、Selectors、Channels等用于高效地处理数据流的类和接口。</font>
8. <font style="color:#117cee;"> </font>**<font style="color:#117cee;">java.awt</font>**<font style="color:#000000;"> 和 </font>**<font style="color:#117cee;">javax.swing</font>**<font style="color:#000000;">： </font>
    - <font style="color:#000000;">GUI编程相关的包，awt提供了基础的图形用户界面构件，而Swing是基于AWT之上构建的一个更加高级和灵活的GUI工具包，包含了各种窗口部件（如JFrame、JButton、JLabel等）。</font>

<font style="color:#000000;">这些包涵盖了Java开发中的基础功能到进阶特性，每个包都有其特定用途和应用场景。</font>

## <font style="color:#000000;">70. JDK 7 有哪些新特性</font>
+ <font style="color:#000000;">Try-with-resource 语句</font>
+ <font style="color:#000000;">NIO2 文件处理 Files</font>
+ <font style="color:#000000;">switch 可以支持字符串判断条件</font>
+ <font style="color:#000000;">泛型推导</font>
+ <font style="color:#000000;">多异常统一处理</font>





> 更新: 2024-01-01 17:36:00  
原文: [https://www.yuque.com/vip6688/neho4x/xwfvwobnw5f6lwry](https://www.yuque.com/vip6688/neho4x/xwfvwobnw5f6lwry)
>



> 更新: 2024-11-25 09:55:35  
> 原文: <https://www.yuque.com/neumx/laxg2e/882f500f20271ac1381538fdbd6dcdb1>