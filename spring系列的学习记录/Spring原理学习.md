### Spring Boot自动配置原理

参考了这篇文章，写得是非常详细了：[Spring Boot面试杀手锏————自动配置原理](https://blog.csdn.net/u014745069/article/details/83820511)，以及这篇文章：[SpringBoot自动配置原理！](https://mp.weixin.qq.com/s?__biz=MzI4Njg5MDA5NA==&mid=2247484637&idx=1&sn=956c14daacc3e09367d9c27458b09f7f&chksm=ebd745dcdca0ccca6c173d32b6f8299f61d950990ee7c6eb2ec676f5ce0ad9b0ba306306a952###rd)

我在这里在用自己的理解描述一遍这个原理。

首先还是注解`@SpringBootApplication`，是由三个注解组合而成：

```java
@SpringBootConfiguration----即表明支持JavaConfig的方式进行配置
@EnableAutoConfiguration----开启**自动配置**功能。这是重点！！！
@ComponentScan----装载@Service,@Repository,@Componet等自定义的类到IOC容器。“将主配置类（@SpringBootApplication注解的类）所在包及其子包里面的组件扫面到Spring容器中”
```

**区别：**其实需要区分的是后两个注解，我感觉区别是，:star:`@EnableAutoConfiguration`是自动载入Spring Boot框架为我们提供的一些**默认**的对象到IOC容器中，比如`jdbcTemplate`、`amqpTemplate`等，框架觉得我们可能会用到于是就帮我们先安排了。而`@ComponentScan`是载入我们自定义的类，比如一些service层的类，dao层的类。

**重点：**`@EnableAutoConfiguration`，它是如何实现自动配置的？

会扫描`META_INF/spring.factory`文件下的配置，导入key为 **`org.springframework.boot.autoconfigure.EnableAutoConfiguration`** 的value列表，一共有113个类，都是`xxxAutoConfiguration`格式。听名字已经很明确这是干啥的了吧：

![image-20201224233006333](C:\Users\liuhaodong\AppData\Roaming\Typora\typora-user-images\image-20201224233006333.png)

（注意那些 "\\" 只是换行符而已，这个value列表的值是以逗号分隔的。）

接下来可以看这些类的源码，发现他们里面大部分都有一个:star:**​`xxxProperties`**私有变量，它和`xxxAutoCongifuration`类紧密联系：

```java
@EnableConfigurationProperties(RabbitProperties.class)//自动配置类，给IOC容器中添加组件
...(其他注解)
public class RabbitAutoConfiguration {
    private final RabbitProperties properties;
    ...
}
```

```java
@ConfigurationProperties(prefix = "spring.rabbitmq")//默认配置信息
public class RabbitProperties {

	/**
	 * RabbitMQ host.
	 */
	private String host = "localhost";

	/**
	 * RabbitMQ port.
	 */
	private int port = 5672;
    ...
}
```

```yaml
//修改默认配置
spring:
	rabbitmq:
		host:127.0.0.1
		port:5677
```



看到没有！其实精髓就是把配置信息和配置类（JavaConfig）分类开来。我画了一个图表述他们之间的联系：

<img src="C:\Users\liuhaodong\AppData\Roaming\Typora\typora-user-images\image-20201224235943330.png" alt="image-20201224235943330" style="zoom: 67%;" />



下面的回答过于精辟，是面试时的标准回答：（来自我上面给的第一个链接）

> Spring Boot启动的时候会通过@EnableAutoConfiguration注解找到META-INF/spring.factories配置文件中的所有自动配置类，并对其进行加载，而这些自动配置类都是以AutoConfiguration结尾来命名的，它实际上就是一个JavaConfig形式的Spring容器配置类，它能通过以Properties结尾命名的类中取得在全局配置文件中配置的属性如：server.port，而XxxxProperties类是通过@ConfigurationProperties注解与全局配置文件中对应的属性进行绑定的。

