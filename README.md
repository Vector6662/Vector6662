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

- `/关键信息搜集.md`：在写遥感二期开题报告时*调研*的信息，有embedding之类的知识点

- `/遥感分工.md`：开发阶段的分工

很明显，这些文件中的内容不是特别合理，比如

1. `/README.md`当中难道不应该记录仅与**项目进度**、**阶段性会议**、新增业务等等相关的内容吗？`/遥感分工.md`放在`/README.md`当中再适合不过了！
2. `/README.md`当中记录的python相关知识学习其实是与**具体项目关联度不高的**，其实是可以分离出来的！这一想法来源于MVC分层架构:laughing: ，dao层和service层。像`/simple-git`仓库中的`/svn学习记录.md`等`/**学习记录`的文件完全可以单独划分为一类；



基于以上思考创建本仓库，并做以下约定：

1. **本仓库尽最大可能与具体开发和项目无关**，主要内容应该为具体的技术，如“python学习”、“eggjs学习“等；
2. 相应地，**除了本仓库以外的仓库尽最大可能仅与项目具体业务、安排规划相关，与具体技术无关**；

具体的落实idea有（待跟新）：

1. 其他仓库（姑且称为项目）中尽量只用一个md文件来记录开发
2. 本仓库应该大部分都是md文件，用来记录各种`**学习`
3. 本`README`文件其实可以比作`java`程序的main函数，但是具体的功能还没有想到