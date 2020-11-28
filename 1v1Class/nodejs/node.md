## node模块系统

nodejs做一些工作时需要加载各种模块，

比如创建http服务需要`http`模块，

进行文档操作需要`fs`模块。

nodejs里有两类模块：

* 核心模块
* 自定义模块

### 核心模块

nodejs自带的。

### 自定义模块

自己写的，在nodejs里，每一个`.js`文件就是一个模块。

------

nodejs有两个对象

* `exports`--导出，是模块对外公开的接口
* `require`--引入，从外部获取一个模块的接口（方法）

## 自建服务器

* 引入`http`模块，使用`http.createServer()`进行创建

* 传入一个`function(req,res){}`函数作为参数

* 函数内部使用`res.writeHead(200,{'Content-Type':'text/plain'})`发送http头部

  * 当传输的文字是中文，并出现乱码时，在`content-type`后面添加上`charset=utf8`就行了

    `'Content-Type':'text/plain;charset=utf8'`

* `res.end('第一个服务器');`向客户端发送数据，响应到浏览器上
* `listen()`监听端口，可以是任意的四个数字

### Content-Type：内容类型

* text/plain:纯文本
* text/html:html文本
* text/xml:很少有人用

## nodejs的事件

nodejs中，基本上所有事件都是基于观察者模式。

js当中存在一个事件队列，当nodejs在异步操作完成的时候，会发送一个事件到队列事件当中，当请求完成时，会把结果返回给用户。

nodejs中有个`events`模块，该模块中有个对象`events.EventEmitter`，这个对象的核心是事件的触发与监听功能的封装。

## buffer

缓存区，其实就是内存。

js语言本身无法操作内存，只能处理字符串类型的数据，不能处理二进制类型的数据。

nodejs要响应http的请求，需要处理tcp的流，或者文件流，必须要使用到二进制的数据，因此buffer应运而生。

它用来创建和操作一个存放二进制数据的缓存区。

当由于js先天的不安全性，几乎没有使用js去操作内存的，所以在实际工作中buffer很少被用到。

### alloc

返回一个指定大小的buffer实例，超出部分不显示

```javascript
//创建一个10个字符的buffer实例
let _buf=Buffer.alloc(10);
//给实例写入内容
_buf.write('123，我是谁谁谁');
//只输出前10个字符的内容，实例的长度始终是10
console.log(_buf.toString());//'123，我' _buf.length=10
```

##  Stream

流，node中一个抽象的接口。

很多对象都实现了它的功能，比如：http服务的request请求。

stream流，有四种类型：

* `Readable`--可读
* `Writable`--可写
* `Duplex`--可读可写
* `Transform`--被写入之后，读出结果

Stream同时也是`EventEmitter`对象的实例，也有事件：

- `data`--有数据可读时
- `end`--没有数据可读时
- `error`--报错时
- `finish`--所有数据已被写入时

### 管道流

`.pipe()`--用于从一个流中获得数据，并将数据传递到另一个流当中。

这是一个链式操作的方法，也就是可以多个`pipe()`连起来使用，称作链式流。

## express

核心特质：

1. 设置中间件来响应http的请求
2. 可以自定义路由来执行不同的http请求动作

```javascript
let app=express();

let _url = "<h1>这里是首页</h1><br>" +
  "<a href='/a'>去A页面</a><br>" ;

app.get('/',function(req,res){
    res.send(_url);
});
app.get('/a',function(req,res){
    res.send("<h1>这里是A页面</h1><br<a href='/'>回首页面</a><br>>");
});
```

### 中间件

中间件本来是工程领域的一个概念，web、软件工程领域把它借鉴了过来。

中间件其实就是把两个事物连接了起来。

比如手机把人与人连接了起来，

express把页面和数据连接了起来。

使用`app.use(express.static(url));`形成静态目录，创建一个中间件。

## 实战

