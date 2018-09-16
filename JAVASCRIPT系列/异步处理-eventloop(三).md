# JAVASCRIPT并发模型EventLoop

## JavaScript Engine 和 JavaScript Runtime

JavaScript 运行起来，要完成两部分工作（当然实际比这复杂的多）：

- **Engine**编译并执行JavaScript代码；（实现了JS的语言特性:原型，作用域）
- **Runtime**为JavaScript提供一些对象或机制，使它能够与外界交互。

> 举个栗子：
Chrome 和 Node.js 都使用了 V8 Engine：V8 实现并提供了 ECMAScript 标准中的所有**数据类型、操作符、对象和方法**（注意并没有 DOM）。
但它们的 Runtime 并不一样：Chrome 提供了 window、DOM、XHR，而 Node.js 则是 require、process、file 等等。 

JavaScript 是一种单线程的编程语言，只有一个调用栈，决定了它在同一时间只能做一件事情,但它又是一个非阻塞（Non-blocking）、异步（Asynchronous）、并发式（Concurrent）的编程语言，这些依赖**JavaScript Runtime**提供的**事件循环（Event Loop）机制**

## JavaScript事件循环执行模型

在执行和协调各种任务时，Event Loop 会维护自己的任务队列。任务队列又分为 Task Queue 和 Microtask Queue 两种。
### Task Queue(micratask)
- XHR回调
- 事件回调(鼠标键盘事件)
- setTimeout、setInterval
- IO操作(indexDB,webSocket)

### microtask
- Promise.resolve().then()
- MutationObserver

```javascript
    //一次循环称为一个tick
    while(waitForMessage()){ //当没有消息的时候，函数同步的等待
        //全局执行上下文，就是task queue中的第一个任务
        1.在多个task queue的队列中，由优先级决定在某一个队列中取一个task执行
        2.检查microtask队列，将队列中的task全部执行
        3.执行一次render(由浏览器自身决定是否执行DOM render,requestAnimateFrame)
    }
    //查看demo例子验证
```




## 应用场景

> 用Brendan Eich（JavaScript的发明人）的话来讲，如果JavaScript运行的时间需要用秒来计算，一定是什么地方搞错了。我个人可以忍受的上限可 能更小一些，不论什么脚本，在任何时间、任何浏览器上执行，都不应该超过100毫秒。如果实际执行的时间长于这个底限，一定要将任务分解成若干更小的代码段

```javascript
 //待处理的数据源
 let ds = [];
 //使用lodash将数据源分组
 let dataChunk = _.chunk(ds,500);
 let chunkLen = dataChunk.length;
 for(let i = 0;i < chunkLen; i++){
    //异步执行 任务队列
    //IIFE 创建闭包,缓存chunkIndex
    /*
    (function(chunkIndex){
        let handler = process.bind(null,chunkIndex)
        setTimeout(handler,0)
    })(i);
    */
    //得益于 let的块级作用域,不必创建
    let handler = process.bind(null,i)
    setTimeout(handler,0)
 }

 //处理任务
 function process(index){
     let data = dataChunk(index);
     //处理数据
     ....
 }

```

### 基于Promise的流程控制
```javascript
//自定义一个延时函数
 function sleep(millisecond){
     return new Promise((next)=>{
         setTimeout(()=>{
             next()
         },millisecond)
     })
 }

//基于链式的写法，顺序的执行
 Promise().resolve()
 .then(()=>{
     task1() //任务1
     return sleep(200)
 })
 .then(()=>{
    task2() //任务2
    return sleep(400)
 })
 .then(()=>{
    task3() //任务3
     return sleep(500)
 })
```

### 将Promise链式写法进一步封装
```javascript
//异步任务的顺序执行器
 let Sequence = function () {
    let i = -1,
        args = arguments,
        l = args.length;
    (function lambda() {
        return new Promise((next) => {
            i++;
            //把resolve当做参数传入
            if (i < l) args[i](next);
        }).then(lambda);
    })();
}

//使用方法：
Sequence(
		n=>{//任务一
			n();
		},
		n=>{//任务二
			n();
        },
        //...
)

```

## 检验理解的小例子
```javascript
    (
    function () {
        console.log('main task begining...')

        setTimeout(() => {
            console.log(`1-异步任务`);
            Promise.resolve().then(() => {
                console.log(`4-强势插入`)
            })
        }, 0);

        Promise.resolve().then(() => {
            console.log(`2`)
        })

        setTimeout(() => {
            console.log(3)
        }, 0);

        console.log('main task ending...')

    }
)()
```
# 参考链接
* https://zhuanlan.zhihu.com/p/25817653 setTimeout(fn,0)使用场景
* https://zhuanlan.zhihu.com/p/34229323 EventLoop
