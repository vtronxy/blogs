# JavaScript EventLoop 及 render UI

一个普遍的常识是DOM Tree的修改是实时的，而修改的**Render到DOM上才是异步**的。
根本不存在什么所谓的等待DOM修改完成，任何时候我在上一行代码里往DOM中添加了一个元素、修改了一个DOM的textContent，
你在下一行代码里一定能立马就读取到新的DOM，

而且浏览器内部为了更快的响应用户UI，内部可能是有多个task queue的：优先处理UI task



## micratask
取出一个task来执行
HTML中的UI事件、网络事件、HTML Parsing等都是使用的task来完成
setTimeout, setInterval, setImmediate, window.postMessage

## microtask
>每次tick过程中依照队列的优先级会取出mircatask中一个任务执行，并且清空microtask中的任务，最后再执行一次render

Vue.nextTick() 是基于MO来实现
Promise.resolve().then(nextTickHandler)
MutationObserver :MO 

## UI render

每轮次的event loop中，**每次执行一个task**，并执行完microtask队列中的所有microtask之后，就会进行UI的渲染

requestAnimateFrame(cb)  及 IntersectionObserver(cb)
## 参考文章


https://segmentfault.com/a/1190000008589736 Vue.nextTick() microtask详解

http://www.ruanyifeng.com/blog/2016/11/intersectionobserver_api.html?utm_source=tuicool&utm_medium=referral