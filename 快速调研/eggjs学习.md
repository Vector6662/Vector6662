#### **为什么标题上说 eggjs 是一个优秀的 Node.js 框架（可跳过）？**

> nodeJs:一种javascript的运行环境，能够使得javascript脱离浏览器运行。

> Express 是一个基于 Node.js 平台的极简、灵活的 **web 应用开发框架**，它提供一系列强大的特性，帮助你创建各种 Web 和移动设备应用。

> [Koa](https://koajs.com/) 是一个新的 web 框架，由 Express 幕后的原班人马打造

> Egg 继承于 Koa

> 全局：执行npm install <模块的名字> -g 就会将模块装在全局路径下，当用户在程序中require(<模块的名字>)的时候不用考虑模块在哪，**如果不修改全局路径，用户下载的模块会默认在C:\Users\Administrator\AppData\Roaming\npm这个路径下**。
>
> 局部：执行npm install <包的名字>（注意少了-g）就会将模块安装在dos窗当前指向的路径下，这时候其他路径项目无法引用到该版本的模块！

#### 安装 **egg 脚手架以及使用 \**egg-init 创建项目\****

```npm
npm i egg-init -g	# 只用安装一次即可，是全局的module。默认位置在：C:\Users\82526\AppData\Roaming\npm\node_modules
egg-init egg-example --type=simple $ cd egg-example		#初始化项目，项目名称是egg-example --type是啥？？？
npm i	#似乎是下载项目文件夹下面的package.json
```

运行项目

```
npm run dev
open localhost:7001
```

#### [package.json](https://www.cnblogs.com/rong88/p/11664337.html)

> Node.js项目遵循模块化的架构，当我们创建了一个Node.js项目，意味着创建了一个模块，这个模块的描述文件，被称为package.json
>
> **亦即：模块的描述文件 = package.json**

> 1. package.json文件记录你项目中所需要的所有模块。**当你执行npm install的时候，node会先从package.json文件中读取所有dependencies信息，然后根据dependencies中的信息与node_modules中的模块进行对比，没有的直接下载，已有的检查更新（最新版本的nodejs不会更新，因为有package-lock.json文件，下面再说）。**另外，package.json文件只记录你通过npm install方式安装的模块信息，而这些模块所依赖的其他子模块的信息不会记录。
> 2. package-lock.json文件锁定所有模块的版本号，包括主模块和所有依赖子模块。当你执行npm install的时候，node从package.json文件读取模块名称，从package-lock.json文件中获取版本号，然后进行下载或者更新。因此，正因为有了package-lock.json文件锁定版本号，所以当你执行npm install的时候，node不会自动更新package.json文件中的模块，必须用npm install packagename（自动更新小版本号）或者npm install packagename@x.x.x（指定版本号）来进行安装才会更新，package-lock.json文件中的版本号也会随着更新。
> 3. 附：当package.json与package-lock.json都不存在，执行"npm install"时，node会重新生成package-lock.json文件，然后把node_modules中的模块信息全部记入package-lock.json文件，但不会生成package.json文件，此时，你可以通过"npm init --yes"来初始化生成package.json文件。

> 总结：
>
> - 项目中引入的包版本号之前经常会加^号，每次在执行npm install之后，下载的包都会发生变化，为了系统的稳定性考虑，每次执行完npm install之后会创建或者更新package-lock文件。该文件记录了上一次安装的具体的版本号，相当于是提供了一个参考，在出现版本兼容性问题的时候，就可以参考这个文件来修改版本号即可。
> - 重新install删掉lock

#### [npm几种方式的区别](https://www.jianshu.com/p/920c1a6e999c)

#### lambda表达式

>  Lambda 表达式的**参数列表的数据类型可以省略不写**，因为JVM编译器通过上下文推断出，数据类型，即“类型推断”



:question: eggJs单线程怎么理解



#### module.exports和exports

> :star: 如果模块输出的是`一个函数`，那就不能定义在`exports`对象上面，而要定义在`module.exports`变量上面。

```js
module.exports = function () {
  console.log("hello world")
}
//调用
require('./example2.js')()
```

> 上面代码中，`require命令调用自身，等于是执行`module.exports`，因此会输出 hello world。

这应该不是一个很好的暴露方式，至少应该封装到一个类里面	



> `Node`程序由许多个模块组成，**每个模块就是一个文件**。
>
> 根据`CommonJS`规范，一个单独的文件就是一个模块。每一个模块都是一个单独的作用域，也就是说，在一个文件定义的变量（还包括函数和类），都是私有的，对其他文件是不可见的。



[这篇文章最为本质：](https://blog.csdn.net/weixin_33957648/article/details/85856590?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

> #### exports = module.exports = {...}
>
> 我们经常看到这样的写法：
>
> ```javascript
> exports = module.exports = {...}
> ```
>
> 上面的代码等价于:
>
> ```javascript
> module.exports = {...}
> exports = module.exports
> ```
>
> 原理很简单：module.exports 指向新的对象时，exports 断开了与 module.exports 的引用，那么通过 exports = module.exports 让 exports 重新指向 module.exports。

也就是说初始状态下module.exports指向的是一个对象，exports是其跟班。如果`module.exports='hello'`，那么这个时候``module.exports`就指向了一个新的，

> 合法的javascript对象--boolean, number, date, JSON, string, function, array等等

此时`module.exports`就不再为`{...}`了！

举例：

```javascript
exports.hello = function() {
  return 'hello';
};

module.exports = 'Hello world';
```

> 上面代码中，`hello`函数是`无法对外输出`的，**因为`module.exports`被重新赋值了**。
> 这意味着，如果一个模块的对外接口，就是一个单一的值，不能使用`exports`输出，只能使用`module.exports`输出。

也就是说，无论是`module.exports`还是`exports`都不能直接光秃秃地`..=xxx`，而最好是`module.exports.yyy=xxx`，也就是说弄一个`yyy`来“隔”一下:laughing:

但是直接`module.exports=xxx`也是非常常用的，就是直接将这个模块导出为一个对象，通常是一个类，这也是很好用的！



#### async和await

> await 关键字 只能放在 async 函数内部， await关键字的作用 就是获取 Promise中返回的内容， 获取的是Promise函数中resolve或者reject的值。
>
> 如果await 后面并不是一个Promise的返回值，则会按照同步程序返回值处理



#### Context

> Context 是一个**请求级别的对象**，继承自 [Koa.Context](http://koajs.com/#context)。在每一次收到用户请求时，框架会实例化一个 Context 对象，这个对象封装了这次用户请求的信息，并提供了许多便捷的方法来获取请求参数或者设置响应信息。**框架会将所有的 [Service](https://eggjs.org/zh-cn/basics/service.html) 挂载到 Context 实例上**，一些插件也会将一些其他的方法和对象挂载到它上面（[egg-sequelize](https://github.com/eggjs/egg-sequelize) 会将所有的 model 挂载在 Context 上）。

#### Config文件

`config/config.{env}.js`

> 配置文件返回的是一个 object 对象，可以覆盖框架的一些配置，应用也可以将自己业务的配置放到这里方便管理。

其实说返回一个json对象也不为过，因为：

> 我们选择了最后一种配置方案，**配置即代码**

关于配置文件的两种写法区别提醒（挺细节的）

1. 返回的是一个方法，**但是这时一定要有return！**

```js
module.exports=appInfo=>{
    config={}
    config.key=appInfo.name + '_1594693641174_1196';
    userConfig={}
    ...
    ...
    return {
        //注意这里用到了扩展运算符"..."
        ...config,
        ...userConfig
    }
}
```

2. 返回的是一个object对象

```javascript
module.exports={
    logger: {
    dir: '/home/admin/logs/demoapp',
  }
}
```

两种方式我更倾向于使用第二种，但是如果有内置对象如appInfo需要调用那就只能第二种了:laughing:

这里的appInfo对象感觉也挺有用的，它的那些属性请注意一下



#### 一些有利于理解EggJs的线索

> eggjs非常重视约定。比如Controller、Service只能在文件夹demo/app/controller、demo/app/service下；而定时任务只要在demo/app/schedule下就可以**自动加载**。每个文件夹、文件根据位置即说明了其功能。	来自：https://www.jianshu.com/p/7bc342d64619

> 每一次用户请求，框架都会实例化**对应**的 Service 实例，由于它继承于 `egg.Service`，故拥有下列属性方便我们进行开发：
>
> ...

对应的service实例！！！！！！！！！！关键信息！！！！

> - 如上面例子中的 `ctx.request.query.id` 和 `ctx.query.id` 是等价的，`ctx.response.body=` 和 `ctx.body=` 是等价的。
> - **需要注意的是，获取 POST 的 body 应该使用 `ctx.request.body`，而不是 `ctx.body`**。



#### 一些ES6基础

1. ```javascript
   let result = await this.ctx.service[this.ctx.params.topic].list(this.ctx.query);
   ```

   es6基础，表达式作为属性名，如：

   ```javascript
   let books = {};
   let c3 = "new css";
   books.js = "javascript";
   books["new html"] = "html5";
   books[c3] = "css3";
   console.log(books);
   ```

   >  我们定义了一个books的空对象，我们可以直接用.给对象添加属性
   > 如果属性名存在空格，可以使用[""],也可以使用变量。
   
2. Js中一切皆为变量，连方法都有点强行的用参数的形式表达，比如：

   ```javascript
   var func0=function(){...}
   var obj={
       func1: function(){...}
   }
   //当然也有传统的方式声明方法
   function func2([params]){...}
   ```

3. `const`类型

   ```javascript
   const config={}
   config.dir='/.'
   config.base=.'///'	//这两种是可以的
   config={}	//不可以！因为const是常量类型，它本身指向的对象是无法改变的
   ```

4. :exclamation:扩展运算符`...`

   > 扩展运算符（spread）是三个点（`...`）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的**参数序列**。    [来源](https://es6.ruanyifeng.com/?search=map&x=0&y=0#docs/array)

   形象一点说就，`a=[0,1,2]`这个数组是被a这个“箱子”装住了，通过扩展运算符就把这个箱子拆掉，把里面的东西给”倒出来“

   在北奔接口项目的配置文件中，有

   ```javascript
   module.exports=appInfo=>{
       const config={}
       ...略
       const userConfig={}
       ...略
       
       return {
           ...config，
           ...userConfig
       }
   }
   ```

   将config和userConfig**拆箱**

   这里必须这么写！因为这个文件（module）只能暴露一个对象，也就是config对象。如果像上面的例子中途有两个对象







#### [CSRF攻击](https://www.cnblogs.com/yulia/p/10347691.html)

> 从以上的例子可知，CSRF 攻击是黑客借助受害者的 cookie 骗取服务器的信任，**但是黑客并不能拿到 cookie，也看不到 cookie 的内容**。
>
> 另外，**对于服务器返回的结果，由于浏览器同源策略的限制，黑客也无法进行解析**。因此，黑客无法从返回的结果中得到任何东西，他所能做的就是给服务器发送请求，以执行请求中所描述的命令，在服务器端直接**改变**数据的值，*而非窃取服务器中的数据*。

#### ES6的异步机制

##### Promise

async  await 如何实现异步调用的？

> **带 async 关键字的函数，它使得你的函数的返回值必定是` promise` 对象**
>
> 所以你看，async 函数也没啥了不起的，以后看到带有 async 关键字的函数也不用慌张，你就想它无非就是把return值包装了一下，其他就跟普通函数一样。 
>
> 来源：https://zhuanlan.zhihu.com/p/52000508

> async的函数中可以有一个或多个异步操作，一旦遇到await就会立即返回一个pending状态的Promise对象，暂时返回执行代码的控制权，使得**函数外的**代码得以继续执行，这是保证非阻塞的部分。
>
> 我们说的把异步变同步指的是在async函数内部异步代码就像被同步执行的那样（继发执行），而不是它会阻塞主线程一直等待异步调用返回。
>
> [JavaScript 中，用 async + await 和直接同步方式执行有什么区别？意义是什么？ - Murphy的回答 - 知乎](https://www.zhihu.com/question/62254462/answer/197769615)



> "同步模式"就是上一段的模式，后一个任务等待前一个任务结束，然后再执行，程序的执行顺序与任务的排列顺序是一致的、同步的；
>
> "异步模式"则完全不同，每一个任务有一个或多个回调函数（callback），**前一个任务结束后，不是执行后一个任务，而是执行回调函数**，*后一个任务则是不等前一个任务结束就执行*，所以程序的执行顺序与任务的排列顺序是不一致的、异步的。

这里的”执行回调函数“里面的应该是**依赖**当前任务的任务，而与当前任务不相关的就不用等待当前任务执行完毕就可以开始执行。比如当前任务是”读取文件“，与之相关的是”发送文件内容“，下一个任务是”打开电视“，那么这两个任务（或说function）就可以异步执行。而”读取文件“结束后执行的则是”发送文件内容“，即callback。

> **异步编程的最高境界，就是根本不用关心它是不是异步**



##### 从Generator中获得一些启发：

> 上面代码就是一个 Generator 函数。它不同于普通函数，**是可以暂停执行的**，所以函数名之前要加星号，以示区别。
>
> 整个 Generator 函数就是一个封装的异步任务，或者说是异步任务的容器。**异步操作需要暂停的地方，都用 yield 语句注明**。
>
> 来自: http://www.ruanyifeng.com/blog/2015/04/generator.html

>  Generator 函数的执行**必须靠执行器**，所以才有了 co 函数库，而 async 函数自带执行器。也就是说，async 函数的执行，与普通函数一模一样，只要一行。

似乎Genertor化的方法不会执行里面的return,也就是剥夺了方法的return功能，但是Promise的优势之一就是可以回复了return功能

> 回调函数真正的问题在于他剥夺了我们使用 return 和 throw 这些关键字的能力。而 Promise 很好地解决了这一切。

> 如果在函数中 `return` 一个直接量，async 会把这个直接量通过 `Promise.resolve()` 封装成 Promise 对象。
>
> 来源：https://segmentfault.com/a/1190000007535316



Promise似乎是内部实现了异步的，所以我觉得只要使用了它就自动帮我们实现了异步，不需要我们考虑这个执行过程



下面例子就很能说明问题了：

```js
async function getSomething() {
  await console.log('something')
  console.log('well')
}

async function testAsync() {
  await console.log('helloooooo')
}

getSomething();
testAsync();
//结果：
something
helloooooo
well
```

一个`async`就是相当于`promise`的`then`链，而且`getSomething`和`testAsync`是**并发的独立的**，而且阻塞都只是then链内部阻塞，比如getSomething里面的well输出就被阻塞了。

##### :exclamation:任务

- 做一个测试，编写一个Promise，里面可以写一个阻塞0.0001秒后执行，然后在这个promise后面写一个console，看看谁先执行？这个是为了证明这个Promise是否是异步执行的？

- 用callback也测试一下

- b站上找一下对promise和async/await的理解！

   

#### middleware

其实就是Java web里面的过滤器:joy:。​中间件不需要我们主动调用，它有点像一个触发器的具体的处理方法，事件就是request，传入几个参数然后进行自定义的处理。

> 中间件函数队列，会在最后一个中间件或一个**没有调用next的中间件**那里停止

> 每个中间件都是一个函数(不是函数将报错)，接收两个参数，**第一个是ctx上下文对象，另一个是next函数(由koa-compose定义)**	
> 来自：https://www.jianshu.com/p/02ed208d4577

中间件的写法比较特别：

```javascript
// app/middleware/gzip.js
const isJSON = require('koa-is-json');
const zlib = require('zlib');

module.exports = (options,app) => {
    //gzip为一个中间件函数
  return async function gzip(ctx, next) {
    await next();

    // 后续中间件执行完成后将响应体转换成 gzip
    let body = ctx.body;
    if (!body) return;

    // 支持 options.threshold
    if (options.threshold && ctx.length < options.threshold) return;

    if (isJSON(body)) body = JSON.stringify(body);

    // 设置 gzip body，修正响应头
    const stream = zlib.createGzip();
    stream.end(body);
    ctx.body = stream;
    ctx.set('Content-Encoding', 'gzip');
  };
};
```

为什么`module.exports`暴露一个有两个参数方法以后还要返回一个方法呢？我猜可能是因为eggjs是基于koa的，在koa中

> 每个中间件都是一个函数(不是函数将报错)

但是在eggjs中每个中间件都是要封装并存在于middleware目录下并且只能接受两个参数`(options,app)`，而在koa当中没有这一功能，只用给出一个中间件函数即可（如上面的`gzip(ctx,next)`）。这大概便是一个矛盾吧，因为在eggjs中的中间件明明是可以接受四个参数的，但是囿于koa定义的中间件只有两个参数(ctx,next)，所以不得不像上面这样**嵌套**一个`function`。



