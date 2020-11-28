## Vue的简介

可传递的参数（需要按照顺序传入，否则无法解码）：

el：选取元素

data：数据（字符串、数组、对象）

methods：方法（函数）

computed：计算属性（写作函数，其实是属性），需要多次调用的表达式，比methods更高性能

filters：过滤器，处理数据的表达式，需要过滤的数据要作为参数传递给过滤方法。调用时的写法`data|过滤器`

## 插值操作

### mustache语法

在标签之间插入值：

```html
<div id="app">
    <p>
        <!--mustache-->
        {{message}}
    </p>
</div>
<script>
const app=new Vue({
    el:'#app',
    data:{
        message:'Hello!'
    }
})
</script>
```

### 其它插值操作

#### v-once

插值一次就不能再对其进行改变

`<p v-once>{{message}}</p>`

#### v-html

直接解析html标签文本

`<div v-html>{{url}}</div>`

`url:'<a href:"http://www.baidu.com">百度一下</a>'`

#### v-text

`<div v-text="message">html<div>`

最终指定的“message”内容会覆盖掉“html”，“html”不会出现。

#### v-pre

`<div v-pre>{{message}}</div>`

原样显示标签内的内容，最终显示的结果是“{{message}}”。

#### v-cloak

```html
<style>
    [v-cloak]{
        display:none;
    }
</style>
<body>
    <div id="app" v-cloak>
        {{message}}
    </div>
    <script>
    const app=new Vue({
        el:'#app',
        data:{
            message:'Hello!'
        }
    })
    </script>
</body>
      
```

在`<script>`中的内容加载出来之前，id为”app“的`<div>`中的内容不会显示。

## 动态绑定属性

### v-bind

语法：

`<img v-bind:src="imgUrl"><img>`

`<a :href="url">百度一下<a>`

语法糖：

`v-bind:src`直接简写成`:src`

### 动态绑定class

对象语法：

```html
<style>
    .active{
        color:red;
    }
</style>
<body>
    <div id="app" :class="{active:isActive}">
        Hello!
    </div>
    <script>
    const app=new Vue({
        el:'#app',
        data:{
            isActive:true
        }
    })
    </script>
</body>
```

数组语法（不常用）：

```html
<style>
    .active{
        color:red;
    }
</style>
<body>
    <div id="app" :class="[active,link]">
        Hello!
    </div>
```

### 动态绑定style

对象语法：

```html
<body>
    <div id="app" :style="{fontSize:finalSize,color:finalColor}">
        Hello!
    </div>
    <script>
    const app=new Vue({
        el:'#app',
        data:{
            finalSize:'50px',
            finalColor:'red'
        }
    })
    </script>
</body>
```

数组语法（不常用）：

```html
<body>
    <div id="app" :style="[style1,style2]">
        Hello!
    </div>
    <script>
    const app=new Vue({
        el:'#app',
        data:{
            style1:{
                color:'red'
            },
            style2:{
                fontSize:'10px'
            }
        }
    })
    </script>
</body>
```

*对象里字符串用“ \' \' ”引用，变量不需要引号来引用。

## 计算属性

### computed与methods的区别

多次调用时，computed中的函数只会被运行一次，而methods中的函数每次调用都会被运行。

实例：

```html
<div id="app">       
        <h2>{{getFullName()}}</h2>
    <!--fullName是属性，调用时后面不需要加双括号-->
        <h2>{{fullName}}</h2>
    </div>
    <script src="../js/vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                firstName: 'Helene',
                lastName: 'Woods'
            },
            computed: {
                //fullName其实是属性，不是函数，以下写法相当于一个语法糖
                fullName: function() {
                    return this.firstName + " " + this.lastName
                }
            },
            methods: {
                getFullName() {
                    return this.firstName + " " + this.lastName
                }
            }
        })
    </script>
```

### 计算属性的setter和getter

上面实例中computed的完整写法应该是：

```javascript
 computed: {
                // fullName() {
                //     return this.firstName + " " + this.lastName
                // }
                //计算属性一般是没有set方法，只读属性
                fullName: {
                    get: function() {
                        return this.firstName + " " + this.lastName
                    }
                }
            }
```

## 事件监听

语法：`v-on:click='btnClick'`

语法糖：`@click='btnClick'`

参数：`@click="btnClick()"`

如果需要参数，但在调用方法是省略了双括号，vue会自动将浏览器生成的event作为参数传入方法。

同时手动传递event和其他参数的语法：`@click='btnClick(123,$event)'`。

v-on修饰符

* `v-on:click.stop='btnClick'`阻止冒泡事件
* .prevent 阻止默认事件
* `@keyup.enter`监听按键事件
* .once 事件只触发一次
* .native 监听组件根元素的原生事件

## 条件判断

v-if、v-else、v-else-if

`<p v-if='true'>你好</p>`

boolean为true时显示，为false时隐藏。

如果需要重用同样的元素（如input），使用v-if指令会导致元素切换时文本框里的内容不会重置，为了避免，需要在标签中添加key。

```html
<div id="app">
        <span v-if="isUser">
           <label for="username">用户账号</label>
        <input type="text" id="username" placeholder="用户账号" key="username">
        </span>
        <span v-else>
            <label for="username">用户邮箱</label>
        <input type="text" id="username" placeholder="e-mail" key="email">
        </span>
        <button @click="isUser=!isUser">切换类型</button>
    </div>
    <script src="../js/vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                isUser: true
            }
        })
    </script>
```



与v-show的区别：

v-if的条件为false时，是将包含指令的元素从DOM中删除，变为true再重新创建一个新的元素。

v-show的条件为false时，是给元素添加一个display为none的行间样式。

如果是需要重复切换状态的场景，用v-show指令的性能更高。

## 循环遍历

遍历数组：

`v-for="(item,index) in array"`遍历过程中可获取索引值

遍历对象：

`v-for="(value,key,index) in obj"`遍历对象取值的顺序是重要的放在前面

在遍历过程中添加`:key='item'`可以提高性能，key需要和取值一一对应，index随时会发生变化，并不具备唯一性，所以最好是使用item变量来绑定取值。

举例：

```html
<div id="app">
        <ul>
            <li v-for="item in letters" :key="item">{{item}}</li>
        </ul>
    </div>
    <script src="../js/vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                letters: ['a', 'b', 'c', 'd', 'e']
            }
        })
    </script>
```

### 响应式数组方法

push()、pop()、shift()、unshift()、splice()、sort()、reverse()都是响应式的。

通过索引值重写数组中的元素是无法响应的：`this.letters[0] = 'bbb';`，后台打印是被重写的，但页面显示的数据并未发生变化。

想要达到以上效果可以使用splice()方法替换数组元素，或者使用vue自带的方法`Vue.set(this.letters, 0, 'bbb')`。

### 高级函数

for循环的高级函数：filter、map、reduce

假设一个场景，需要在数组中过滤出小于100的数，然后将每个数都乘以2，最后得到这几个数的和。

```html
const nums=[10, 20, 111, 222, 444, 40, 50];
let newNums=nums.filter(function(n){
	return n<100;
});
let new2Nums=newNums.map(function(n){
	return n*2;                  
});
let total=new2Nums.reduce(function(preValue,n){
     return preValue+n;             
});
//可以写成:                 
// let total = nums.filter(function(n) {
            //     return n < 100;
            // }).map(function(n) {
            //     return n * 2;
            // }).reduce(function(preValue, n) {
            //     return preValue + n;
            // }, 0)
```

箭头函数的写法：

```javascript
let total=   nums.filter(n=>n<100).map(n=>n*2).reduce((preValue,n)=>preValue+n);
```

## 过滤器

```html
<body>
    <div id="app">
        <div>
            <!--需要过滤的数据+“|”+过滤器-->
            {{price|showFinalPrice}}
        </div>
    </div>
    <script>
    new Vue({
        el:'#app',
        data:{
            price:82.00
        },
        //过滤器的参数就是需要过滤的数据
        filters:{
            showFinalPrice(price){
                return '￥'+price.toFixed(2);
            }
        }
    })
    </script>
</body>
```



## v-model的使用

实现表单的双向绑定，主要用于前后端交互相互传递数据。

语法：`<input type="text" v-model="message">`

### 原理

1. 用v-bind动态绑定input的value的值为“message”
2. 用v-on监听input的input事件，使“message”的值等于输入到input中的值

### v-model与不同表单类型的结合

#### radio

```html
 <div id="app">
        <label for="male">
            <!-- 绑定了v-model之后就不需要定义name了-->
            <!-- 本例中默认选择男-->
            <input type="radio"  id="male" value="男" v-model="sex">男
        </label>
        <label for="female">
            <input type="radio" id="female" value="女" v-model="sex">女
        </label>
        <h2>{{sex}}</h2>
    </div>
    
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                message: '你好啊',
                sex: '男'
            }
        })
    </script>
```

#### checkbox

单选框：

v-model的值是一个布尔值，为true时选中，为false时不选中。

```html
<label for="agreement">
            <input type="checkbox" name="" id="agreement" v-model="agree">同意协议
        </label>
        <h2>{{agree}}</h2>
        <button :disabled="!agree">下一步</button>

 <script>
        const app = new Vue({
            el: '#app',
            data: {
                agree: false, //单选框   
        })
    </script>
```



多选框：

**必须要有value值**，value值会传输到data里v-model绑定的参数里。

如果不传value值，点击任何一个选项都会全选。

```html
 <input type="checkbox" value="篮球" v-model="hobbies">篮球
 <input type="checkbox" value="足球" v-model="hobbies">足球
 <input type="checkbox" value="乒乓球" v-model="hobbies">乒乓球
 <input type="checkbox" value="羽毛球" v-model="hobbies">羽毛球
 <h2>您的爱好是：{{hobbies}}</h2>

<script>
        const app = new Vue({
            el: '#app',
            data: {
                hobbies: [], //多选框
            }
        })
    </script>
```

值绑定(for循环)：

```html
 <label :for="item" v-for="item in originHobbies">
            <input type="checkbox" :value="item" :id="item" v-model="hobbies">{{item}}
        </label>

 <script>
        const app = new Vue({
            el: '#app',
            data: {
                hobbies: [], 
                originHobbies: ['篮球', '足球', '乒乓球', '羽毛球', '台球', '高尔夫球']
            }
        })
    </script>
```

#### select

```html
 <div id="app">
        <!-- 选择一个 -->
        <select name="abc" id="" v-model="fruit">
            <option value="苹果" >苹果</option>
            <option value="香蕉" >香蕉</option>
            <option value="西瓜" >西瓜</option>
            <option value="榴莲" >榴莲</option>
            <option value="葡萄">葡萄</option>
        </select>
        <h2>{{fruit}}</h2>

        <!-- 选择多个 -->
        <select name="box" id="" v-model="fruits" multiple>
            <option value="苹果" >苹果</option>
            <option value="香蕉" >香蕉</option>
            <option value="西瓜" >西瓜</option>
            <option value="榴莲" >榴莲</option>
            <option value="葡萄">葡萄</option>
        </select>
        <h2>{{fruits}}</h2>
    </div>
  
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                //单选可以设置默认值
                fruit: '香蕉',
                //多选
                fruits: []
            }
        })
    </script>
```

### v-model的修饰符

```html
 <div id="app">
        <!-- 1.修饰符：lazy -->
        <!-- 让数据在失去焦点或敲击回车时才更新 -->
        <input type="text" v-model.lazy="message">
        <h2>{{message}}</h2>

        <!-- 2.修饰符：number -->
        <!-- 当input的type是number时，加入修饰符可以保证输入的数据类型都是数字类型，否则输入的数字默认为字符串类型 -->
        <input type="number" v-model.number="age">
        <h2>{{age}}-{{typeof(age)}}</h2>

        <!-- 3.修饰符：trim -->
        <!--去掉多余的空格-->
        <input type="text" v-model.trim="name">
        <h2>{{name}}</h2>
    </div>
    
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                message: '你好啊',
                age: 18,
                name: ''
            }
        })
    </script>
```

## 组件化开发

将大的项目细分成树模型之后，对各个小组件进行单独开发。

能够最大限度地对组件进行重用，提高开发效率。

组件十分类似于Vue实例，Vue实例可以看做是root组件。

### 全局组件和局部组件

```html
 <script>
       //创建组件构造器对象
        const cpnC = Vue.extend({
            template: `
            <div>
                <h2>我是标题</h2>
                <p>我是内容</p>
            </div>`
        })

        // 注册组件
        //(全局组件，意味着可以在多个vue的实例下面使用）
        // Vue.component('mycpn', cpnC);

        const app = new Vue({
            el: '#app',
            data: {
                message: '你好啊'
            },
            components: { 
                //局部组件
                cpn: cpnC
            }
        })

        const app2 = new Vue({
            el: '#app2'
        })
    </script>
```

### 父组件和子组件

子组件要在父组件中注册，父组件能够直接使用子组件。

Vue实例可以看做是root组件，可以直接使用父组件。

```javascript
	    // 1.创建第一个组件构造器(子组件)
        const cpnC1 = Vue.extend({
                template: `
            <div>
                <h2>我是标题1</h2>
                <p>我是内容哈哈哈哈</p>    
            </div>
            `
            })
        // 2.创建第二个组件构造器(父组件)
        //在父组件中使用子组件
        const cpnC2 = Vue.extend({
            template: `
            <div>
                <h2>我是标题2</h2>
                <p>我是内容呵呵呵呵</p>   
                <cpn1></cpn1> 
            </div>
            `,
            components: {
                //注册子组件
                cpn1: cpnC1
            }
        })
```

### 组件语法糖

原始写法：

```html
const cpn=Vue.extend({
	template:`
		<div>
            <h2>
                我是标题
            </h2>
		</div>`
});
Vue.component('cpn',cpn);
```

语法糖：

```html
Vue.component('cpn',{
	template:`
		<div>
            <h2>
                我是标题
            </h2>
		</div>`
});
```

将模板分离：

script标签语法：

```html
<script type="text/x-template" id="cpn1">
        <div>
            <h2>我是标题1</h2>
            <p>我是内容，哈哈哈哈</p>
        </div>
</script>
```

*注意script的type属性

template标签语法：

```html
<template id="cpn2">
        <div>
            <h2>我是标题2</h2>
            <p>我是内容，呵呵呵</p>
        </div>
</template>
```

创建组件时直接引用id即可：

```html
Vue.component('cpn1', {
            template: '#cpn1'
})
```

### 组件中的数据

组件无法直接使用Vue实例中的数据，组件有自己的数据储存位置。

```html
Vue.component('cpn1', {
            template: '#cpn1',
            data() {
                return {
                    title: 'abc',
                    txt: '我是内容'
                }
            }
})
```

*组件的data是一个函数，且只能是一个函数

因为组件需要被重用，data只有是函数才能保证不会被共用，这样组件的数据变化就不会相互影响。

### 组件通信

#### 父传子

原理：使用props传递；

在使用组件时，在组件标签里动态绑定props的key，将父组件中的数据传递给子组件的props。

```html
 <div id="app">
        <cpn :cmessage='message' :cmovies='movies'></cpn>
    </div>

    <template id="cpn">
        <div>
            <ul>
                <li v-for="item in cmovies">{{item}}</li>
            </ul>
            <h2>{{cmessage}}</h2>
        </div>
    </template>

    <script>
        const cpn = {
            template: '#cpn',
            props: {
                cmessage: {
                    type: String,
                    default: 'aaaa',
                    required: true
                },
                cmovies: {
                    type: Array,
                    default () {
                        return [];
                    }
                }
            }
        }

        const app = new Vue({
            el: '#app',
            data: {
                message: '你好啊',
                movies: ['海王', '海贼王', '海尔兄弟']
            },
            components: {
                cpn
            }
        })
    </script>
```



props的数组写法(不常用)：

`props: ['cmovies', 'cmessage']`

props的对象写法：

```html
props:{
	//必须是字符串
	cmessage:String,
	//默认值（不传参数时显示）
	default:'abc',
	//规定为必传值
	required:true
}
```

*当规定数据是字符串或数组时，默认值必须是一个函数`default(){return []}`

*数据的key最好不要使用驼峰写法，因为v-bind不支持驼峰。

#### 子传父

原理：通过监听事件传递数据到父组件。

1. 给子组件的模板绑定事件，要传递的参数传入到事件当中；
2. 用$emit（每个组件都有的一个变量）发射自定义事件到父组件；
3. 父组件绑定该事件，并接受通过事件传来的参数。

```html
<div id="app">
        <cpn @itemclick="cpnClick"></cpn>
    </div>

    <template id="cpn">
        <div>
            <button v-for="item in categories" @click="btnClick(item)">{{item.name}}</button>
        </div>
    </template>

    <script>
        const cpn = {
            template: '#cpn',
            data() {
                return {
                    categories: [{
                        id: 'aaa',
                        name: '热门推荐'
                    }, {
                        id: 'bbb',
                        name: '手机数码'
                    }, {
                        id: 'ccc',
                        name: '家用电器'
                    }, {
                        id: 'ddd',
                        name: '电脑办公'
                    }, ]
                }
            },
            methods: {
                btnClick(item) {
                    this.$emit('itemclick', item)
                }
            }
        }

        const app = new Vue({
            el: '#app',
            data: {
                message: '你好啊'
            },
            components: {
                cpn
            },
            methods: {
                cpnClick(item) {
                    console.log(item);
                }
            }
        })
    </script>
```

`$emit()`要传入两个参数，第一个是自定义事件的名称，第二个是要传递的参数。

按理说，如果事件需要传递参数，但在引用的时候没有写双括号，会默认将浏览器事件作为参数，但在这个场景下，则是默认传递子组件传入的参数。

### 组件访问

#### 父访问子 -$children&$refs

$children(很少用)，在父组件中使用，返回的是个数组类型，通过其访问子组件的data、methods等。

语法：

```javascript
methods:{
    btnClick(){
        for(let c of this.$children){
            console.log(c.name);//使用子组件中的数据name
            c.showMessage();//使用子组件中的methods
        }
    }
}
```

$refs比较常用，返回的是一个对象类型，必须在父组件模板中引用自定义子组件标签时加上一个`refs`属性`<cpn ref="aaa"></cpn>`，就可以在多次引用子组件时，指定那个子组件访问子组件的方法。

语法：

```javascript
methods:{
    btnClick(){
       console.log(this.$refs.aaa.name);
        }
    }
}
```

#### 子访问父 -$parent&root

这两个方法都不怎么常用。因为引用子组件的父组件是不确定的，并不可能知道父组件或根组件中是否含有要访问的属性。

语法：

`console.log(this.$parent.name);`

`console.log(this.$root.message);`

### 插槽slot

在子组件中使用插槽标签`<slot>`，当父组件使用子组件时，可以在子组件标签内放入任意多个内容来替代插槽标签，甚至是一个组件。

可以为`<slot>`标签设置默认内容，当父组件里没有为插槽添加任何内容时，默认使用预先设置的内容进行渲染。

例如在一个`<submit-button>`组件中

```javascript
<button type="submit">
  <slot>Submit</slot>
</button>
```

父级组件不提供任何插槽内容

```javascript
<submit-button></submit-button>
```

最终渲染出的效果就会是这样：

```javascript
<button type="submit">
  Submit
</button>
```

#### 具名插槽

> 新版本已废弃slot作为属性的语法

当需要多个插槽来放置相应内容时，可以为插槽添加`name`属性：

```html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

向这些具名插槽提供内容时，必须在`<template>`元素上使用`v-slot`指令，并用`v-slot`的参数来指定放在哪个具名插槽里：

```html
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

任何没有被包裹在带有 `v-slot` 的 `<template>` 中的内容都会被视为默认插槽的内容。

#### 作用域插槽

> 新版本废弃了`slot-scope`属性的语法

组件插槽可以访问使用该组件的父级组件所在的作用域，但不能访问定义这个插槽的组件的作用域。而作用域插槽可以做到访问定义这个插槽的组件的作用域。

比如一个`<current-user>`组件：

```html
<span>
	<slot>{{user.lastname}}</slot>
</span>
```

为了能够让父级组件访问到`user`这个变量，可以将`user`作为`slot`的属性绑定上去：

```html
<span>
	<slot v-bind:user='user'>{{user.lastname}}</slot>
</span>
```

这个绑定在`slot`上的属性称为插槽的`prop`，现在在父级作用域中，我们可以使用带值的 `v-slot` 来定义我们提供的插槽 prop 的名字：

```html
<current-user>
	<template v-slot:default='slotProps'>
    	{{slotProps.user.firstName}}
    </template>
</current-user>
```

#### 默认插槽语法糖

当只有默认插槽时，`v-slot`可以直接添加到组件标签上：

```html
<current-user v-slot='slotProps'>
	{{slotProps.user.firstName}}
</current-user>
```

**这种语法不能和具名插槽混用，因为会导致作用域不明确。**

```html
<!-- 无效，会导致警告 -->
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
  <template v-slot:other="otherSlotProps">
    slotProps is NOT available here
  </template>
</current-user>
```

#### 解构插槽Prop

作用域插槽的内部工作原理是将你的插槽内容包括在一个传入单个参数的函数里：

```javascript
function (slotProps) {
  // 插槽内容
}
```

也就意味着，可以使用ES2015解构来传入具体的插槽prop，使模板更简洁，甚至可以给prop重命名：

```html
<current-user v-slot="{user:person}">
	{{person.firstName}}
</current-user>
```

甚至定义插槽的默认内容：

```html
<current-user v-slot="{user={firstName:'Guest'}}">
	{{user.firstName}}
</current-user>
```

#### 动态插槽名

动态指令参数也可以用在 `v-slot`上，来定义动态的插槽名：

```html
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

#### 具名插槽语法糖

`v-slot:`可以替换成`#`，比如`v-slot:header`可以被重写成`#header`



## 前端模块化

### 为什么要使用模块化

* 简单写js代码可能带来全局命名冲突的问题
* 闭包可解决全局命名冲突，但大大降低了代码的重用性
* 使用模块化可以完美解决以上问题

模块化规范：

* AMD/CMD/CommonJS

### ES6中模块化的使用

* export 导出

  导出方法：

  * `export{flag,sum};`
  * `export var num1=100;`
  * `export function sum(num1,num2){return num1+num2;}`
  * export default

* import导入

  导入方法：

  * `import{sum} from "./aaa.js";`

  * 导入export default的变量：

    `import addr from "./aaa.js";//addr为导入时命的名`

  * 统一全部导入

    `import * as aaa from "./aaa.js"`将'aaa.js'里所有导出的内容合成一个对象返回。

## webpack(了解)

*学习webpack的目的主要是为了更好地理解vue cli配置的原理。*

进行模块化开发，需要用到commonJS，但浏览器并不能识别这个库，就需要通过webpack对其进行打包，并编译成浏览器可以识别的js文件。

![使用流程](F:\编程\MD笔记\img\webpack1.jpg)

### 什么是webpack

* webpack和gulp对比
* webpack依赖环境：node
* 安装webpack

### webpack的起步

* webpack配置

  在webpack.config.js中对webpack进行一些基础配置

  * 指定打包的入口和出口。

    而output出口的值是一个对象，path的路径需要是一个绝对路径。

    想要获取该绝对路径需要引用node包里的一个模块“path”，然后在output里定义属性`path:path.resolve(__dirname, 'dist')`即可。

* webpack映射

  * 在package.json中的script对象里，可以进行指令的映射。

    比如需要执行打包命令“webpack”时，加入`"build":"webpack"`对象就能实现指令的映射，命令变为“npm run build”。

### webpack的loader

加载各种文件，使其能够被编译打包起来。

首先使用npm指令对需要的loader进行下载。

* css-loader/style-loader(加载并绑定css)
* less-loader/less(加载less并将less编译成css)
* url-loader/file-loader(加载图片)
* babel-loader(将ES6编译成ES5)

安装loader后还要在**webpack.config.js中的module>rules>use**配置。

### webpack中配置vue

安装vue，默认使用runtime-only模式。

vue-template-compiler用来解析代码中的template代码。

在webpack.config.js里加入一个配置就可使用runtime-compiler：

```javascript
resolve:{
    alias:{
        'vue$':'vue/dist/vue.esm.js'
    }
}
```

加载`.vue`文件需要下载相应的loader：vue-loader和vue-template-compiler。

### webpack的插件

安装插件后在webpack.config.js中的plugin中配置。

`html-webpack-plugin`插件可以将index.html文件打包进dist文件夹中。

`uglifyjs-webpack-plugin`可以在项目发布的时候压缩bundle.js文件，压缩后的js文件可读性差但性能高。

### webpack搭建本地服务器

安装`webpack-dev-server`，需要在webpack.config.js文件中配置一个`devServer:{contentBase:'./dist',inline:true}`，之后在package.json文件中的script中配置`"dev":"webpack-dev-server"`，就可以用`npm run dev`指令将项目在本地服务器中跑起来。

### 配置文件的分离

将webpack.config.js中的基础配置、开发时需要的配置和发布产品时需要的配置进行分离。

配置文件合并时需要安装`webpack-merge`插件，导出配置时的格式：

```javascript
const webpackMerge=require('webpack-merge');
module.exports=webpackMerge(需要合并的配置,{
    当前的配置内容
})
```

然后再`package.json`中将“dev”和“build”的映射后面加上`--config '配置文件的相对定位url'`以找到相应的配置文件。

## Vue CLi

### 什么是CLi

* 脚手架是什么

* CLi依赖webpack、node、npm

* 安装CLi

  `npm install -g @vue/cli`
  
  拉取CLi2模块
  
  `npm install -g @vue/cli-init`

### CLi初始化项目的过程

CLi2初始化项目：`vue init webpack my-project`

CLi3初始化项目：`vue create my-project`

### CLi生成的目录结构的解析

CLi3、CLi4生成项目后，配置文件`.config.js`都移入到了`node_modules`文件夹里，而这个文件夹里的文件不要轻易修改。

后期需要加入其它配置时，只要在根目录下添加一个`vue.config.js`文件，在里面进行相应配置。

```javascript
module.exports={
    configureWebpack:{
        resolve:{
        	alias:{
        		'assets':'@/assets'
    		}
    	}
    }
}
```

如果引用路径不是通过import方法，就需要在别名前加一个`~`。

而CLi2的配置文件在`build->webpack.base.config.js`。

### runtime-compiler和runtime-only的区别

runtime-only性能更高，代码量更少。

原理：

runtime-compiler:

```javascript
new Vue({
  el: '#app',
  components: { App },
  template: '<App/>'
})
//运行过程：template->ast->render->v DOM->UI
```

runtime-only:

```javascript
new Vue({
  el: '#app',
  render: h => h(App)
})
//运行过程：render->v DOM->UI
```

### Vue-router

#### 什么是路由

路由器的两种机制：路由和传送

* 路由是决定数据包从来源到目的地的路径
* 传送将输入端的数据转移到合适的输出端

路由中一个非常重要的概念：路由表。其本质是一个映射表，决定了数据包的指向。

#### 前端渲染和后端渲染

* 以前，后端不仅要处理后端逻辑，还要负责渲染页面，此时使用的路由是后端路由。

  后端服务器将页面渲染好，当客户输入url后，通过路由向后端请求对应url的页面信息，就将后端渲染的页面传输到前端。

* 现在，前端实现了渲染，前后端的职责划分更加明显，就开始使用前端路由，实现了SPA（单页富应用）。

  不再需要请求后端服务器，前端直接将所有页面代码html、css、js资源从静态资源服务器中提出，客户输入url后便通过路由映射请求对应的组件来进行页面的渲染。

### url的hash和HTML5的history

可以达到改变url但页面并不跳转的效果。

#### hash

`location.hash="home"`指令可以改变url。

#### history

`history.pushState({},'','home')`在栈结构中push一个数据。

*栈结构是先入后出的。

`history.back()`从栈结构中去掉一个最新push的数据，也就相当于浏览器的后退键，返回到了上一个页面。

`history.replaceState({},'','about')`直接替换掉当前的数据，也就意味着当前页面无法再跳转到上一个页面里。

`history.go()`可以传一个值，当传入的值为“-1”时，相当于`history.back()`；传入的值为"1"时，相当于`history.forward`；当然，也可以传入其它任何整数进行页面的改变。

### Vue-router的使用

#### 路由的配置

1. 导入路由对象，并且调用`Vue.use(VueRouter)`

2. 创建路由实例，并且传入路由映射设置

   ```javascript
   const routes=[{
   //当进入页面的时候url路径就指向"/home"
   path:'',
   redirect:'/home'
   }，{
   path：'home',
   component:'home'
   },{
   path:'/about',
   component:about
   }]
   
   const rounter=new VueRouter({
   //配置路由和组件的映射关系
   routes,
   //默认url是hash模式，一下配置将hash改为history模式
   mode:'history'
   })
   ```

   

3. 在Vue实例中挂载创建的路由实例

   首先输出router：`export default router;`

   再挂载：

   ```javascript
   import router from './router'//默认会直接找到router文件夹下面的index文件，所以可以省略路径的写法
   new Vue({
       el:"#app",
       router,
       render:h=>h(App)
   })
   ```

#### 路由的使用

在组件`App.vue`文件中的模板中使用`<router-link>`和`<router-view>`来运用路由。

router-link标签时vue-router中内置的组件，默认被渲染成`<a>`标签。

`<router-link>`的属性：

* `tag`属性指定其被渲染成其它标签。`tag="button"`

* `to`属性指定渲染哪个url的组件。`to="/home"`

* `replace`属性让页面无法后退至前一个页面。

* `active-class`属性更改标签被点击后的class名。

  也可直接在创建router对象时，传入`linkActiveClass`属性来批量修改。

#### 通过代码跳转路由

上面是通过vue-router的`<router-link>`标签实现路由跳转的，还可以通过绑定事件实现跳转。

但最好不要直接使用history.pushState()之类的方法进行跳转，而是用vue-router自带的$router属性来实现。

```vue
<template>
	<div id="app">
        <button @click="homeClick">首页</button>
    	<button @click="aboutClick">关于</button>
    </div>
</template>
<script>
	export default{
        name:'App',
        methods:{
            homeClick(){
      			this.$router.push('/home');
   			 },
   			 aboutClick(){
     			 this.$router.push('/about');
   			 }
        }
    }
</script>
```

但是这种方法如果多次点击同一个按钮会报错，只要在调用push的时候设置这两个回调函数就能解决：

`this.$router.push({path:'/home'}, onComplete => { }, onAbort => { });`

或者直接在mian.js或者路由配置文件里加上：

```javascript
const originalPush = Router.prototype.push;
Router.prototype.push = function push(location) {
    return originalPush.call(this, location).catch(err => err);
}
```



#### 动态路由

场景：在开发用户界面时，一般在url后面加上用户id的hash值，在配置路由路径时如下：

`path:'/user/:userId(可以任意赋值)'`

从后台获取的用户id就能通过在`router-link`标签上动态绑定`to="'/user/'+userid"`属性，添加入hash值。

想要在user页面的组件里获取到用户id的hash值，就可以使用`this.$route.params.userId`方法来得到。

> `$route`是当前活跃的路由

> `userId`就是在配置路由path时在`/user/`	后面命名的变量。

#### 路由懒加载

当打包时，随着项目越来越大，js包也会越来越大，

这时候就需要把不同路由对应的组件分割成不同的代码块，

当被路由访问时才加载对应组件，从而提高页面加载效率。这就是懒加载。

懒加载的用法就是在配置路由时改变组件的引用方式：

```javascript
path:'/home',
component:()=>import('../components/Home')
```

#### 路由的嵌套

子路由的配置是要放在父路由的`children`属性里。

```javascript
path: '/home',
component: Home,
children: [{
      path: 'news',
      component: News
    }, {
      path: 'message',
      component: Message
    }]
```

然后在父路由对应的组件里加入`<router-link>`和`<router-view>`标签，注意`to="/home/news"`属性的路径要写完整。

#### 传递参数

在`<router-link>`里动态绑定属性`:to="{path:'/profile', query:{name:'helene',height:1.6,age:18}}"`，to的值是一个对象，指定路由的path，并且传入"query"属性，把需要传递的参数以对象的形式写入。

在需要接收数据的组件里使用`$route.query`来获取参数。

#### 导航守卫

通过监听路由的跳转来触发一些事件。

##### 全局导航守卫

全局导航守卫能够监听所有路由的跳转。

路由跳转之前可以调用beforeEach函数（前置钩子）：

```javascript
router.beforeEach((to, from, next) => {
  //title为给路由配置的meta属性的参数，这行代码实现的是跳转路由后修改title的值
  document.title = to.matched[0].meta.title;
  next();//必须调用next函数
})
```

* to：即将要进入的目标的路由对象
* from：当前导航即将要离开的路由对象
* next：调用该方法后，才能进入下一个钩子

路由跳转之后可以调用afterEach函数（后置钩子）：

```javascript
router.afterEach((to,from)=>{
  //没有next参数，就不需要调用next函数
})
```

##### 路由独享守卫

想要单独监听一个路由的跳转，只需要在该路由里直接定义`beforeEnter`守卫：

```javascript
path:'/home',
component:Home,
beforeEnter:(to,from,next)=>{}
```

##### 组件内守卫

直接在组件里定义的守卫：

* `beforeRouteEnter(to,from,next){}`
* `beforeRouteUpdate(to,from,next){}`
* `beforeRouteLeave(to,from,next){}`

#### keep-alive

router-view也是个组件（vue-router的组件），如果直接被包在keep-alive里，所有路径匹配到的视图组件都会被缓存。

keep-alive是Vue内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染，可以提高页面性能。

它有两个重要的属性，可传入字符串或正则表达式：

* include
* exclude：值为组件的name

## 生命周期

从vue的实例的创建、运行、到销毁的整个过程中发生的事件就是vue的生命周期函数。

vue生命周期主要有三个阶段：

* 创建：
  * `beforeCreate`--实例刚刚在内存中创建出来，此时data、methods还没有初始化
  * `created`--实例已经在内存中创建，data、methods都已经ok，但模板还没有编译
  * `beforeMount`--已经完成模板（template）的编译，但数据还未挂载到页面上，这时获取到的只有模板上的字符串
  * `mounted`--模板已经编译并挂载在页面上了
* 运行
  * `beforeUpdate`--状态更新之前调用此函数，这是data的值已经是最新的，但相应的视图还没有开始更新
  * `updated`--实例更新完毕之后调用此函数，data和视图上显示的数据是一致的
* 销毁
  * `beforeDestroy`--实例在被销毁之前调用，在这一步，实例仍是可用的
  * `destroyed`--实例销毁之后调用，调用之后，vue实例的所有东西都会被解除绑定，所有事件监听器都会被移除，所有实例都被销毁。

## Promise

Promise是ES6的一个特性。

是异步编程的一种解决方案。一般常见的场景就是网络请求。

当网络请求很复杂的时候，就会出现一些问题。

### Promise的基本使用

```javascript
new Promise((resolve,reject)=>{
    //本例中的异步操作是定时器
    setTimeout(()={
        resolve(data)
    	reject(err)
    })
}).then(data=>{},err=>{})
```

Promise是一个类，创建之后有一个参数，参数是一个函数，并且该函数有两个参数resolve和reject(这两个参数也是函数)，异步操作被包裹在Promise里，在异步操作中调用resolve函数；

然后执行then()，then函数也有个参数是函数，在该函数里进行具体的数据处理。

整个过程实现的是链式编程，使得代码逻辑更加清晰。

### Promise的三种状态

* pendding：等待状态，正在进行网络请求或定时器还没有到时间。
* fullfill：满足状态，主动回调了`resolve`时，就处于该状态，并且会回调`.then()`。
* reject：拒绝状态，主动回调了`reject`时，就处于该状态，并且会回调`.catch()`——catch可以不写，其参数可以写在`.then()`的参数一起。

### Promise的简写

如果只需要进行一次网络请求，可以在不需要进行网络请求的地方对Promise进行简写。

完整写法：

```javascript
return new Promise(resolve=>{resolve(res+'111')})
```

简写：

```javascript
return Promise.resolve(res+'111')
```

进一步简写：

```javascript
return res+'111'
```

reject的简写：

```javascript
return Promise.reject('error message')
//最后别忘了还要加一个.catch(err=>{console.log(err)})
```

reject的另一个简写：`throw 'error message'`

### Promise.all方法

Promise.all可以处理两次不同的网络请求得到的数据。

```javascript
Promise.all([
    new Promise((resolve,reject)=>{
        //这里使用setTimeout模拟网络请求
        setTimeout(()=>{
            resolve('result1')
        })
    }),
    new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve('result2')
        })
    })
]).then(results=>{
    console.log(results);
    //最终得到的是一个数组[result1,result2]
})
```

## Vuex

Vuex是一个专为Vue.js应用程序开发的状态管理模式。

状态管理是什么？

* 简单来说，状态可以看做是变量，而状态管理就是将***多个组件共享的变量***存储到一个对象里；
* 这个对象里的数据是响应式的。

什么时候会用到VueX：

* 比如用户的登录状态、用户名称、头像、地理位置信息等；
* 比如商品的收藏、购物车中的物品等。

### Vuex的使用

* 首先将Vuex下载下来；

* Vuex的配置和router差不多，创建一个store文件夹，再创建一个`index.js`文件进行配置；

  * 引用`Vue`和`Vuex`
  * 通过`Vue.use(Vuex)`进行安装
  * `const store=new Vuex.Store({})`创建一个Vuex里的类：Store。里面有固定的几个参数：
    * state:{}--需要共用的响应式数据
    * mutations:{}--修改状态
    * actions:{}--异步操作
    * getters:{}--类似于计算属性
    * moduls:{}--划分模块
  * `export default store`导出store
  * 挂载到main.js里

* 在各组件里就可以通过`$store.state.[变量]`来获取状态了；

* 对于状态的修改，官方推荐必须要通过mutations（虽说组件也能够修改成功），因为将会有一个devtools插件来记录修改状态的历史。

  修改状态可以跳过action，action是用来处理异步操作的，当mutations里面有异步操作时才不能跳过action。
  
* mutations里面可以定义修改状态的方法，方法函数自带一个state参数。在组件事件绑定的methods里，用`this.$store.commit()`来调用mutations进行状态更新。

### 核心概念

#### state

##### 单一状态树

只创建一个store来对状态进行管理。

##### 响应规则

响应式的数据必须在store里初始化好需要的属性。

如果直接使用`state.info['address']='Los Angelas'`，页面是不会进行刷新的，而需要使用Vue自带的方法`Vue.set(state.info,'address','Los Angelas')`来进行响应。

删除时用`Vue.delete(state.info.'age')`。

##### 响应式原理

```javascript
info:{
    name:'kobe',
    age:40,
  	height:1.98
}
```

info的每一个属性初始化后都会有一个Dep来监听它的变化，一旦发生变化，就会根据一个数组`[Watcher,Watcher,Watcher]`来确定有几个地方是需要进行数据刷新的。

#### Getters

类似计算属性。

* 函数同样有一个state参数，第二个参数是getters。

* 定义好getters方法后可以在组件直接使用`$store.getters`来调用需要的属性。

* 方法内部还可以通过getters参数来获得其他属性。

* 如果想要接收组件传来的值，可以在getters方法内部返回一个函数，组件调用时就能够传递参数。

  ```html
  Vuex:
  getter:{
  	rightStudents(state,getters){
  		return function(age){
  			return state.student.filter(s => s.age > age)
  		}
  	}
  }
  组件：
  <ul v-for="s in $store.getters.rightStudents(15)"></ul>
  <li>{{s}}</li>
  ```

##### mapGetters

能够在计算属性中直接映射getters方法

* 首先在组件中导入`import {mapGetters} from 'vuex'`
* 然后在计算属性中进行映射`...mapGetters(['',''])`，或者也可以给getters改名`...mapGetters({XXX:'',XXX:''})`

#### mutations

Vuex状态更新唯一指定方式。

mutation可以分为两个部分，一个是事件类型，一个是回调函数。

就以下面的例子来说，“increCounter”就是事件类型，后面的就是回调函数。

##### 传参

被称作是mutation的payload（负载）

用`$store.commit`方法传入两个参数，一个是mutation的名称，一个是要传递的参数，这个mutation方法就多了一个参数来接收。

```javascript
methods:{
	addCount(count){
		this.$store.commit('increCounter',count)
	}
}

mutations:{
	increCounter(state,count){
		state.counter+=count
	}
}
```

还可以传递一个对象：

```javascript
methods:{
	addStudent(){
		const stu={id:114,name:'helene',age:16}
		this.$store.commit('addStudent',stu)
	}
}

mutations:{
	addStudent(state,stu){
		state.students.push(stu)
	}
}
```

##### 提交风格

之前使用的是`this.$store.commit()`来提交，

还可以使用包含type属性的对象：

```html
methods:{
	addCount(count){
		this.$store.commit({
			type:'increCounter',
			<!--也可写成count:count-->
			count
		})
	}
}

mutations:{
	<!--第二参数变成了一个对象-->
	increCounter(state,payload){
		state.counter+=payload.count
	}
}
```

##### 类型常量

在给mutation方法命名和methods里commit类型（type）时，单纯的复制粘贴字符串很容易出错，可以在另一个文件夹里将类型作为一个常量传出。

在组件和Vuex文件里导入所有的常量`import * as myMutation`，再进行命名的时候就不容易出错了。

Vuex里mutation的写法是`[myMutation.INCREMENT]（state）{}`。

ES6允许字面量定义对象，用表达式作为对象的属性名和方法名，即把表达式放在方括号内。

#### actions

不能在mutation里进行异步操作，因为Devtools无法追踪，而要在action里进行。

action的意义是进行异步请求，而不是改状态，在action里改状态Devtools同样无法追踪。

action和mutation差不多，都有两个参数，第二个参数都是payload，但第一个参数是'context'，可以看做是`store`。

```javascript
actions:{
    aUpdateInfo(context,payload){
        setTimeout(()=>{
            context.commit('updageInfo')
        	console.log(payload.message)
        	payload.success()
        },1000)
       
    }
}

//组件的methods里的写法
methods:{
    updataInfo(){
        this.$store.dispatch('aUpdateInfo',{
            message:'我是携带信息',
            success:()=>{
                console.log('状态修改完成')
            }
        })
    }
}
```

上面的例子改成Promise写法：

```javascript
action:{
    aUpdateInfo(context,payload){
        return new Promise((resolve,reject)=>{
            setTimeout(()=>{
            context.commit('updageInfo')
        	console.log(payload)
            resolve('11111')
        	},1000)
        })
    }
}

//组件里的写法
methods:{
    updataInfo(){
        this.$store.dispatch('aUpdateInfo','我是携带信息').then(res=>{
            console.log(res)
            console.log('状态修改完成')
        })
    }
}
```

> 同样可以使用mapActions进行映射

#### modules

可以独立拥有自己的states,getters,mutations,actions,modules。

```javascript
const moduleA={
    state:{
        
    },
    mutations:{
        
    },
    getters:{
        
    },
    mutations:{
        
    },
    modules:{
        
    }
}

const store=new Vuex.store({
    modules:{
        a:moduleA
    }
})
```

和store大体相似，不过modules里的getters的方法还有一个参数`rootstate`，可以获取到store里的state。

```
 getters: {
    fullName(state) {
      return state.name + '1111'
    },
    fullName2(state, getters) {
      return getters.fullName + '2222'
    },
    fullName3(state, getters, rootstate) {
      return getters.fullName2 + rootstate.counter
    }
  }
```



modules里的actions的方法的context就不再是指store，而是指这个modules，`context.commit`可以获取到modules里的mutation方法。

```javascript
actions: {
    aUpdateName(context) {
      setTimeout(() => {
        context.commit('updateName', 'wangwu')
      }, 1000);
    }
  }
```

组件里对modules数据、方法的引用和store的一样，除了modules的state有点特殊：`$store.state.a.name`，相当于modules给store里的state加入了一个命名为'a'的对象。

ES6语法中对象的解构：

```javascript
const obj={
	name:'kobe',
	height:1.98,
	age:45
}
```

如果想要将obj对象的三个属性拿出来，本来需要这样写：`const name=obj.name`，现在同时取三个属性出来只需要写成：`const {name,age,height}=obj`，并且key的顺序并不影响拿到对应的值。

## axios

官方推荐的网络请求框架。并不是Vue自带的。

首先下载框架，然后依赖，之后就能使用了。

基础使用：

```javascript
axios.default.baseUrl='http://152.136.185.210:8000/api/n3'
axios.default.timeout=5000

axios({
  url: '/home/multidata',
  method: 'get'
}).then(res => {
  console.log(res);
})
```

axios自带Promise,爽！

### 传值

get方法的传值是`params:{}`

post方法的传值是`data:{}`

### axios.all

类似Promise.all。

```javascript
axios.all([axios({
    url:''
}),axios({
    url:''
})]).then(results=>{
    console.log(results[0])
    console.log(results[1])
})
//更简便地获取两个网络请求的数据可以使用axios.spread()
.then(axios.spread((res1,res2)=>{
    console.log(res1);
    console.log(res2);
}))
```

### 创建axios的实例

之前的例子都是使用的全局配置进行网络请求，共用url不能改变。

当需要针对不同的服务器进行网络请求的时候，就需要创建axios实例。

```javascript
const instance=axios.create({
    baseUrl='',
    timeout:5000
})

instance({
    url:'',
    //参数
    params:{}
}).then(res=>[
    console.log(res)
])
```

### axios的模块封装

当写项目的时候，可能有多个组件需要进行网络请求，这时候最好对axios进行一个封装，防止项目对第三方框架产生过多的依赖，导致后期维护成本太高。

```javascript
//./src/network/request.js

import axios from 'axios'
//可能要对多个服务器进行网络请求，所以不使用'export default'
export function request(config){
    const instance=axios.create({ 		baseURL:'http://152.136.185.210:8000/api/n3',
        timeout:5000
        //本身axios就是一个Promise，把Promise return给组件，就可以在组件里用‘.then(res=>{})’来处理config
        return instance(config)
    })
}

//组件里进行网络请求
import {request} from './network/request'

request({
    url:''
}).then(res=>{
    console.log(res)
}).catch(err=>{
    console.log(err)
})
```

### axios拦截器

axios提供了拦截器，用于在发送请求或得到响应后，进行一些处理。

#### 请求拦截

可以对axios全局进行拦截，也可以对实例进行拦截。

`axios.interceptors.request.use(config=>{},err=>{})`

从上面代码可以看出use()里有两个函数作为参数，第一个是请求成功时的拦截的参数，第二个是请求失败时拦截的参数。

* 请求成功时，拦截的参数就是给axios传递的url、timeout等config

* 拦截后进行完处理后，不要忘了将config给return回去

  ```javascript
  axios.interceptors.request.use(config=>{
      console.log(config);
      return config
  },err=>{
      console.log(err);
  })
  ```

什么时候需要进行请求拦截：

* 比如config中的一些信息不符合服务器的要求
* 每次发送网络请求时，都希望在界面显示一个loading动画
* 某些网络请求（比如登录），必须写到一些t特殊信息

#### 响应拦截

```javascript
axios.interceptors.response.use(res=>{
    console.log(res);
    return res.data
},err=>{
    console.log(err);
})
```

通常网络请求返回的结果只有data是有用的，所以在响应拦截后直接返回`res.data`就好。


