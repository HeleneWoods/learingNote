## 实例记录

### 小问题

* 组件的style里可以通过`@import 'url'`来引用外部css文件。

* 现在box-shadow的颜色值只能是16进制值，不能是rgba。

* 当设置插槽时，如果需要在插槽上添加其他属性，最好将插槽包在一个div标签里，把其他属性写在div标签里：

```html
<div :class="{active:isActive}">
      <slot name="item_text"></slot>
</div>
```

因为父组件替换插槽时`<slot>`会被其他标签替换掉，导致里面的一些属性消失，而slot标签外的div标签并不会被替换。

#### 配置别名

* 当路径里存在太多`../../`可以在`webpack.config.js`里配置别名`alias`：

```javascript
 resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      '@': resolve('src'),
      'assets': resolve('src/assets')
    }
```

如果不是通过import引用路径，就需要在别名前加上一个`~`，比如：`~assets/img/tabbar/home.svg`。

* 路由配置完后，url发生了变化，但页面不渲染

  * 原因：router>index.js里中的`component`写成了`components`

* css里的公共配置

  * 引用`normalize.css`来统一不同浏览器的css样式

  * `:root`伪类--获取根元素，也就是html

  * `--color-text`css变量的写法，在后面需要定义字体颜色的时候直接写成`color:var(--color-text)`

  * 常用伪类：`:hover`,`:link`,`:active`,`:focus`

  * 常用伪元素：`::before`,`::after`,`::first-line`,`::selection`

    * ::before和::after下特有的content，用于在css渲染中向元素逻辑上的头部或尾部添加内容。这些添加不会出现在DOM中，不会改变文档内容，不可复制，仅仅是在css渲染层加入。一般用来添加图标。

      ```css
      .phoneNumber::before {
          content:'\260E';
          font-size: 15px;
      }
      ```

* 修改页面图标要在`public/index.html`文件夹里，有个`<link rel='icon href="">'`的标签。

* 移动端设置吸顶效果可以用`position:sticky`

### better-scroll

* 移动端的滚动用`better-scroll`框架

* 创建BScroll实例传入两个参数，第一个是需要滚动的内容的`wrapper`，最好使用`ref`来选中，第二个是一个对象。

* 创建好实例之后就可以使用`scroll.on('事件',()=>{})`来监听滚动事件。

  * 监听滚动
    * 首先在创建实例时传入`probeType`参数，它有四个值，0和1都表示不监听，2表示监听滚动但滚动位置但不监听惯性位置，3表示监听滚动位置也监听惯性位置
    * `scroll.on('scroll',(position)=>{})`可以使用函数的position参数来确定滚动的位置，position有两个属性x,y
  * 上拉刷新
    * 创建实例传入`pullUpLoad`参数，它的值是布尔值，默认false，为true时上拉到底部刷新
    * 同样用`scroll.on('pullingUp',()=>{})`来获取更多数据进行加载，加载结束后还要用`scroll.finishPullUp()`方法来取消上拉加载，否则上拉加载只会进行一次。可以在方法上套一个定时器，防止短时间的频繁刷新。

* better-scroll内部的`<a>`标签点击会失效，在创建实例的时候加上`click：true`参数；另外`span`,`div`这些标签想要加上点击事件也需要加上click的参数。

* better-scroll有一个方法`scrollTo`，传入参数可以指定到某一位置，比如回到顶部：

  `scroll.scrollTo(0,0,500)`-参数分别是x,y,毫秒

* 无法直接监听组件的事件，加个`.native`就行了

* better-scroll可滚动区域的问题

  * better-scroll在决定有多少区域可以滚动时，是根据srcollerHeight属性决定的
    * scrollerHeight属性是根据放在better-scroll的content中的子组件的高度决定
    * 但首页中，在计算scrollerHeight属性时，图片还没有加载完成，所以没有把图片的高度算进去，计算出的数据是错的
    * 图片加载完成后，有了新的高度，但scrollerHeight没有进行更新

  * 如何解决

    * 监听每一张图片是否加载完成，只要有一张加载完成，就执行一次`refresh()`
    * 如何监听图片的加载
      * 原生js监听：img.onload=function()
      * Vuejs中监听：`@load='方法'`
    * 调用scroll的refresh


  * 如何将GoodsListItem.vue中的事件传入到Home.vue中

    * 因为涉及到非父子组件的通信，所以通过**事件总线$bus**来实现
      * `Vue.prototype.$bus=new Vue()`
      * `this.$bus.$emit('事件',参数)`
      * `this.$bus.$on('事件'，回调函数(参数))`

  * 问题一：refresh找不到的问题

    * 报错信息为“Cannot read property 'wrapper' of undefined at BScroll.refresh”
    * 解决方法：在Scroll.vue中，调用this.scroll的方法之前，判断this.scroll对象是否有值
    * 在mounted生命周期函数中使用this.$refs.scroll而不是在created中

  * 问题二：对于refresh非常频繁的问题，进行防抖操作

    * 防抖debounce/节流throttle

      * 直接执行refresh会被执行30次

      * 将refresh传入debounce中，生成一个新的函数

        ```javascript
        debounce(func,delay){
              let timer=null
              return function(...args){
                if(timer) clearTimeout(timer)
        
                timer=setTimeout(()=>{
                  func.apply(this,args)
                },delay)
              }
            }
        ```

### 吸顶效果

* tabControl的吸顶效果
* 获取tabControl的`offsetTop`
  * 无法直接在Home.vue里获取正确的offsetTop
  * 监听HomeSwiper中img的加载
  * 加载完成后，在HomeSwiper中用`$emit`发出事件，再在Home.vue中监听
  * 优化：
    * 为了防止HomeSwiper多次发出事件，
    * 用`isLoad`变量进行状态的记录
    * 一旦发出一次事件之后就改变状态
    * 这个方法和防抖操作不一样，防抖操作是等待多个请求完成后再一起调用其它方法，而这个方法是只需要进行一次请求
  * 监听滚动，动态改变tabControl的样式
  * 以上方法会导致tabControl脱离标准流
  * 复制一个新的tabControl，在滚动到正确位置之前隐藏，一旦滚动到位就显示

### 商品详情页

* 配置动态路由，在url后面添加上物品的`iid`
* 然后在详情页组件中的`created`周期函数中获取到这个`iid`，不要忘了用data吧iid储存起来。
* 通过iid向服务器请求数据，对数据进行一个展示

#### 面对对象取需要的数据

* 有时候一个详情页的子组件中需要的数据很杂，需要用对象函数将数据进行一个整合封装，方便组件对数据进行传递和提取。

### 时间戳转成格式化字符串

* 将时间戳转成Date对象
  * `const date=new Date(时间戳*1000)`
* 将Date进行格式化，转成对应的字符串

### mixin混入

当多个组件中有相同的代码，可以使用mixin；

mixin和继承的不同在于，继承是类的复用，mixin是对象的复用。

* 创建一个`mixin.js`的文件，相同代码在mounted里，就在mounted函数中写，还可以写data、methods等代码
* 在需要的组件中引入文件，并且用`mixins:[]`注册引入的代码，就会自动在相应的地方引用代码

mixin的合并

* 在周期函数中，比如created、mounted，在与组件同名的函数中写的一行代码可以和组件中已存在的同名函数代码合并
* 但在methods里相同函数名中的代码并不会合并，而会被替换掉，所以想要抽取methods函数中的一行代码可以把那行代码单独封装进一个函数中来进行抽取，不要忘记在原函数中调用新封装的函数

### 点击标题滚动到对应位置

* 在detail中监听标题的点击，获取index
* 创建一个数组参数，将对应位置的offsetTop push到数组当中
* 如何获取offsetTop
  * 通过组件的`$el`方法来获得组件的offsetTop
  * 但在什么地方获取是个问题
    * 直接在created里不行，因为这时获取不到组件元素
    * 在mounted里不行，created获取数据是异步操作，数据还没有获取到
    * created获取到数据的回调中也不行，此时的数据是不准确的，因为DOM还没有渲染好
    * $nextTick里数据不准确，因为图片的高度没有被计算进去
    * 最后在methods中的imageLoad方法里获取offsetTop，最终高度才是正确的，因为此时的数据已经获取完了，图片也都加载好了
  * 为了防止频繁获取offsetTop，在created中，将获取方法用debounce封装一下，成为一个新的回调函数后再在methods里调用一下

### 内容滚动显示正确的标题

#### 常规做法

* 重点是对现在滚动的位置和标题对应位置进行一个对比
* 首先遍历存放了标题对应位置的数组，然后对其金星秀判断进行判断
* `(this.currentIndex!==i) && ((i<length - 1 && positionY>=this.themeTopYs[i] && positionY<this.themeTopYs[i+1])||(i===length-1 && positionY>=this.themeTopYs[i]))`
  * 保存标题对应位置的数组的长度是`length`
  * 当i小于`length-1`时，并且当前位置大于等于数组下标为i的值，小于数组下标为i+1的值，
  * 或者，i等于`length-1`时，当前位置大于等于下标为i的值
  * 就让`currentIndex=i`，然后改变标题组件里的currentIndex值
  * 前面的`this.currentIndex!==i`是为了防止判断后的重复多次操作，只在临界点时进行操作

#### hack方法

性能更高，书写简单，但稍微占多内存

* 给保存标题位置的数组后面再push一个js中最大的值`this.themeTopYs.push(Number.MAX_VALUE)`

* 最终的遍历和判断这样写：

  ```javas
  for(let i=0;i<length-1;i++){
  	if(this.currentIndex!==i && (positionY>=this.themeTopYs[i] && positionY<this.themeTopYs[i+1]))
  ```

### 图片的懒加载

使用vue-lazyload

* 安装`npm install vue-lazyload --save`

* 在`main.js`导入`import VueLazyLoad from 'vue-lazyload'`
* 安装插件`Vue.use(VueLazyLoad)`
* 修改`img :src`=>`img v-lazy`

#### 添加占位图片

在use时传入options =>loading

` Vue.use(VueLazyLoad,{loading:require('imgURL')})`

### css单位转化插件

* 安装`postcss-px-to-viewport --save-dev`
* 配置`postcss.config.js`文件

### main-tab-bar可能影响页面可视高度的问题

问题：在电脑端用滚轮不会滚动页面，但在手机上可是高度发生了变化

* 思路一：给main-tab-bar设定v-show，后面的布尔值用vuex来管理，需要显示的时候在created里修改状态

### svg的简单使用

* 下载依赖`npm install svg-sprite-loader --save-dev`

* 在`components`文件夹下新建一个存放svg图形的组件

* 在`app.vue`中引入svg组件、注册并使用

* 在需要的地方使用，语法如下：

  ```html
  <!--fill是指定颜色-->
  <svg data-v-735ff1be="" fill="#fff" class="icon-mobile">
      <!--use标签中的xlink:href的值是svg图形的id
      xmlns:xlink是声明，意味着文档可访问 XLink 的属性和特性-->
      <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#mobile"></use>
  </svg>
  ```

  