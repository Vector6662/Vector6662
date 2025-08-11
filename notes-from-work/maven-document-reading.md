# Introduction to the Dependency Mechanism

[Introduction to the Dependency Mechanism – Maven](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Dependency_Scope)

## <scope> tag

Overview: [maven中强大的scope标签详解_maven中scope标签详解-CSDN博客](https://blog.csdn.net/wohaqiyi/article/details/119631500)

##### import

只在`<dependencyManagement>`下，且<type>pom</type>。实现类似多继 - 一个pom project只能有一个`<parent>`标签，将所有`<dependencyManagement>`都放在parent中不利于管理。

似乎可以将其理解为类似#include？仅作简单的文本合并。

##### provided - 不具备传递性

只影响compile, test 阶段，仅仅为了让代码能够通过编译，不被打包进jar。runtime classpath提供运行时环境（比如JDK, container）。

运行时环境比较抽象，看这个例子：[[1117]maven依赖中scope=compile和provided区别](https://cloud.tencent.com/developer/article/1979673?shareByChannel=link) - servlet classpath下的servlet-api与pom中的冲突

```java
Caused by: java.lang.LinkageError: loader constraint violation: loader (instance of org/apache/catalina/loader/WebappClassLoader) previously initiated loading for a different type with name "javax/servlet/ServletContext"
```

## Tip: `<scope>provided</scope>` :vs: `<optional>true</optional>`

[Maven中scope=provided和optional=true的区别 - CharyGao - 博客园](https://www.cnblogs.com/Chary/p/18230649)

简单来说`<optional>`要求子项目**按需引入**。`provided`在上边已有解释。
两者基本不想干。不要被两者所谓的“最终效果相同”混淆，这是看到很多文章中强行找相同之处所带来的不必要的误导。

这里以多种数据库解决方案引入作为例子能帮助理解，也可以阅读：[maven 中 provided 与 optional 的区别](https://zhuanlan.zhihu.com/p/172470329)

## 依赖调解简介

Overview: [Maven依赖调解机制解析-CSDN博客](https://blog.csdn.net/wohaqiyi/article/details/119908921)

官方文档：[maven的依赖调解机制](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Transitive_Dependencies)

## [TODO]如何理解(BOM)Bill Of Materials?

[Introduction to the Dependency Mechanism – Maven](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Bill_of_Materials_.28BOM.29_POMs)

## [TODO]MOJO - Maven Plugin Development

core pom tag: `<packaging>maven-plugin</packaging>`

[Maven插件编写之初识Mojo](https://qchery.github.io/2017/03/26/Maven%E6%8F%92%E4%BB%B6%E7%BC%96%E5%86%99%E4%B9%8B%E5%88%9D%E8%AF%86Mojo/):  每一个MOJO类就是一个执行目标（executable goal）

Two ways to create MOJO class: [Apache Maven 自定义plugin插件的开发和使用](https://blog.csdn.net/yy8623977/article/details/125541653)

- maven-plugin-api
- maven-plugin-annotations

## Maven依赖冲突troubleshooting

[解决Maven依赖冲突-CSDN博客](https://blog.csdn.net/xyr05288/article/details/51438500)

```shell
mvn dependency:tree -Dverbose
```

## classpath概念

compile, test, runtime各有一套classpath

## POJO(Plain Old Java Object)
