# vue-router 路由组件的使用

> this.$router 和 this.route 都会注入到子组件当中

## this.$router vue-router 实例

> 路由实例对象，用于页面路由的

## this.$route 当前的路由对象

> 包含了当前 URL 解析得到的信息，还有 URL 匹配到的路由记录 (route records)。
> 路由对象是不可变 (immutable) 的，每次成功的导航后都会产生一个新的对象。
> 路由对象出现在多个地方:

1. 在组件内，即 this.$route
2. 在 $route 观察者回调内
3. router.match(location) 的返回值
4. 导航守卫的参数：

```javascript
router.beforeEach((to, from, next) => {});

//scrollBehavior 方法的参数
const router = new VueRouter({
  scrollBehavior(to, from, savedPosition) {
    // `to` 和 `from`都是路由对象
  }
});
```

## 权限认证跳转死循环

```javascript
router.beforeEach((to, from, next) => {
  if (登录) {
    //执行队列中的下一个钩子函数，不会触发beforeEach
    next();
  } else {
    //这里发生路由的跳转，又会触发 beforeEach的钩子函数
    next({ name: "login" });
  }
});

//改进办法，修改if的判断条件
if (登录 || to.name === "login") {
  next();
}

//也可以使用后置钩子来实现
import router from "./router";
router.afterEach((to, from) => {
  if (未登录 && to.name !== "login") {
    router.push({ name: "login" });
  }
});
```

## 权限认证的动态路由 addRouter

## 缓存路由组件

```javascript
<keep-alive>
   <router-view v-if="$route.meta.keepAlive">
       <!--这里是会被缓存的路由-->
   </router-view>
</keep-alive>

<router-view v-if="!$route.meta.keepAlive">
   <!--因为用的是v-if 所以下面还要创建一个未缓存的路由视图出口-->
</router-view>

//router配置

new Router({
 routes: [
   {
     path: '/',
     name: 'home',
     component: Home,
     meta: {
       keepAlive: true // 需要被缓存
     }
   },

   {
     path: '/:id',
     name: 'edit',
     component: Edit,
     meta: {
       keepAlive: false // 不需要被缓存
     }
   }
 ]

});

<keep-alive include='a'>
  <router-view></router-view>
</keep-alive>
```

##结论
1. ajax请求最好放在created里面，因为此时已经可以访问this了，请求到数据就可以直接放在data里面。
这里也碰到过几次，面试官问：ajax请求应该放在哪个生命周期。

2. 关于dom的操作要放在mounted里面，在mounted前面访问dom会是undefined。

3. 每次进入/离开组件都要做一些事情，用什么钩子：
 
不缓存：
进入的时候可以用created和mounted钩子，离开的时候用beforeDestory和destroyed钩子,beforeDestory可以访问this，destroyed不可以访问this。

缓存了组件：
缓存了组件之后，再次进入组件不会触发beforeCreate、created 、beforeMount、 mounted，如果你想每次进入组件都做一些事情的话，你可以放在activated进入缓存组件的钩子中。

同理：离开缓存组件的时候，beforeDestroy和destroyed并不会触发，可以使用deactivated离开缓存组件的钩子来代替。

vue渲染template的时间在beforeMount之后，mounted之前，所以


beforeRouteLeave：组件独享
beforeEach:全局
beforeEnter:路由独享守卫
beforeRouterEnter:路由组件的组件进入路由前钩子
beforeResolve:全局
afterEach：全局
beforeCreate:
created:
beforeMount:
deactivated:
mounted:
activated:
执行beforeRouteEnter回调函数next，cb(vm)以参数的形式拿到vm对象