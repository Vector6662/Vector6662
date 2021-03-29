#### 一些基本概念

image和container的关系相当于面向对象中类和对象的关系

#### 创建docker镜像的全过程

阅读[官方文档](https://docs.docker.com/get-started/02_our_app/)，收获颇丰，我对这个过程的每个步骤作出自己的理解：

:one: Get the app：项目该怎么写就怎么写，不受docker的影响；

:two:Build the app’s container image：关键是在于**Dockerfile**文件，

> In order to build the application, we need to use a `Dockerfile`. A Dockerfile is simply a text-based script of instructions that is used to create a container image.
>
> 为了构建这个应用，我们需要使用DockerFile文件。简单来说DockerFile是一个文本形式的脚本，用来创建container image

[**Dockerfile best practices**](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)对这个文件进行了详细的指南。

接下来是输入命令创建这个image，如官方指南中的` docker build -t getting-started .`

到此为止一个image就创建完毕了！发现没有，一个docker的image和普通项目相比就是多了一个Dockerfile文件。

:three: Start an app container：没啥好说的，相当于`new image(...)`，`...`无非就是参数列表。







#### Dockerfile

参考：[**Dockerfile best practices**](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

`FROM`需要提一嘴，比如`FROM node:12-alpine`，

> You might have noticed that a lot of “layers” were downloaded. This is because we instructed the builder that we wanted to start from the `node:12-alpine` **image**. But, since we didn’t have that on our machine, that image needed to be downloaded.
>
> 你可能已经注意到很多的layers被下载了。这是因为我们告诉builder我们需要从node:12-alpine这个**image**开始。...

[来源](https://docs.docker.com/get-started/02_our_app/#start-an-app-container)

注意到没！！！`FROM`的内容也是一个image！也就是说`FROM`的内容也是image而已，具体来说就是当前我们要构建的image所倚赖的image（用依赖不是很合适，直接用英文最好： we wanted to **start from** the `node:12-alpine` image）

找到一个对Dockerfile文件比较全面的描述：:hot_pepper::hot_pepper:

- 一个文本文件
- 包含了一条条指令
- **每一条指令构建一层，基于基础镜像，最终构建出一个新的镜像**

:star:最后一句最有价值，里面的**基础镜像**也就是`FROM`后跟的image。同时我也注意到，原来一个条命令都会构建一个镜像的，最终的镜像是叠罗汉叠出来的。