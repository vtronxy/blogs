
> 文件既模块,TS里的namespace是跨文件的，JS里的module是以文件为单位的，一个文件一个module

TS里的 **namespace主要是解决命名冲突的问题**,因为是注册到全局的，所以跨文件也能正常使用，不同的文件能够读取其他文件注册在全局的命名空间内的信息，也可以注册自己的。namespace其实比较像其他面向对象编程语言里包名的概念。

而JS里的 **module主要是解决加载依赖关系**。跟文件绑定在一起，一个文件就是一个module。在一个文件中访问另一个文件必须要加载另一个文件

namespace 提供了一种方式 将逻辑上一个整体的代码，分散到各个文件中单独管理




## 参考
https://idom.me/articles/838.html ts1.5 代码中不再出现module关键字

https://www.tslang.cn/docs/handbook/namespaces.html 官网关于namespace的解释