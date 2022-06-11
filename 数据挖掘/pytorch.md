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

想了很久`dim=0`或1,2等等取哪一个维度，过了很久发现其实还是很好理解，比如这张图上的是`(3, 6, 4)`的张量，那么`dim=0`就表示去3这个维度的，`dim=1`表示取6这个维度的，结合张量来看，就好理解了。



#### zero_grad()

[这篇文章](https://blog.csdn.net/u011959041/article/details/102760868?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)接近了我想要的答案：

> optimizer.zero_grad()意思是把梯度置零，**也就是把loss关于weight的导数变成0**.

该方法是为了清除先前进行backward()产生的梯度，因为每一个叶子结点的梯度grad属性是要叠加的。

[这篇文章](https://blog.csdn.net/weixin_43901214/article/details/105035215?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param)讲了梯度累加带来的一些tricks

#### python中魔法方法





#### Ellipses

- [10个不为人知的 Python 冷知识](https://zhuanlan.zhihu.com/p/63909763)

  >据说它是Numpy的语法糖，不玩 Numpy 的人，可以说是没啥用的。在网上只看到这个 用 ... 代替 pass ，稍微有点用，但又不是必须使用的。

- [Python 的 Ellipsis 对象](https://farer.org/2017/11/29/python-ellipsis-object/)，这里面说得就比较详细了，从他的描述中发现这个只是一个很鸡肋的语法糖

- [来源](https://www.cnblogs.com/zhuyunbk/p/11452921.html)：

  ![img](https://pic3.zhimg.com/80/v2-b4e4d2118b0de6f45af3c96af05a7f59_hd.png)

  ​	给我翻译翻译：

  - 作为尚未编写的代码的占位符（比如`if error: ....`），但故意空着的的代码最好用`pass`

  - （:exclamation: 没怎么看懂）当用类型模块（如`Callable[..., str]`）指定代码提示（*type hints*）的时意味着一个 function会返回一个string类型但是不会指定call signature

  - 作为默认参数值（如`def fn(x=...)`），尤其是你想在不传递参数值和传递`None`值上做区分

    因为`...`是一个单例，你应该用`is`来检查而不是`==`



#### Type Hints

上面的问题引入了type hints这个知识点

For example, here is a simple function whose argument and return type are declared in the annotations:

```python
def greeting(name: str) -> str:
    return 'Hello ' + name
```







#### log_softmax、NULLLoss和CrossEntropyLoss()

- [这篇文章写的很好](https://blog.csdn.net/qq_28418387/article/details/95918829)详细讲解了log_softmax和NULLLoss的关系，和比较厉害的CrossEntropyLoss()

  其实损失函数基本来说就两个：**交叉熵损失函数（CrossEntropyLoss）**和**均方差损失函数（MSELoss**），分别用于分类问题和回归问题，其他的损失函数都是这两个的变种。
  
  这篇文章最重要的是用例子阐述了$log\_softmax$搭配$nulloss$的计算方式，和我理解的有很大出入。文中的这句话就很精髓了：
  
  > 这里说一下我自己对**NLLLoss**的理解，为什么NLLLoss的计算方式可以用来求损失值。经过上面的计算我们知道，Softmax计算出来的值范围在[0, 1]，值的含义表示对应类别的概率，也就是说，每行（代表每张图）中最接近于1的值对应的类别，就是该图片概率最大的类别，那么经过log求值取绝对值之后，就是最接近于0的值，如果此时每行中的最小值对应的类别值与Target中的类别值相同，那么每行中的最小值求和取平均就是最小，极端的情况就是0。总结一下就是，input的预测值与Target的值越接近，NLLLoss求出来的值就越接近于0，这不正是损失值的本意所在吗，所以NLLLoss可以用来求损失值。



#### 计算图

计算图的释放时机是调用backward()后，如果加上了`retain_graph=True`，那么计算图就不会立即被释放

还有一个细节，对于叶子节点$$w_1$$，它的梯度应该是$$\frac{\partial loss}{\partial w_{1}}$$，其中$$loss$$是损失函数的且是一个标量，也就是说每个叶子结点的梯度实际上是输出$$loss$$对每个叶子结点的梯度

[虽然输入的训练数据是默认不求导的，但是，我们的 model 中的所有参数，它默认是求导的，这么一来，其中只要有一个需要求导，那么输出的网络结果必定也会需要求的。来看个实例：](https://zhuanlan.zhihu.com/p/67184419)











### python基础

#### lambda表达式：

- https://www.cnblogs.com/lida585/p/10375819.html

  ```python
  a = [('a',1),('b',2),('c',3),('d',4)]
  a_1 = list(map(lambda x:x[0],a))   # 注意第二个参数要是可迭代的
  ```

  > map函数第一个参数是一个lambda表达式，**输入一个对象，返回该对象的第一个元素**。第二个就是需要作用的对象，此处是一个列表

  我现在倒是看明白了哈哈哈哈！原来`lambda x:x[0]`中的x只是**形参**，**而后面的a才是实参**！！！哈哈哈

  其实完全可以改写为：

  ```python
  a = [('a',1),('b',2),('c',3),('d',4)]
  def f(x):
      return x[0]
  f(a)
  ```

  看到没有，lambda表达式只是把函数申明和调用换了一种写法而已把需要多行解决的问题改到一行解决，看起来简洁一些。