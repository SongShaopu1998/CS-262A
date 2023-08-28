## System R

- 如何衡量`optimizer cost`? 

  > optimizer cost measure should be a weighted sum of CPU time and I/O count, with weights adjustable according to the system configuration. (System R is CPU bound, it will cost lots of time in CPU)

- `Convoy Problem`

锁可以分为两种：`high-traffic locks`和`low-traffic locks`。如果很多进程都经常试图获取某个锁，并且只在占用一小段时间后即释放，该类锁就被称为`high-traffic locks`，常见于控制对`buffer pool`和`system log`的访问。这里试图阐述的问题是：当进程A拿到锁并做了一些事情后，进程A释放锁，如果锁使用的是**FCFS**队列，那么此时排在队列最前边的进程B会拿到锁，这导致进程A在再次试图抢占锁失败后，被放在了FCFS队列的最后端。

由于每一个可派遣(dispachable)的进程都被分配了一个时间片S(quanta)，当时间片被用完后，进程就会调度到`sleep`状态，一个很昂贵的派遣(dispatch)操作必须被执行以重新将该进程加入`multipramming set`中。

这意味着按照第一段的锁队列执行机制，进程A在执行了T timestamp后，释放锁，之后被放到队列的最后端，在接下来的S- T时间段内，队列前边的进程还没有执行完毕，于是A被sleep。在时间片S中，A的有效工作时间只有T，远远小于其被重新调度的开销，所以系统进入了thrashing状态。

解决`convoy problem`的一个方法是：

> when a process P releases a lock, all processes which are enqueued for the lock are made dispatchable, but the lock is not granted to any particular process. Therefore, the lock may be regranted to process P if it makes a subsequent request. (抢占式的分配锁) Process P may acquire and release the lock many times before its time slice is exhausted. It is highly probable that process P will not be holding the lock when it goes into a long wait.

- **OS** and **DBMS**: Philosophical Similarities

<img src="https://shaopu-blog.oss-cn-beijing.aliyuncs.com/img/2023-08-28-214819.png" alt="image-20230828144819442" style="zoom:50%;" />

<img src="https://shaopu-blog.oss-cn-beijing.aliyuncs.com/img/2023-08-28-214851.png" alt="image-20230828144851385" style="zoom:50%;" />

OS：提供了一个**简洁的系统**实现和支持机制，将很多可以扩展的功能和想法留给上层程序自己解决（比如Unix对于文件锁的观点）(Bottom-Up)

DBMS：为了实现上层**简洁的**，满足数据独立性的**查询**语法，在下方构建了一个足够完整，提供了各种功能的系统（Top-Down）

<img src="https://shaopu-blog.oss-cn-beijing.aliyuncs.com/img/2023-08-28-215047.png" alt="image-20230828145047563" style="zoom:50%;" />

