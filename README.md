### Hi there 👋

<!--
**Vector6662/Vector6662** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

创建此仓库的目的是，经过一小段时间地参与导师的项目和学习，尝试创建了一些仓库用来管理每一个持续时间较长的开发，比如遥感影像`/Project-remote-sense`、企业实习`/Practice-in-enterprise`，并且将每个开发过程中学到的知识、心得放在相应的项目当中。但渐渐显露出了知识记录混乱的问题，最明显的例子就是`/Project-remote-sense`下的三个markdown文件：

- `/RabbitMQ调研.md`：顾名思义，这是本次开发过程中用到的有关rabbitmq的随手的记录；

- ` /README.md`：记录了python相关的知识点，如python中self的理解；

- `/关键信息搜集.md`：在写遥感二期开题报告时*调研* 的信息，有embedding之类的知识点

- `/遥感分工.md`：开发阶段的分工

很明显，这些文件中的内容不是特别合理，比如

1. `/README.md`当中难道不应该记录仅与**项目进度**、**阶段性会议**、新增业务等等相关的内容吗？`/遥感分工.md`放在`/README.md`当中再适合不过了！
2. `/README.md`当中记录的python相关知识学习其实是与**具体项目关联度不高的**，其实是可以分离出来的！这一想法来源于MVC分层架构:laughing: ，dao层和service层。像`/simple-git`仓库中的`/svn学习记录.md`等`/**学习记录`的文件完全可以单独划分为一类；



基于以上思考创建本仓库，并做以下约定：

1. **本仓库尽最大可能与具体开发和项目无关**，主要内容应该为具体的技术，如“python学习”、“eggjs学习“等；
2. 相应地，**除了本仓库以外的仓库尽最大可能仅与项目具体业务、安排规划相关，与具体技术无关**；

具体的落实idea有（待跟新）：

1. 其他仓库（姑且称为项目）中尽量只用一个md文件来记录开发进度等
2. 本仓库应该大部分都是md文件，用来记录各种`**学习`
4. 对上一点有了实例化：记录每天的**学习回顾**！比如今天学习了conda，在这里谈谈它与pip的区别和一些常用的命令等；
5. 这里还是主要以**天**为单位记录一下每天的开发心得，学习心得，而不要太设计具体的项目，特别不能提及如“xxx项目目前进展缓慢”之类的。同时，这里是开发心得学习心得，而不是开发计划！
6. 如果每天的日志没来得及写，那么可以临时记录在本子上，空了再总结到这里
6. 本来是想尝试将**零碎**的知识点也归纳到这个仓库上面，但是发现其实没有必要的，就**记录在本子上也是可以的**。这个仓库还是主要记录那些比较系统性的知识点吧。但是排版还是得尽量规范写吧:joy:



#### Logger

---

##### *2020/7/19    Sun.*

在开发Spring Boot接口的时候在*浏览器*上输入params来模拟提交时，参数的值为字符串的时候是不需要加 "" 的！我TM现在才注意到，我的天哪，我因为这个走了好多弯路！

在调试上面提到的参数问题时，用到了IDEA的断点测试功能，才发现这是一个极为强大的工具，不必再使用console打印这种稍微麻烦的方式了。而且还能发现console打印不能发现的问题，比如上面例子，localhost...?city="成都"，在断点测试上会出现""成都“”这种能表示出值的类型，便于更好的发现错误

其实eggjs的[开发教程](https://eggjs.org/zh-cn/tutorials/index.html)写得挺清楚的，只是我没认证看，或者说看不懂，因为一些基础性的知识没有很好的掌握，比如最近在看的Passport鉴权部分，对token的理解不到位。

还不是很清楚“日志”是个啥，感觉用处很大的样子

Spring Boot跨域问题解决方案似乎是个很复杂的问题？

##### *2020/7/21 Tues.*

开始学习keras，这个框架是一个比较轻松的框架，可能还得慢慢接触tensorflow。

昨天学习了eggjs的中间件和插件的使用方式：方法当中嵌套方法:laughing:这算是eggjs整合koa时候的一些缺陷吧。

使用JDBC Template再次实现了遥感项目的dao层，但是还是没法配置好mybatis依赖...

##### *2020/7/22 Wed.*

conda其实和maven、npm那些一样，都是包管理工具，当然也有pip但是那不是最先进的了。同时要注意conda的发型版本主要有两个：Anaconda和Miniconda。conda主要就三个关键概念：环境、通道、包，不同的环境(*Environment*)可以配置不同的包，包在Anaconda的pkgs目录下，通道也就是下载包的地址，还是用中科大的镜像好些。

对异步、高并发、多线程这样的名次的理解有了进步，简单来说，高并发就是一种**现象**，即大量的服务器请求，而异步和多线程都是处理这种现象的两种常用的方法。异步是在单线程的nodejs下常用的方式。

通过阅读keras的官方文档学习了张量(*tensor*)，这确实是一个很重要的概念。其实更重要的是如何根据情景表达一个张量。我本子上的纽交所的例子便是可以都思考的。

##### ***2020/7/29 Wed***

用mybatis的xml文件写mapper时，命名一定要手动加上`.xml`！这个为你一定要注意，这是个非常耗时的坑！

##### ***2020/8/1 Saturday***

一定程度上解决了push到GitHub上的问题，似乎用梯子有时也是无效的，同时发现一个很好的镜像：github.com.cnpmjs.org。多试试几种方法便会发现有效的

明天应该好好整理下这个周开发中的收获，也就是将本子上的东西整理到相应的*markdown*文件里边，最重要的是总结异步问题！

应该可以开始设计业务交流群的草图了，进度还是加快一点

##### ***2020/8/2 Sunday***

花费了大量的时间来弄那个阿里云图床，还是不可以。最近用闲暇的时间再搞搞。

本来是想尝试将零碎的知识点也归纳到这个仓库上面，但是发现其实没有必要的，就记录在本子上也是可以的。这个仓库还是主要记录那些比较系统性的知识点吧。

##### *2020/8/19 Wednesday*

公司进度：1. 完成数据库和接口文档的一轮评估，已提交修改结果，数据库表中有一个字段尚未确定，等待下一次评审。预计明天能够确定最终版本，并开始编码。

学校工作进度：1. 开始遥感一期接口测试，等待反馈，做出相应的修改；推送算法的python脚本尚未整合到系统中，等待后台接口测试完毕后整合如系统中。2. 教材翻译工作，目前进展较慢，需要较快进度。



##### *2020/8/22 Saturday*

业务交流群：1. 表注解，加上设计思路和对一些重要字段的详细说明； 2. 将接口文档合并在一起，不必分开，并且修改每个接口的顺序； 3. 尽快开始编码，数据库和接口基本已经确定；

遥感项目：

1. 推送列表需要修改：新增了一批遥感图像后应该是只推送给用户这一批新图像，而不是将该新增图像所对应的标签的全部图像都给推送了。
2.  picture图像的时间改成正常的时间，因为在测试的时候将图像的时间改为了如2027年这样的。（已完成）

##### *2020/8/23 Sunday*

下周任务清单：

1. 业务交流群的各个文档基本确定，尽快将业务层代码完成；
2. 教材翻译已完成12章，在周二完成第四章；
3. 遥感开发文档尽快撰写完成；

##### *2020/8/24 Monday*

遥感项目bug：

1. 精确检索部分重构代码，将开始时间 结束时间、标签、地名 分开查询，采用判断为空的神器StringUtils.isBlack()来判断，然后取交集。（已完成）
2. 管理员得到的是按照用户分类的生僻标签列表（已完成）
3. 模糊检索那边还是再找找bug
4. 删除生僻标签，通过userid和要删除的标签列表（代码已完成，但还未测试）
5. 考虑完全抛弃mq，用传统的httputil



业务交流群：

1. 调整接口文档顺序，如创建群聊、加入群聊、发送消息。。。（未完成）
2. 发送消息时要看群聊是否已经关闭了（未完成）
3. 创建群聊之前要判断该群聊是否已经存在，用business_uuid（未完成）
4. 前端参数全用business_uuid而非group_uuid，用user_uuid而非member_uuid（完成）
5. groups好像是MySQL的关键字，不利于用于作为表名（解决，改为chat_groups）



今天的收货：

1. StringUtils.isBlack()
2. mysql的safe-updates模式下update的where中要加上主键才能执行成功
3. 用equals方法比较的是字符串的内容是否相同，

##### *2020/8/25 Tuesday*

业务交流群：

1. 重复加入群聊和重复创建群判断；（已完成）
2. 发送群消息之前要判断是否群聊已经关闭；
3. 接口文档顺序
4. 昨天将groups表改为了chat_groups，因为MySQL关键字冲突问题。记得在sql脚本上面改一下，以及很多表的字段也得改



今日收获：

1. 普通对象的equals方法和String类型重写的equals，但是string类型的==是比较什么呢？

2. list的判断为空好像没有一个封装好的方法，只能两种方式同时使用（obj==null||obj.isEmpty()），那么可以自己写一个[对象工具类](https://blog.csdn.net/yaomingyang/article/details/78754486?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

3. 不是所有的对象赋值都是将它本身的地址赋值给你的变量，还是得看源码，有时方法会声明一个新对象作为返回值，如

   ```java
   List<CommonBean> newMembers = dataBean.getCommBeanList("members");
   ```
   
   







<!--用户登录名称 dong6662@1040353427875408.onaliyun.com
登录密码 2qLkJAJJ9wFnDZeZ!Xj(hx6H{vXn8&jw
AccessKey ID LTAI4G5E7dZM2CDowHL88UZP
AccessKey Secret 6Vs8xsrwe2Zty1p0izdQJ8TNnQflJ5

-->









#### PS:

##### EMOJI CHEAT SHEET

<img src="https://img-blog.csdn.net/20181007202942931" style="zoom:200%;" />