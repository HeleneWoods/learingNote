## Node.js是什么

可以用JS写后台。

### 好处

* 性能高
  * 用Chrome的v8引擎作为核心
* 简单易用
  * 单线程、异步IO
* 前端友好
  * 开发习惯不变、前后台共享代码

### 应用场景

* 后台开发
  * web、应用、游戏等
* 开发工具
  * webpack、gulp、vue、react、运维工具等
* 中间层/微服务开发
  * 快速开发、性能高  

## Node简单的应用

### 操作file

#### 读取文件中的内容

* 首先用`require`引入node模块`fs`

  * `const fs=require('fs');`

* 使用`fs`模块的`readfile()`方法来读取文件信息

  * 方法有两个参数

    * 第一个参数用来指定文件所在位置，比如`'data/1.txt'`
    * 第二个参数是一个回调函数，回调函数又有两个参数
      * 第一个参数是错误信息
      * 第二个参数是读取到的文件信息

    ```javascript
    fs.readfile('data/1.txt', (err, data){
    	if(err){
        console.log("读取文件失败", err.code);
        }else{
            console.log(data.tostring());
        }            
    })
    ```

* 在控制行中输入`node xx.js`来运行这个js文件，就能得到运行结果。

#### 重命名文件

* 不仅要引入`fs`模块，还要引入`path`模块

* 使用`fs`模块的`readdir()`方法访问文件夹中的文件，方法中同样有两个参数

  * 第一个是文件夹的名字
  * 第二个是一个拥有两个参数的回调函数
    * 第一个是错误信息
    * 第二个是含有文件夹中的文件信息的数组

* 在回调函数中遍历数组，通过`path`得到文件名和文件的扩展名

* 再用`fs`模块的`rename()`方法，对扩展名进行判断和修改

  * `rename()`方法有三个参数
    * 第一个是文件的路径
    * 第二个是重命名后的路径
    * 第三个是一个错误信息作为参数的回调函数

  ```javascript
  fs.readdir('data',(err, list){
  	if(err){
          console.log("读取目录失败", err.code);
      }else{
          for(let i=0; i<list.length; i++){
              let { name, ext } = path.parse(list[i]);
              
              if(ext.toLowerCase() == '.txt'){
                  fs.rename(
                  	'data/' + list[i],
                      'data/' + name + '.jpg',
                      err=>{
                          if(err){
                              console.log("重命名失败", err.code);
                          }
                      }
                  )
              }
          }
      }           
  })
  ```

* 最后运行

### 创建一个简单的服务器

* 引用`http`和`fs`模块
* 使用`http`模块的`createServer()`方法建一个服务器
  * 方法里面有一个回调函数，每当服务器被访问时就会被回调；
  * 回调函数中有两个参数，`request`和`repsonse`
    * request是浏览器发送过来的数据
    * response是服务器发送给浏览器的内容
* 通过`fs.readFile()`方法读取到文件中的内容，然后通过`response.write()`和`response.end()`方法渲染到浏览器
* 使用`listen()`指定监听的端口

```javascript
let server = http.createServer(function(request, response){
	//获取request的url属性，取得客户请求的域名
	let {url}=request;
	fs.readFile('www'+url,(err, data)=>{
        if(err){
            response.write("404");
        }else{
            response.write(data);
        }
        //由于是异步操作，end方法不能在回调函数之外使用，会导致end在write之前执行
        response.end();
    })
});
server.listen(8080);
```

## http数据解析

### get

通过form表单发送get请求，表单信息会通过url传递到后台。

* 引用`http`和`url`模块
* 用`http.createServer((req,res)=>{})`创建服务器
* 在方法中的回调函数中通过`new URL(req.url, '补全绝对路径');`对url进行解析。
  * 一般参数传递过来的路径是相对路径，需要再添加一个参数对路径进行补全，解析成绝对路径。
* 通过`searchParams.forEach()`方法循环每个键值对，将它们一一保存到一个json对象中。

```javascript
const http=require('http');
const urlLib=require('url');

http.createServer((req,res)=>{
    let obj=new URL(req.url, 'http:localhost:8080');
    let url=obj.pathname;
    let GET={};
    obj.searchParams.forEach((value,key)=>{
        GET[key]=value;
    })
    console.log(url,GET);
    
    res.write('aaa');
    res.end();
}).listen(8080);
```

### post

post方式和get方式两个最大的区别就是，get是通过url来传递信息，所以能够传递的信息大小有限。

当接收post数据时，会触发`data`事件，由于post传递来的数据可能很大，会被自动切割分段传递，所以data事件可能会被触发很多次。

当所有数据传输完毕之后，会触发`end`事件。

```javascript
http.createServer((req,res)=>{
    var str="";  //用来存放post过来的数据
    
    req.on('data',data=>{
        str+=data;
    });
    req.on('end',()=>{
        let json=JSON.stringify(querystring.parse(str));
        let POST=JSON.parse(json);
        console.log(POST);
    })
}).listen(8080);
```

## 实战（登录/注册）

* 确定接口

  `/user?act=reg&user=aaa&pass=12345`=>`{"ok":false,"msg":"原因"}`

  `/user?act=login&user=aaa&pass=12345`=>`{"ok":true,"msg":"原因"}`

* 确定用户是请求文件还是请求接口

  `http://localhost:8080/1.html`->请求文件

  `http://`localhost:8080/user?act=xxx...`->请求接口

