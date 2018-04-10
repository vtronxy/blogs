# JavaScript 事件循环

- 每次tick过程中依照队列的优先级会取出mircatask中一个任务执行，并且清空microtask中的任务，最后再执行一次render

- 一个普遍的常识是DOM Tree的修改是实时的，而修改的Render到DOM上才是异步的

## micratask

## microtask
- Promise.resolve().then()
- MutationObserver
