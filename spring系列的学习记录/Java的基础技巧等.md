### 零碎知识：



##### [Java 到底是值传递还是引用传递？](https://www.zhihu.com/question/31203609/answer/50992895)

本质上都是值传递，包括引用类型。只不过引用类型传递的是地址。

要想真正修改引用类型，表象上看就得调用方法，而不是赋值。赋值会改变该引用类型变量的值，也即指向的地址空间（heap）。

如经典的例子：

```java
String str = "123";
func(str);
void func(String str){
    str="987";
}
```

用文中的图来理解，调用`func()`时两个`str`（注意这是两个不同的`str`变量哦）值相同，也就是指向了同一段heap。

<img src="https://pic1.zhimg.com/80/d8b82e07ea21375ca6b300f9162aa95f_1440w.jpg?source=1940ef5c" alt="img" style="zoom: 50%;" />

然后执行`str="987"`将会成这样：

<img src="https://pic4.zhimg.com/80/46fa5f10cc135a3ca087dae35a5211bd_1440w.jpg?source=1940ef5c" alt="img" style="zoom:50%;" />

想要真正改变外层`str`变量（全局的`str`），就得而且只能通过**调用方法**来改变，比如`str.replace('1','0');`。

这和c语言的指针是相通的，要改变指针的**指向**的值就得通过`*pt=123;`，其实也可也理解为调用了方法，方法名称为`*`:joy:





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

##### 换行符\n还是\r\n？

这个甚至和操作系统都无关了，变换太大，最直接的方式就是充分利用正则表达式：

```java
String message="good\n,hello,\r\n";
message.split("\n|\r\n");//符号 | 便是匹配上一个即可
```



### :exclamation:[泛型](https://segmentfault.com/a/1190000014120746)

1. [泛型类](https://www.cnblogs.com/coprince/p/8603492.html)

> 定义的泛型类，就一定要传入泛型类型实参么？在使用泛型的时候如果传入泛型实参，**则会根据传入的泛型实参做相应的限制**，*此时泛型才会起到本应起到的限制作用*。如果不传入泛型类型实参的话，在泛型类中使用泛型的方法或成员变量定义的类型可以为**任何的类型**。

2. [**泛型方法**](https://blog.csdn.net/weixin_43819113/article/details/91042598)

   这是主要的难点！泛型方法上对泛型的声明让拓宽了思路，我发现方法上的参数申明其实包含两个方面：泛型参数声明和一般参数申明。

> 与类，接口中泛型参数不同的是，方法中的泛型参数**无须显式**传入实际类型参数，如上面程序所示，当程序调用 fromArrayToCollection() 方法时，无须在调用该方法前传入String、Object 等类型，但系统依然可以知道类型参数的数据类型，因为**编译器根据实参推断类型实参的值**，它通常推断出最直接的类型参数。

这应该指的是**类型推导**:stars:

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

3. [泛型擦除](https://segmentfault.com/a/1190000014120746)，整点底层的知识

   > 泛型是**提供给javac编译器使用的**，它用于限定集合的输入类型，让编译器在源代码级别上，即挡住向集合中插入非法数据。但编译器编译完带有泛形的java程序后，**生成的class文件中将不再带有泛形信息**，以此使程序运行效率不受到影响，这个过程称之为“擦除”。

   刚开始不是很理解什么叫 ”提供给 javac 编译器使用的“这句话，其实是因为我忽视了**javac**这个词。javac就是用来编译Java源码的，会进行一些语法句法等的解析然后编译成 .class 文件。

   我到现在才发现其实泛型只是一个**语法糖**而已，因为语法糖只作用在编译时期。在生成`class`文件过后就不会有泛型的信息了，只保留该类型的**类型上限**，一般是`Object`类。



**TIPS:**

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

这原来也是可以的！也就是其实泛型类是可以不指定类型的`List<String> list=...`，指定类型其实某种程度上是给自己看的，让自己写代码的时候不至于不小心传错了参数。



### 设计模式

我认为设计模式的三大类型还是需要搞清楚分类依据的。我大概思考了一下，感觉很有意思：

1. **创建型：**关注如何创建对象，核心：对象的创建和使用相分离；:arrow_right: 如何对象创建
2. **结构型：**如何组合对象​；:arrow_right: 对象之间的关系
3. **行为型：**算法（业务逻辑更具体点）和对象间的职责分配；:arrow_right:业务代码和对象间的关系

箭头后面是我自己的话提炼了一下。发现没有，其实这三大类是一个递进关系：首先要创建对象，如何创建？于是有了创建型设计模式；然后创建好了的对象之间的关系，如何组合更优？于是有了结构型；最后将这些对象如何更好地融入业务逻辑中，于是有了行为型。

#### 工厂方法或者抽象工厂

我甚至懒得区分二者之间的差别了，感觉都差不多，区别都不是本质。

我看了好些文章，发现有一个总结比较到位：[抽象工厂模式和工厂模式的区别？ - 黑暗中的灯光的回答 - 知乎]( https://www.zhihu.com/question/20367734/answer/266328444)

> 简单的说，抽象工厂是对简单工厂（工厂方法模式、工厂模式）中的**工厂类进一步抽象成接口**，解决了工厂方法中的硬编码问题，因为以后如有新增新的对象，只要再实现一个对应的工厂类，就完成了扩展。无需修改以前的代码。

简单工厂只抽象了产品，而抽象工厂连工厂也抽象出来了。:star::star::star:这个总结简直很难更好！！！



#### 动态代理

##### 实现方式一：JDK的Proxy类

**基于接口代理**。这种方式要求target类要有实现接口。

1. 区别于静态代理，动态代理的这两个方式都是代理（拦截）的方法，是一种粗粒度的

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

4. 对`InvocationHandler`的

   > 实现接口是一个类认干爹的过程。**接口无法创建对象，但实现该接口的类可以**。

5. [对`InvocationHandler`类的理解](https://www.zhihu.com/question/20794107/answer/658139129)

   个人理解：从字面意思理解，Invocation+Handler，即调用的处理类，调用谁？调用这个被代理的类中的方法

   > 所有对动态代理对象的方法调用都会转发到 `InvocationHandler` 中的 invoke() 方法中实现

   > `InvocationHandler`对象成了代理对象和目标对象的**桥梁**，不像静态代理这么直接

   相当于所有对目标对象的调用都会先进入到`invoke()`方法中，从而实现代理（代理=代理逻辑+目标对象）：

   ![img](https://pic1.zhimg.com/80/v2-b5fc8b279a6152889afdfedbb0f611cc_1440w.jpg?source=1940ef5c)

6. ```java
   public static Object newProxyInstance(ClassLoader loader,
                                             Class<?>[] interfaces,
                                             InvocationHandler h)
           throws IllegalArgumentException
   ```

   其实第一个参数loader应该随便哪个类，在动态代理下主要还是看interfaces这个参数，必须和目标类相通的接口或者抽象类等。然后就可以构造一个代理类了，不需要手动实现每个 

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

##### 实现方式二：cglib代理

**基于子类代理**。这种方式相较于第一种实现方式，变得更加”智能“，不要求target（被代理类）要实现接口。因为这种方式引入了`Enhancer`类，**将target类作为其父类**。

参考了好几篇文章，也在我的GitHub上有实现。

参考：[Spring【AOP模块】就这么简单](https://mp.weixin.qq.com/s?__biz=MzI4Njg5MDA5NA==&mid=2247483954&idx=1&sn=b34e385ed716edf6f58998ec329f9867&chksm=ebd74333dca0ca257a77c02ab458300ef982adff3cf37eb6d8d2f985f11df5cc07ef17f659d4&scene=21#wechat_redirect)、[cglib动态代理](https://my.oschina.net/u/264914/blog/4455981)





#### 原型模式

`Cloneable`接口中有这么一段：

> Invoking Object's clone method on an instance that does not implement the Cloneable interface results in the exception CloneNotSupportedException being thrown.

翻译：在一个对象中调用`Object`类的`clone`方法但是没有implement `Cloneable`接口会导致抛出`CloneNotSupportedException`异常。

只提这一句话，好好读读`Cloneable`类的doc，很有意思















### 2020/11/7，一个猜测：（待解决）

学习了一下反射机制，里面提及了`ClassLoader`，目前的看法是应该很多的class文件加载到内存都会共用一个`ClassLoader`，到底用哪个`ClassLoader`是由JVM决定的，但是注意，别用`Object.getClassLoader()`，

线索：

1. 其实`ClassLoader`类才是”老祖宗“，`Class.forName()`其实也是调用`ClassLoader`中的方法来创建（load）class对象的：

   > 在Java中`Class.forName()`和`ClassLoader`都可以对类进行加载。`ClassLoader`就是遵循**双亲委派模型**最终调用启动类加载器的类加载器，实现的功能是“通过一个类的全限定名来获取描述此类的二进制字节流”，获取到二进制流后放到JVM中。`Class.forName()`方法实际上也是调用的`CLassLoader`来实现的。

2. [这篇文章](https://www.jianshu.com/p/ebdf0eb76088)说得挺好，其中的例子：

   ```java
   class CustomClassLoader extends ClassLoader {
   
     Class findClass(String name) {
       // 寻找字节码
       byte[] code = findCodeFromSomewhere(name);
       // 组装Class对象
       return this.defineClass(code, name);
     }
   }
   ```

     非常好！`findCodeFromSomewhere()`这个方法是核心，也是实现`findClass()`的核心部分：自定义、手动寻找需要的字节码文件（.class），然后将其与name参数绑定（调用`defineClass()`）

3. 这是`loadClass()`的源码：

   ```java
   protected Class<?> loadClass(String name, boolean resolve)
           throws ClassNotFoundException
       {
           synchronized (getClassLoadingLock(name)) {
               // 查看是否已经加载过该类，加载过的类会有缓存，是使用native方法实现的
               Class<?> c = findLoadedClass(name);
               if (c == null) {
                   long t0 = System.nanoTime();
                   try {
                       //父类不为空则先让父类加载
                       if (parent != null) {
                           c = parent.loadClass(name, false);
                       } else {
                       //父类是null就是BootstrapClassLoader，使用启动类类加载器加载
                           c = findBootstrapClassOrNull(name);
                       }
                   } catch (ClassNotFoundException e) {
                       // 父类类加载器不能加载该类
                   }
   
                   //如果父类未加载该类
                   if (c == null) {
                       // If still not found, then invoke findClass in order
                       // to find the class.
                       long t1 = System.nanoTime();
                       //让当前类加载器加载
                       c = findClass(name);
                   }
               }
               return c;
           }
       }
   ```

   

































### 快速调研

> spring、mybaties等框架的底层都是基于注解和反射

#### 注解：

- @SupressWarnings("all") 算是一个黑科技，可以把一个类乃至一个变量的黄色警告等给镇压了，但是并不建议这么做，除非自太多 自己看着烦了:joy:

- 元注解（meta-annotation）：

  @Target({ElementType.TYPE, ElementType.METHOD})

  @Retention(RetentionPolicy.RUNTIME)

  @Inherited、@Document

- `@Retention(Value="RetentionPolicy.RUNTIME")`

  表示我们的注解在什么地方有效

  runtime>class>source

- `@Inherited`

  子类可以继承父类的注解

- 自定义注解中的参数如果设置`int age() default -1`，”如果为-1，则代表不存在“。好比以前的indexof，如果找不到就返回-1

- 有一个不是很友好的不成为的规定，那就是定义的一个注解如果只有一个参数，那么这个参数只能是String value()，命名为value就可以在使用的时候省略value这个参数名了，如果是String name()这种，就不能省略参数名

- > 我们可以通过反射机制编程实现对注解的访问



#### 反射：

- > 加载完类之后，在堆内存的方法区中就产生了一个**Class类型的对象**（**一个类在内存中只有一个Class对象**），这个对象包含了该类的完整信息
  >
  > 这个对象就像一面镜子，透过这个镜子看到了类的结构
  >
  > 一个类被加载后，类的整个结构都会被封装在class对象中

- 获取Class类的实例的四种方法：

  1. `Class clazz = Person.class;`：若已知某具体的**类**，通过类的class属性来获得
  2. `Class clazz = person.getClass();`：若已知某个类的**实例**，通过该实例的getClass()方法来获取
  3. `Class clazz = Class.forName("demo.Student");`：前提是知道该类的**全包名**，而非类的路径。如`Class.forName("com.mysql.cj.jdbc.Driver");`
  4. （了解）`Integer.TYPE`：基本**内置**数据类型的封装类有一个TYPE属性

- 这里就可以补充以前学JDBC的时候`Class.forName("com.mysql.cj.jdbc.Driver");`这段代码的作用 了：（[参考](https://blog.csdn.net/m0_45067620/article/details/109169247)）

  >我都知道 `Class.forName()`是要加载一个类，不知道大家还记得不记得**类被加载时**是会执行**静态代码块**的，好像执行之一次
  >所以A和B看似没关系，其实它们俩有“不可告人的秘密”，我敢打保证`com.mysql.cj.jdbc.Driver`这个类绝对有个**static代码块**，其中肯定有与B相关的代码结构：

  <img src="https://img-blog.csdnimg.cn/20201019211839536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ1MDY3NjIw,size_16,color_FFFFFF,t_70#pic_center" alt="在这里插入图片描述" style="zoom:70%;" />

- [学习java应该如何理解反射？](https://www.zhihu.com/question/24304289/answer/694344906)这个回答信息量很大！

  - 先说说对`ClassLoader`的理解：将*.class字节码文件**load**到内存中，并且返回Class对象。该类主要就做这一项工作。
  
  - Class类的理解：
  
    - <img src="https://pic1.zhimg.com/80/v2-d4cb040cceebe0dab651c95a0c22c7f0_1440w.jpg?source=1940ef5c" alt="img" style="zoom:50%;" />
  
      >可以发现，Class类的构造器是私有的,我们无法手动new一个Class对象,**只能由JVM创建**。JVM在构造Class对象时，需要传入一个类载器，然后才有我们上面分析的一连串加载、创建过程。
  
      ```java
       /*
           * Private constructor. Only the Java Virtual Machine creates Class objects.
           * This constructor is not used and prevents the default constructor being
           * generated.
           */
          private Class(ClassLoader loader) {
              // Initialize final field for classLoader.  The initialization value of non-null
              // prevents future JIT optimizations from assuming this final field is null.
              classLoader = loader;
          }
      ```
  
      翻译：私有构造器，**只有JVM能够创建Class类的对象**。这个构造器是不被使用的，它存在的目的只是为了**避免**默认的构造器（`public Class(...)`）被调用
  
      ```java
      以下是对Class类的表述：
      /*
      *Class has no public constructor. Instead Class objects are constructed automatically by the Java *Virtual Machine as classes are loaded and by calls to the defineClass method in the class loader.
      */
      Class对象没有public修饰的构造器。相反，Class类的对象是由JVM在类们被装载入内存后自动构造的，并由ClassLoader的`defineClass`方法调用来构造的。
      ```
  
      
  
  - 一个比较细节的点：`Constructor`和`Class`类都有`newInstance()`方法，但是`Class`对象的`newInstance()`：
  
    > 底层就是调用**无参**构造（Constructor）对象的`newInstance()`。
  
    >所以，本质上Class对象要想创建实例，其实都是通过**构造器对象**。<u>如果没有空参构造对象,就无法使用</u>`clazz.newInstance()`，必须要获取其他有参的构造对象然后调用构造对象的`newInstance()`。
  
    > 所以，要想调用`clazz.newInstance()`，必须保证编写类的时候有个**无参构造**。
  
- 学到这里，我倒是渐渐能够理解`Class`对象的一种经典定义了：包含一个类（准确的说是A.class文件）的所有信息的对象，且**一个类在内存中有且只有一个`Class`对象**。

- 一个无意中捕获的线索：（[来源](https://www.zhihu.com/question/29996850/answer/47429324)）

  > 另一个是由于使用反射或动态语言而导致不断有新类加载，但之前被加载的类没有被卸载导致类元数据所使用的内存空间越来越多。

  我的理解是，Java如果没有引入反射，就是静态语言，在程序编译过后产生的.class字节码文件会被加载到内存生成class对象，**并且在RUNTIME阶段 这些class对象就会一直在内存中，不会有任何改变或新增class对象，只能读取用来生成相应的对象**，这也是静态的原因。根据此思考，反射带来的动态应该就是能够修改这些class对象（只是猜测，很有可能不可以，看了 `Class` 源码基本都只有get方法，没有set或add），或者**创建新的class对象**，比如动态代理。









### Spring AOP

- [Spring AOP的实现原理 ?](https://www.zhihu.com/question/23641679/answer/704897152) 

  其中一个例子很恰当：

  >目标方法的return是整个递归责任链的精华所在，就像一个弹簧,被压到最大限度,开始return了。所以，原路返回，执行每个拦截器`invoke()`方法中两个语句的下一句。

  弹簧很形象，开始压弹簧的时候是在执行before链，压到最低点之后便执行目标方法（文中的`target.add()`），最后释放弹簧执行after链。





---

### 函数式接口：Java 8的新特性

> 写在前面：本质是**回调机制**，而且一个很有意思但我过了很久才发现的点是，那些能够采用lambda表达式，更本质说是实现回调的参数，无一列外都是`Interface`，这其实是显而易见的，但是很少有总结这个现象。

这个概念的定义是非常精辟的：**有且仅有一个抽象方法的的接口**(`Interface类型的类`)

我特么发现，这个所谓的函数式接口不也还是回调吗？？？只不过“有且仅有一个抽象方法”，所以可以用lambda表达式简化而已嘛。

其实可以复盘一下下面的forEach()的调用过程就清楚了：

首先是forEach()的源码：

```java
default void forEach(Consumer<? super T> action) {
    Objects.requireNonNull(action);
    for (T t : this) {
        action.accept(t);//这是函数式编程的核心部分，只给出调用方式，然后内容（业务逻辑）开发者自己写
    }
}
```

一目了然，本质上还是回调，回调的是action.accept()；

如何调用？实现这个accept方法：

```java
list.forEach(i->System.out.println(i));
```

上面的`action.accept(t)`其实最终是调用了这个$$lambda$$表达式。



关于**回调**：通过上面的例子明白了，其实**回调业务逻辑**在于` action.accept(t);`，这里的逻辑还是比较简单的，仅仅是将数据t传入然后让具体业务进行处理，我把这个过程称为**数据业务逻辑**，这里便会才用到lambda表达式了。

为了清晰理解回调的过程，我分为 回调业务逻辑 和 数据业务逻辑 这两个part，感觉就好理解些了。

在写回调业务逻辑的时候，是不用考虑数据业务逻辑的，毕竟也是面向接口编程。我非常”崇拜“那种有返回值的回调，比如：

```java
T value=action.callBack(
    args1,
    args2,
    i->{//数据业务逻辑
        i++;
        ...;
        }
)
```

value就是返回值，这是不是很妙。





参考了[这篇文章](https://www.cnblogs.com/dgwblog/p/11739500.html)，这篇文章爱了爱了​ :heart:

#### 函数引用和lambda表达式

感觉这两个都是语法糖，<u>并且都是为了配合函数式编程才创造的</u>。看两个例子就会了：

```java
List<Integer> list = new ArrayList();
list.add(1);list.add(2);
//lambda表达式的使用
list.forEach(i->System.out.println(i));

//函数引用
list.forEach(this::func1);//注意。如果func1是static的，那么就应该是Main::func1，Main是类名
void func1(int i){
    System.out.println(i);
}
```

lambda表达式目前发现只能用在函数式接口这种场景下，也就是是对**函数式接口的简化，而不是对某个普通方法的简化**。

参考[这篇文章](https://developer.51cto.com/art/202009/627588.htm?pc)

[在Java代码中写Lambda表达式是种怎样的体验？](https://www.zhihu.com/question/37872003/answer/1009015660)这篇文章的这句话真的很精辟：

> 我们使用Lambda表达式创建线程的时候，**并不关心接口名，方法名，参数名**。我们**只关注他的参数类型，参数个数，返回值**。

注意哦，下面图中的都是`Interface`，本质上是要实例化的，也就是`new`的。

![img](https://pic2.zhimg.com/80/v2-d4ff58a1bad0b37266d009e7047dec95_1440w.jpg?source=1940ef5c)

[java基础---->java8中的函数式接口](https://blog.csdn.net/weixin_30260399/article/details/97747649)这篇文章对这几种类有详细的例子

[详解JAVA8函数式接口{全}](https://www.cnblogs.com/dgwblog/p/11739500.html)



lambda表达式里面其实还是有一个细节需要在此强调一下，看下面的代码和注释：

```java
//注意这里有个细节，用lambda表达式但是setCallback()的参数接受的是Callback类型，虽然MethodInterceptor是其子类但是这里还是要强转一下
enhancer.setCallback((MethodInterceptor) (o, method, objects, methodProxy) -> {
            System.out.println("before invoke");
    		//为什么是invokeSuper()而不是直接invoke？这是因为这个enhancer类是作为我的目标类的子类的回调
            Object obj =  methodProxy.invokeSuper(o,objects);
            System.out.println("after invoke");
        });
```

#### 重点阅读文章：[Lambda表达式](https://www.yuque.com/books/share/2b434c74-ed3a-470e-b148-b4c94ba14535/gilh34#ELAV7)

这篇文章收获巨大，关键还是思维的转变，也就是Java的面向对象的思维需要转变，编程函数式编程的思维。这样的话这个知识点就比较好理解了。

这篇文章有一句话挺关键：

> 不要关注当前lambda将会如何被调用，出现在代码的哪一块，而是关注如何编写lambda，要实现什么样的逻辑。

这篇文章同时也回答了**为什么传递给匿名内部类的参数必须声明为final？**。我特地复习了一下C语言的内存结构，联系了起来，我发现其实关键字`new`得到的对象其实是放在**堆**中的，相当于C语言`malloc()`得到的。于是除非显式释放掉这段空间或者整个程序结束，而不会像栈中的方法一样，调用完毕后就自行释放。于是在方法作用域内的变量传递给内部类是不合法的，因为该变量一定会在方法调用完毕后释放。





---

### 类加载器，双亲委派模型

##### 参考文章一：[好怕怕的类加载器](https://zhuanlan.zhihu.com/p/54693308)

- 这篇文章写得太全面了，我简直跪了：

  > **面试官插嘴：ExtClassLoader为什么没有设置parent？**
  >
  > 因为BootstrapClassLoader是由c++实现的，所以并不存在一个Java的类，因此会打印出null，所以在ClassLoader中，null就代表了BootstrapClassLoader（有些片面）。

  文章中有提到如何实现一个`ClassLoader`，惊喜的是使用到了**模板方法**模式！原来实现一个`ClassLoader`用到了该模式：只需要重写`findClass()`方法即可，看完`ClassLoader#loadClass()`源码其实就能明白原因。

  （越看到后面越看不懂。。。歇歇再看。。。:joy:）

- 但是目前看到的地方得注意一下，有一点讲的非常关键:star::star::star:

  > 会抛出ClassCastException，因为一个类，就算包路径完全一致，**但是加载他们的ClassLoader不一样**，那么这两个类也会被认为是两个不同的类。

  这一点是新的知识，很厉害！

- > Returns the class with the given binary name if this loader has been recorded by the Java virtual machine as an initiating loader of a class with that binary name. Otherwise null is returned.
  >
  > 将输入的binary name（即Java类的全名）对应的类返回，前提是***当前* 加载器**已经被JVM记录为具有这个binary name的类的启动类，反之返回`null`。（前提是...后面的话很重要）

  注意上面加粗，只有被**当前**类加载器加载的才会返回哦，其他类加载器加载的是不会返回的。

##### 参考文章二：[Java 类加载器（ClassLoader）的实际使用场景有哪些？](https://www.zhihu.com/question/46719811/answer/1739289578)

- > 因为一个类的**全限定名**以及**加载该类的加载器**两者共同形成了这个类在JVM中的惟一标识，这也是阿里pandora实现依赖隔离的基础。

  与上面参考文章的第三点表达的是同一个意思。这里有空的时候可以调研一下潘多拉。

- 总结一下双亲委派模型下各个加载器所加载的类：

  - `Bootstrap ClassLoader`：<JAVA_HOME>/lib 下，即以`java`开头的**基础**类，如`java.io.* `；
  - `Extention Classloader`：<JAVA_HOME>/lib/ext下，即以`javax`开头的JVM**扩展**类；
  - `Application Classloader`：自己编写的代码和第三方jar包，写pom文件下的依赖涉及的类都得是这个加载器加载；
  - `Custom Classloader`：自定义。





---

### SPI—service provider interface

> 一旦代码里涉及**具体的**实现类，就违反了可拔插的原则，如果需要替换一种实现，就需要修改代码。

用我的大白话讲，就是将一个接口的**实现**通过配置文件的形式装配，这一点至少思想上和IOC一致，只是实现的方式有所不同。

参考：[Java中的spi机制](https://blog.csdn.net/sigangjun/article/details/79071850)、[Java SPI思想梳理](https://zhuanlan.zhihu.com/p/28909673)

<img src="https://pic3.zhimg.com/80/v2-9db1c292ae743dad89615c2a41d967b6_1440w.png" alt="img" style="zoom:67%;" />



