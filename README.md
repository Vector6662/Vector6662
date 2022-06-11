### Hi there 👋







> 第一性原理：完成一个目标只有少数几个必要条件。只需要专注他们，不要总是有奇奇怪怪的无效假设。不要被乌合之众的观点/做法绑架。[^1]

> 所以，Zookeeper这里声明，**自己最开始就不支持线性一致性**，来解决这里的技术问题。如果不提供这个能力，那么（为读请求返回旧数据）就不是一个bug。这实际上是一种经典的解决性能和强一致之间矛盾的方法，也就是不提供强一致。[^2]

> 有些方案，纯粹就是糟糕的方案。[^3]

> 同时，论文也提出了一个当时**非常异类的观点**：存储系统具有弱一致性也是可以的。当时，学术界的观念认为，存储系统就应该有良好的行为，如果构建了一个会返回错误数据的系统，就像前面（详见3.2）介绍的糟糕的多副本系统一样，那还有什么意义？[^4]

> 因此CAP要理解为，当网络分区发生时（P），A和P只能二选一。[^5]

> 正因为每个Java对象都有Mark Word，而Mark Word能标记锁状态（**把自己当做锁**），所以Java中任意对象都可以作为synchronized的锁。[^6]

[^1]:https://github.com/apachecn/home

[^2]:[8.4 Zookeeper](https://mit-public-courses-cn-translatio.gitbook.io/mit6-824/lecture-08-zookeeper/8.4-zookeeper)。“我玩完不给钱，就不算嫖喽”🤣。

[^3]: [WEB开发中，使用JSON-RPC好，还是RESTful API好？ ](https://www.zhihu.com/question/28570307/answer/47876255)这篇文章diss了Restful的无脑拥护者，并提出其实JSON-RPC就能很好地替代Restful的工作

[^4]: [GFS的设计目标](https://mit-public-courses-cn-translatio.gitbook.io/mit6-824/lecture-03-gfs/3.3-gfs-te-dian)。这和Zookeeper很相似�

[^5]:CAP理论并不遵循“三选二定律”，实际上是二选一。
[^6]:来自《[漫画：从JVM锁扯到Redis分布式锁](https://juejin.cn/post/6958250838103949343#:~:text=%E6%AD%A3%E5%9B%A0%E4%B8%BA%E6%AF%8F%E4%B8%AAJava%E5%AF%B9%E8%B1%A1%E9%83%BD%E6%9C%89Mark%20Word%EF%BC%8C%E8%80%8CMark%20Word%E8%83%BD%E6%A0%87%E8%AE%B0%E9%94%81%E7%8A%B6%E6%80%81%EF%BC%88%E6%8A%8A%E8%87%AA%E5%B7%B1%E5%BD%93%E5%81%9A%E9%94%81%EF%BC%89%EF%BC%8C%E6%89%80%E4%BB%A5Java%E4%B8%AD%E4%BB%BB%E6%84%8F%E5%AF%B9%E8%B1%A1%E9%83%BD%E5%8F%AF%E4%BB%A5%E4%BD%9C%E4%B8%BAsynchronized%E7%9A%84%E9%94%81%EF%BC%9A)》