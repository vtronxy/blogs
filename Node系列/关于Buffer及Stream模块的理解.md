# stream && Buffer

## Buffer概念
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

4. Buffer拼接问题

使用Buffer对象的concat，将多个小的Buffer对象拼接为一个大的Buffer
不可以使用 **+=** 拼接符，字符串拼接符会优先调用Buffer对象的 toString方法

5. Buffer与网络传输的效率
res.end(str);网络上传输的内容都是字节流，预先使用Buffer转化为字节后，可提升网络的响应

```javascript
 fs.createReadStream(path,opts);
 
 {
   flags:'r',
   encoding:null,
   fd:null,
   mode:0666, //8进制数据
   highWaterMark:64 *1024
 }

 /*
    fs.createReadStream()的工作方式是在内存中准备一段Buffer，
    然后在fs.read()读取时逐步从磁盘中将字节复制到Buffer中
    完成一次读取时，则从这个Buffer中通过slice()方法取出部分数据
    作为一个小Buffer对象，再通过data事件传递给调用方

    由于内部采用fs.read()实现，将会引起对磁盘的system call，对于
    大文件而言，highWaterMark的大小决定会触发 system call和data事件的次数

 */
```

## stream概念

node中的流模块的基本操作符叫做 .pipe()
pipe()方法可以自动的监听data与end事件


五种类型的流：readable，writable，transform，duplex，classic

stream的编程神奇之处在于:绑定了data的event handler，数据就可以不停的接收直到消耗完毕

可读流readable内部具备_read()方法，用于实现输出数据，并一个新的Buffer对象来触发 data事件
直接消费一个 可读流
```javascript
  process.stdin.on('readable',function(){
    
  })
```

```javascript
  let http = require('http');
  let fs = require('fs');
  let server = http.createServer(function(req,res){
    fs.readFile(__dirname + '/data.txt',function(err,data){
      res.end(data); 
    })
  })
  server.listen(8000)
  
  每次请求时，我们都会把整个data.txt文件读入到内存中，然后再把结果返回给客户端

  req,res都是流对象
```