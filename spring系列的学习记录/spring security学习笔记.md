### Spring Security的原理

[这是一篇很好的文章](https://www.cnblogs.com/demingblog/p/10874753.html#1681857130)

众所周知 （1）想要对对Web资源进行保护，最好的办法莫过于Filter，（2）要想对方法调用进行保护，最好的办法莫过于AOP。

所以Spring Security在我们进行用户认证以及授予权限的时候，通过**各种各样的拦截器**来控制权限的访问，从而实现安全。

> 目前我的理解是Spring Security是所有拦截器（Filter）的管理配置者



Spring Security 应用级别的安全主要包含两个主要部分，即登录认证（**Authentication**）和访问授权（**Authorization**）。

首先用户登录的时候传入登录信息，登录验证器完成登录认证并将登录认证好的信息（应该是Authentication类）存储到请求上下文（应该指的是`SecurityContextHolder`），然后再进行其他操作，如接口访问、方法调用时，<u>权限认证器从上下文中获取登录认证信息</u>，

然后根据认证信息获取权限信息，通过权限信息和特定的授权策略决定是否授权。



---

### [HTTPBasic模式](https://baijiahao.baidu.com/s?id=1650164072428116018&wfr=spider&for=pc)和表单模式

服务器在收到这样的请求时，到达**BasicAuthenticationFilter**过滤器，将提取“ Authorization”的Header值，并使用用于验证用户身份的相同算法Base64进行解码。

> 上面的是一个线索，发现确实这种模式下还是使用了一个filter，这个也应该是在FilterChain上
>
> 不论是那种模式，应该能够肯定的是spring security规定都一定是要先登录的

两种登录模式对应的filter：

1. 表单模式：*UsernamePasswordAuthenticationFilter*
2. basic模式：*BasicAuthenticationFilter*

这两种方式都是由spring security本身提供的两种方式，这两种方式的账号密码都是由spring security自动生成的

<img src="https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20180707111356.png" alt="QQ截图20180707111356.png" style="zoom:80%;" />

> 除了上面两种过滤器以外还有别的过滤器，可以根据相应的配置开启

---

### [Filter、Interception、AOP](https://www.jianshu.com/p/c4ef6d232e8d)

<img src="https://upload-images.jianshu.io/upload_images/11382761-484d129572258d9c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" alt="img" style="zoom:67%;" />

### 一些极其重要的接口

- Authentication:

![image-20200629220305485](C:\Users\82526\AppData\Roaming\Typora\typora-user-images\image-20200629220305485.png)

- AuthenticationProvider

  > 

  AuthenticationProvider处理认证请求，然后返回授信后的完整认证对象。

- AuthenticationManger

  > ```
  > Processes an {@link Authentication} request.
  > ```

  

- AuthenticationManager和AuthenticationProvider这两个顶级接口

[AuthenticationManager也是一个顶级接口，可以看到它其中也定义了一个跟AuthenticationProvider一摸一样的方法。如果说AuthenticationProvider是对认证的具体实现，则AuthenticationManager则是对我们众多AuthenticationProvider的一个统一管理。AuthenticationManager的实现有很多，通常使用ProviderManager对认证请求链进行管理。](https://blog.csdn.net/chihaihai/article/details/104840650/)





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



## 重要线索!!!

[认证是通过AuthenticationManager#authenticate()实现的。也就是通过AuthenticationManager实现类ProviderManager的authenticate函数认证。认证的结果是返回一个Authentication实例](https://www.cnblogs.com/drizzlewithwind/p/6196080.html)

下面的过程概括为:  **对于一个Authentication，会被ProviderManager管理的`AuthenticationProvider`s轮询**

ProviderManager的authenticate函数会轮询ProviderManager的`List<AuthenticationProvider>  providers`成员变量，如果该providers中如果有一个AuthenticationProvider的supports函数返回true，那么就会调用该AuthenticationProvider的authenticate函数认证，如果认证成功则整个认证过程结束。如果不成功，则继续使用下一个合适的AuthenticationProvider进行认证，只要有一个认证成功则为认证成功。

如果上述过程没有认证成功，且该ProviderManager的成员变量AuthenticationManager parent不为null，那么会使用该parent继续认证。一般不会用到该AuthenticationManager parent，稍微留意以下即可。



来源：[![logo](http://www.tianshouzhi.com/logo-default.png)](http://www.tianshouzhi.com/)



---

### 非spring security的核心知识：

#### 有关异常Exception

- [Java中throws和try catch 的区别：](https://www.cnblogs.com/shuai7boy/p/12659156.html)

  Java异常处理主要通过5个关键字控制：try、catch、throw、throws和finally。

  try的意思是**试试**它所包含的代码段中是否会发生异常；而catch当有异常时抓住它，并进行相应的处理，使程序不受异常的影响而继续执行下去；

  throw是在程序中**明确**引发异常；

  :star:throws的作用是如果一个方法可能引发异常，**而它本身并不对该异常处理，那么它必须将这个异常抛给调用它的方法**，而调用它的方法**必须**handle the Exception，或者再次抛给它的调用者:joy:；

  finally是**无论发不发生异常**都要被执行的代码。

> 所以出现了`Unhandled Exception` 异常的话，说明某个方法在**方法声明**上已经声明了会抛异常,那么在调用这个方法的时候,就必须做异常处理,处理的方式有2种,要么**try-catch**这个异常,**要么继续往上一层抛出这个异常**,这是java语法要求的,必须这么做。

比如我曾经写过的代码：

```java
public static <T> void transToZip(String fromPath,String toPath, String fileName, List<T> picIdList) {
    ...
    FileInputStream fis = new FileInputStream(manyFiles.get(i));
	out.putNextEntry(new ZipEntry(manyFiles.get(i).getName()));
    ...
}
```

其中的`FileInputStream()`和`out.putNextEntry(...)`在方法声明上已经申明了抛出异常，所以要么使用`try{...} catch(Exception e){...}`来直接将这个异常处理掉，要么在这个方法`transToZip`上再次申明`throws IOException`抛给上一层来处理，**丢包袱，踢皮球**。

这里其实深一点讲涉及到了[异常的分类](https://blog.csdn.net/weixin_33915554/article/details/91865697?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)（这篇文章一定要看！），

> 只有java语言提供了Checked异常，Java认为Checked异常都是可以被处理的异常，所以Java程序必须**显示处理**Checked异常。如果程序没有处理Checked异常，该程序在编译时就会发生错误无法编译。这体现了Java的设计哲学：没有完善错误处理的代码根本没有机会被执行。对Checked异常处理方法有两种：
>
> 1. 当前方法**知道**如何处理该异常，则用try...catch块来处理该异常。
> 2.  当前方法**不知道**如何处理，则在定义该方法是声明抛出该异常。



#### [什么时候使用静态方法呢？](https://blog.csdn.net/u012442317/article/details/28960959)（调研不彻底，继续）

- 适当地使用static方法本身并没有什么，<u>当一个人从来不懂使用多态、接口设计时，很自然地会滥用static方法</u>。
- 个人理解在**多个类中**需要调用并且是与对象无关的方法可设为静态方法，方便调用。

#### :star:[为什么局部内部类和匿名内部类只能访问final的局部变量?](https://www.runoob.com/w3cnote/inner-lambda-final.html)（调研不足够，继续）

先需要知道的一点是: 内部类和外部类是处于**同一个级别**的，内部类不会因为定义在方法中就会随着方法的执行完毕就被销毁。

这里就会产生问题：当外部类的方法结束时，局部变量就会被销毁了，但是内部类对象可能还存在(只有没有人再引用它时，才会死亡)。这里就出现了一个矛盾：内部类对象访问了一个不存在的变量。为了解决这个问题，就将局部变量复制了一份作为内部类的成员变量，这样当局部变量死亡后，内部类仍可以访问它，实际访问的是局部变量的"copy"。这样就好像延长了局部变量的生命周期。我们可以通过反编译生成的 .class 文件来实验：...

> :collision: java中规定，内部类只能访问**外部类中的成员变量**，不能访问**方法中**定义的变量，如果要访问方法中的变量，就要把方法中的变量声明为final（常量）的，因为这样可以使变量*全局化*，就相当于是在外部定义的而不是在方法里定义的。

#### [final关键字的功能概述](https://www.cnblogs.com/chhyan-dream/p/10685878.html)

当用final修饰类时，该类成为最终类，无法被继承。简称为“*断子绝孙类*”。

如果引用为引用数据类型，比如对象、数组，则该对象、数组本身可以修改，但指向该对象或数组的地址的引用不能修改。

（具体例子见引用）

#### [Java中static class使用方式](https://blog.csdn.net/quanaianzj/article/details/82348982)

**在java中，不能用static修饰顶级类（top level class）。只有内部类可以为static。**





#### [JSON Web Token(JWT)](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)

JWT 的原理是，服务器认证以后，生成一个 JSON 对象，发回给用户，就像下面这样。

```json
{
  "姓名": "张三",
  "角色": "管理员",
  "到期时间": "2018年7月1日0点0分"
}
```

以后，用户与服务端通信的时候，都要发回这个 JSON 对象。**服务器完全只靠这个对象认定用户身份**。为了防止用户篡改数据，服务器在生成这个对象的时候，会加上签名（详见后文） Token 令牌。

> 相当于一张通行证，上面有领导签名，而不像以前的你进入学校后要登记，即不需要在门卫那里登记，所有的信息都在这张通行证上，包括有效期。

<img src="https://www.wangbase.com/blogimg/asset/201807/bg2018072303.jpg" alt="img" style="zoom: 67%;" />





#### @ManyToMany和@OneToMany和@OneToOne







---

#### Apache Ant匹配规则

| Wildcard | Description             |
| :------- | ----------------------- |
| ?        | 匹配任何单字符          |
| *        | 匹配0或者任意数量的字符 |
| **       | 匹配0或者更多的目录     |



#### Java中子类调用父类的构造器`super(...)`的理解

```java
public UsernamePasswordAuthenticationFilter() {
        super(new AntPathRequestMatcher("/login", "POST"));
    }
```

　在继承父类时要使用super关键字执行父类的构造方法，进行父类的创建，因为必须先创建父类才能创建子类，如果子类中没有写super(),会自动默认创建super()，

如果父类中**只有有参构造**，这时候子类不添加会报错，即需要手动实现。

> 原来要调用上面的例子中的父类只有有参构造器，于是此时的子类就必须实现这个父类构造器了。（这真的很基础，都给忘了:smile:）



#### [Java调用接口，就可以调用到接口实现类里面的方法](https://blog.csdn.net/drdongshiye/article/details/77145673?utm_medium=distribute.pc_relevant.none-task-blog-baidujs-1)

如`List<String> list=new ArrayList<String>();`这里的List就是接口，ArrayList是List的实现类

多态的特性，实现多态的方式有三种：重写、接口、抽象类和抽象方法。这里是接口的多态特性，并且使用的是**动态绑定**。

**注意！list只能使用ArrayList中已经实现了的List接口中的方法，而ArrayList中那些自己的、没有在List接口定义的方法是不可以被访问到的**

接口的灵活性就在于“规定一个类**必须**做什么，而不管你如何做”



---

### 翻译一些句子

1. ```java
   protected void configure( HttpSecurity httpSecurity ) throws Exception{...}
   ```

   Typically subclasses should not invoke this method by calling super as it may override their configuration

   > 通常子类不能通过调用`super`来激活这个方法，因为这样可能会**覆盖**掉他们的配置

2. 

   ```java
   /**
    * A strategy for storing security context information against a thread.
    *
    * <p>
    * The preferred strategy is loaded by {@link SecurityContextHolder}.
    *
    * @author Ben Alex
    */
   public interface SecurityContextHolderStrategy{
       ...
   }
   ```

   上面源码中的against应该翻译为“针对”好些，即*一种针对线程存储安全上下文信息的策略*。

   接下来的preferred应该译为“首选的”
