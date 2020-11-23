# Node基础

## 1、Node.js 简介

- 简单说Node.js就是运行在服务端的JavaScript

## 2、Node.js创建第一个应用

- Node.js应用由那几个部分组成

  1、**引入的required模块**：我们可以使用**require**指令来载入Node.js模块

  2、**创建服务器**：服务器可以监听客户端请求，类似于Apache、Nginx等Http服务器

  3、**接收请求和响应请求**：服务器创建很容易，客户端可以使用浏览器或终端发送HTTP请求，服务器接收请求后返回响应数据。

  ```js
  //使用require指令来载入http模块
  var http = require('http')
  
  http.createServer(function (request, response) {
      //发送 HTTP 头部
      //HTTP 状态值：200：OK
      //内容类型：text/plain
      response.writeHead(200, {'Content-Type': 'text/plain'});
      
      //发送响应数据 "Hello World"
      response.end('Hello World\n');
  }).listen(8888);
  
  //终端打印如下信息
  console.log('Server running at http://127.0.0.1:8888/')
  
  ```

  以上代码我们完成了一个可以工作的HTTP服务器

  分析Node.js 的 HTTP 服务器

  - 第一行请求（require）Node.js 自带的http模块，并把它赋值给 http 变量
  - 接下来我们调用 htttp 模块提供的函数：createServer。这个函数会返回一个对象，这个对象有一个叫做 listen 的方法，这个方法有一个数值参数，指定这个 HTTP 服务器监听的端口号。

## 3、NPM 使用介绍

NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的许多问题，常见使用场景：

- 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

```
npm install express //安装
npm uninstall express //卸载
```

安装好之后，express 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 **require('express')** 的方式就好，无需指定第三方包路径。

```
var express = require('express');
```

**全局安装与本地安装**

```
npm install express          # 本地安装 可以通过 require() 来引入本地安装的包。
npm install express -g   # 全局安装 可以直接在命令行里使用。
```

## 4、Node.js REPL(交互式解释器)

Node.js REPL(Read Eval Print Loop:交互式解释器) 表示一个电脑的环境，类似 Window 系统的终端或 Unix/Linux shell，我们可以在终端中输入命令，并接收系统的响应。

Node 自带了交互式解释器，可以执行以下任务：

- **读取** - 读取用户输入，解析输入了Javascript 数据结构并存储在内存中。
- **执行** - 执行输入的数据结构
- **打印** - 输出结果
- **循环** - 循环操作以上步骤直到用户两次按下 **ctrl-c** 按钮退出。

我们可以输入以下命令来启动 Node 的终端：

```
$ node
> 
```

## 5、Node.js 回调函数

Node.js 异步编程的直接体现就是回调。异步编程依托于回调来实现，但不能说使用了回调后程序就异步化了。

回调函数在完成任务后就会被调用，Node使用了大量回调函数，Node所有API都支持回调函数。

例如，我们可以一边读取文件，一边执行其他命令，在文件读取完成后，我们将文件内容作为回调函数的参数返回。这样在执行代码是就没有阻塞或等待文件I/O操作。这样就大大提高了Node.js的性能，可以处理大量并发请求。

回调函数一般作为函数的最后一个参数出现：

```js
function foo1(name, age, callback) { }
function foo2(value, callback1, callback2) { }
```

**阻塞代码实例**

创建一个文件 input.txt ，内容如下：

```
菜鸟教程官网地址：www.runoob.com
```

创建 main.js 文件, 代码如下：

```js
var fs = require("fs");

var data = fs.readFileSync('input.txt');

console.log(data.toString());
console.log("程序执行结束!");
```

以上代码执行结果如下：

```
$ node main.js
菜鸟教程官网地址：www.runoob.com

程序执行结束!
```

**非阻塞代码实例**

创建一个文件 input.txt ，内容如下：

```
菜鸟教程官网地址：www.runoob.com
```

创建 main.js 文件, 代码如下：

```js
var fs = require("fs");

fs.readFile('input.txt', function (err, data) {
    if (err) return console.error(err);
    console.log(data.toString());
});

console.log("程序执行结束!");
```

以上代码执行结果如下：

```
$ node main.js
程序执行结束!
菜鸟教程官网地址：www.runoob.com
```

**总结**：阻塞是按顺序执行的，而非阻塞是不需要按顺序的，所以如果需要处理回调函数的参数，我们就需要写在回调函数内。

***理解回调函数***

你到一个商店买东西，刚好你要的东西没有货，于是你在店员那里留下了你的电话，过了几天店里有货了，店员就打了你的电话，然后你接到电话后就到店里去取了货。在这个例子里，你的电话号码就叫回调函数，你把电话留给店员就叫登记回调函数，店里后来有货了叫做触发了回调关联的事件，店员给你打电话叫做调用回调函数，你到店里去取货叫做响应回调事件。

## 6、Node.js 事件循环

- Node.js 是单进程单线程应用程序，但是因为V8引擎提供的异步执行回调接口，通过这些接口可以处理大量的并发，所以性能非常高。

- Node.js基本上所有的事件机制都是用设计模式中的观察者模式实现。(观察者模式：当对象间存在一对多关系时，使用观察者模式。比如，当一个对象被修改时，则会自动通知他的依赖对象。)

- Node.js单线程类似进入一个while(true)的事件循环，直到没有事件观察者时退出，每个异步事件都生成一个事件观察者，如果有事件发生就调用该回调函数。

***事件驱动程序***（非阻塞I/O或者事件驱动I/O）

- Node.js使用事件驱动程序。在事件驱动模型中，会生成一个主循环来监听事件，当检测到事件时触发回调函数。

- Node.js有多个内置事件，我们可以通过引入events模块，并通过实例EventEmitter类来绑定和监听事件。

  ``` js
  //引入events模块
  var events = require('events');
  //创建EventEmitter对象
  var eventEmitter = new events.EventEmitter();
  
  //创建事件处理程序
  var connectHandler = function connected() {
      console.log("连接成功");
      
      //触发 data_received 事件
      eventEmitter.emit('data_received');
  }
  
  //绑定 connection 事件处理程序
  eventEmitter.on('connection', connectHandler);
  
  //使用匿名函数绑定 data_received 事件
  eventEmitter.on('data_received', function(){
      console.log("数据接收成功");
  });
  
  //触发 connection 事件
  eventEmitter.emit('connection');
  
  console.log("程序执行完毕");
  ```

## 7、Node.js EventEmitter

- Node.js 所有的异步I/O操作在完成时都会发送一个事件到事件队列
- Node.js 里面的许多对象都会分发事件：一个 net.Server 对象会在每次有新连接时触发一个事件，一个fs.readStream对象会在文件被打开时促发一个事件。所有这些产生事件的对象都是event.EventEmitter的实例。

***EventEmitter*** 类

- events模块只提供了一个对象：events.EventEmitter。EventEmitter的核心就是事件触发和事件监听功能的封装。

| 序号 | 方法 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **addListener(event, listener)** 为指定事件添加一个监听器到监听器数组的尾部。 |
| 2    | **on(event, listener)** 为指定事件注册一个监听器，接受一个字符串 event 和一个回调函数。`server.on('connection', function (stream) {  console.log('someone connected!'); });` |
| 3    | **once(event, listener)** 为指定事件注册一个单次监听器，即 监听器最多只会触发一次，触发后立刻解除该监听器。`server.once('connection', function (stream) {  console.log('Ah, we have our first user!'); });` |
| 4    | **removeListener(event, listener)** 移除指定事件的某个监听器，监听器必须是该事件已经注册过的监听器。它接受两个参数，第一个是事件名称，第二个是回调函数名称。`var callback = function(stream) {  console.log('someone connected!'); }; server.on('connection', callback); // ... server.removeListener('connection', callback);` |
| 5    | **removeAllListeners([event])** 移除所有事件的所有监听器， 如果指定事件，则移除指定事件的所有监听器。 |
| 6    | **setMaxListeners(n)** 默认情况下， EventEmitters 如果你添加的监听器超过 10 个就会输出警告信息。 setMaxListeners 函数用于提高监听器的默认限制的数量。 |
| 7    | **listeners(event)** 返回指定事件的监听器数组。              |
| 8    | **emit(event, [arg1], [arg2], [...])** 按监听器的顺序执行执行每个监听器，如果事件有注册监听返回 true，否则返回 false。 |

```js
var events = require('events');
var eventEmitter = new events.EventEmitter();

//监听器 #1
var listener1 = function listener1() {
    console.log('监听器 listener1 执行。');
}

//监听器 #2
var listener2 = function listener2() {
    console.log('监听器 listener2 执行。');
}

//绑定 connection 事件，处理函数为 listener1
eventEmitter.addListener('connection', listener1);

//绑定 connection 事件，处理函数为 listener2
eventEmitter.on('connection', listener2);

var eventListeners = eventEmitter.listenerCount('connection');//返回指定事件的监听器数量
console.log(eventListeners + "个监听器监听连接事件");

//处理 connection 事件
eventEmitter.emit('connection');

//移除监听的listener1事件
eventEmitter.removeListener('connection',listener1);//移除connection事件的listener1事件
console.log("listener1 不再监听");

//触发连接事件
eventEmitter.emit('connection');

eventListeners = eventEmitter.listenerCount('connection');
console.log(eventListeners + "个监听器监听连接事件");

console.log("程序执行完毕。");
```

以上代码，执行结果如下所示：

```
$ node main.js
2 个监听器监听连接事件。
监听器 listener1 执行。
监听器 listener2 执行。
listener1 不再受监听。
监听器 listener2 执行。
1 个监听器监听连接事件。
程序执行完毕。
```

***error事件***

- EventEmitter 定义了一个特殊的事件 error，它包含了错误的语义，我们在遇到 异常的时候通常会触发 error 事件。当 error 被触发时，EventEmitter 规定如果没有响 应的监听器，Node.js 会把它当作异常，退出程序并输出错误信息。我们一般要为会触发 error 事件的对象设置监听器，避免遇到错误后整个程序崩溃。

  ```js
  var events = require('events'); 
  var emitter = new events.EventEmitter(); 
  emitter.emit('error',function(){
  	//异常处理代码
  }); 
  ```

***继承EventEmitter***

- 大多数时候我们不会直接使用 EventEmitter，而是在对象中继承它。包括 fs、net、 http 在内的，只要是支持事件响应的核心模块都是 EventEmitter 的子类。

- 为什么要这样做呢？原因有两点：首先，具有某个实体功能的对象实现事件符合语义， 事件的监听和发生应该是一个对象的方法。其次 JavaScript 的对象机制是基于原型的，支持 部分多重继承，继承 EventEmitter 不会打乱对象原有的继承关系。

## 8、Node.js Buffer(缓冲区)

- JavaScript 语言自身只有字符串数据类型，没有二进制数据类型。但在处理像TCP流或文件流时，必须使用到二进制数据。因此在Node.js中，定义了一个Buffer类，该类用来创建一个专门存放二进制数据的缓存区。
- 一个Buffer类似于一个整数数组

***创建Buffer类***

Buffer 提供了以下 API 来创建 Buffer 类：

- **Buffer.alloc(size[, fill[, encoding]])：** 返回一个指定大小的 Buffer 实例，如果没有设置 fill，则默认填满 0

- **Buffer.allocUnsafe(size)：** 返回一个指定大小的 Buffer 实例，但是它不会被初始化，所以它可能包含敏感的数据

- **Buffer.allocUnsafeSlow(size)**

- **Buffer.from(array)：** 返回一个被 array 的值初始化的新的 Buffer 实例（传入的 array 的元素只能是数字，不然就会自动被 0 覆盖）

- **Buffer.from(arrayBuffer[, byteOffset[, length]])：** 返回一个新建的与给定的 ArrayBuffer 共享同一内存的 Buffer。

- **Buffer.from(buffer)：** 复制传入的 Buffer 实例的数据，并返回一个新的 Buffer 实例

- **Buffer.from(string[, encoding])：** 返回一个被 string 的值初始化的新的 Buffer 实例

  ``` js
  //创建一个长度为10、且用0填充的Buffer
  const buf1 = Buffer.alloc(10);
  
  //创建一个长度为10、且用 0x1 填充的 Buffer
  const buf2 = Buffer.alloc(10,1);
  
  //创建一个长度为10、且未初始化的Buffer
  //这个方法比调用 Buffer.alloc() 更快
  //但返回的 Buffer实例可能包含旧数据
  //因此需要使用fill()或write()进行重写
  const buf3 = Buffer.allocUnsafe(10);
  
  //创建一个包含[0x1,0x2,0x3]的Buffer
  const buf4 = Buffer.from([1,2,3]);
  
  // 创建一个包含 utf-8 字节[0x74, 0xc3, 0xa9, 0x73, 0x74] 的Buffer
  const bug5 = Buffer.from('test');
  
  // 创建一个包含 Latin-1 字节[0x74, 0xc3, 0xa9, 0x73, 0x74] 的Buffer
  const bug5 = Buffer.from('test', 'latin1');
  ```

**写入缓存区**

- 语法

  ```js
  buf.write(string[, offset[, length]][, encoding])
  ```

- 参数

  string-写入缓存区的字符串

  offset-缓存区开始写入的索引值，默认为0

  length-写入的字节数，默认为buffer.length

  encoding-使用的编码，默认为'utf-8'

```js
buf = Buffer.alloc(256);
len = buf.write("www.runoob.com");
console.log("写入字节数："+ len); //写入字节数 : 14
```

**从缓冲区读取数据**

- 语法

  ```
  buf.toString([encoding[, start[, end]]])
  ```

- 参数

  endoding-使用的编码。默认为'utf-8'

  start-指定开始读取索引的位置，默认为0

  end-结束位置，默认为缓冲区的末尾

```js
buf = Buffer.alloc(26);
for (var i = 0; i < 26; i++) {
    buf[i] = i + 97;
}

console.log( buf.toString('ascii')); //输出：abcdefghijklmnopqrstuvwxyz
console.log( buf.toString('ascii',0,5)); //使用 'ascii' 编码, 并输出：abcde
console.log( buf.toString('utf8',0,5)); //使用 'utf8' 编码, 并输出：abcde
console.log( buf.toString(undefined,0,5)); //使用默认的 'utf8' 编码, 并输出：abcde
```

**将 Buffer 转换为 JSON 对象**

- 语法

  ```js
  buf.toJSON() //返回JSON对象
  ```

  当字符串化一个 Buffer 实例时，[JSON.stringify()](https://www.runoob.com/js/javascript-json-stringify.html) 会隐式地调用该 **toJSON()**。

```js
const buf = Buffer.from([0x1,0x2,0x3,0x4,0x5]);
const json = JSON.stringify(buf);

console.log(json);//输出 {"type":"Buffer","data":[1,2,3,4,5]}

const copy = JSON.parse(json, (key,value) => {
    return value && value.type === 'Buffer' ?
        Buffer.from(value.data) :
    	value;
 })

console.log(copy);//输出：<Buffer 01 02 03 04 05>
```

JSON.parse() 方法用于将一个 JSON 字符串转换为对象。

- 语法

```
JSON.parse(text[, reviver])
```

- 参数说明

  text-必需， 一个有效的 JSON 字符串。

  reviver-可选，一个转换结果的函数， 将为对象的每个成员调用此函数。

**缓冲区合并**

- 语法

  ```js
  Buffer.concat(list[, totalLength]) //返回一个多个成员合并的新 Buffer 对象
  ```

- 参数

  list-用于合并的Buffer对象数组列表

  totalLength-指定合并后Buffer对象的总长度

```js
var buffer1 = Buffer.from(('菜鸟教程'));
var buffer2 = Buffer.form(('www.runoob.com'));
var buffer3 = Buffer.concat([buffer1,buffer2]);
console.log("buffer3内容：" + buffer3.toString());//buffer3 内容: 菜鸟教程www.runoob.com
```

**缓冲区比较**

- 语法

  ```
  buf.compare(otherBuffer);// 返回一个数字，表示 buf 在 otherBuffer 之前，之后或相同。
  ```

- 参数

  **otherBuffer** - 与 **buf** 对象比较的另外一个 Buffer 对象。

```js
var buffer1 = Buffer.from('ABC');
var buffer2 = Buffer.from('ABCD');
var result = buffer1.compare(buffer2);
console.log(result);//-1
if(result < 0) {
   console.log(buffer1 + " 在 " + buffer2 + "之前");
}else if(result == 0){
   console.log(buffer1 + " 与 " + buffer2 + "相同");
}else {
   console.log(buffer1 + " 在 " + buffer2 + "之后");
}
//ABC在ABCD之前
```

**拷贝缓冲区**

- 语法

  ```
  buf.copy(targetBuffer[, targetStart[, sourceStart[, sourceEnd]]])
  ```

- 参数

  targetBuffer-要拷贝的Buffer对象

  targetStart- 数字, 可选, 默认: 0

  sourceStart- 数字, 可选, 默认: 0

  sourceEnd- 数字, 可选, 默认: buffer.length

```js
var buf1 = Buffer.from('abcdefghijkl');
var buf2 = Buffer.from('RUNOOB');

//将 buf2 插入到 buf1 指定位置上
buf2.copy(buf1,2);

console.log(buf1.toString());//abRUNOOBijkl
```

**缓冲区裁剪**

```js
var buffer1 = Buffer.from('runoob');
// 剪切缓冲区
var buffer2 = buffer1.slice(0,2);
console.log("buffer2 content: " + buffer2.toString());//buffer2 content: ru
```

**缓冲区长度**

```js
var buffer = Buffer.from('www.runoob.com');
//  缓冲区长度
console.log("buffer length: " + buffer.length);//buffer length: 14
```

## 9、Node.js Stream(流)

- Stream是一个抽象接口，Node中有很多对象实现了这个接口

- Stream有四种流类型：1、Readable:可读操作，2、Writable:可写操作，3、Duplex:可读可写操作，4、Transfrom:操作被写入数据，然后读出结果

- 所有的Stream对象都是EventEmitter的实例，常用的事件：

  1、**data** - 当有数据可读时触发。

  2、**end** - 没有更多的数据可读时触发。

  3、**error** - 在接收和写入过程中发生错误时触发。

  4、**finish** - 所有数据已被写入到底层系统时触发。

**从流中读取数据**

```js
var fs = require('fs');
var data = '';

//创建可读流
var readerStream = fs.createReadStream('input.txt');//菜鸟教程官网地址：www.runoob.com

//设置编码为utf8
readerStream.setEncoding('utf8');

//处理流事件 --> data, end, and error
readerStream.on('data', function(chunk){
    data += chunk;
});

readerStream.on('end', function(){
    console.log(data);
});

readerStream.on('error', function(){
    console.log(err.stack);
});

console.log("程序执行完毕");
```

以上代码执行结果如下：

```
程序执行完毕
菜鸟教程官网地址：www.runoob.com
```

**写入流**

```js
var fs = require("fs");
var data = '菜鸟教程官网地址：www.runoob.com';

// 创建一个可以写入的流，写入到文件 output.txt 中
var writerStream = fs.createWriteStream('output.txt');

// 使用 utf8 编码写入数据
writerStream.write(data,'UTF8');

// 标记文件末尾
writerStream.end();

// 处理流事件 --> data, end, and error
writerStream.on('finish', function() {
    console.log("写入完成。");
});

writerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");
```

**管道流**

- 管道提供了一个输出流到输入流的机制。通常我们用于从一个流中获取数据并将数据传递到另一个流中

  ![img](https://www.runoob.com/wp-content/uploads/2015/09/bVcla61)

  如上面的图片所示，我们把文件比作装水的桶，而水就是文件里的内容，我们用一根管子(pipe)连接两个桶使得水从一个桶流入另一个桶，这样就慢慢的实现了大文件的复制过程。

- 以下实列我们通过读取一个文件内容并将内容写入到另一个文件中

  ```js
  var fs = require("fs");
  
  var read = fs.createReadStream('input.txt');
  
  var write = fs.creatWriteStream('output.txt');
  
  //管道读写操作
  read.pipe(write);
  
  console.log("程序执行完毕");
  ```

**链式流**

- 链式是通过连接输出流到另外一个流并创建多个流操作链的机制。链式流一般用于管道操作。

- 用管道和链式来压缩和解压文件

- zlib.createGzip()压缩 zlib.createGunzip()解压

  压缩

  ```js
  var fs = require('fs');
  var zlib = require('zlib');
  
  //压缩 input.txt 文件为 input.txt.gz
  fs.createReadStream('input.txt')
      .pipe(zlib.createGzip())
      .pipe(fs.createWriteStream('input.txt.gz'));
  
  console.log("程序执行完毕");
  ```

  解压

  ```js
  var fs = require('fs');
  var zlib = require('zlib');
  
  //解压input.txt.gz 为input.txt
  fs.createReadStream("input.txt.gz")
  	.pipe(zlib.createGunzip())
  	.pipe(fs.createWriteStream("input1.txt"));
  
  console.log("程序执行完毕");
  ```

## 10、Node.js模块系统

- 为了能让Node.js的文件能够相互调用，Node.js提供了一个简单的模块系统
- 模块是Node.js应用程序的基本组成部分，文件和模块一一对应。

**创建模块**

- Node.js 提供了 exports 和 require 两个对象，其中的 exports 是模块公开的接口，require 用于从外部获取一个模块的接口，及所获取的 exports 对象

  ```js
  exports.world = function() {
  	console.log("Hello World!");
  }
  ```

  ```js
  var hello = require('./hello')// ./表示当前目录
  hello.world(); // 输出 Hello World!
  ```

- 把一个对象封装到模块中

  ```js
  function Hello() {
      var name;
      this.setName = function(thyName) {
          name = thyName;
      };
      this.sayHello = function() {
          console.log('Hello' + name);
      };
  };
  module.exports = Hello;
  ```

  ```js
  var Hello = require('./hello');
  hello = new Hello();
  hello.setName('zhang');
  hello.sayHello();
  ```

  模块接口的唯一变化是使用 module.exports = Hello 代替了 exports.world = function(){}。其接口对象就是要输出的Hello 对象本身，而不是原先的 exports。

  

  **exports 和 module.exports 的使用**

  如果要对外暴露属性或方法，就用 **exports** 就行，要暴露对象(类似class，包含了很多属性和方法)，就用 **module.exports**。而且不建议同时使用 exports 和 module.exports

- require 加载优先级

  先从文件模块的缓冲中加载，其次从原生模块加载，最后从文件加载

## 11、Node.js 函数

- 在JavaScript中，一个函数可以作为另一个函数的参数。我们可以先定义一个函数，然后传递，也可以在传递参数的地方直接定义函数。

  ```js
  function say(world) {
      consloe.log(world);
  }
  
  function excute(someFunction, value){
      someFunction(value);
  }
  
  excute(say,'Hello');
  ```

  在以上代码中，我们把 say 函数作为 excute 函数的第一个变量进行了传递。这里传递的不是 say 的返回值，而是say本身。

**匿名函数**

- 我们可以把一个函数作为一个变量进行传递。但是我们不一定要"先定义，再传递"，我们可以直接在另一个函数的括号中定义和传递这个函数

  ```js
  function excute(someFunction, value) {
      someFunction(value);
  }
  excute(function(word){
      console.log(word)
  },"Hello");
  ```

  我们在 excute 接受第一个参数的地方直接定义了我们准备传递给 excute 的函数。

  用这种方式，我们甚至不用给这个函数起名字，这也为什么它被叫做匿名函数。

**函数传递是如何让HTTP服务器工作的**

- 再来看看HTTP服务器

  ```js
  var http = require("http");
  
  http.createServer(function(request,response) {
      response.writeHead(200, {"Content-Type": "text/plain"});
      response.write("Hello World");
      response.end();
  }).listen(8888);
  ```

- 现在他看起来清晰多了：我们向createServer传递了一个匿名函数。

- 用这样的代码也可以达到同样的目的

  ```js
  var http = require("http");
  
  function onRequest(request, response) {
      response.writeHead(200,{"Content-Type": "text/plain"});
      response.write("Hello world");
      response.end();
  }
  
  http.createServer(onRequest).listen(8888);
  ```

## 12、Node.js 路由

- 我们要为路由提供的请求的URL和其他需要的 GET 和 POST 参数，随后路由需要根据这些数据来执行相应的代码。
- 解析url，使用Node.js的 url 和 querystring 模块

```js
            url.parse(string).query
                                           |
           url.parse(string).pathname      |
                       |                   |
                       |                   |
                     ------ -------------------
http://localhost:8888/start?foo=bar&hello=world
                                ---       -----
                                 |          |
                                 |          |
              querystring.parse(queryString)["foo"]    |
                                            |
                         querystring.parse(queryString)["hello"]
```

**实例**

- service.js

  ```js
  var http = require('http');
  var url = require('url');
  
  function start(route){
      function onRequest(request, response) {
          var pathname = url.parse(request.url).pathname;
          console.log("Request for " + pathname + " received.");
          
          route(pathname);
          
          response.writeHead(200,{"Content-Type": "text/plain"});
          response.write("Hello world");
          response.end();
      }
      
      http.createServer(onRequest).listen(8888);
      console.log("Server has started");
  }
  
  exports.start = start;
  ```

- route.js

  ```js
  function route(pathname) {
  	console.log("About to route request for " + pathname);
  }
  
  exports.route = route;
  ```

- index.js

  ```js
  var server = require('./server');
  var route = require('./route');
  
  server.start(route.route);
  ```

- 结果

  ```
  $ node index.js
  Server has started.
  ```

浏览器访问 **http://127.0.0.1:8888/**，出来 Hello world

## 13、Node.js 全局对象

- JavaScript中有一个特殊的对象，称为全局对象，它及其所有属性都可以在程序的任何地方访问，即全局变量。
- 在浏览器JavaScript中，通常 window 是全局对象，而Node.js中的全局对象是global，所有全局变量（除了global本身之外）都是global对象的属性

- **注意**：最好不要使用var定义变量以避免引入全局变量，因为全局变量会污染命名空间，提高代码的耦合风险

**__filename**

- 表示当前正在执行的脚本的文件名，它将输出文件所在位置的绝对路径 ///web/com/runoob/nodejs/main.js

**__dirname**

- 表示当前执行脚本所在的目录 ///web/com/runoob/nodejs

**setTimeout(cb，ms)**

- setTimeout(cb，ms)全局函数在指定的毫秒(ms)数后执行指定的函数(cb)。setTimeout()只执行一次指定函数

  ```js
  function printHello(){
  	console.log("Hello World!");
  }
  //两秒后执行以上函数
  setTimout(printHello, 2000);
  ```

**clearTimeout(t)**

- clearTimeout(t) 全局函数用于停止一个之前用setTimeout()函数创建的定时器。参数t是使用setTimeout()创建的定时器

  ```js
  function printHello() {
  	console.log("Hello World!");
  }
  
  var t = setTimeout(printHello, 2000);
  
  clearTimeout(t);
  ```

**setInterval(cb，ms)**

- setInterval(cb，ms)全局函数在指定的毫秒数(ms)后执行指定函数(cb)

- 返回一个代表定时器的句柄值。可以使用clearInterval(t)函数来清除定时器

- setInterval()方法会不停的调用函数，直到 clearInterval() 被调用或窗口被关闭

  ```js
  function printHello() {
  	console.log("Hello World!");
  }
  
  setInterval(printHello, 2000);
  //Hello, World! Hello, World! Hello, World! Hello, World! Hello, World!...会一直执行下去
  ```

**clearInterval(t)**

- 停止setInterval()方法

**process**

- process是一个全局变量，即global对象的属性
- 它用于描述当前Node.js进程状态的对象，提供了一个与操作系统打交道的简单接口。

## 14、Node.js 常用工具

- util 是一个 Node.js 核心模块，提供常用的函数，用于弥补核心 JavaScript 的功能过于精简的不足

- 使用方法如下

  ```js
  const util = require('util');
  ```

- **util.callbackify**

## 15、Node.js 文件系统

- Node.js 提供一组类似UNIX标准的文件操作API

**异步和同步**

- Node.js文件系统(fs)模块中的方法均有异步和同步版本，例如读取文件内容的函数有异步的 fs.readFile()和同步的 fs.readFileSync()。异步的方法最后一个参数为回调函数，回调函数的第一个参数包含了错误信息。
- 建议大家用异步方法，比起同步，异步方法性能更高，速度更快，而且没有阻塞。

```js
var fs = require('fs');

//异步读取
fs.readFile('input.txt', function (err, data){
    if (err) {
        return console.error(err);
    }
    console.log("异步读取：" + data.toString());
});

//同步读取
var data = fs.readFileSync("input.txt");
console.log("同步读取：" + data.toString());
```

**打开文件**

- 语法

  ```
  fs.open(path, flags[, mode], callback)
  ```

- 参数

  path-文件的路径

  flags-文件的打开行为（r、r+....）只读、读写

  mode-设置文件模式（权限），文件创建默认权限为0666（可读，可写）

  callback-回调函数，带两个参数如：callback(err,fd)

  ```js
  var fs = require('fs');
  
  //异步打开文件
  console.log("准备打开文件！")
  fs.open('input.txt', 'r+', function(err, fd) {
      if (err) {
          return console.log(err);
      }
      console.log("文件打开成功")
  })
  ```

**获取文件信息**

- 语法

  ```
  fs.stat(path, callback)
  ```

- 参数

  path-文件路径

  callback-回调函数，带有两个参数：(err, stats), stats是fs.Stats对象

- fs.stat(path)执行后，会将stats类的实例返回给其调用函数。可以用stats类中提供的方法判断文件的相关属性。例如判断是否为文件：

  ```js
  var fs = require('fs');
  
  fs.stat('/Users/liuht/code/itbilu/demo/fs.js', function(err, stats) {
      console.log(stats.isFile()); // true
  });
  ```

  ```js
  var fs = require('fs');
  
  console.log("")
  ```

**写入文件**

- 语法

  ```
  fs.writeFile(file, data[, options], callback)
  ```

- 参数

  file-文件名或文件描述符

  data-要写入文件的数据，可以是String(字符串)或Buffer(缓冲)对象。

  options-该参数是一个对象，包含{encoding，mode，flag}。默认编码utf8，模式为0666，flag为"w"

  callback- 回调函数，回调函数只包含错误信息(err)，再写入失败时返回

  ```js
  var fs = require('fs');
  
  console.log("准备写入文件");
  fs.writeFile('input.txt', '我是通过fs.writeFile 写入文件的内容', function(err){
      if(err) {
          return console.error(err);
      }
      console.log("数据写入成功！");
      console.log("--------我是分割线----------");
      console.log("读取写入的数据！");
      fs.readFile('input.txt', function (err, data) {
          if (err) {
              return console.error(err);
          }
          console.log("异步读取文件数据：" + data.toString());
      });
  });
  ```

**读取文件**

- 语法

  ```
  fs.read(fd, buffer, offset, length, position, callback)
  ```

- 参数

  fd-通过 fs.open() 方法返回的文件描述符

  buffer-数据写入缓存区

  offset-缓存区写入的写入偏移量

  length-要从文件中读取的字节数

  postion- 文件读取的起始位置，如果position的值为null，则会从文件的指针位置读取

  callback-回调函数 - 有三个参数err，bytesRead，buffer，err为错误信息，bytesRead表示的读取字节数，buffer为缓存区对象

  ```js
  var fs = require('fs');
  var buf = new Buffer.alloc(1024);
  
  console.log("准备打开已存在的文件");
  fs.open('input.txt', 'r+', function(err, fd) {
      if (err) {
          return console.error(err);
      }
      console.log("文件打开成功！");
      console.log("准备读取文件：");
      fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
          if(err) {
              console.log(err);
          }
          console.log(bytes + " 字节被读取！");
  
          //仅输出读取的字节
          if(bytes > 0) {
              console.log(buf.slice(0, bytes).toString());
          }
      });
  });
  ```

**关闭文件**

- 语法：以下为异步模式下关闭文件的语法格式：

  ```
  fs.close(fd, callback)
  ```

- 参数

  fd-通过fs.open()方法返回的文件描述符

  callback-回调函数，没有参数

  ```js
  var fs = require('fs');
  var buf = new Buffer.alloc(1024);
  
  console.log("准备打开已存在的文件");
  fs.open('input.txt', 'r+', function(err, fd) {
      if (err) {
          return console.error(err);
      }
      console.log("文件打开成功！");
      console.log("准备读取文件：");
      fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
          if(err) {
              console.log(err);
          }
          console.log(bytes + " 字节被读取！");
  
          //仅输出读取的字节
          if(bytes > 0) {
              console.log(buf.slice(0, bytes).toString());
          }
  
          //关闭文件
          fs.close(fd, function(err) {
              if (err) {
                  console.log(err);
              }
              console.log("文件关闭成功");
          });
      });
  });
  ```

**截取文件**

- 语法

  ```
  fs.ftruncate(fd, len, callback)
  ```

- 参数

  fd - 通过 fs.open() 方法返回的文件描述符。

  len - 文件内容截取的长度。

  callback - 回调函数，没有参数。

**删除文件**

- 语法

```
fs.unlink(path, callback)
```

- 参数

  path - 文件路径。

  callback - 回调函数，没有参数。

## 16、Node.js GET/POST请求

**获取GET请求内容**

- 由于GET请求直接被嵌入在路径中，URL是完整的请求路径，包括了?后面的部分，因此你可以手动解析后面的内容作为GET请求的参数。

  ```js
  var http = require('http');
  var url = require('url');
  var util = require('util');req
  
  http.createServer(function(req, res){
      res.writeHead(200, {'Content-Type': 'text/plain; charset=utf-8' });
      res.end(util.inspect(url.parse(req.url, true))); //util.inspect(object,[showHidden],[depth],[colors]) 是一个将任意对象转换 为字符串的方法，通常用于调试和错误输出。
  }).listen(3000);
  ```

- 在浏览器中访问 **http://localhost:3000/user?name=菜鸟教程&url=www.runoob.com** 然后查看返回结果:

  ![img](https://www.runoob.com/wp-content/uploads/2014/06/4A1C02B2-2EB8-4976-9F35-F3760713D495.jpg)

**获取 URL 参数**

- 我们可以使用 url.parse 方法来解析 URL 中的参数

  ```js
  var http = var http = require('http');
  var url = require('url');
  
  http.createServer(function(req, res){
      res.writeHead(200, {'Content-Type': 'text/plain;charset=utf-8'});
  
      //解析 url 参数
      var params = url.parse(req.url, true).query;
      res.write("网站名：" + params.name);
      res.write("\n");
      res.write("网站URL：" + params.url);
      res.end();
  }).listen(3000);
  ```

**获取POST请求内容**

```js
var http = require('http');
var querystring = require('querystring');

var postHTML = 
'<html><head><meta charset="utf-8"><title>菜鸟教程 Node.js 实例</title></head>' +
'<body>' +
'<form method="post">' +
'网站名： <input name="name"><br>' +
'网站 URL： <input name="url"><br>' +
'<input type="submit">' +
'</form>' +
'</body></html>';

http.createServer(function(req, res){
    var body = "";
    // 通过req的data事件监听函数，每当接受到请求体的数据，就累加到post变量中
    req.on('data', function(chunk){
        body += chunk;
    });
    // 在end事件触发后，通过querystring.parse将post解析为真正的POST请求格式，然后向客户端返回
    req.on('end', function(){
        //解析参数
        body = querystring.parse(body);
        //设置响应头部信息及编码
        res.writeHead(200, {'Content-Type': 'text/html; charset=utf8'});


        if(body.name && body.url) { //输出提交数据
            res.write("网站名：" + body.name);
            res.write("<br>");
            res.write("网站 URL：" + body.url);
        } else { //输出表单
            res.write(postHTML);
        }
        res.end();
    });
}).listen(3000);
```

## 17、Node.js 工具模块

- OS模块—提供基本的系统操作函数

- Path模块—提供了处理和转换文件路径的工具。

- Net模块—用于底层的网络通信。提供了服务端和客户端的操作

  **server.js**

  ```js
  var net = require('net');
  var server = net.createServer(function(connection) { 
     console.log('client connected');
     connection.on('end', function() {
        console.log('客户端关闭连接');
     });
     connection.write('Hello World!\r\n');
     connection.pipe(connection);
  });
  server.listen(8080, function() { 
    console.log('server is listening');
  });
  ```

  **client.js**

  ```js
  var net = require('net');
  var client = net.connect({port: 8080}, function() {
     console.log('连接到服务器！');  
  });
  client.on('data', function(data) {
     console.log(data.toString());
     client.end();
  });
  client.on('end', function() { 
     console.log('断开与服务器的连接');
  });
  ```

- DNS域名-用于解析域名

- Domain模块-简化异常代码的处理，可以捕捉处理try catch 无法捕捉的

# Node 进阶

	## 1、Node 单线程为什么支持高并发

Node架构：

![img](https://picb.zhimg.com/80/v2-7deb3758434eafd680a9272550eaa4f5_720w.jpg)

**·** Node.js 标准库，这部分是由 Javascript 编写的，即我们使用过程中直接能调用的 API。在源码中的 lib 目录下可以看到。

**·** Node bindings，这一层是 Javascript 与底层 C/C++ 能够沟通的关键，前者通过 bindings 调用后者，相互交换数据。

**·** 这一层是支撑 Node.js 运行的关键，由 C/C++ 实现。

**V8**：Google 推出的 Javascript VM，也是 Node.js 为什么使用的是 Javascript 的关键，它为Javascript 提供了在非浏览器端运行的环境，它的高效是 Node.js 之所以高效的原因之一。
**Libuv**：它为 Node.js 提供了跨平台，线程池，事件池，异步 I/O 等能力，是 Node.js 如此强大的关键。
**C-ares**：提供了异步处理 DNS 相关的能力。
**http_parser、OpenSSL、zlib** 等：提供包括 http 解析、SSL、数据压缩等其他的能力。

**为什么一个单线程的效率可以这么高，同时处理数万级的并发而不会造成阻塞呢？就是我们下面所说的--------事件驱动。**

1、每个Node.js进程只有一个主线程在执行程序代码，形成一个**执行栈**（**execution context stack**)。

2、主线程之外，还维护了一个"**事件队列**"（Event queue）。当用户的网络请求或者其它的异步操作到来时，node都会把它放到Event Queue之中，此时并不会立即执行它，代码也不会被阻塞，继续往下走，直到主线程代码执行完毕。

3、主线程代码执行完毕完成后，然后通过Event Loop，也就是**事件循环机制**，开始到Event Queue的开头取出第一个事件，从线程池中分配一个线程去执行这个事件，接下来继续取出第二个事件，再从**线程池**中分配一个线程去执行，然后第三个，第四个。主线程不断的检查事件队列中是否有未执行的事件，直到事件队列中所有事件都执行完了，此后每当有新的事件加入到事件队列中，都会通知主线程按顺序取出交EventLoop处理。当有事件执行完毕后，会通知主线程，主线程执行回调，线程归还给线程池。

4、主线程不断重复上面的第三步。

我们所看到的node.js单线程只是一个js主线程，本质上的异步操作还是由线程池完成的，node将所有的阻塞操作都交给了内部的线程池去实现，本身只负责不断的往返调度，并没有进行真正的I/O操作，从而实现异步非阻塞I/O，这便是node单线程和事件驱动的精髓之处了。

**单线程的好处：**

- 多线程占用内存高
- 多线程间切换使得CPU开销大
- 多线程由内存同步开销
- 编写单线程程序简单
- 线程安全

**单线程的劣势：**

- CPU密集型任务占用CPU时间长（可通过cluster方式解决）
- 无法利用CPU的多核（可通过cluster方式解决）
- 单线程抛出异常使得程序停止（可通过try catch方式或自动重启机制解决）

## 2、Node Cluster

## 3、Node 如何处理异常导致奔溃的问题

