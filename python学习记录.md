7. [怎样理解阻塞非阻塞与同步异步的区别？ - 萧萧的回答 - 知乎](https://www.zhihu.com/question/19732473/answer/241673170)

   <img src="C:\Users\82526\AppData\Roaming\Typora\typora-user-images\image-20200707133820632.png" alt="image-20200707133820632" style="zoom:80%;" />

8. 由rabbitmq引入的阻塞式和非阻塞式的通信方式思考

   [参考](http://rabbitmq.mr-ping.com/AMQP/AMQP_0-9-1_Model_Explained.html)

   > AMQP代理在什么时候删除消息才是正确的？AMQP 0-9-1 规范给我们两种建议：
   >
   > - 当消息代理（broker）将消息发送给应用后立即删除。（使用AMQP方法：basic.deliver或basic.get-ok）
   > - 待应用（application）发送一个确认回执（acknowledgement）后再删除消息。（使用AMQP方法：basic.ack）

   

#### [pip国内源](https://blog.csdn.net/qq_30754565/article/details/82777253)

​		阿里云 http://mirrors.aliyun.com/pypi/simple/ 
  中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/ 
  豆瓣(douban) http://pypi.douban.com/simple/ 
  清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/ 
  中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/

**临时使用：** 
可以在使用pip的时候在后面加上-i参数，指定pip源 
eg: pip install scrapy -i https://pypi.tuna.tsinghua.edu.cn/simple



#### conda和pip

<img src="https://upload-images.jianshu.io/upload_images/12713060-25623c3d88871cdf.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" alt="img" style="zoom: 50%;" />

[conda中重要的三个概念：环境、通道和包](https://www.cnblogs.com/dadream/p/10820379.html)

[python包管理工具：Conda和pip比较](https://www.jianshu.com/p/5601dab5c9e5)

[Conda 环境的正确使用姿势](https://blog.csdn.net/sigmarising/article/details/89446397)

> 至于你说的那个env的话，是anaconda的虚拟环境，就是你可以创建多个虚拟环境，各个虚拟环境之间的库是不共用的。anaconda的根目录是有一个Python的，如果你没有创建虚拟环境，就可以把库直接安装在该Python下。

`conda`有一个`base(root)`环境，但是自己创建的环境会在Anaconda/env/下

`conda`就是一个通用的包管理工具，和`pip`、`pip3`作用相似，也和`npm`作用相似，`maven`作用相似

`conda env list` 查看conda环境(*Environments*)列表

清华源好像不咋滴:laughing:

#### [使用python中的pymysql模块](https://blog.csdn.net/kelanj/article/details/82792254)

```sql
sql = 'insert into interest_pic_tag(interest_tag,pic_tag) values ("%s","%s")'%(interest_tag, pic_tag)
```

这里的`("%s","%s")`原来还要双引号，但是目前不知道为啥，:warning:需要过后查一下



#### python多线程

<img src="C:\Users\82526\AppData\Roaming\Typora\typora-user-images\image-20200709162444764.png" alt="image-20200709162444764" style="zoom:80%;" />

#### [RabbitMQ获取消息的两种方式](https://blog.csdn.net/joeyon1985/article/details/43406609?utm_source=blogxgwz4)

其中的Poll方式应该是最常用的，注意这种方式得`GetResponse response = channel.basicGet(QUEUE_NAME,autoAck=false);`即最好自动回复确认，否则消息会进入unacked中。

我觉得应该得研究一下当`autoAck=false`的时候该如何处理？basicAck()`函数的第一个参数应该传什么值呢？



#### python学习的一些知识细节：

1. 在Python中，

   ```python
   False,0,'',[],{},()
   ```

   都可以视为假，则比如判断一个列表是否为空，可以

   ```python
   1 list_temp = []
   2 if list_temp:
   3     # 存在值即为真。这句话有点哲学感:joy:
   4 else:
   5     # list_temp是空的
   ```

2. [各种下划线](https://blog.csdn.net/tcx1992/article/details/80105645)

   > 单个下划线是一个Python命名约定，表示这个名称是供内部使用的。 它通常不由Python解释器强制执行，仅仅作为一种对程序员的提示。

   > \_\_init\_\_()方法又被称为构造器（constructor）

3. str.format()方法这是个基础方法

4. python数据类型转换，感觉比Java中的直观得多：

   ```python
   a=10 str(a)   				int->string
   a='10' int(a) float(a)		string->int or float #好像3.0版本后就没有了long()？
   ```
   
   对比Java
   
   ```java
   int->String		i+""   or     String.ValueOf(i)
String->int		Integer.ParseInt(str)    
   ```

5. java切片

   ```python
   words="adedehny"
   words[0:2] -> ad
   words[3:6] ->deh
   ```
   
   words[m:n]**切片范围为[m,n)**
   
6. 一些基础的语法事例，直接快速学习

```python
def filter(l):	# def表示定义函数
    x=[]
    for i in range(0,len(l)):	# len()表示获取数组长度
        if l[i]%2!=0:
            x.append(l[i])	# 追加
    return x
```

7. ## [一文详解python的类方法，普通方法和静态方法](https://www.cnblogs.com/jayliu/p/9030725.html)

#### :star:python上的正则化要学习



#### 对`self`的理解

```py
class Test:
	def prt(self):
		printf(self)

t=Test()
t.prt()   -> Test.prt(t) 这种方式更接近本质
```

- self可以不写吗？

> 在python解释器内部，**当我们调用`t.prt()`时，实际上Python解释成`Test.prt(t)`**，也就是说把self替换成类的实例
>
> https://www.zhihu.com/question/39264541/answer/685673258

- 个人理解

1. <u>python将类的实例以类中**参数**(self)的形式独立出来</u>，如上面的`Test.prt(t)`，最终始终调用的是Test类而非其实例t。 若实例的方法不传self，则会自行调用传self，这算是python的一个语法糖了。
2. 在类中方法的参数中加上self，表明这是一个**实例方法**，而非类方法。这是一个简单实用的理解方法:joy:

#### [python无法导入自己的模块的解决办法](https://www.cnblogs.com/yibeimingyue/p/11367614.html)

#### python3新增数据结构：set()集合

- https://www.runoob.com/python3/python3-set.html

  > 可以使用大括号 **{ }** 或者 **set()** 函数创建集合，注意：创建一个空集合必须用 **set()** 而不是 **{ }**，因为 **{ }** 是用来创建一个空字典。

可以这么理解，{}符号是被集合和字典公用的符号，具体是什么得看里边的内容，如果是元素则为集合，若为键值对则为字典。所以声明空集合的时候不能用{}，因为编辑器不知道你到底想申明的是集合还是字典，所以{}就用作字空典的申明了，而set()用作空集合的申明。