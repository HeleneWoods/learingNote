## ReactJs是什么

它是用于构建用户界面的一个js库。

是对原生js的一个封装，本质还是js。

ReactJs起源于facebook的项目，后来进行开源。

性能比较高，逻辑相对简单。

### 特点

1. 声明式设计

2. 高效

3. 灵活

4. jsx，把html、js按照xml格式进行了规范

5. 组件式开发，易用于复用、维护

6. 单向数据流，只能从父组件向子组件传递数据

   单向数据流的思路是：数据驱动视图

### 安装react

`create-react-app`--facebook官方推出的脚手架，用于快速大剑react的开发环境。

* `npm install -g create-react-app`--全局安装
* `create-react-app test-app`--创建项目
  * 这一步主要引入了三个文件：
    * react.min.js--reactJs的核心文件
    * react-dom.js--react与dom操作相关功能的库文件
    * babel.main.js--用于es6转化为es的文件
* `npm start`

### react的基本命令

`ReactDOM.render()`--把DOM添加到页面

```html
<!-- 定义组件 -->
class Xxobj extends React.Component{
    render(){
        return <h1>第一个react页面</h1>;
    }
}
```

```html
<!-- 传值 -->
function Fnxx(props){
    return <h1>我是Helene{props.abc}</h1>
}
let elementObj=<Fnxx abc=',今年18岁'/>;
ReactDOM.render(
	elementObj,
	document.getElementById('rootId');
)
```

### state

react的state类似props，不同之处是props传递的是确定的值，state传递的值会改变。

设置state：

`this.state={a:123}`

获取state值：

`{this.state.a}`

改变state的值

`this.setState({a:456})`

setState一般是放在某个事件里：

```html
<inpt type="button" onclick={this.submitBtnFn.bind(this)}></inpt>

<script>
    submitBtnFn(){
        this.setState({
            a:666
        })
    }
</script>

```

## router

## Redux

Redux就是ReactJS的状态管理。

ReactJs有两方面没有涉及：

* 代码间的组织结构
* 组件间的通讯

Redux就是为了将数据从组件上剥离开，但，并不是必须的，是为了解决复杂的应用。

### store

用来保存数据的地方，整个app应用，应该只有一个store对象。

#### createStore()

用来生成store对象

`import {createStore} from 'redux'`

`let store = createStore(fn);`

fn是一个参数，是`reducers`，叫计算过程。

#### State

`store.getState()`从store当中取得状态。

redux约定，一个state对应一个view。

#### Action

导致state发生变化。

例如，点击一个按钮，就触发了一个action。

这时我们说，view发出了一个action，它会通知state，从store中取得数据。

action是一个对象，结构如下：

```javascript
let action={
    type:'xxx',
    info:'abc 123 xxx'
}
```

type属性是必须的，其它随意。

在redux中，改变state的唯一方法就是action。

#### dispatch

它是view发出action的唯一方法。

`store.dispatch( action )`

#### reducer

action里有一个type属性，用来分辨不同的操作，这就是reducer。

reducer是一个函数，有两个参数：

* 当前的state
* action

当state变化的时候，会自动触发`subscribe()`方法，调用`ReactDOM.render（）`方法，重新渲染页面，给出一个新的state，导致view视图的变化，这个过程就叫reducer。