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

##### Java构造器的继承问题

> 父类 仅仅声明了有参构造函数，没有自己声明无参构造函数，则子类必须通过`super()`来调用父类的构造器

> 当我们不写有参构造函数的时候，系统会自动生成一个无参构造函数

原因还是很显而易见的，子类之所以要继承父类是因为子类要调用父类的一些方法或者属性等，那么这一定就得初始化父类否则调用无从谈起





##### 代理模式---动态代理

1. 不论是动态还是静态代理，目标都是一样的：**代理和目标对象（或说被代理对象）都要实现同一个或多个相通的接口**

2. 有点意思，似乎所有对象（实例）的父类都是Object，而所有类的父类都是Class对象。[来源](https://www.zhihu.com/question/20794107)

   > 所谓的Class对象，是Class类的实例，而Class类是描述所有类的，比如Person类，Student类

   > 代理类和目标类理应实现同一组接口

3. ```java
   Method method = new ...;
   method.invoke(Object obj, Object... args);
   /**
       Params:
       obj – the object the underlying method is invoked from(用我自己的话说：哪个实例要调用该方法？这里就填写这个示例)
       args – the arguments used for the method call
   */
   ```

4. 对InvocationHandler的

   > 实现接口是一个类认干爹的过程。**接口无法创建对象，但实现该接口的类可以**。

5. [对`InvocationHandler`类的理解](https://www.zhihu.com/question/20794107/answer/658139129)

   > 所有对动态代理对象的方法调用都会转发到 InvocationHandler 中的 invoke() 方法中实现

   > InvocationHandler对象成了代理对象和目标对象的**桥梁**，不像静态代理这么直接

   相当于所有对目标对象的调用都会先进入到`invoke()`方法中，从而实现代理（代理=代理逻辑+目标对象）：

   ![img](https://pic1.zhimg.com/80/v2-b5fc8b279a6152889afdfedbb0f611cc_1440w.jpg?source=1940ef5c)

6. ```java
   public static Object newProxyInstance(ClassLoader loader,
                                             Class<?>[] interfaces,
                                             InvocationHandler h)
           throws IllegalArgumentException
   ```

   其实第一个参数loader应该随便那个类，在动态代理下主要还是看interfaces这个参数，必须和目标类相通的接口或者抽象类等。然后就可以构造一个代理类了，不需要手动实现每个

7. > 代理对象的本质就是：和目标对象实现相同接口的实例。代理Class可以交任何名字，whatever，只要他实现某个接口，就能成为该接口类型。

8. 动态代理的作用是什么？

   > 1. Proxy类的**代码量被固定下来**，不会因为业务的逐渐庞大而庞大；（个人感觉这才是最关键的因素，静态代理代码量实在太多。动态代理）
   > 2. 可以实现AOP编程，实际上静态代理也可以实现，总的来说，AOP可以算作是代理模式的一个典型应用；
   > 3. 解耦，通过参数就可以判断真实类，不需要事先实例化，更加灵活多变。

9. [这篇文章也是写得好啊](https://www.zhihu.com/question/20794107/answer/151028753)！提到了**通用代理类**这个概念：

   > 只要我们在调用代码处使用这个通用代理类去包装任意想要需要包装的被代理类即可 （拓展思考-优点）

10. 其实我感觉静态代理虽然麻烦但是更加灵活，因为它其实是对类的方法进行代理。而动态代理更是对对象本身进行代理，虽然也可以代理其中的方法，但是显得很麻烦。

11. [和AOP相结合](https://blog.csdn.net/rock154/article/details/80059344)

    > 放到 InvocationHandler 实现类invoke 方法里面的 这些出代码片段就是一个**切面**，本文Demo 中真实业务add() 就是**切入点**。



### :exclamation:[泛型](https://segmentfault.com/a/1190000014120746)

1. [泛型类](https://www.cnblogs.com/coprince/p/8603492.html)

> 定义的泛型类，就一定要传入泛型类型实参么？在使用泛型的时候如果传入泛型实参，**则会根据传入的泛型实参做相应的限制**，*此时泛型才会起到本应起到的限制作用*。如果不传入泛型类型实参的话，在泛型类中使用泛型的方法或成员变量定义的类型可以为**任何的类型**。

2. 主要难点还是[**泛型方法**](https://blog.csdn.net/weixin_43819113/article/details/91042598)

> 与类，接口中泛型参数不同的是，方法中的泛型参数**无须显式**传入实际类型参数，如上面程序所示，当程序调用 fromArrayToCollection() 方法时，无须在调用该方法前传入String、Object 等类型，但系统依然可以知道类型参数的数据类型，因为**编译器根据实参推断类型实参的值**，它通常推断出最直接的类型参数。

这应该指的是**类型推导**

```java
//这是一个泛型方法，<T>就是声明一个方法持有一个类型T   这句话非常重要！！！
//这里的?是T的子类，如果要把?也给显示化，那么：<T,K> test(Collection<K extends T> from, ...){...}
public <T> T test(Collection<? extends T> from, Collection<T> to){
    for(T element:from){
        to.add(element);
    }
}
//下面是调用test，类型推导出as的类型参数是String，ao的是Object
...
List<String> as=new ArrayList<>();
List<object> ao=new ArrayList<>();
test(as,ao);
```

**2020/11/2**：“类型推导”这个词反而不利于理解，应该说**String类型被作为参数，和变量as和ao一起被赋值**了，这个String类型的参数对应的形参就是**T**！（同时as和ao对应的形参是from和to）

真的不是啥很难很神奇的点，不就是类型参数化了吗，很难吗？？？一点也不难好吧！



##### 	TIPS:

1. [以及**泛型通配符**`?`和`T`的区别：](https://www.cnblogs.com/minikobe/p/11547220.html)

   ```java
   // 通过 T 来 确保 泛型参数的一致性
   public <T extends Number> void
   test(List<T> dest, List<T> src)
   
   //通配符是 不确定的，**所以这个方法不能保证两个 List 具有相同的元素类型**		这句话非常重要！！！
   public void
   test(List<? extends Number> dest, List<? extends Number> src)
   ```

2. 对泛型的理解有些“肤浅”，不能仅仅将泛型理解为`List<T> list` 中的T，表示这个列表每个元素的类型而已，如果是这样的化`Class<T> clazz`就无法解释了。用`Class<T>`来理解泛型更好，因为T就仅仅是将类型参数化。

3. 思考来自于Gson的用法之一[fromJson()](https://www.jianshu.com/p/bca8117ad49e)：

   ```java
   Person person = new Gson().fromJson(str, Person.class);
   ```

   ```json
   json字符串为：[{“name”:”name0”,”age”:0}]
   ```

   看看该方法的源码就知道了，的确如上面所说，不需要显式传入实际类型的参数，而且关键在于，调用该方法时根本不需要关注泛型符号，因为泛型的涉及本质就是为了让接口的调用者不用关注于类型，而**直接传入实参即可而不会因为考虑参数类型而犹豫不决。**

   ```java
   public <T> T fromJson(String json, Class<T> classOfT) throws JsonSyntaxException
   ```

4. [Class的泛型处理](https://segmentfault.com/a/1190000011743906)

   > 现在，Class有一个类型参数T, 你很可能会问，T 代表什么？它代表Class对象代表的类型。比如说，String.class类型代表 Class<String>，Serializable.class代表 Class<Serializable>。这可以被用来提高你的反射代码的类型安全。



我怎么感觉泛型是Java为了像python的字典一样能够传入各种类型的参数，但是不想局限于自己的强类型语言，而搞出的幺蛾子？这不是讽刺，我个人特别喜欢Java，对python dislike，比如下面的代码：

```java
List list = new ArrayList<>();
        list.add(11);
        list.add("dw");
        list.add(0.154);
        Map map=new HashMap();
        map.put(12,12);//相当于python中的字典dict
        list.add(map);
        System.out.println(list);//输出[11, dw, 0.154, {12=12}]
```

这原来也是可以的！也就是其实泛型类是可以不指定类型的`List<String> list=...`，指定类型其实是给自己看的，让自己写代码的时候不至于不小心传错了参数。

