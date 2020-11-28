## 动态路由

当不同的商品或者用户信息需要使用同一个组件进行渲染，就需要使用动态路由。

在路由路径中使用`动态路径参数`，给动态路径参数传的值可以通过`this.$route.params`来获取。

```javascript
//路由配置
const router = new VueRouter({
    routes: [
        {path: '/user/:id'. component: User}
    ]
})
//组件中传值，生成动态路由
data(){
    return{
        user.id: "123"
    }
}
this.$router.push('/user/'+ this.user.id);
//在User组件中获取到当前路由路径参数的值
this.$route.params.id === "123"; //true
```

### 动态路由的问题

由于动态路由映射的组件实例会被复用，因此组件的生命周期钩子不会被调用。

可以使用`watch`监听`$route`对象的变化，也可以使用`beforeRouteUpdate`导航守卫。

```javascript
 watch: {
    $route(to, from) {
      // 对路由变化作出响应...
    }
  }

beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
  }
```

