##### Java时间格式

[这篇文章](https://www.cnblogs.com/zhengwanmeixiansen/p/7391411.html)

这里有个较为标准的例子，是在我写代码的时候尝试出来的：

```java
try{
    String str="2020-8-2";
    //yyyy-MM-dd记住这个格式！注意大小写，或者 yyyy-MM-dd HH:mm:ss
	SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
	str=sdf.format(sdf.parse(str))
} catch(ParseException e){
    return "日期格式不合法";
}

```

使用这种方式可以对前端的日期格式的字符串输入进行校验，如果格式不合法则会被捕获。。

但是SQL中的时间格式是`%Y-%m-%d`，这和Java中的区别很大啊，一定要注意！

```sql
select * from pictures where DATE_FORMAT(acquisition_time,'%Y-%m-%d') between ? and ? 
```

[这篇文章给出了SQL的可用格式](https://www.w3school.com.cn/sql/func_date_format.asp),发现SQL日期同一个字母的大小写意思区分非常大，这一点和倒是和Java很像

##### `StringBuffer`、`StringBuider`，动态SQL解决方案

[这篇文章](https://blog.csdn.net/itchuxuezhe_yang/article/details/89966303)

可以在**动态SQL**中使用`StringBuffer`，下次写SQL时可以尝试学一下。同时mybatis也有自己的语句来写动态SQL，这可能就是mybatis现在还很常用的原因之一吧。

##### 使用mybaties的时候踩的坑

1. dao层如果有多个输入的参数，如

```java
public List<Pictures> findByStartTimeAndEndTimeAndLabel(String startTime, String endTime, String label){...}
```

那这种写法就是错的，**必须**加上`@Param`参数，如`@Param("startTime") String startTime`来指定，这样mapper层才能正确接受参数。

2. 关于mapper层，我他妈真的醉了，注意创建这个xml文件的时候要加上`.xml`这个后缀，虽然IDEA在你取好文件名的时候会让你选择文件类型，但**那只是方便IDEA自己识别文件类型而已**，不代表它在**运行**的时候被识别为`.xml`类型的文件。IDEA真的很“自私啊”:joy:给我的启发是，还是自己手动添加后缀吧，别偷这个懒。

##### 放弃lombok

在遥感项目中发现Lombok有自己的局限性，比如我想稍微重写下getter/setter的时候，还有一个很大的问题，就是当Date类型的属性用String类型输出的时候会被转换成时间戳，这是很不方便的！

##### 数组转换为List

```java
Arrays.asList(T[] array);
```

