# MySQL可重复读隔离级别，完全解决幻读了吗？

# MySQL 可重复读隔离级别，完全解决幻读了吗？
<font style="color:rgb(44, 62, 80);">MySQL InnoDB 引擎的默认隔离级别虽然是「可重复读」，但是它很大程度上避免幻读现象（并不是完全解决了），解决的方案有两种：</font>

+ <font style="color:rgb(44, 62, 80);">针对</font>**<font style="color:rgb(48, 79, 254);">快照读</font>**<font style="color:rgb(44, 62, 80);">（普通 select 语句），是</font>**<font style="color:rgb(48, 79, 254);">通过 MVCC 方式解决了幻读</font>**<font style="color:rgb(44, 62, 80);">，因为可重复读隔离级别下，事务执行过程中看到的数据，一直跟这个事务启动时看到的数据是一致的，即使中途有其他事务插入了一条数据，是查询不出来这条数据的，所以就很好了避免幻读问题。</font>
+ <font style="color:rgb(44, 62, 80);">针对</font>**<font style="color:rgb(48, 79, 254);">当前读</font>**<font style="color:rgb(44, 62, 80);">（select ... for update 等语句），是</font>**<font style="color:rgb(48, 79, 254);">通过 next-key lock（记录锁+间隙锁）方式解决了幻读</font>**<font style="color:rgb(44, 62, 80);">，因为当执行 select ... for update 语句的时候，会加上 next-key lock，如果有其他事务在 next-key lock 锁范围内插入了一条记录，那么这个插入语句就会被阻塞，无法成功插入，所以就很好了避免幻读问题。</font>

<font style="color:rgb(44, 62, 80);">这两个解决方案是很大程度上解决了幻读现象，但是还是有个别的情况造成的幻读现象是无法解决的。</font>

<font style="color:rgb(44, 62, 80);">这次，就跟大家好好聊这个问题。</font>

## [](https://xiaolincoding.com/mysql/transaction/phantom.html#%E4%BB%80%E4%B9%88%E6%98%AF%E5%B9%BB%E8%AF%BB)<font style="color:rgb(44, 62, 80);">什么是幻读？</font>
<font style="color:rgb(44, 62, 80);">首先来看看 MySQL 文档是怎么定义幻读（Phantom Read）的:</font>

_**<font style="color:rgb(48, 79, 254);">The so-called phantom problem occurs within a transaction when the same query produces different sets of rows at different times. For example, if a SELECT is executed twice, but returns a row the second time that was not returned the first time, the row is a “phantom” row.</font>**_

<font style="color:rgb(44, 62, 80);">翻译：当同一个查询在不同的时间产生不同的结果集时，事务中就会出现所谓的幻象问题。例如，如果 SELECT 执行了两次，但第二次返回了第一次没有返回的行，则该行是“幻像”行。</font>

<font style="color:rgb(44, 62, 80);">举个例子，假设一个事务在 T1 时刻和 T2 时刻分别执行了下面查询语句，途中没有执行其他任何语句：</font>



```sql
SELECT * FROM t_test WHERE id > 100;
```

<font style="color:rgb(44, 62, 80);">只要 T1 和 T2 时刻执行产生的结果集是不相同的，那就发生了幻读的问题，比如：</font>

+ <font style="color:rgb(44, 62, 80);">T1 时间执行的结果是有 5 条行记录，而 T2 时间执行的结果是有 6 条行记录，那就发生了幻读的问题。</font>

## [](https://xiaolincoding.com/mysql/transaction/phantom.html#%E5%BF%AB%E7%85%A7%E8%AF%BB%E6%98%AF%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D%E5%B9%BB%E8%AF%BB%E7%9A%84)<font style="color:rgb(44, 62, 80);">快照读是如何避免幻读的？</font>
<font style="color:rgb(44, 62, 80);">可重复读隔离级是由 MVCC（多版本并发控制）实现的，实现的方式是开始事务后（执行 begin 语句后），在执行第一个查询语句后，会创建一个 Read View，</font>**<font style="color:rgb(48, 79, 254);">后续的查询语句利用这个 Read View，通过这个 Read View 就可以在 undo log 版本链找到事务开始时的数据，所以事务过程中每次查询的数据都是一样的</font>**<font style="color:rgb(44, 62, 80);">，即使中途有其他事务插入了新纪录，是查询不出来这条数据的，所以就很好了避免幻读问题。</font>

<font style="color:rgb(44, 62, 80);">做个实验，数据库表 t_stu 如下，其中 id 为主键。</font>

![1732497618999-24e0450f-b6fe-47ed-8e1b-1a9bd26619af.png](./img/yarh9QQwWNFMmt0P/1732497618999-24e0450f-b6fe-47ed-8e1b-1a9bd26619af-519550.png)

<font style="color:rgb(44, 62, 80);">然后在可重复读隔离级别下，有两个事务的执行顺序如下：</font>

![1732497619132-0da1dd2d-2e14-4758-9704-1dc6db64ec37.png](./img/yarh9QQwWNFMmt0P/1732497619132-0da1dd2d-2e14-4758-9704-1dc6db64ec37-729225.png)

<font style="color:rgb(44, 62, 80);">从这个实验结果可以看到，即使事务 B 中途插入了一条记录，事务 A 前后两次查询的结果集都是一样的，并没有出现所谓的幻读现象。</font>

## [](https://xiaolincoding.com/mysql/transaction/phantom.html#%E5%BD%93%E5%89%8D%E8%AF%BB%E6%98%AF%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D%E5%B9%BB%E8%AF%BB%E7%9A%84)<font style="color:rgb(44, 62, 80);">当前读是如何避免幻读的？</font>
<font style="color:rgb(44, 62, 80);">MySQL 里除了普通查询是快照读，其他都是</font>**<font style="color:rgb(48, 79, 254);">当前读</font>**<font style="color:rgb(44, 62, 80);">，比如 update、insert、delete，这些语句执行前都会查询最新版本的数据，然后再做进一步的操作。</font>

<font style="color:rgb(44, 62, 80);">这很好理解，假设你要 update 一个记录，另一个事务已经 delete 这条记录并且提交事务了，这样不是会产生冲突吗，所以 update 的时候肯定要知道最新的数据。</font>

<font style="color:rgb(44, 62, 80);">另外，</font><font style="color:rgb(71, 101, 130);">select ... for update</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这种查询语句是当前读，每次执行的时候都是读取最新的数据。</font>

<font style="color:rgb(44, 62, 80);">接下来，我们假设</font><font style="color:rgb(71, 101, 130);">select ... for update</font><font style="color:rgb(44, 62, 80);">当前读是不会加锁的（实际上是会加锁的），在做一遍实验。</font>

![1732497619211-2c2fa1c9-f43e-4b52-91d6-6142e582fe78.png](./img/yarh9QQwWNFMmt0P/1732497619211-2c2fa1c9-f43e-4b52-91d6-6142e582fe78-524243.png)

<font style="color:rgb(44, 62, 80);">这时候，事务 B 插入的记录，就会被事务 A 的第二条查询语句查询到（因为是当前读），这样就会出现前后两次查询的结果集合不一样，这就出现了幻读。</font>

<font style="color:rgb(44, 62, 80);">所以，</font>**<font style="color:rgb(48, 79, 254);">Innodb 引擎为了解决「可重复读」隔离级别使用「当前读」而造成的幻读问题，就引出了间隙锁</font>**<font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">假设，表中有一个范围 id 为（3，5）间隙锁，那么其他事务就无法插入 id = 4 这条记录了，这样就有效的防止幻读现象的发生。</font>

![1732497619274-2b9048bd-b81b-4e64-a740-8f7f17b0a7d7.png](./img/yarh9QQwWNFMmt0P/1732497619274-2b9048bd-b81b-4e64-a740-8f7f17b0a7d7-611567.png)

<font style="color:rgb(44, 62, 80);">举个具体例子，场景如下：</font>

![1732497619411-8c5ea37f-28c1-42b8-bc6e-11742e670866.png](./img/yarh9QQwWNFMmt0P/1732497619411-8c5ea37f-28c1-42b8-bc6e-11742e670866-587571.png)

<font style="color:rgb(44, 62, 80);">事务 A 执行了这面这条锁定读语句后，就在对表中的记录加上 id 范围为 (2, +∞] 的 next-key lock（next-key lock 是间隙锁+记录锁的组合）。</font>

<font style="color:rgb(44, 62, 80);">然后，事务 B 在执行插入语句的时候，判断到插入的位置被事务 A 加了 next-key lock，于是事物 B 会生成一个插入意向锁，同时进入等待状态，直到事务 A 提交了事务。这就避免了由于事务 B 插入新记录而导致事务 A 发生幻读的现象。</font>

## [](https://xiaolincoding.com/mysql/transaction/phantom.html#%E5%B9%BB%E8%AF%BB%E8%A2%AB%E5%AE%8C%E5%85%A8%E8%A7%A3%E5%86%B3%E4%BA%86%E5%90%97)<font style="color:rgb(44, 62, 80);">幻读被完全解决了吗？</font>
**<font style="color:rgb(48, 79, 254);">可重复读隔离级别下虽然很大程度上避免了幻读，但是还是没有能完全解决幻读</font>**<font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">我举例一个可重复读隔离级别发生幻读现象的场景。</font>

### [](https://xiaolincoding.com/mysql/transaction/phantom.html#%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%8F%91%E7%94%9F%E5%B9%BB%E8%AF%BB%E7%8E%B0%E8%B1%A1%E7%9A%84%E5%9C%BA%E6%99%AF)<font style="color:rgb(44, 62, 80);">第一个发生幻读现象的场景</font>
<font style="color:rgb(44, 62, 80);">还是以这张表作为例子：</font>

![1732497619500-6ce9cb01-28b4-4c08-a7f2-4084c9a1ba43.png](./img/yarh9QQwWNFMmt0P/1732497619500-6ce9cb01-28b4-4c08-a7f2-4084c9a1ba43-115262.png)

<font style="color:rgb(44, 62, 80);">事务 A 执行查询 id = 5 的记录，此时表中是没有该记录的，所以查询不出来。</font>



```sql
 事务 A
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from t_stu where id = 5;
Empty set (0.01 sec)
```

<font style="color:rgb(44, 62, 80);">然后事务 B 插入一条 id = 5 的记录，并且提交了事务。</font>



```sql
 事务 B
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into t_stu values(5, '小美', 18);
Query OK, 1 row affected (0.00 sec)

mysql> commit;
Query OK, 0 rows affected (0.00 sec)
```

<font style="color:rgb(44, 62, 80);">此时，</font>**<font style="color:rgb(48, 79, 254);">事务 A 更新 id = 5 这条记录，对没错，事务 A 看不到 id = 5 这条记录，但是他去更新了这条记录，这场景确实很违和，然后再次查询 id = 5 的记录，事务 A 就能看到事务 B 插入的纪录了，幻读就是发生在这种违和的场景</font>**<font style="color:rgb(44, 62, 80);">。</font>



```sql
 事务 A
mysql> update t_stu set name = '小林coding' where id = 5;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from t_stu where id = 5;
+----+--------------+------+
| id | name         | age  |
+----+--------------+------+
|  5 | 小林coding   |   18 |
+----+--------------+------+
1 row in set (0.00 sec)
```

<font style="color:rgb(44, 62, 80);">整个发生幻读的时序图如下：</font>

![1732497619600-30f0bdec-c6e1-4d87-9ddf-bde09ee9e7df.png](./img/yarh9QQwWNFMmt0P/1732497619600-30f0bdec-c6e1-4d87-9ddf-bde09ee9e7df-897992.png)

<font style="color:rgb(44, 62, 80);">在可重复读隔离级别下，事务 A 第一次执行普通的 select 语句时生成了一个 ReadView，之后事务 B 向表中新插入了一条 id = 5 的记录并提交。接着，事务 A 对 id = 5 这条记录进行了更新操作，在这个时刻，这条新记录的 trx_id 隐藏列的值就变成了事务 A 的事务 id，之后事务 A 再使用普通 select 语句去查询这条记录时就可以看到这条记录了，于是就发生了幻读。</font>

<font style="color:rgb(44, 62, 80);">因为这种特殊现象的存在，所以我们认为</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(48, 79, 254);">MySQL Innodb 中的 MVCC 并不能完全避免幻读现象</font>**<font style="color:rgb(44, 62, 80);">。</font>

### [](https://xiaolincoding.com/mysql/transaction/phantom.html#%E7%AC%AC%E4%BA%8C%E4%B8%AA%E5%8F%91%E7%94%9F%E5%B9%BB%E8%AF%BB%E7%8E%B0%E8%B1%A1%E7%9A%84%E5%9C%BA%E6%99%AF)<font style="color:rgb(44, 62, 80);">第二个发生幻读现象的场景</font>
<font style="color:rgb(44, 62, 80);">除了上面这一种场景会发生幻读现象之外，还有下面这个场景也会发生幻读现象。</font>

+ <font style="color:rgb(44, 62, 80);">T1 时刻：事务 A 先执行「快照读语句」：select * from t_test where id > 100 得到了 3 条记录。</font>
+ <font style="color:rgb(44, 62, 80);">T2 时刻：事务 B 往插入一个 id= 200 的记录并提交；</font>
+ <font style="color:rgb(44, 62, 80);">T3 时刻：事务 A 再执行「当前读语句」 select * from t_test where id > 100 for update 就会得到 4 条记录，此时也发生了幻读现象。</font>

**<font style="color:rgb(48, 79, 254);">要避免这类特殊场景下发生幻读的现象的话，就是尽量在开启事务之后，马上执行 select ... for update 这类当前读的语句</font>**<font style="color:rgb(44, 62, 80);">，因为它会对记录加 next-key lock，从而避免其他事务插入一条新记录。</font>

## [](https://xiaolincoding.com/mysql/transaction/phantom.html#%E6%80%BB%E7%BB%93)<font style="color:rgb(44, 62, 80);">总结</font>
<font style="color:rgb(44, 62, 80);">MySQL InnoDB 引擎的可重复读隔离级别（默认隔离级），根据不同的查询方式，分别提出了避免幻读的方案：</font>

+ <font style="color:rgb(44, 62, 80);">针对</font>**<font style="color:rgb(48, 79, 254);">快照读</font>**<font style="color:rgb(44, 62, 80);">（普通 select 语句），是通过 MVCC 方式解决了幻读。</font>
+ <font style="color:rgb(44, 62, 80);">针对</font>**<font style="color:rgb(48, 79, 254);">当前读</font>**<font style="color:rgb(44, 62, 80);">（select ... for update 等语句），是通过 next-key lock（记录锁+间隙锁）方式解决了幻读。</font>

<font style="color:rgb(44, 62, 80);">我举例了两个发生幻读场景的例子。</font>

<font style="color:rgb(44, 62, 80);">第一个例子：对于快照读， MVCC 并不能完全避免幻读现象。因为当事务 A 更新了事务 B 插入的记录，那么事务 A 前后两次查询的记录条目就不一样了，所以就发生幻读。</font>

<font style="color:rgb(44, 62, 80);">第二个例子：对于当前读，如果事务开启后，并没有执行当前读，而是先快照读，然后这期间如果其他事务插入了一条记录，那么事务后续使用当前读进行查询的时候，就会发现两次查询的记录条目就不一样了，所以就发生幻读。</font>

<font style="color:rgb(44, 62, 80);">所以，</font>**<font style="color:rgb(48, 79, 254);">MySQL 可重复读隔离级别并没有彻底解决幻读，只是很大程度上避免了幻读现象的发生。</font>**

<font style="color:rgb(44, 62, 80);">要避免这类特殊场景下发生幻读的现象的话，就是尽量在开启事务之后，马上执行 select ... for update 这类当前读的语句，因为它会对记录加 next-key lock，从而避免其他事务插入一条新记录。</font>



> 更新: 2024-02-01 02:53:42  
原文: [https://www.yuque.com/vip6688/neho4x/er4zygqdedq85g8b](https://www.yuque.com/vip6688/neho4x/er4zygqdedq85g8b)
>



> 更新: 2024-11-25 09:20:20  
> 原文: <https://www.yuque.com/neumx/laxg2e/62ae54bb95100f956c4a72c940149b41>