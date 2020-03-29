##router

####配置步骤

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

####声明式导航

```html
  <!-- 使用 router-link 组件来导航. -->
  <!-- 通过传入 `to` 属性指定链接. -->
  <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
<router-link to="/bar">路由跳转</router-link>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
<router-view></router-view>
```

####编程式导航

```js
router.push(...)
// 该方法的参数可以是一个字符串路径，或者一个描述地址的对象。例如：

// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: 123 }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

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
    //  meta 用于设置一个标识   所以必须在需要验证是否登录的组件中加一个meta参数  meta:{requireAuth:true}
    if (to.meta.requireAuth){
        //  表示此时路径是要判断是否登录的
        if (store.state.token){
            next()
        }else{
            // query  用于给这个重定向路由设置一个参数  用于在登录成功后重定向的路径
            //  设置此参数后需要在登录成功后做一个判断  this.$route.query.backUrl  获取设置的参数
            //  有值，表示是需要跳转回的，然后通过重定向将取的值，定位即可
            next({path:'/login',query:{backUrl:to.fullPath}})
        }
    }else{
        next()
    }
});
```

