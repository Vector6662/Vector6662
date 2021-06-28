### Hi there 👋



**最近的阅读计划：**

- [ ] **MIT-6.824**：

  2020年：https://mit-public-courses-cn-translatio.gitbook.io/mit6-824/

  2021年似乎没有更新完？https://github.com/chaozh/MIT-6.824

  这个课程太有意思了，但我感觉它主要的重点还是在分布式一致性算法，有点淡化分布式计算部分。🐱‍👤

- [ ] 深入理解Java虚拟机：JVM高级特性与最佳实践（第3版）周志明

- [ ] IPv4, IPv6, and a sudden change in attitude：尝试翻译一下，训练自己的英文阅读能力

- [ ] **凤凰架构**：Paxos你赢了，我要战略性撤退了。先看看“演进中的架构”吧！



**正在学习的开源项目：**

- [ ] [mini-spring](https://github.com/code4craft/tiny-spring)：解决了循环依赖，现在可以试试拦截器啥的了🐱‍👤
- [ ] [秒杀系统](https://github.com/qiurunze123/miaosha)：很经典的项目，完成了大部分，但是停滞了一段时间



**我的面试八股学习进度：**

- [ ] 阅读[JavaGuide](https://github.com/Snailclimb/JavaGuide)：
  - 分布式部分，虽然和MIT6.824比起来算是科普了，但是有些点还是值得学习的，比如对BASE有比较好的解释。
  - 并发容器，这个应该是纯八股了，有空看一看就行。
- [ ] [JVM 垃圾回收](https://snailclimb.gitee.io/javaguide/#/docs/java/jvm/JVM垃圾回收)，boring😪。JavaGuide上面也有的。
- [ ] 阅读[能解答一切的答案 · 语雀 (yuque.com)](https://www.yuque.com/books/share/2b434c74-ed3a-470e-b148-b4c94ba14535)



**LeeCode刷题进度：**

🐶🐶🐶（抵触）但是还是得安排时间刷啊😭





> 第一性原理：完成一个目标只有少数几个必要条件。只需要专注他们，不要总是有奇奇怪怪的无效假设。不要被乌合之众的观点/做法绑架。[^1]

> 所以，Zookeeper这里声明，自己最开始就不支持线性一致性，来解决这里的技术问题。如果不提供这个能力，那么（为读请求返回旧数据）就不是一个bug。这实际上是一种经典的解决性能和强一致之间矛盾的方法，也就是不提供强一致。[^2]









---

[^1]:https://github.com/apachecn/home
[^2]:https://mit-public-courses-cn-translatio.gitbook.io/mit6-824/lecture-08-zookeeper/8.4-zookeeper