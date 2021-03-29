#### 基础调研：

应用场景：

- [Redis【入门】就这一篇！](https://zhuanlan.zhihu.com/p/37982685)

  1. 读取操作>>写入操作

  2. >  但是使用内存进行数据存储开销也是比较大的,限于成本的原因，一般我们**只是使用 Redis 存储一些常用和主要的数据**，比如用户登录的信息等。

  3. > **该业务数据是读操作多，还是写操作多？**如果写操作多，频繁需要写入数据库，也没有必要使用缓存；

  4. > 触发事件将 Redis 的缓存的数据以批量的形式**一次性写入数据库**，从而完成持久化的工作。

  5. 大概率会使用到**事件**

  6. 相当于一个数据库中间件！

  7. > Redis 采用的是定期删除+惰性删除策略

  8. 一个重点：Redis持久化



#### 2020/12/28

今天算是浅尝则止了一下redis的底层实现原理，[链接在此](https://mp.weixin.qq.com/s?__biz=MzI4Njg5MDA5NA==&mid=2247484359&idx=1&sn=0994c6246990b7ad42a2d3f294042316&chksm=ebd742c6dca0cbd0a826ace13f4d4eeff282052f4a97b31654ef1b3b32f991374f5c67a45ae9&token=620000779&lang=zh_CN&scene=21###wechat_redirect)，我很有自知之明这些知识虽然能够理解但是可能明天就忘了很多了，但是我觉得还是得记住一些基本的思想，否则今天花这么多时间看这个就很不值了：

1. **sentinel机制**应该是实现redis**分布式**的核心：“*能够作为配置中心，提供当前 master服务器的信息* ”

2. redis分布式的具体实现架构：主从架构。sentinel机制是为了解决master服务器宕机等问题

3. redis存储信息的键值对的具体实现也很重要而且很有意思，**顶层**都是采用了***redisobject***这个结构体：不论是键还是值，顶层的数据结构都是一样的的：redisobject

4. 突然发现c语言真的博大精深，redis这么一个复杂的数据库就用了很多结构体就能实现复杂的数据结构。在此我觉得需要重点关注的结构体有：

   - redisDb：默认一共有16个数据库（作为“资深”码农，第一个数据库的编号别给我说是1:slightly_smiling_face:）。里面的字段有两个比较重要，dict和expire：

     ```c
     typedef struct redisDb{
         int id;
         dict* dict;//键空间
         dict* expire;
         dict* watched_key;
         long long avg_ttl;
     }
     ```

     我现在发现其实dict类型其实也是实现哈希表的底层数据结构，即也是*redisobject*当中的`void* ptr`字段可能拥有的值之一。

     其实如果不纠结具体的数据结构，看下面这幅图就非常明了了：

     <img src="https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib0LSf7wiaom7XfZ9RhpUWsW2DMeziao6kQ1lEShvTh1V0sH6T1tVZY0hXJIlTI5s7pknCpXMVB0T17Q/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="图片" style="zoom:67%;" />

   - redisobject，看链接，不多bb。

5. 如果接下来要继续学习redis的知识的话，我觉得应该以每一种技术为单位分模块学习，比如持久化技术、redis实现设计的数据结构、sentinel机制、内存淘汰机制，这样分开学习，这样有利于对每个技术进行深刻理解。

6. ***热点数据***，感觉是应用redis的一个关键问题。我看的这两篇文章感觉都没有说明如何判断热点数据，也就是说**数据命中率**问题是如何解决的，只说了内存淘汰、过期数据删除等技术。难道热点数据判定主要是依靠自己的业务代码吗？

7. 在[这篇文章](https://github.com/Snailclimb/JavaGuide/blob/master/docs/database/Redis/redis-all.md#1-%E7%AE%80%E5%8D%95%E4%BB%8B%E7%BB%8D%E4%B8%80%E4%B8%8B-redis-%E5%91%97)中有提到两个概念：缓存穿透和缓存雪崩。这也是需要理解并记住的概念。

8. redis单线程模型和reactor模式之间的联系，请一定要明白，抽象模型的设计是比较关键的，是具体实现技术的基础。我感觉对应于reactor模式应该为**多reactor单线程**。

9. 因为redis涉及到了reactor模式，所以复习了一下，总结一下它的优势在于：**将耗时操作（比如读取数据）和业务进程（handler）相分离，这样handler内部就不会有等待了。**

​     

#### 2021/2/23

学习docker上redis的安装方法，发现我之前安装的redis用到的msi文件其实是有两个功能：1.安装redis的**服务**，绑定端口到6379；2.安装命令行客户端，比如可以使用`redis-cli`命令。但是现在我把redis服务运行在了docker上，那么我只用后面那个服务就可以了，只使用指令。

