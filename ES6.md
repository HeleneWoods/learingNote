## 变量

var：可重复定义，无法控制修改，没有块级作用域。

let：不可重复定义，块级作用域。

const：常量，不可修改，不可重复定义。

## 扩展运算符(...)

### 对象扩展

#### 拷贝对象

```javascript
let aa={
    name:'aaa',
    age:18
}

let bb={...aa}
console.log(bb) //{name:'aaa',age:18}
```

#### 合并对象

```javascript
let aa={
    age:18,
    name:'aaa'
}
let bb={
    sex:'male'
}
let cc={...aa,...bb}

console.log(ccc) //{age:18,name:'aaa',sex:'male'}

//效果等同于
//let cc = Object.assign({}, aa, bb)
```

#### 拷贝并修改对象

```javascript
let aa={
    age:18,
    name:'aaa'
}
let dd={...aa, name:'ddd'}

console.log(dd) //{age:18,name:'ddd'}

//等同于
//let dd={...aa, ...{name:'ddd'}}
//let dd=Object.assign({}, aa, {name:'ddd'})
```

#### 拷贝与引用的区别

##### 引用赋值

```javascript
let aa={
    age:18,
    name:'aaa'
}
let bb=aa
bb.age=22
console.log(aa.age) //22
//相当于aa和bb都是一个指针指向同一个内存对象
//当bb.age重新赋值时，是对bb所指向的内存对象进行重赋值
//同样指向这个内存对象的aa的age属性就也发生了变化

//一句话总结，引用相当于指针
```

##### 解构赋值

```javascript
let aa={
    age:18,
    name:'aaa'
}
let bb={...aa}
bb.age=22
console.log(aa.age) //18

//bb是对aa内存的一个拷贝
//aa和bb分别指向两个不同的内存对象
//他们各自的属性发生任何改变都不会对对方产生影响
```

> 解构赋值是深拷贝还是浅拷贝？

```javascript
let aa={
    age:18,
    name:'aaa',
    address:{
        city:'shanghai'
    }
}
let bb={...aa}

bb.address.city='shenzhen'
console.log(aa.address.city) //shenzhen

//以上例子可以看出，bb中address属性的city属性依然是aa中这个属性的属性的引用
//bb只对aa进行了一层拷贝，这就是浅拷贝
```

如何对aa.address.city进行拷贝？

```javascript
let aa={
    age:18,
    name:'aaa',
    address:{
        city:'shanghai'
    }
}
let bb={
    ...aa,
    address:{...aa.address}
}
```

### 剩余参数

#### 收集参数

```javascript
function show(a, b, ...c){
    alert(c);
}
show(2,3,4,5,123,53);
```

剩余参数必须是形参的最后一个。

#### 数组的展开

```javascript
let arr1=[12,5,8];
function add(a,b,c){
    alert(a+b+c);
}

add(...arr1);//25
```

```javascript
let arr1=[12,5,8];
let arr2=[45,2,4];
let arr=[...arr1,...arr2];
alert(arr);
```

## 箭函数的this

普通函数的this取决于调用该函数的对象。

```javascript
function fn(){
    console.log(this);
}
fn();//"Window"
document.onclick=fn;//"document"
```

箭头函数的this取决于定义时函数所处的运行环境的this是谁。

```javascript
const fn=()=>{
    console.log(this);
}
fn();//"window"
fn.call("aaa");//"window"
document.onclick=fn;//"window"
```

## 模板字符串

\`第$(index)个：$(item)\`相当于`“第”+index+"："+item`。

## json

json的书写规范：

* 存在字符串必须使用双引号{"name" : "Helene"}
* Key的周围必须有双引号 json={"a" : 12, "b" : 5}

序列化：

JSON.stringify({a: 12, b: 5})   =>'{"a": 12, "b": 5}'

反序列化：

JSON.parse('{"a": 12, "b": 5}')   =>{a: 12, b: 5}

## Babel

使低版本浏览器适配es6。

* 安装node

* 安装babel

  `npm i @babel/core @babel/cli @babel/preset-env`

  `npm i @babel/polyfill`--如果需要兼容IE7之前的浏览器，还要多安装一个这个

* 添加脚本

  `"build":"babel src -d dest`

* 添加配置

  ```javascript
  {
      "presets":[
          "@babel/preset-env"
      ]
  }
  ```

* 执行

  `npm run build`

## 异步操作

### Promise

本身不进行异步操作，只是对异步操作进行一个封装。

```javascript
new Promise(function(resolve,reject){
    resolve(result);
    reject("err");
}).then(function(res){
    alert(res);
},function(err){
    alert(err);
})
```

Promise方法：

* Promise.all -- 处理多个异步操作返回的值
* Promise.race -- 多个异步操作，谁先请求到值就处理谁的值

### async/await

```javascript
async function show(){
    XXX;
    XXX;
    
    let data = await $.ajax();
    
    XXX;
}

show();
```

## 面向对象

ES5的面向对象是“假的”，没有统一的写法。

```javascript
function Person(name,age){
    this.name=name;
    this.age=age;
}
Person.prototype.showName(){
    alert(this.name);
}
Person.portotype.showAge(){
    alert(this.age);
}

let p=new Person("Helene", 18);
//继承
function Worker(name, age, job){
    Person.call(this, name, age);
    
    this.job=job;
}
Worker.prototype=new Person();
Worker.prototype.constructor=Worker;

Worker.prototype.showJob=function(){
    alert(this.job);
}

p.showName();
p.showAge();

let w=new Worker("blue",23,"engineer");
w.showJob();
w.showName();
w.showAge();
```



ES6提供了四个新的关键字来统一面向对象的写法。

* `class`--单独的类的声明
* `constructor`--构造函数的声明
* `extends`--继承
* `super`--超类

```javascript
class Person{
    constructor(name,age){
        this.name=name;
        this.age=age;
    }
    showName(){
       alert(this.name);
    }
    showAge(){
       alert(this.age);
    }
}

let p=new Person("Helene", 18);

//继承
class Worker extends Person{
    constructor(name, age, job){
        super(name, age);
        
        this.job=job;
    }
    
    showJob(){
        alert(this.job);
    }
}

p.showName();
p.showAge();

let w=new Worker("blue",23,"engineer");
w.showJob();
w.showName();
w.showAge();
```

## ES6模块系统

浏览器并不支持，需要用webpack进行编译。

* export--输出
* import--输入