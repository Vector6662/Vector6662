那个遥感项目需要使用到svn作为版本控制器，好在git没有白学，两者有很多相似的地方

1. 使用svn update更新本地的svn库，与服务器的库版本相同，与git pull的功能一样
2. svn add 和 svn commit都是一样的，但是要注意**每次commit之前都要update**
3. TortoiseSVN图标介绍：

![image-20200620222307141](C:\Users\82526\AppData\Roaming\Typora\typora-user-images\image-20200620222307141.png)

​	1->2：svn add; 

​	2->3：svn commit（在此之前要注意什么？）；

​	4：是modified文件状态，重复前面两个；

​	5：手动解决冲突，然后重复前面两个

还是截图一下吧，中英文对照：

<img src="C:\Users\82526\AppData\Roaming\Typora\typora-user-images\image-20200620232444295.png" alt="image-20200620232444295" style="zoom: 80%;" />