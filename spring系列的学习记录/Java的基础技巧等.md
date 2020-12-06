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

这原来也是可以的！也就是其实泛型类是可以不指定类型的`List<String> list=...`，指定类型其实是给自己看的，让自己写代码的时候不至于不小心传错了参数。





### 设计模式---动态代理

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

### ***2020/11/7，一个猜测：（待解决）***

学习了一下反射机制，里面提及了`ClassLoader`，目前的看法是应该很多的class文件加载到内存都会共用一个`ClassLoader`，到底用哪个`ClassLoader`是由JVM决定的，但是注意，别用`Object.getClassLoader()`，

线索：

1. 其实`ClassLoader`类才是”老祖宗“，`Class.forName()`其实也是调用`ClassLoader`中的方法来创建class对象的：

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



### 设计模式---原型模式

`Cloneable`接口中有这么一段：

> Invoking Object's clone method on an instance that does not implement the Cloneable interface results in the exception CloneNotSupportedException being thrown.

在一个对象中调用`Object`类的`clone`方法但是没有implement `Cloneable`接口会导致抛出`CloneNotSupportedException`异常。

只提这一句话，好好读读`Cloneable`类的doc，很有意思





































### 快速调研

注解和反射

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
  3. `Class clazz = Class.forName("demo.Student");`：知道该类的**全包名**，而非类的路径。如`Class.forName("com.mysql.cj.jdbc.Driver");`
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

