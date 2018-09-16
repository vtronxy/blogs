# stream && Buffer

## Buffer概念
1. Buffer对象内存不是V8的堆内存中，在Node的C++层实现的 **内存申请** 在JS中分配内存的策略

> 频繁的调用malloc造成大量的内存申请的系统调用，对OS有很大的压力，所以有了 **内存池**的使用场景

2. 8KB来区分Buffer是大对象 还是 小对象。小对象采取slab（默认8KB）的方式(full,partial,empty)三种状态，大对象 直接分配一个SlowBuffer对象作为slab

```javascript
 let pool; //全局变量
 function allocPool(){
   //SlowBuffer 是C++层实现
   pool = new SlowBuffer(Buffer.poolSize);
   pool.use = 0;
 }
```
3. Buffer与字符串

4. Buffer拼接问题

使用Buffer对象的concat，将多个小的Buffer对象拼接为一个大的Buffer

5. Buffer与网络传输的效率
res.end(str);网络上传输的内容都是字节流，预先使用Buffer转化为字节后，可提升网络的响应

```javascript

```

## stream概念
