#### Optimization

> 求得**损失函数**对于**权重**的梯度，然后按照某个步长去更新这个权重。这会使得损失函数会减小。

##### SGD

局部最优值、鞍点  震荡

![img](https://img-blog.csdn.net/20180515233015431)

##### SGD+Momentum

当前点的gradient和当前点的velocity做向量和

> 最直观的理解就是，若当前的梯度方向与累积的历史梯度方向一致，则当前的梯度会被加强，从而这一步下降的幅度更大。若当前的梯度方向与累积的梯度方向不一致，则会减弱当前下降的梯度幅度。

##### Nesterov Momentum

当前点的velocity和**下一点**的gradient做向量和作为最终的更新方向。

---

##### AdamGrad

> 梯度很大的时候后边的点会具有很大的动量，这时候震荡会非常大
>
> 作为actual step当中的**惩罚项**，作为分母。抑制了陡峭方向上的震荡

##### RMSProp



---

##### Adam

综合考虑了

第一动量：velocity

第二动量：AdamGrad/RMSProp 陡峭方向上的**惩罚**

actual step则为第一动量除以第二动量

---



> 以上只是引入了一阶梯度，还有二阶梯度，即看是凹凸性
>
> ...这样会有更快的收敛，但是会带来更大的计算量
>
> 深度学习是承受不住二阶优化这样的计算量的:joy:



#### Activation

##### Sigmoid

> 常用于逻辑回归层（二分类）

##### Softmax

> 多分类

##### Relu

> 计算机视觉

##### Thanh

> 循环神经网络、深层对抗神经网络



#### Metrics

> 评价函数和 [损失函数](https://keras.io/losses) 相似，**只不过评价函数的结果不会用于训练过程中**。

这句话的意思应该是*Metrics*不会参与优化



>  `Dense`层就是所谓的全连接神经网络层

> 模型需要知道它所期望的**输入**的尺寸。出于这个原因，顺序模型`sequential`中的第一层（**且只有第一层**，因为下面的层可以**自动地**推断尺寸）需要接收关于其输入尺寸的信息。



#### **理解**`input_dim`和`input_shape`

**其实官网上的解释已经很清楚了**：

> 某些 2D 层，例如 `Dense`，支持通过参数 `input_dim` 指定输入尺寸，某些 3D 时序层支持 `input_dim` 和 `input_length` 参数

也就是说`input_dim`是用来指定一维张量或说n维向量的输入的，而`input_shape`是用来指定n维张量的输入的，功能更强大。

为什么不好好读文档！！！

example:

`input_shape=(100,64,32)`表示这是一个100x64x32的3维张量，应该是样本量的张量维度，不包括数据量，数据量是在`batch_size`中给出的。

`input_dim=784`表示输入是784维向量，也就是一维张量，也可以表示为`input_shape=(784,)`

> 元组中只包含一个元素时，需要在元素后面添加逗号
>
> ```python
> tup1 = (50,)
> ```



> 如果你需要为你的输入指定一个固定的 batch 大小（这对 stateful RNNs 很有用），你可以传递一个 `batch_size` 参数给一个层。如果你同时将 `batch_size=32` 和 `input_shape=(6,8)` 传递给一个层，那么每一批输入的尺寸就为 `(32，6，8)`。

意思就是，`input_shape`和`input_dim`都是指定**数据量**的张量维度，而`batch_size`用来“静态”地指定样本量的维度。如一个彩色图片的输入应该为`input_shape=(64,64,3)`。



##### 关于张量`tensor`，请好好读[这篇文章](https://blog.csdn.net/ztf312/article/details/72859014?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

> [[1],[2],[3]] 这个张量的shape为（3,1）或写作3x1-->2维张量（因为有3、1两个数字:joy:）
> [[[1,2],[3,4]],[[5,6],[7,8]],[[9,10],[11,12]]]这个张量的shape为（3,2,2）或写作3x2x2-->3维张量
> [1,2,3,4]这个张量的shape为（4，） 或写作4-->1维张量

感觉生活中说的二维三维都是说的向量，他们都是一维(1D)张量

- 这篇博客有一点没有讲太清楚：
- 多维张量其实是由多个1维张量组成，这一点非常重要！每一个向量只是一个实体的特征集合，**只用来描述特征**，有多少个特征就有多少维。比如说一个rgb通道的图像，宽度上的特征有1080个，也就是有1080个像素，长度有860个像素，而颜色有3个坑（特征）需要填上数字。
- 也就是说，**每维张量的值是与具体的值无关的，只于有多少个特征有关**！
- 理解我在本子上记录的纽交所股票问题，我提出了套娃的类比，其实是不准确的。



#### 理解embedding层

总体流程：一般使用到embedding层的都是分类问题，用来理解一句话的情感，比如积极或消极。将一句话用embedding层将句子中每一个词语都嵌入到同一个向量空间中后（即得到词向量），将这个句子中所有的词向量平铺为一个高维向量，用这个高位向量作为dense层的输入来进行分类问题。

> 如果我能用一个向量来表示这个字，这个向量能够表达这个字的特点。
>
> 比如(255,255,255)这是白色的rgb颜色码，（100,100,100）是灰色的rgb颜色码。那么如果我的向量这样表达——白：（255,255,255,0,0） 灰：（100,100,100,0,0）诶，**这个向量他有灵性**。
>
> 那如果我能**把编码与字的属性合一**，那么想当然，会给我的网络带来很多便利。

**参数：**

**input_dim**: 输入的每个词的维度，**可以**用独热向量编码，则这时就为唯一单词的个数。比如一句话中使用了100个单词，那么用独热向量编码的话此时的`input_dim`就为100。

**output_dim**:  输出的维度，将词语赋予了灵性，让它具有在语义中的特点，这个时候就可以不用像`input_dim`的维度那么高了，只要能表达它在`input_length`中的语义就行了，比如取32可能是个很好的选择。官网文档用词就非常准确，只是自己理解不深刻：词向量的维度

:disappointed: ​**input_length**: 这个参数最难理解，过分难，[深度学习中Keras中的Embedding层的理解与使用](https://blog.csdn.net/sinat_22510827/article/details/90727435?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)这篇文章中阐释得非常好！

> input_length：这是输入**序列**的长度，就像您为Keras模型的任何输入层所定义的一样，**也就是一次输入带有的词汇个数**。例如，如果您的所有输入文档都由1000个字组成，那么input_length就是1000。

当然大部分情况下一个句子的长度是不固定的，比如“天霸动霸tuang”和“灰鸡公尾灰”两句话的`input_length`分别为5、6，需要预处理序列化为固定长度的输入(`pad_sequences()`)。所以才会有[官网](https://keras-zh.readthedocs.io/layers/embeddings/)文档上的说明：

> **input_length**: 输入序列的长度，**当它是固定的时**。 如果你需要连接 `Flatten` 和 `Dense` 层，则这个参数是必须的 （没有它，dense 层的输出尺寸就无法计算）。

:star::star:**这么一说，`embedding`层的输入其实是以一个文本或说一个句子为单位作为输入的，一个句子有n单词 输入向量就是n（`input_size`），这个向量的每个元素其实是每个单词的独热编码，用十进制表示**。那么输出其实是$$n \times m$$的，$$n$$ 是这个句子中单词的个数，$$m$$是每个单词$$embedding$$向量维度。

> 重要的是，嵌入层的输出将是每个8维的4个矢量，每个单词一个。我们将其**平铺到**一个32个元素的向量上以传递到密集输出层。4x8作为Flatten层的输入，Flatten层进行平铺操作。

并且一般要规定一个文档的单词数`input_length`。真正进行独热向量编码等操作是在embedding层的内部，我们只需要给出每个文本的长度，每个单词独热向量编码的10进形式等就行了（用预处理函数 $$one\_hot()$$ 就可以实现）。

解释一下这一句话：

> 如果您希望直接将Dense层接到Embedding层后面，则必须先使用Flatten层将Embedding层的2D输出矩阵**平铺为一维矢量**。

的确哈，`Dense`层的输入应该是一个向量，这是基础知识哈。所以这个时候embedding层必须要固定句子长度。



对于NLP模型，输入用张量形式表示为：(句子样本数, 每个句子的长度`input_length`, 句子中每个词向量的维度`input_dim`)

#### 理解`Word2Vector`

- https://zhuanlan.zhihu.com/p/26306795

> 但这里的数学模型 f（比如神经网络、SVM）只接受**数值型**输入，而 NLP 里的词语，是人类的抽象总结，是符号形式的（比如中文、英文、拉丁文等等），所以需要把他们转换成数值形式，或者说——嵌入到一个数学空间里

不应该说是数学空间，准确说是向量空间。

> :star::star::star:  当模型训练完后，最后得到的其实是**神经网络的权重**，比如现在输入一个 x 的 one-hot encoder: [1,0,0,…,0]，对应刚说的那个词语『吴彦祖』，则在输入层到隐含层的权重里，只有对应 1 这个位置的权重被激活，这些权重的个数，跟隐含层节点数是一致的，从而这些权重组成一个向量$$v_x$$ 来表示x，而因为每个词语的 one-hot encoder 里面 1 的位置是不同的，所以，这个向量 $$v_x$$ 就可以用来唯一表示 $$x$$。

上面加粗的 神经网络的权重 更准确说应该是**神经网络的权重向量构成的矩阵！**即**权重矩阵**

这句引用非常之关键，再解读一下那就是这个模型的输出层结果没啥大用，就只是为了计算并降低损失函数而已，仅此而已。最终我们要的其实是这个权重向量。

这应该就是使用独热向量编码的精妙之处，不仅唯一编码一个词语，同时他本身就已经表示了output layer的最优情况：$$x_i$$为1就是概率百分百，对于输出的优化目标就是对应的$$y_i$$也为1，二其余的所有$$y_j$$都为零。



同时还要提醒，虽然这个权重向量矩阵其实是共享的，即所有的独热向量输入都共享这个矩阵，但是正如上面的三:star:引用所说，实际上这个**共享矩阵**的列向量（$$W \cdot x$$，$$x$$ 是输入独热向量 $$v$$ 维列向量，做矩阵乘法虽然是和权重矩阵$$W$$的一行相乘，但是它是独热的所以只会激活$$W$$的一列）其实是只被一个独热输入独享的，即一个独热向量输入只可能激活隐含层矩阵的一个列向量。

同时，这个词向量$$v_x$$的维度其实是由隐含层的维度所决定的，如图：

<img src="https://my-typora-img-bed.oss-cn-chengdu.aliyuncs.com/img/word2vec.png" alt="word2vec" style="zoom:80%;" />

这一点感悟来源于 [通俗理解word2vec](https://www.jianshu.com/p/471d9bfbd72f)中对CBOW模型阐述的第二点：

> 2 所有onehot分别乘以共享的输入权重矩阵W. {V*N矩阵，N为自己设定的数，初始化权重矩阵W}*



对文中`3.2.1`的那张图得有下面这样的理解才算到位：

<img src="https://pic3.zhimg.com/80/v2-a1a73c063b32036429fbd8f1ef59034b_720w.jpg" alt="img" style="zoom:67%;" />

输入层是个独热向量，然而输出层是每一个神经元都是有值的，而非输入层这样独热的，表示的是**概率分布**(因为是onehot嘛，其中的每一维斗都代表着一个单词)，优化的方法和普通的神经网络的优化方式是一样的，如SGD，Nestrov，Momentum等。



#### 再次理解word2Vector

- [一文详解 Word2vec 之 Skip-Gram 模型（结构篇）](https://blog.csdn.net/qq_24003917/article/details/80389976)

> 当这个模型训练好以后，我们并不会用这个训练好的模型处理新的任务，我们真正需要的是这个模型通过训练数据所学得的参数，例如隐层的权重矩阵——后面我们将会看到这些权重在Word2Vec中实际上就是我们试图去学习的“word vectors”。

也就是说隐含层的输出维度n维才是我们的最终目的。先前的对word2vec我也强调过这一点，足以见得这个观点的重要性。

输入一个one-hot编码的单词维度为m，然后接入一个dense层，维度为n，也就是说这时的**权重矩阵**为mxn。这是一定的，输出为n维矩阵也是一定的。

> **所以我们最终的目标就是学习这个隐层的权重矩阵。**

如果这篇博客只有一句不是废话，那么一定是这一句。

文中还提到了计算这个极高维度的input（100000维甚至更高）的one-hot向量和权重矩阵相乘的低效问题的解决方法，总结一下就是：**检查表**。查找对应one-hot输入为1的 $$i$$ 列对应的权重矩阵的i行即可，这变得异常easy了：

![一文详解 Word2vec 之 Skip-Gram 模型（结构篇）](https://static.leiphone.com/uploads/new/article/740_740/201706/594b322ae0c72.png?imageMogr2/format/jpg/quality/90)

> 。。。这样模型中的隐层权重矩阵便成了一个”查找表“（lookup table），进行矩阵计算时，直接去查输入向量中取值为1的维度下对应的那些权重值。隐层的输出就是每个输入单词的“嵌入词向量”。

关于输出层：

![一文详解 Word2vec 之 Skip-Gram 模型（结构篇）](https://static.leiphone.com/uploads/new/article/740_740/201706/594b31d0920ef.png?imageMogr2/format/jpg/quality/90)

注意一下输出层是一个和Input同样维度的向量就行，因为这是 $$softmax$$ 的分类器。













#### 结合协同过滤模型

上面的 $$embedding$$ 和协同过滤的原理相似，感觉都是进行反推或说逆向操作。在协同过滤模型中，$$Rating\,\,Matrix$$矩阵其实是最终的优化标准（目标），而通过训练得到的其实是全连接层的参数：

<img src="https://pic2.zhimg.com/80/v2-fea147450dbfc25328c8ed33476ea2ff_720w.jpg" alt="img" style="zoom: 33%;" />

其实我觉得确实这可以用全连接神经网络来做，将$$User\,\,Matrix$$的**行向量**作为全连接层的输入，$$Item\,\,Matrix$$**就是4个神经元的全连接层了**（我靠我发现了新大陆:laughing: ），而$$Rating\,\,Matrix$$就是最终的优化目标，最终我们需要的其实是这个$$Item\,\,matrix$$的列向量，其实际含义是item的向量化表示。

如果用keras来搭建dense的话，`batch_size`则为4，因为图中有4个用户(A, B, C, D)；`input_dim`为2，或者直接`input_shape=(2,)`

在[万物皆可embedding](https://zhuanlan.zhihu.com/p/109935332)一文中提到的$$Deep\,\,Neural\,\,Networks\,\,for\,\,YouTubeRecommendations$$模型，解决了我的一个疑问：$$User\,\,Matrix$$如何得到呢？是通过召回模型得到的。



#### RNN循环神经网络

- [一文搞懂RNN（循环神经网络）基础篇](https://zhuanlan.zhihu.com/p/30844905)

  这篇文章无意之中让我对以前忽视的隐含层的偏置值$$\,bias\,$$有了一定的理解。以前总以为其实隐含层重点就只在于构造出权重矩阵，它其中的神经元是没有啥值的意义的，不像输入层一样每一个神经元都有一个值。但是！我忽视了$$\,bias\,$$！，它其实就是每个神经元的值！

  - [每个神经元为什么要加上偏置？](https://blog.csdn.net/xwd18280820053/article/details/70681750)、[神经网络中偏置的作用](https://blog.csdn.net/mmww1994/article/details/81705991)

    > 如果没有偏置的话，我们所有的分割线都是经过原点的，但是现实问题并不会那么如我们所愿.都是能够是经过原点线性可分的。

    > 偏置值允许将激活函数向左或向右移位
    >
    > 改变w0的值就是改变sigmoid函数的陡峭程度，如果想让x=2时，输出值为0，**只改变wo的不同取值是无济于事的**，你需要把曲线往右移动。我们加入偏置...

- [Keras 函数[TimeDistributed]理解](https://blog.csdn.net/gukedream/article/details/86539640?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

  > 这样就实现了张量的**批量降维**
  
  











#### K-Means

- [K-Means（聚类）](https://blog.csdn.net/sinat_36710456/article/details/88019323)

  1. 其实很好理解这个算法的过程，这个过程感觉和我在LeeCode上刷的一个算法题的思路一样，初始化k个**质心***（Centroid），然后比较

  2. 选定质心之后的分类很妙！其实就是比较小弟离哪个质心的距离更近而已，这样说才发现很简单粗暴啊:joy:

  3. > 这里要注意选大哥的方法是每个人X坐标的平均值和Y坐标的平均值组成的新的点，为新大哥，也就是说这个大哥是“**虚拟的**”...









#### CNN学习

- [【子豪兄】深度学习之卷积神经网络](https://www.bilibili.com/video/BV1AJ411Q72b?from=search&seid=13226986090752640872)

  1. <img src="C:\Users\liuhaodong\AppData\Roaming\Typora\typora-user-images\image-20200921203724363.png" alt="image-20200921203724363" style="zoom:50%;" />

     看了这张图感觉CNN算是一种**表示学习**？因为最终都是提取了**特征图**这个东西，最终都是要拿到全连接层进行训练。

     为什么说是表示学习？来自刘老师的口述：（记录可能有些遗漏）

     > 深度学习区别于表示学习，表示学习对特征进行了提取
     >
     > 如像素图片用openCV做了个啥啥啥；TransE做的是h+r=t做损失函数、图表示学习

  2. 基本流程：卷积+池化+卷积+池化+全连接+全连接

     - 卷积层：提取图像的底层特征。简单来说，是将大像素的图像变成小的输出。（专业点说是**特征映射**？）
     - 池化层用于防止过拟合，并将数据维度减小
     - 学习目标是优化卷积核

  3. **卷积核**：可以理解为映射关系$$f$$！！而原始图像像素矩阵就相当于一个输入了，

     卷积核为$$n \times n$$，对应的感受眼也得是$$n \times n$$，每一次只映射出一个像素点。

     一个卷积核生成一个feature map（下图中的output）。

     输出结果称为**feature map**。若有多个卷积核，那么就会生成多个feature map，即**多通道。**

     <img src="C:\Users\liuhaodong\AppData\Roaming\Typora\typora-user-images\image-20200921205939473.png" alt="image-20200921205939473" style="zoom:50%;" />

  4. 池化（pooling）（下采样）：对feature map每个局部$$m \times m$$选择代表值，最大池化（最常用，有效防止过拟合）、平均池化。

     作用：

     - 减少参数量，防止过拟合
     - 减少参数的数据量的同时，保留数据的原始特征
     - 来带平移不变性

  <img src="C:\Users\liuhaodong\AppData\Roaming\Typora\typora-user-images\image-20200921214351059.png" alt="image-20200921214351059" style="zoom:50%;" />

  5. padding：防止像素边缘的特征被忽略掉，卷积眼

- [Convolution Visualizer](https://ezyang.github.io/convolution-visualizer/index.html)

  参数解释：

  1. input size:  输入矩阵$$n \times n$$
  2. kernel size: 卷积核$$m\times m$$
  3. padding:
  4. dilation（棋盘）:跨像素的感受眼
  5. stride（步长）:

- 多通道

  以RGB图像为例，输入为$$m \times n \times 3$$，3维张量，则卷积核应该为$$p \times q \times 3$$，这个时候就是多通道了（这里是3通道）。

  ##### 一句话，有多少个卷积核就有多少个feature map

  多通道是输入矩阵带来的，3通道的输入会导致卷积核也是三通道的，同时注意还是只能得到一个feature map

  <img src="C:\Users\liuhaodong\AppData\Roaming\Typora\typora-user-images\image-20200921213829112.png" alt="image-20200921213829112" style="zoom:50%;" />

6. 其实我觉得卷积层和池化层的区别应该是卷积核的策略不同吧，卷积层相当于使用了权重和每个像素点相乘，而池化层只是找到像素点的平均或者最大值啥的

##### 可以这么理解吗，卷积就是矩阵的全连接，而普通的全连接默认的是向量的



- 
- 卷积层提取出图像的局部特征，如下
- ![image-20200921223607563](C:\Users\liuhaodong\AppData\Roaming\Typora\typora-user-images\image-20200921223607563.png)