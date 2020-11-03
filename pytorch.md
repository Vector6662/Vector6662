#### [torch1.4和1.6的区别](https://blog.csdn.net/winter2121/article/details/108799776?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)



#### [python中类的**\__call__()**方法](https://www.cnblogs.com/xinglejun/p/10129823.html)

> Python中有一个有趣的语法，只要定义类型的时候，实现__call__函数，这个类型就成为**可调用**的。换句话说，我们可以把这个类型的对象当作函数来使用，相当于 重载了括号运算符。

重载括号运算符   这句话说得好啊:joy:。我可不可以这样理解：对象函数化

> 单看 p('Tim') 你无法确定 p 是一个函数还是一个类实例，所以，在Python中，**函数也是对象，对象和函数的区别并不显著**。



#### [pytorch中前向传播forward()](https://blog.csdn.net/u011501388/article/details/84062483)

调用这个方法的



#### [函数申明中 `def foo(*args, **kwarg)`:（*，**作用）](https://blog.csdn.net/liuxingen/article/details/50113923?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)



#### [softmax和max之类的函数的`dim`参数如何理解？](https://blog.csdn.net/Will_Ye/article/details/104994504?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param)

:exclamation: 其实还是不是很理解，先记录一下

<img src="https://img-blog.csdnimg.cn/20200320204758144.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dpbGxfWWU=,size_16,color_FFFFFF,t_70#pic_center" alt="softmax" style="zoom:67%;" />

dim这个参数很重要，如果不加这个参数的化则对整个张量取max或softmax，这显然不是我们期望的。



#### zero_grad()

[这篇文章](https://blog.csdn.net/u011959041/article/details/102760868?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)接近了我想要的答案：

> optimizer.zero_grad()意思是把梯度置零，**也就是把loss关于weight的导数变成0**.

该方法是为了清除先前进行backward()产生的梯度，因为每一个叶子结点的梯度grad属性是要叠加的。

[这篇文章](https://blog.csdn.net/weixin_43901214/article/details/105035215?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param)讲了梯度累加带来的一些tricks

#### python中魔法方法





#### Ellipses





#### [这篇文章写的很好](https://blog.csdn.net/qq_28418387/article/details/95918829)详细讲解了log_softmax和NULLLoss的关系，和比较厉害的CrossEntropyLoss()



#### 计算图

计算图的释放时机是调用backward()后，如果加上了`retain_graph=True`，那么计算图就不会立即被释放

还有一个细节，对于叶子节点$$w_1$$，它的梯度应该是$$\frac{\partial loss}{\partial w_{1}}$$，其中$$loss$$是损失函数的且是一个标量，也就是说每个叶子结点的梯度实际上是输出$$loss$$对每个叶子结点的梯度

[虽然输入的训练数据是默认不求导的，但是，我们的 model 中的所有参数，它默认是求导的，这么一来，其中只要有一个需要求导，那么输出的网络结果必定也会需要求的。来看个实例：](https://zhuanlan.zhihu.com/p/67184419)

