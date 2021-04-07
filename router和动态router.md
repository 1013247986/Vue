##router

###配置步骤

一、安装router

```npm
npm install vue-router
```

二、import 导入vue-router，再通过 `Vue.use()` 明确地安装路由功能：

```js
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)
```

三、设置一个router.js文件然后配置router

```
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]
```

四、创建和挂载根实例。

```js
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app')
```

通过注入路由器，我们可以在任何组件内通过 `this.$router` 访问路由器，也可以通过 `this.$route` 访问当前路由：

**备注**：$router中，push方法常用，在 \$​route中，path（获取当前访问的路径）和fullpath（获取当前访问的全路径）方法常用

**配置好了路由，接下来就该在HTML中使用了**

###声明式导航

```html
  <!-- 使用 router-link 组件来导航. -->
  <!-- 通过传入 `to` 属性指定链接. -->
  <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
<router-link to="/bar">路由跳转</router-link>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
<router-view></router-view>
```

###编程式导航

####router.push()方法

```js

//使用路由的path值
this.$router.push("/router1");
 
//使用包含路由path键值的对象
this.$router.push({path:'/router1'});
 
//使用包含路由name键值的对象
this.$router.push({name:'routerNum1'});
 
//使用包含name键值和params参数对象的对象，
//注意：该方式params传参浏览器页面不在url中显示参数，但是会页面刷新的时候丢失参数
this.$router.push({name:'routerNum1',params:{userId:123}}); 
 
//path键值结合params的形式传参无效
this.$router.push({path:'/router1',params:{userId:123}});
 
//可以使用path键值结合query的形式传参
this.$router.push({path:'/router1',query:{userId:123}});
 
//使用path值拼接参数来传参，注意这里使用的不是单引号！！！
const userId=123;
this.$router.push({path:`/router1/${userId}`}); 


// 编程式跳转注意点
const userId = 123

1、router.push({ path: `/router1/${userId}` }) // 注意使用反引号

2、router.push({ path: '/router1', params: { userId }}) // 注意path键值结合params传参无效

3、router.push({ name: '/routerNum1', params: { userId }}) // 注意该方式在浏览器url不显示参数，但是页面刷新会导致参数丢失

<template>
  <div class="hello">
    <h3>{{ msg }}</h3>
    <button @click="goBack()">js编程式go方法返回history页面</button>
    <hr>
    <h3>js编程式导航中push方法的使用：</h3>
    <button @click="pathTo()">push方法的url参数使用path值导航</button>
    <button @click="pathObjectTo()">push方法的url参数使用path对象导航</button><br>
    <button @click="nameObjectTo()">push方法,包含路由命名name键值的对象导航</button><br>
    <button @click="nameParamTo()">push方法,包含路由命名name键值的对象结合params传参</button><br>
    <button @click="pathParamTo()">push方法,path键值对象导航结合params传参，参数传递失败</button>
    <button @click="pathQueryTo()">push方法,path键值对象导航结合query传参</button><br>
    <button @click="pathandTo()">push方法,path值拼接参数来传参</button>
  </div>
</template>
 
<script>
export default {
  name: 'Home',
  data () {
    return {
      msg: 'Welcome to Home 主页!!!!' 
    }
  },
  methods:{
     goBack:function(){
        this.$router.go(-1);   
     },
     pathTo:function(){     //使用路由的path值
        this.$router.push("/router1");
     },
     pathObjectTo:function(){   //使用包含路由path键值的对象
        this.$router.push({path:'/router1'});
     },
     nameObjectTo:function(){  //使用包含路由name键值的对象
        this.$router.push({name:'routerNum1'});
     },
     nameParamTo:function(){  //使用包含name键值和params参数对象的对象
        this.$router.push({name:'routerNum1',params:{userId:123}}); 
     },
     pathParamTo:function(){  //path键值结合params的形式传参无效
         this.$router.push({path:'/router1',params:{userId:123}}); 
     },
     pathQueryTo:function(){  //可以使用path键值结合query的形式传参
         this.$router.push({path:'/router1',query:{userId:123}}); 
     },
     pathandTo:function(){
        const userId=123;
        this.$router.push({path:`/router1/${userId}`});  
     }
  }
}

```

#### router.replace()方法

跟 `router.push` 很像，唯一的**不同就是，它不会向 history 添加新记录**，而是跟它的方法名一样 —— **替换掉当前的 history 记录；**

####**router.go(**n)方法

参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 `window.history.go(n)`。 [Browser History APIs](https://developer.mozilla.org/en-US/docs/Web/API/History_API)点击了解更多

##动态router

注：在使用动态路由或者编程式的路由时，会有这样一个问题，路由切换后，页面不切换，解决方法：
使用watch监听路由的变化，并做数据的覆盖处理

**动态路由使用方式**

```html
<template>
  <!--两种方式触发切换-->
  <!--方式一：通过点击事件的切换-->
    <div v-for="(item,index) in theshow.recommend_courses" @click="toaimcourse(item)"><a href="javascript:vild(0);">{{item.title}}</a></div>
  <!--方式二：通过动态路由的切换-->
    <router-link v-for="(item,index) in theshow.recommend_courses" :to="{ name: 'coursedetail', params: { id:item.id }}">{{item.title}}</router-link>

</template>
```

## 懒加载router

* 全加载

```js

import Vue from 'vue'
import Router from 'vue-router'
import Parent from '@/components/Parent'
 
Vue.use(Router)
 
export default new Router({
  routes: [
    {
      path: '/',
      name: 'Parent',
      component: Parent //可以定义路由路径、命名、组件、参数、别名、重定向
    }
  ]
})
```

* 懒加载

```js

import Vue from 'vue'
import Router from 'vue-router'
 
Vue.use(Router)
 
export default new Router({
  routes: [
    {
      path: '/',
      name: 'Parent',
      component: resolve => require(['@/components/Parent'], resolve)
    }
  ]
})
```





##导航守卫

####完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用离开守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。

####拦截过滤认证

单个组件：

　　可以在mounted方法中做拦截，通过判断全局变量中的token值，来判断有无登录。

多个组件：可以在全局做拦截

```js
# 在main.js中配置
router.beforeEach(function (to,from,next) {
    //  meta 用于设置一个标识   所以必须在需要验证是否登录的组件中加一个meta参数  meta:                     {requireAuth:true}
    if (to.meta.requireAuth){
        //  表示此时路径是要判断是否登录的
        if (store.state.token){
            next()
        }else{
            //  query  用于给这个重定向路由设置一个参数  用于在登录成功后重定向的路径
            //  设置此参数后需要在登录成功后做一个判断  this.$route.query.backUrl  获取设置的参数
            //  有值，表示是需要跳转回的，然后通过重定向将取的值，定位即可
            next({path:'/login',query:{backUrl:to.fullPath}})
        }
    }else{
        next()
    }
});
```



##router重定向

路由配置{ path: '/a', redirect: '/b' },路由重定向是指当用户访问 `/a`时，**路由跳转到b路由，URL 将会被替换成 `/b`**

####redirect 基本重定向

* 普通的重定向(不需要传递任何的参数):

```js

const router = new VueRouter({
  routes: [
    { path: '/router1', redirect: '/router2' },  //是从 /route1 重定向到 /router2
 
    { path: '/b', redirect: {name:'foo'} },  //重定向的目标也可以是一个命名的路由
 
    { path: '/c', redirect:(to)=>{
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
      //注意：导航守卫应用在导航跳转目标to上
    }},
  ]
```

在重定向时，不需要配置component组件，只需要配置redirect，设置重定向的路由

注意在给**路由设置有路由重定向时，再使用的路由导航会应用在跳转to指向的路由上**，如下路由router1上的导航守卫无效

```js

    {
      path: '/router1',
      redirect: '/router2',
      name: 'routerNum1',
      component: router1,
      beforeEnter:(from,to,next)=>{       //导航守卫无效
        console.log("在路由1上进行重定向");   
        next();
      }
    },
    {
      path: '/router2',
      name: 'routerNum2',
      component: router2,
      beforeEnter:(from,to,next)=>{
        console.log("进入到了路由2");
        next();
      }
    }
// 重定向指向自身会报错：[Vue warn]: Error in beforeCreate hook: "RangeError: Maximum call stack size exceeded，使用动态路径参数传参除外
```



####重定向时传递参数

```js
export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld,
      children: [
        {//在地址为空时，直接跳转cell路由
          path:'',
          redirect:'/cell'
        },
        {
        path: '/cell',
        component: Cell
      },{
        path:'/city/:cityid',
        component:City
      },{
        //本来配置的路由路径  /city/:cityid 是一个城市页面 传递的参数为 cityid 但是 我们可以设置重定向 /province/:cityid 同样到城市页面
        path:'/province/:cityid',
        redirect:'/city/:cityid'
      }]
    }
  ]
})
```

注意在给**路由设置有路由重定向时，再使用的路由导航会应用在跳转to指向的路由上**，如下路由router1上的导航守卫无效

####路由别名

路由别名**可以理解为路由的 '小名'**，可以使用路由别名进行路由导航

路由 /router2 的别名 alias 为 /secondRouter ，那么**使用别名访问 /secondRouter 时，url是/secondRouter，**导航跳转到路由 /router2 ，如下

```js
    {path: '/router2', name: 'routerNum2', alias:'/secondRouter',component: router2 },
```

