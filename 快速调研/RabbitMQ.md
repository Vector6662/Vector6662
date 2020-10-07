- [消息队列之 RabbitMQ](https://www.jianshu.com/p/79ca08116d57)

这篇是我看过的最好的解释

重点是理解这张图

![img](https://upload-images.jianshu.io/upload_images/5015984-7fd73af768f28704.png?imageMogr2/auto-orient/strip|imageView2/2/w/484/format/webp)

以及上面的概念，特别是Broker里面的三个概念。还有就是Exchange的三种模式非常重要，

![img](https://upload-images.jianshu.io/upload_images/5015984-13db639d2c22f2aa.png?imageMogr2/auto-orient/strip|imageView2/2/w/385/format/webp)

direct模式是最能说明binding key和routing key关系的模式！注意避开误区：没有queue_name这种字段，其实这个字段相当于把binding key 和 routing key两个字段合并起来了，这种理解只在direct模式下勉强能够解释，但是在fanout和topic模式下就不适用了。

**routing key**和**binding key**这两个字段才是消息队列的灵魂！

对于这个的理解我在本子上也有写心得。



别忘了channel这个概念，复用的TCP连接。