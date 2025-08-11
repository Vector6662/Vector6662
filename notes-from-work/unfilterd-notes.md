## application/x-www-form-urlencoded 历史故事

[在http请求中，空格被encode成'+' or '%20的历史 - 夏敏的博客 | Anderson's Blog](https://jerey.cn/android/2019/01/27/Http%E5%8D%8F%E8%AE%AE%E4%B8%AD%E7%A9%BA%E6%A0%BC%E7%9A%84%E7%BC%96%E7%A0%81%E5%A4%84%E7%90%86/)

## ElasticSearch调研

倒排索引原理： https://blog.csdn.net/qq_43403025/article/details/114779166

[全文搜索引擎 Elasticsearch 入门教程 - 阮一峰的网络日志](https://ruanyifeng.com/blog/2017/08/elasticsearch.html)

[ElasticSearch (ES从入门到精通一篇就够了) - 不吃紫菜 - 博客园](https://www.cnblogs.com/buchizicai/p/17093719.html)

## Webhook与Websocket

https://stackoverflow.com/a/24747947/17094981

不应该将两者进行对比，基本风马牛不相及；

前者使用的仍然是http协议，充其量是一种server间通信（server-server）的技巧（反向 API）；后者基于`wss://`，且仅用于bowser-server通信。

## Docker In Docker

:star:[mafeifan 的编程技术分享 - Docker In Docker](https://mafeifan.com/DevOps/Docker/Docker-%E5%AD%A6%E4%B9%A0%E7%B3%BB%E5%88%9727-Docker-in-Docker.html)

## EIP ESB SOA 微服务

重复一个基本事实：HTTP是一个应用层协议。这句话包含了“应用层协议”的责任和功能边界，关注的是通信双方的报文格式和语义，而不是传输的具体实现。

> ESB是SOA时代的技术，现在普遍微服务了，不会走总线了。（[评论区](https://zhuanlan.zhihu.com/p/97815422)）

> 对EIP的直接实现叫做EIP框架，知名的有两个：Apache Camel，Spring Integration。[EIP企业集成模式](https://zhuanlan.zhihu.com/p/97816198)

> EIP可以作为ESB的基础框架，在这个基础上填充其他必要的部分，定制出一个ESB容器（RestCloud是一种ESB产品）

MMT应该属于一种ESB架构，进一步说是iPaas（集成平台即服务）- SAP Cloud Integration（CPI）

### SOA与微服务

目前来看根本区别在于通信异构上，服务粒度只能算微服务的特点，因为SOA实际上也是可以根据具体需求做到细粒度。

:one:SOA - 有观点认为，SOA适合庞大且**异构**的企业级系统，系统久远，各个服务具有异构（不同的企业级技术、内部开发 外部采购），大规模重构成本巨大，存在<mark>通信异构</mark>，于是只能采用**兼容**的方式  :arrow_right: ESB。

:two:微服务 - 非常适合互联网项目，因为基本都是基于Web，所以即使开发技术（.Net Java PHP...）差异很大，但接口层提供的都是Restful、gRPC，不存在通信异构的问题。

:point_right:所以ESB的核心功能是解决这个通信异构问题，其中的<u>多协议转换能</u>力（协议适配）便是解决这个问题。

> 首先SOA和微服务架构是一个层面的东西，而对于ESB和微服务网关是一个层面的东西，一个谈到是架构风格和方法，一个谈的是实现工具或组件。（[来源](https://www.cnblogs.com/xuwc/p/13989081.html)）

#### 通信异构

参考：[SOA、ESB、微服务架构的区别和联系_servicemix soa 和esb的区别-CSDN博客](https://blog.csdn.net/liufangbaishi2014/article/details/123952192?fromshare=blogdetail&sharetype=blogdetail&sharerId=123952192&sharerefer=PC&sharesource=qq_37760028&sharefrom=from_link)

> ...但是ESB服务总线由于要考虑遗留系统的接入，因此增加了：
> 
> - **大量适配器实现对遗留系统的遗留接口适配，多协议转换能力**
> - **进行数据的复制映射，路由等能力**

<img src="file:///Users/I589335/Desktop/Vector6662/notes-from-work/images/ESB-API-gateway.png" title="" alt=" " width="612">

### WebService与Restful

[RPC是什么，与WebService有什么异同？](https://zhuanlan.zhihu.com/p/97640202)
总述。核心是基于XML的SOAP，将HTTP傀儡化。

[Restful与webService区别 - 星火燎原智勇 - 博客园](https://www.cnblogs.com/liang1101/p/6266305.html)

> REST是一种架构风格，其核心是<mark>面向资源</mark>；而webService底层SOAP协议，主要核心是<mark>面向活动</mark>；

> ...而是将Http协议的<u>设计初衷作了诠释</u>，在Http协议被广泛利用的今天，越来越多的是将其作为传输协议，而非原先设计者所考虑的应用协议。SOAP类型的WebService就是最好的例子，<mark>SOAP消息完全就是将Http协议作为消息承载</mark>，以至于对于Http协议中的各种参数（例如编码，错误码等）都置之不顾。

> 其实，最轻量级的应用协议就是Http协议（<mark>本身</mark>）。

[理解web service 和 SOA - Wayne-Zhu - 博客园](https://www.cnblogs.com/zhuzhenwei918/p/8745733.html)

简介了实现(XML) WebService的三大技术，这篇文章的价值并不在此。

但让我印象深刻的是从历史层面谈到了为什么SOA和WebService的深度绑定，这也是我之前没注意到的SOA的特征之一。有时思考有些老系统为什么要用SOAP这样老的协议的时候，和思考三国时期为什么不用火枪是一个道理。

### SOAP和SOA为什么联系紧密？

### 服务编排:question:







## Docker commands

[ENV 设置环境变量 · Docker -- 从入门到实践](https://docker-practice.github.io/zh-cn/image/dockerfile/env.html)

start a container with root:

```bash
docker run -u 0  -it devxci/mbtci-java17-node20 sh
```

## `npm install` vs `npm ci`

[continuous integration - What is the difference between &quot;npm install&quot; and &quot;npm ci&quot;? - Stack Overflow](https://stackoverflow.com/a/53325242/24890894)

## package-lock.json

深度解析package-lock.json作用 - Thomas Lin的文章 - 知乎
https://zhuanlan.zhihu.com/p/463697890 



## TODO List

### groovy基础语法学习 - 充分利用语法糖

目的在于优化pipeline语法，以及CPI开发过程。重点关注classpath使用

### 关于GitHub hook与iflow deploy集成

在集成的基础上考虑与AI的集成



### How to change docker image tag then push to remote?
