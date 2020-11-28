## Vue.extend动态创建组件

比如我们手写一个Toast组件，就需要动态创建组件，将其挂载到新创建的那个DOM上去。

* 首先准备一个toast组件，把它的`<template>`和样式都写好；现在它缺少一些数据，这些需要动态传递

* 新建一个`toast.js`文件，引入`Toast`组件和`Vue`组件

* 使用`Vue.extend()`创建一个`Toast`组件的构造器

  `const ToastConstructor =  vue.extend(Toast)`

* 然后就可以像`new Vue()`一样，new一个Toast的实例，并向里面传值

  ```javascript
  function showToast(text, duration){
    const toastDOM = new ToastConstructor ({
      //将组件挂载到一个动态创建的div上
      el: document.createElement('div'),
      //需要传入到Toast组件的数据
      data(){
          return{
              text: text,
              show:true
              }
          }
      });
      //将创建的实例插到body当中，使其渲染出来
      //Toast挂载到的DOM元素会被保存在toastDOM的$el属性里
      document.body.appendChild(toastDOM.$el);
      
    //弹窗在n秒后需要关闭  
      setTimeout(()=>{
          toastDOM.show=false;
      },duration);
  }
  ```

  * 在vue的原型上添加一个`$toast`属性，使其等于上面的那个方法

    ```javascript
    function toastRegistry(){
        Vue.prototype.$toast = showToast;
    }
    export default toastRegistry;
    ```

  * 在`main.js`里导入`toastRegistry`方法，并使用`Vue.use(toastRegistry)`方法执行它，就可以全局使用`Toast`组件了。访问语句是`this.$toast('弹窗内容',2000)`

