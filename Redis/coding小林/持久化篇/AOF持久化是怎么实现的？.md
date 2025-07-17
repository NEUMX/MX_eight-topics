# AOF持久化是怎么实现的？

# AOF 持久化是怎么实现的？
![1732497706894-423bf375-85c2-48e7-bf87-f56b6d4e7d6c.png](./img/jdF0GXKUM9iohiOg/1732497706894-423bf375-85c2-48e7-bf87-f56b6d4e7d6c-542517.png)

## [](https://xiaolincoding.com/redis/storage/aof.html#aof-%E6%97%A5%E5%BF%97)<font style="color:rgb(44, 62, 80);">AOF 日志</font>
<font style="color:rgb(44, 62, 80);">试想一下，如果 Redis 每执行一条写操作命令，就把该命令以追加的方式写入到一个文件里，然后重启 Redis 的时候，先去读取这个文件里的命令，并且执行它，这不就相当于恢复了缓存数据了吗？</font>

![1732497707015-5472dbd2-a859-4879-862c-262d76c2cbc6.png](./img/jdF0GXKUM9iohiOg/1732497707015-5472dbd2-a859-4879-862c-262d76c2cbc6-026300.png)

<font style="color:rgb(44, 62, 80);">这种保存写操作命令到日志的持久化方式，就是 Redis 里的</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(48, 79, 254);">AOF(</font>**_**<font style="color:rgb(200, 73, 255);">Append Only File</font>**_**<font style="color:rgb(48, 79, 254);">)</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">持久化功能，</font>**<font style="color:rgb(48, 79, 254);">注意只会记录写操作命令，读操作命令是不会被记录的</font>**<font style="color:rgb(44, 62, 80);">，因为没意义。</font>

<font style="color:rgb(44, 62, 80);">在 Redis 中 AOF 持久化功能默认是不开启的，需要我们修改</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">redis.conf</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">配置文件中的以下参数：</font>

![1732497707100-471a64be-8d98-4007-841d-2cb7b01e6821.png](./img/jdF0GXKUM9iohiOg/1732497707100-471a64be-8d98-4007-841d-2cb7b01e6821-125067.png)

<font style="color:rgb(44, 62, 80);">AOF 日志文件其实就是普通的文本，我们可以通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">cat</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">命令查看里面的内容，不过里面的内容如果不知道一定的规则的话，可能会看不懂。</font>

<font style="color:rgb(44, 62, 80);">我这里以「</font>_<font style="color:rgb(200, 73, 255);">set name xiaolin</font>_<font style="color:rgb(44, 62, 80);">」命令作为例子，Redis 执行了这条命令后，记录在 AOF 日志里的内容如下图：</font>

![1732497707225-ae20c120-38b7-4006-94a8-b5126265f163.png](./img/jdF0GXKUM9iohiOg/1732497707225-ae20c120-38b7-4006-94a8-b5126265f163-141165.png)

<font style="color:rgb(44, 62, 80);">我这里给大家解释下。</font>

<font style="color:rgb(44, 62, 80);">「</font><font style="color:rgb(71, 101, 130);">*3</font><font style="color:rgb(44, 62, 80);">」表示当前命令有三个部分，每部分都是以「</font>$ +数字</font><font style="color:rgb(44, 62, 80);">」开头，后面紧跟着具体的命令、键或值。然后，这里的「</font><font style="color:rgb(71, 101, 130);">数字</font><font style="color:rgb(44, 62, 80);">」表示这部分中的命令、键或值一共有多少字节。例如，「</font><font style="color:rgb(71, 101, 130);"> $<font style="color:rgb(71, 101, 130);">3 set</font><font style="color:rgb(44, 62, 80);">」表示这部分有 3 个字节，也就是「</font><font style="color:rgb(71, 101, 130);">set</font><font style="color:rgb(44, 62, 80);">」命令这个字符串的长度。</font>

<font style="color:rgb(44, 62, 80);">不知道大家注意到没有，Redis 是先执行写操作命令后，才将该命令记录到 AOF 日志里的，这么做其实有两个好处。</font>

<font style="color:rgb(44, 62, 80);">第一个好处，</font>**<font style="color:rgb(48, 79, 254);">避免额外的检查开销。</font>**

<font style="color:rgb(44, 62, 80);">因为如果先将写操作命令记录到 AOF 日志里，再执行该命令的话，如果当前的命令语法有问题，那么如果不进行命令语法检查，该错误的命令记录到 AOF 日志里后，Redis 在使用日志恢复数据时，就可能会出错。</font>

<font style="color:rgb(44, 62, 80);">而如果先执行写操作命令再记录日志的话，只有在该命令执行成功后，才将命令记录到 AOF 日志里，这样就不用额外的检查开销，保证记录在 AOF 日志里的命令都是可执行并且正确的。</font>

<font style="color:rgb(44, 62, 80);">第二个好处，</font>**<font style="color:rgb(48, 79, 254);">不会阻塞当前写操作命令的执行</font>**<font style="color:rgb(44, 62, 80);">，因为当写操作命令执行成功后，才会将命令记录到 AOF 日志。</font>

<font style="color:rgb(44, 62, 80);">当然，AOF 持久化功能也不是没有潜在风险。</font>

<font style="color:rgb(44, 62, 80);">第一个风险，执行写操作命令和记录日志是两个过程，那当 Redis 在还没来得及将命令写入到硬盘时，服务器发生宕机了，这个数据就会有</font>**<font style="color:rgb(48, 79, 254);">丢失的风险</font>**<font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">第二个风险，前面说道，由于写操作命令执行成功后才记录到 AOF 日志，所以不会阻塞当前写操作命令的执行，但是</font>**<font style="color:rgb(48, 79, 254);">可能会给「下一个」命令带来阻塞风险</font>**<font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">因为将命令写入到日志的这个操作也是在主进程完成的（执行命令也是在主进程），也就是说这两个操作是同步的。</font>

![1732497707693-95b0d11b-d747-4b3d-a658-23e82ef6d48a.png](./img/jdF0GXKUM9iohiOg/1732497707693-95b0d11b-d747-4b3d-a658-23e82ef6d48a-316609.png)

<font style="color:rgb(44, 62, 80);">如果在将日志内容写入到硬盘时，服务器的硬盘的 I/O 压力太大，就会导致写硬盘的速度很慢，进而阻塞住了，也就会导致后续的命令无法执行。</font>

<font style="color:rgb(44, 62, 80);">认真分析一下，其实这两个风险都有一个共性，都跟「 AOF 日志写回硬盘的时机」有关。</font>

## [](https://xiaolincoding.com/redis/storage/aof.html#%E4%B8%89%E7%A7%8D%E5%86%99%E5%9B%9E%E7%AD%96%E7%95%A5)<font style="color:rgb(44, 62, 80);">三种写回策略</font>
<font style="color:rgb(44, 62, 80);">Redis 写入 AOF 日志的过程，如下图：</font>

![1732497707791-029af939-22f0-4b2c-9d5b-b588f9d87fbf.png](./img/jdF0GXKUM9iohiOg/1732497707791-029af939-22f0-4b2c-9d5b-b588f9d87fbf-275722.png)

<font style="color:rgb(44, 62, 80);">我先来具体说说：</font>

1. <font style="color:rgb(44, 62, 80);">Redis 执行完写操作命令后，会将命令追加到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">server.aof_buf</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">缓冲区；</font>
2. <font style="color:rgb(44, 62, 80);">然后通过 write() 系统调用，将 aof_buf 缓冲区的数据写入到 AOF 文件，此时数据并没有写入到硬盘，而是拷贝到了内核缓冲区 page cache，等待内核将数据写入硬盘；</font>
3. <font style="color:rgb(44, 62, 80);">具体内核缓冲区的数据什么时候写入到硬盘，由内核决定。</font>

<font style="color:rgb(44, 62, 80);">Redis 提供了 3 种写回硬盘的策略，控制的就是上面说的第三步的过程。</font>

<font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">redis.conf</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">配置文件中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">appendfsync</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">配置项可以有以下 3 种参数可填：</font>

+ **<font style="color:rgb(48, 79, 254);">Always</font>**<font style="color:rgb(44, 62, 80);">，这个单词的意思是「总是」，所以它的意思是每次写操作命令执行完后，同步将 AOF 日志数据写回硬盘；</font>
+ **<font style="color:rgb(48, 79, 254);">Everysec</font>**<font style="color:rgb(44, 62, 80);">，这个单词的意思是「每秒」，所以它的意思是每次写操作命令执行完后，先将命令写入到 AOF 文件的内核缓冲区，然后每隔一秒将缓冲区里的内容写回到硬盘；</font>
+ **<font style="color:rgb(48, 79, 254);">No</font>**<font style="color:rgb(44, 62, 80);">，意味着不由 Redis 控制写回硬盘的时机，转交给操作系统控制写回的时机，也就是每次写操作命令执行完后，先将命令写入到 AOF 文件的内核缓冲区，再由操作系统决定何时将缓冲区内容写回硬盘。</font>

<font style="color:rgb(44, 62, 80);">这 3 种写回策略都无法能完美解决「主进程阻塞」和「减少数据丢失」的问题，因为两个问题是对立的，偏向于一边的话，就会要牺牲另外一边，原因如下：</font>

+ <font style="color:rgb(44, 62, 80);">Always 策略的话，可以最大程度保证数据不丢失，但是由于它每执行一条写操作命令就同步将 AOF 内容写回硬盘，所以是不可避免会影响主进程的性能；</font>
+ <font style="color:rgb(44, 62, 80);">No 策略的话，是交由操作系统来决定何时将 AOF 日志内容写回硬盘，相比于 Always 策略性能较好，但是操作系统写回硬盘的时机是不可预知的，如果 AOF 日志内容没有写回硬盘，一旦服务器宕机，就会丢失不定数量的数据。</font>
+ <font style="color:rgb(44, 62, 80);">Everysec 策略的话，是折中的一种方式，避免了 Always 策略的性能开销，也比 No 策略更能避免数据丢失，当然如果上一秒的写操作命令日志没有写回到硬盘，发生了宕机，这一秒内的数据自然也会丢失。</font>

<font style="color:rgb(44, 62, 80);">大家根据自己的业务场景进行选择：</font>

+ <font style="color:rgb(44, 62, 80);">如果要高性能，就选择 No 策略；</font>
+ <font style="color:rgb(44, 62, 80);">如果要高可靠，就选择 Always 策略；</font>
+ <font style="color:rgb(44, 62, 80);">如果允许数据丢失一点，但又想性能高，就选择 Everysec 策略。</font>

<font style="color:rgb(44, 62, 80);">我也把这 3 个写回策略的优缺点总结成了一张表格：</font>

![1732497707869-0bd22a02-7487-4d2c-a35d-65f5021530b0.png](./img/jdF0GXKUM9iohiOg/1732497707869-0bd22a02-7487-4d2c-a35d-65f5021530b0-967034.png)

<font style="color:rgb(44, 62, 80);">大家知道这三种策略是怎么实现的吗？</font>

<font style="color:rgb(44, 62, 80);">深入到源码后，你就会发现这三种策略只是在控制</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">fsync()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数的调用时机。</font>

<font style="color:rgb(44, 62, 80);">当应用程序向文件写入数据时，内核通常先将数据复制到内核缓冲区中，然后排入队列，然后由内核决定何时写入硬盘。</font>

![1732497707939-92086277-88b8-47d4-a924-58f3ff4a2e67.png](./img/jdF0GXKUM9iohiOg/1732497707939-92086277-88b8-47d4-a924-58f3ff4a2e67-641104.png)

<font style="color:rgb(44, 62, 80);">如果想要应用程序向文件写入数据后，能立马将数据同步到硬盘，就可以调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">fsync()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，这样内核就会将内核缓冲区的数据直接写入到硬盘，等到硬盘写操作完成后，该函数才会返回。</font>

+ <font style="color:rgb(44, 62, 80);">Always 策略就是每次写入 AOF 文件数据后，就执行 fsync() 函数；</font>
+ <font style="color:rgb(44, 62, 80);">Everysec 策略就会创建一个异步任务来执行 fsync() 函数；</font>
+ <font style="color:rgb(44, 62, 80);">No 策略就是永不执行 fsync() 函数;</font>

## [](https://xiaolincoding.com/redis/storage/aof.html#aof-%E9%87%8D%E5%86%99%E6%9C%BA%E5%88%B6)<font style="color:rgb(44, 62, 80);">AOF 重写机制</font>
<font style="color:rgb(44, 62, 80);">AOF 日志是一个文件，随着执行的写操作命令越来越多，文件的大小会越来越大。</font>

<font style="color:rgb(44, 62, 80);">如果当 AOF 日志文件过大就会带来性能问题，比如重启 Redis 后，需要读 AOF 文件的内容以恢复数据，如果文件过大，整个恢复的过程就会很慢。</font>

<font style="color:rgb(44, 62, 80);">所以，Redis 为了避免 AOF 文件越写越大，提供了</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(48, 79, 254);">AOF 重写机制</font>**<font style="color:rgb(44, 62, 80);">，当 AOF 文件的大小超过所设定的阈值后，Redis 就会启用 AOF 重写机制，来压缩 AOF 文件。</font>

<font style="color:rgb(44, 62, 80);">AOF 重写机制是在重写时，读取当前数据库中的所有键值对，然后将每一个键值对用一条命令记录到「新的 AOF 文件」，等到全部记录完后，就将新的 AOF 文件替换掉现有的 AOF 文件。</font>

<font style="color:rgb(44, 62, 80);">举个例子，在没有使用重写机制前，假设前后执行了「</font>_<font style="color:rgb(200, 73, 255);">set name xiaolin</font>_<font style="color:rgb(44, 62, 80);">」和「</font>_<font style="color:rgb(200, 73, 255);">set name xiaolincoding</font>_<font style="color:rgb(44, 62, 80);">」这两个命令的话，就会将这两个命令记录到 AOF 文件。</font>

![1732497708019-7637804f-cb3c-4557-b970-f1097a7882dc.png](./img/jdF0GXKUM9iohiOg/1732497708019-7637804f-cb3c-4557-b970-f1097a7882dc-813041.png)

<font style="color:rgb(44, 62, 80);">但是</font>**<font style="color:rgb(48, 79, 254);">在使用重写机制后，就会读取 name 最新的 value（键值对） ，然后用一条 「set name xiaolincoding」命令记录到新的 AOF 文件</font>**<font style="color:rgb(44, 62, 80);">，之前的第一个命令就没有必要记录了，因为它属于「历史」命令，没有作用了。这样一来，一个键值对在重写日志中只用一条命令就行了。</font>

<font style="color:rgb(44, 62, 80);">重写工作完成后，就会将新的 AOF 文件覆盖现有的 AOF 文件，这就相当于压缩了 AOF 文件，使得 AOF 文件体积变小了。</font>

<font style="color:rgb(44, 62, 80);">然后，在通过 AOF 日志恢复数据时，只用执行这条命令，就可以直接完成这个键值对的写入了。</font>

<font style="color:rgb(44, 62, 80);">所以，重写机制的妙处在于，尽管某个键值对被多条写命令反复修改，</font>**<font style="color:rgb(48, 79, 254);">最终也只需要根据这个「键值对」当前的最新状态，然后用一条命令去记录键值对</font>**<font style="color:rgb(44, 62, 80);">，代替之前记录这个键值对的多条命令，这样就减少了 AOF 文件中的命令数量。最后在重写工作完成后，将新的 AOF 文件覆盖现有的 AOF 文件。</font>

<font style="color:rgb(44, 62, 80);">这里说一下为什么重写 AOF 的时候，不直接复用现有的 AOF 文件，而是先写到新的 AOF 文件再覆盖过去。</font>

<font style="color:rgb(44, 62, 80);">因为</font>**<font style="color:rgb(48, 79, 254);">如果 AOF 重写过程中失败了，现有的 AOF 文件就会造成污染</font>**<font style="color:rgb(44, 62, 80);">，可能无法用于恢复使用。</font>

<font style="color:rgb(44, 62, 80);">所以 AOF 重写过程，先重写到新的 AOF 文件，重写失败的话，就直接删除这个文件就好，不会对现有的 AOF 文件造成影响。</font>

## [](https://xiaolincoding.com/redis/storage/aof.html#aof-%E5%90%8E%E5%8F%B0%E9%87%8D%E5%86%99)<font style="color:rgb(44, 62, 80);">AOF 后台重写</font>
<font style="color:rgb(44, 62, 80);">写入 AOF 日志的操作虽然是在主进程完成的，因为它写入的内容不多，所以一般不太影响命令的操作。</font>

<font style="color:rgb(44, 62, 80);">但是在触发 AOF 重写时，比如当 AOF 文件大于 64M 时，就会对 AOF 文件进行重写，这时是需要读取所有缓存的键值对数据，并为每个键值对生成一条命令，然后将其写入到新的 AOF 文件，重写完后，就把现在的 AOF 文件替换掉。</font>

<font style="color:rgb(44, 62, 80);">这个过程其实是很耗时的，所以重写的操作不能放在主进程里。</font>

<font style="color:rgb(44, 62, 80);">所以，Redis 的</font>**<font style="color:rgb(48, 79, 254);">重写 AOF 过程是由后台子进程</font>************<font style="color:rgb(48, 79, 254);"> </font>**_**<font style="color:rgb(200, 73, 255);">bgrewriteaof</font>**_**<font style="color:rgb(48, 79, 254);"> </font>************<font style="color:rgb(48, 79, 254);">来完成的</font>**<font style="color:rgb(44, 62, 80);">，这么做可以达到两个好处：</font>

+ <font style="color:rgb(44, 62, 80);">子进程进行 AOF 重写期间，主进程可以继续处理命令请求，从而避免阻塞主进程；</font>
+ <font style="color:rgb(44, 62, 80);">子进程带有主进程的数据副本（</font>_<font style="color:rgb(200, 73, 255);">数据副本怎么产生的后面会说</font>_<font style="color:rgb(44, 62, 80);">），这里使用子进程而不是线程，因为如果是使用线程，多线程之间会共享内存，那么在修改共享内存数据的时候，需要通过加锁来保证数据的安全，而这样就会降低性能。而使用子进程，创建子进程时，父子进程是共享内存数据的，不过这个共享的内存只能以只读的方式，而当父子进程任意一方修改了该共享内存，就会发生「写时复制」，于是父子进程就有了独立的数据副本，就不用加锁来保证数据安全。</font>

<font style="color:rgb(44, 62, 80);">子进程是怎么拥有主进程一样的数据副本的呢？</font>

<font style="color:rgb(44, 62, 80);">主进程在通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">fork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">系统调用生成 bgrewriteaof 子进程时，操作系统会把主进程的「</font>**<font style="color:rgb(48, 79, 254);">页表</font>**<font style="color:rgb(44, 62, 80);">」复制一份给子进程，这个页表记录着虚拟地址和物理地址映射关系，而不会复制物理内存，也就是说，两者的虚拟空间不同，但其对应的物理空间是同一个。</font>

![1732497708106-6cdcd08a-5750-4a18-9843-d08f8a63ef8d.png](./img/jdF0GXKUM9iohiOg/1732497708106-6cdcd08a-5750-4a18-9843-d08f8a63ef8d-822483.png)

<font style="color:rgb(44, 62, 80);">这样一来，子进程就共享了父进程的物理内存数据了，这样能够</font>**<font style="color:rgb(48, 79, 254);">节约物理内存资源</font>**<font style="color:rgb(44, 62, 80);">，页表对应的页表项的属性会标记该物理内存的权限为</font>**<font style="color:rgb(48, 79, 254);">只读</font>**<font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">不过，当父进程或者子进程在向这个内存发起写操作时，CPU 就会触发</font>**<font style="color:rgb(48, 79, 254);">写保护中断</font>**<font style="color:rgb(44, 62, 80);">，这个写保护中断是由于违反权限导致的，然后操作系统会在「写保护中断处理函数」里进行</font>**<font style="color:rgb(48, 79, 254);">物理内存的复制</font>**<font style="color:rgb(44, 62, 80);">，并重新设置其内存映射关系，将父子进程的内存读写权限设置为</font>**<font style="color:rgb(48, 79, 254);">可读写</font>**<font style="color:rgb(44, 62, 80);">，最后才会对内存进行写操作，这个过程被称为「</font>**<font style="color:rgb(48, 79, 254);">写时复制(</font>**_**<font style="color:rgb(200, 73, 255);">Copy On Write</font>**_**<font style="color:rgb(48, 79, 254);">)</font>**<font style="color:rgb(44, 62, 80);">」。</font>

![1732497708178-f82ad3ac-3fcc-4588-99cc-c47f6e5c5692.png](./img/jdF0GXKUM9iohiOg/1732497708178-f82ad3ac-3fcc-4588-99cc-c47f6e5c5692-810879.png)

<font style="color:rgb(44, 62, 80);">写时复制顾名思义，</font>**<font style="color:rgb(48, 79, 254);">在发生写操作的时候，操作系统才会去复制物理内存</font>**<font style="color:rgb(44, 62, 80);">，这样是为了防止 fork 创建子进程时，由于物理内存数据的复制时间过长而导致父进程长时间阻塞的问题。</font>

<font style="color:rgb(44, 62, 80);">当然，操作系统复制父进程页表的时候，父进程也是阻塞中的，不过页表的大小相比实际的物理内存小很多，所以通常复制页表的过程是比较快的。</font>

<font style="color:rgb(44, 62, 80);">不过，如果父进程的内存数据非常大，那自然页表也会很大，这时父进程在通过 fork 创建子进程的时候，阻塞的时间也越久。</font>

<font style="color:rgb(44, 62, 80);">所以，有两个阶段会导致阻塞父进程：</font>

+ <font style="color:rgb(44, 62, 80);">创建子进程的途中，由于要复制父进程的页表等数据结构，阻塞的时间跟页表的大小有关，页表越大，阻塞的时间也越长；</font>
+ <font style="color:rgb(44, 62, 80);">创建完子进程后，如果子进程或者父进程修改了共享数据，就会发生写时复制，这期间会拷贝物理内存，如果内存越大，自然阻塞的时间也越长；</font>

<font style="color:rgb(44, 62, 80);">触发重写机制后，主进程就会创建重写 AOF 的子进程，此时父子进程共享物理内存，重写子进程只会对这个内存进行只读，重写 AOF 子进程会读取数据库里的所有数据，并逐一把内存数据的键值对转换成一条命令，再将命令记录到重写日志（新的 AOF 文件）。</font>

<font style="color:rgb(44, 62, 80);">但是子进程重写过程中，主进程依然可以正常处理命令。</font>

<font style="color:rgb(44, 62, 80);">如果此时</font>**<font style="color:rgb(48, 79, 254);">主进程修改了已经存在 key-value，就会发生写时复制，注意这里只会复制主进程修改的物理内存数据，没修改物理内存还是与子进程共享的</font>**<font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">所以如果这个阶段修改的是一个 bigkey，也就是数据量比较大的 key-value 的时候，这时复制的物理内存数据的过程就会比较耗时，有阻塞主进程的风险。</font>

<font style="color:rgb(44, 62, 80);">还有个问题，重写 AOF 日志过程中，如果主进程修改了已经存在 key-value，此时这个 key-value 数据在子进程的内存数据就跟主进程的内存数据不一致了，这时要怎么办呢？</font>

<font style="color:rgb(44, 62, 80);">为了解决这种数据不一致问题，Redis 设置了一个</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(48, 79, 254);">AOF 重写缓冲区</font>**<font style="color:rgb(44, 62, 80);">，这个缓冲区在创建 bgrewriteaof 子进程之后开始使用。</font>

<font style="color:rgb(44, 62, 80);">在重写 AOF 期间，当 Redis 执行完一个写命令之后，它会</font>**<font style="color:rgb(48, 79, 254);">同时将这个写命令写入到 「AOF 缓冲区」和 「AOF 重写缓冲区」</font>**<font style="color:rgb(44, 62, 80);">。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/45178513/1732497708291-e937e978-03af-411b-9804-2a56ce356336.png)

<font style="color:rgb(44, 62, 80);">也就是说，在 bgrewriteaof 子进程执行 AOF 重写期间，主进程需要执行以下三个工作:</font>

+ <font style="color:rgb(44, 62, 80);">执行客户端发来的命令；</font>
+ <font style="color:rgb(44, 62, 80);">将执行后的写命令追加到 「AOF 缓冲区」；</font>
+ <font style="color:rgb(44, 62, 80);">将执行后的写命令追加到 「AOF 重写缓冲区」；</font>

<font style="color:rgb(44, 62, 80);">当子进程完成 AOF 重写工作（</font>_<font style="color:rgb(200, 73, 255);">扫描数据库中所有数据，逐一把内存数据的键值对转换成一条命令，再将命令记录到重写日志</font>_<font style="color:rgb(44, 62, 80);">）后，会向主进程发送一条信号，信号是进程间通讯的一种方式，且是异步的。</font>

<font style="color:rgb(44, 62, 80);">主进程收到该信号后，会调用一个信号处理函数，该函数主要做以下工作：</font>

+ <font style="color:rgb(44, 62, 80);">将 AOF 重写缓冲区中的所有内容追加到新的 AOF 的文件中，使得新旧两个 AOF 文件所保存的数据库状态一致；</font>
+ <font style="color:rgb(44, 62, 80);">新的 AOF 的文件进行改名，覆盖现有的 AOF 文件。</font>

<font style="color:rgb(44, 62, 80);">信号函数执行完后，主进程就可以继续像往常一样处理命令了。</font>

<font style="color:rgb(44, 62, 80);">在整个 AOF 后台重写过程中，除了发生写时复制会对主进程造成阻塞，还有信号处理函数执行时也会对主进程造成阻塞，在其他时候，AOF 后台重写都不会阻塞主进程。</font>

## [](https://xiaolincoding.com/redis/storage/aof.html#%E6%80%BB%E7%BB%93)<font style="color:rgb(44, 62, 80);">总结</font>
<font style="color:rgb(44, 62, 80);">这次小林给大家介绍了 Redis 持久化技术中的 AOF 方法，这个方法是每执行一条写操作命令，就将该命令以追加的方式写入到 AOF 文件，然后在恢复时，以逐一执行命令的方式来进行数据恢复。</font>

<font style="color:rgb(44, 62, 80);">Redis 提供了三种将 AOF 日志写回硬盘的策略，分别是 Always、Everysec 和 No，这三种策略在可靠性上是从高到低，而在性能上则是从低到高。</font>

<font style="color:rgb(44, 62, 80);">随着执行的命令越多，AOF 文件的体积自然也会越来越大，为了避免日志文件过大， Redis 提供了 AOF 重写机制，它会直接扫描数据中所有的键值对数据，然后为每一个键值对生成一条写操作命令，接着将该命令写入到新的 AOF 文件，重写完成后，就替换掉现有的 AOF 日志。重写的过程是由后台子进程完成的，这样可以使得主进程可以继续正常处理命令。</font>

<font style="color:rgb(44, 62, 80);">用 AOF 日志的方式来恢复数据其实是很慢的，因为 Redis 执行命令由单线程负责的，而 AOF 日志恢复数据的方式是顺序执行日志里的每一条命令，如果 AOF 日志很大，这个「重放」的过程就会很慢了。</font>

---

<font style="color:rgb(44, 62, 80);">参考资料</font>

+ <font style="color:rgb(44, 62, 80);">《Redis设计与实现》</font>
+ <font style="color:rgb(44, 62, 80);">《Redis核心技术与实战-极客时间》</font>
+ <font style="color:rgb(44, 62, 80);">《Redis源码分析》</font>

---

![1732497708376-c0ebcaa7-33db-4d66-90ba-1c05c11791df.png](./img/jdF0GXKUM9iohiOg/1732497708376-c0ebcaa7-33db-4d66-90ba-1c05c11791df-009888.png)



> 更新: 2024-04-08 17:44:18  
原文: [https://www.yuque.com/vip6688/neho4x/klavgz08nr4qtddr](https://www.yuque.com/vip6688/neho4x/klavgz08nr4qtddr)
>



> 更新: 2024-11-25 16:07:05  
> 原文: <https://www.yuque.com/neumx/laxg2e/f8fa7fcafe909d2d9b6194e2a1bee46d>