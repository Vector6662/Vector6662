### Hi there 👋



**最近的阅读计划：**

- [x] **MIT-6.824**：

  2020年：https://mit-public-courses-cn-translatio.gitbook.io/mit6-824/

  2021年似乎没有更新完？https://github.com/chaozh/MIT-6.824

  读得有点迷糊了，接下来看看分布式事务部分？已看，越看越师傅念经。。。😭

- [ ] 《深入理解Java虚拟机》：JVM高级特性与最佳实践（第3版）周志明

- [ ] IPv4, IPv6, and a sudden change in attitude：尝试翻译一下，训练自己的英文阅读能力

- [ ] **《凤凰架构》**：Paxos你赢了，我要战略性撤退了。先看看“演进中的架构”吧！

- [x] 《[MySQL是怎样运行的](https://juejin.cn/book/6844733769996304392?referrer=5bff96c6e51d45452f2d6f95)》：首先调研一下这本书，看看有没有事务相关的章节。



**正在学习的开源项目：**

- [ ] [**mini-spring**](https://github.com/code4craft/tiny-spring)：解决了循环依赖，现在可以试试拦截器啥的了。正在看别人实现spring的源码，突然想到之前学习设计模式其中的一种，很重要的灵感！同时项目中的CglibSubclassingInstantiationStrategy我觉得可以尝试写一下，好歹还可以混一个PR

- [ ] [秒杀系统](https://github.com/qiurunze123/miaosha)：很经典的项目，完成了大部分，但是停滞了很长一段时间



**我的面试八股学习进度：**

- [ ] 阅读[JavaGuide](https://github.com/Snailclimb/JavaGuide)：
  - [ ] 分布式部分，虽然和MIT6.824比起来算是科普了，但是有些点还是值得学习的，比如对BASE有比较好的解释。
  - [ ] 并发容器，这个应该是纯八股了，有空看一看就行。
  - [ ] 关于Redis的分布式部分，我找到了一个比较不错的资源：[Redis](http://www.cyc2018.xyz/%E6%95%B0%E6%8D%AE%E5%BA%93/Redis.html#%E5%8D%81%E4%B8%80%E3%80%81%E5%A4%8D%E5%88%B6)
  - [x] ❗volatile关键字防止指令重排序部分我觉得需要再深入学习一下
- [ ] [JVM 垃圾回收](https://snailclimb.gitee.io/javaguide/#/docs/java/jvm/JVM垃圾回收)，boring😪。JavaGuide上面也有的。
- [ ] 阅读[能解答一切的答案 · 语雀 (yuque.com)](https://www.yuque.com/books/share/2b434c74-ed3a-470e-b148-b4c94ba14535)



**LeeCode刷题进度：**

🐶🐶🐶（抵触）但是还是得安排时间刷啊😭





> 第一性原理：完成一个目标只有少数几个必要条件。只需要专注他们，不要总是有奇奇怪怪的无效假设。不要被乌合之众的观点/做法绑架。[^1]

> 所以，Zookeeper这里声明，**自己最开始就不支持线性一致性**，来解决这里的技术问题。如果不提供这个能力，那么（为读请求返回旧数据）就不是一个bug。这实际上是一种经典的解决性能和强一致之间矛盾的方法，也就是不提供强一致。[^2]

> 有些方案，纯粹就是糟糕的方案。[^3]

> 同时，论文也提出了一个当时**非常异类的观点**：存储系统具有弱一致性也是可以的。当时，学术界的观念认为，存储系统就应该有良好的行为，如果构建了一个会返回错误数据的系统，就像前面（详见3.2）介绍的糟糕的多副本系统一样，那还有什么意义？[^4]







---

[^1]:https://github.com/apachecn/home
[^2]:[8.4 Zookeeper](https://mit-public-courses-cn-translatio.gitbook.io/mit6-824/lecture-08-zookeeper/8.4-zookeeper)。“我玩完不给钱，就不算嫖喽”🤣。
[^3]: [WEB开发中，使用JSON-RPC好，还是RESTful API好？ ](https://www.zhihu.com/question/28570307/answer/47876255)这篇文章diss了Restful的无脑拥护者，并提出其实JSON-RPC就能很好的替代Restful的工作
[^4]: [GFS的设计目标](https://mit-public-courses-cn-translatio.gitbook.io/mit6-824/lecture-03-gfs/3.3-gfs-te-dian)。这和Zookeeper很相似，渣男😅

