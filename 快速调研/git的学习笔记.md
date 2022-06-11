<a name="1"></a>

#### git和GitHub之间的区别 

> Git是一款免费、开源的分布式版本控制系统

> Github是用Git做版本控制的代码托管平台



*![图片](https://pic3.zhimg.com/80/8c69712c0162dfafe8d921895a7341e6_720w.jpg)*



**[**参看资料见这里**](***https://www.zhihu.com/question/21907548***)**

---



#### HEAD和master等分支之间的关系



>  *git 中的分支，其实本质上仅仅是个指向 commit 对象的可变指针*



> *问：git 是如何知道你当前在哪个分支上工作的呢？*

> 答：它保存着一个名为 HEAD 的特别指针。在 git 中，**它是一个指向你正在工作中的本地分支的指针**，可以将 HEAD 想象为当前分支的别名。如图：



*![1.jpg](https://pic.leetcode-cn.com/4754a6ab31dcddfe35988be7420a3a5997e55d6f21e93921c6a4b9173e1521e1-1.jpg)*



[先阅读这篇文章，写的很好！](*https://blog.csdn.net/bdss58/article/details/40537859***)

https://www.jianshu.com/p/4419f6a76005

> 其实答案也很简单，它保存着一个名为 HEAD 的特别指针。在 git 中，它是一个指向你正在工作中的本地分支的指针，可以将 HEAD 想象为**当前分支**的别名。

HEAD=当前分支

- 每一个分支都有一个指针，如master分支有个master指针，dev分支有个dev指针。同时，**每一个指针都指向最近的一次提交**。

- **切换分支(checkout)**本质上是切换HEAD指针所指向的分支(master或dev)

- **合并分支(merge)**本质上是切换当前分支的指针到目标指针。如当前是master分支(也就是说HEAD指向master)，使用了**`git merge dev`**命令之后就将master指针指向了dev分支所在的commit。如图：



*![1.png](https://pic.leetcode-cn.com/a2dfdb669b0b8e5a3d90605eb66d304a4abd8cc55ec3656e64dbcdf3e52fa96b-1.png)*



---





####  从GitHub上推送(push)和获取(pull)分支



*![图片.png](https://pic.leetcode-cn.com/06b8162b33bb12eb2928e9296f9e6e9fbee3c83b5f0599c1457894272182bca0-%E5%9B%BE%E7%89%87.png)*



***\*[参考](https://www.zhihu.com/question/38305012)\****



***\*一些错误解决方案：\****



**-** 冲突解决



*![图片.png](https://pic.leetcode-cn.com/74a4bb81ee3481b04e66463d918ef4b805471cb434ddb57b1831ddc727393c0e-%E5%9B%BE%E7%89%87.png)*



这表示的是当前**本地**master分支的落后与**远端**的master分支，需要通过pull拉取并合并远端分支。



使用了pull命令后发现又出现了问题，也就是文件中产生了**冲突**：



*![图片.png](https://pic.leetcode-cn.com/7802b8efadc65945ae8fb7e672004b68ceef5f192ae432594a5b7d42943c3d5e-%E5%9B%BE%E7%89%87.png)*

>  Git 作了合并，但没有提交，它会停下来等你解决冲突。要看看哪些文件在合并时发生冲突，可以用 git status 查阅



文件中具体情况：

![图片.png](https://pic.leetcode-cn.com/382221759149b8579b0ba17c5559fa1566b7ff403e23bc4abbe500b15c720511-%E5%9B%BE%E7%89%87.png)

手动修改里面的内容然后再次add commit push 即可。

---

#### git rebase和 git merge

rebase移动的是分支，而merge命令移动的是分支指针





---

#### 分支branch

[先阅读一下，尤其是其中的分支的合并，因为例子中的合并情况比较特殊(non-faster-forward)](https://www.cnblogs.com/guge-94/p/11281724.html)



*![图片.png](https://pic.leetcode-cn.com/535c1883c5ef4ef915898bc6ec2d2dba169d1ffa7d8270997825abac715b0254-%E5%9B%BE%E7%89%87.png)*



感觉这种方式下就不能使用 **`git reset HEAD~n`** 这个命令了

#### 关于git commit -am ""的细节

```
$ git commit -am "变更的说明信息"     
```

加上-a参数，则不需要上一步的git add过程，可以直接将修改过的变更直接提交到版本库，**但不会把新增的文件提交**



---

#### 一些小的经验

##### 1. GitHub中分支之间的比较

*![图片.png](https://pic.leetcode-cn.com/16029c6eee9254934da5d5cce1d9fadede6503f90e313f26b9a785635d114b24-%E5%9B%BE%E7%89%87.png)*

记住这个路径即可.

##### 2. 一些markdown语法 

- [x] 选项1

- [ ] 选项2

如何实现的呢？·`- [x] 选项1`    `- [ ] 选项2`  注意要有每一个字符都有空格

:sparkles: moji表情

如何实现的呢？ 用关键词`:sparkles:`

[链接跳转测试](#1)



#### 设计一个任务来练习git指令的目的

1. Q：`git init` 指令和 `git clone` 指令使用的目录区别？

   > A：init指令用在建好的目录内，比如~/gitStudy/ 目录之下使用，而clone用在比如桌面

2. 查看当前username和Email,并且创建一个名叫gitStudy的仓库

3. 添加名叫1.md和2.md的文件，然后将其提交到版本库，变更信息为"第一次修改"。请使用两种办法

4. 随意修改，并且提交到版本库两次。变更信息为"第一、二次修改"

5. 用命令的方式查看当前版本(HEAD)的变更情况，并用语言简单描述

6. 回退两个版本

7. 创建一个名叫bc-a的分支，并且进行五次提交

#### [git报错：Pull is not possible because you have unmerged files解决方法](https://www.cnblogs.com/zhujiabin/p/10148803.html)

>  在git pull的过程中，如果有冲突，那么除了冲突的文件之外，其它的文件都会做为staged区的文件保存起来。

> 说明本地和远程的东西同时修改了workspace.xml文件，直接还原本地即可



#### 一次commit内容太多导致的问题解决方案

首先用`git remote -v`来查看提交的地址信息，可能是这样的：

```
$ git remote -v
origin  https://github.com.cnpmjs.org/Vector6662/Vector6662.git (fetch)
origin  https://github.com.cnpmjs.org/Vector6662/Vector6662.git (push)
```

接下来就是修改地址远端地址的信息了：

```
git remote set-url origin http://github.com/Vector6662/project-remote-sense
```

主要是关注协议，目前用http是可以提交的，**可能除此之外还要配置缓存大小**

除此之外也要修改下域名，不要那个啥cnpmjs，不知道这是啥东东。

> github.com.cnpmjs.org

哦，这是个代理，GitHub的镜像网站，大概实国内的。所以也许可以只改协议为http即可！



#### 使用JetBrain系列管理git仓库

这非常重要，至今才发现这是一个非常提升效率的工具，大大简化了git繁琐的操作！

