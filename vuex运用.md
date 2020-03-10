## 全局状态管理vuex         为前端创造的数据库！

#### 一、引用vuex

```js
npm  install vuex --save
```

```js
import Vue from 'vue';//引用vue
import Vuex from 'vuex';//引用vuex
Vue.use(Vuex);//使用vuex

export default new Vuex.Store({////暴露Store对象
    state
})
```

* 在main.js当中引入在store.js文件当中创建的store对象，并在Vue实例中添加

  ```js
  import Vue from 'vue'
  import App from './App'
  import router from './router'
  import store from './vuex/store'//导入store.js
  Vue.config.productionTip = false
  new Vue({
      el: '#app',
      router,
      store,//添加store
      components: { App },
      template: '<App/>'
  })
  ```

* 首先在App.vue当中的script内引入vuex

  ```
  import {mapState,} from "vuex";
  ```

  

#### 二、属性介绍

[参考网址](https://www.jianshu.com/p/ab539e9a7955)

**State 属性   数据库------------------------------------------------------------------------------------------------------------**  

* vuex中的数据源，我们需要保存的数据就保存在这里，可以在页面通过 this.$store.state来获取我们定义的数据

* state就是根据你项目的需求，自己定义一个数据结构。Store中至少要注入两项，state 和 mutation

  ```js
  const state = {
    currentPage: 1,
    user: '0121213',
    change: 0,
    page,
    ctrl,
    meta,
    configs,
    datas
  }
  
  export default new Vuex.Store({
    state,
    mutations,
  })
  ```

* 直接使用数据方式

  （1）{{$store.state.nodeVoteCount}} 直接在组件中使用

* mapState 使用数据方式

  （1）**在computed：里面写**       ...mapState({cname:'name'})

**Getters 属性   获取和加工--------------------------------------------------------------------------------------------------** 

* getters 是一个纯函数，接收参数 state，返回你想取的值，都不需要贴数据结构，很清晰吧。

```js
// 获取store各项信息
export function getMeta (state) {
  return state.meta
}
// 第二种
export default {
    list: state => state.list,
    routerChange: state => state.routerChange,
    downLoadMore: state => state.downLoadMore,
}
```

* 直接使用方法

  （1）this.$store.getters.方法名字（''参数"）可带参数，可以不带,不带参数不用写（）

* ...mapGetters使用方法 写在**computed计算钩子**里

  （1）...mapGetters([''方法名字''，''方法名字''])
  
  ```js
  csname:(state)=>(val)=>{
  postfix = "";
  if(state.name === "张三"){
  postfix = val
  }
  return state.name + postfix;
}
  ```
  
  

**Mutations  属性   修改数据方法 --------------------------------------------------------------------------------------------**

```js
// 公共控制变量 ctrl
  [SHOW_PAGE] (state) {
    state.ctrl.showPage = true
  },
```

​    一个最简单的
​    mutation，就是把
​    state.ctrl.showData 的值改成 true，好吧，我觉得我是在说废话。Mutation除了接收
​    state 作为第一个参数外，还可以接收其他的参数，比如：

```js
 [NEW_DATA] (state, payload, id){
    state.num = Object.assign({}, {payload,id})
  },
```

* 直接使用数据方式 写在：**methods方法函数里面**

  （1）{{this.$store.commit("方法名字"，参数)}} 直接在组件中使用，参数可以传值，可以不传，负载传输

  （2）{{this.$store.commit({type:"方法名字"，值})}} 直接在组件中使用，对象传输
  
* mapMutations 使用数据方式   **在methods里使用** 

  （1）  ...mapMutations([方法名字，方法名字]) 直接使用，传值在方法（）里加值，对象多个值，负载值加一个值

**action 属性 数据中间处理--------------------------------------------------------------------------------------------------**

* dispatch使用mutation，第一个参数是方法名字，第二个是方法需要的参数，可以用来对一些数据的处理

```js
export const updateName = function({ dispatch, state }, name) {
  const payload = {name}
  dispatch('UPDATE_PAGE', payload)
}
```

* 下面的 action 叫 updateData,里面先把 data 改了个名字，然后加了判断，如果 id 是‘initial’
  就怎么存。。。否则怎么存。。。

  ```js
  export const updateData = function({ dispatch, state }, data) {
    const payload = data
    const id = state.meta.currentData
    if (id === 'initial') {
      const id = createDataId()
      dispatch('NEW_DATA', payload, id)
    } else {
      dispatch('UPDATE_DATA', payload)
    }
  }
  ```
  **希望大家可以看到的是，各个类型的 API各司其职**
  
  **mutation 只管存，你给我（dispatch）我就存；**
  
  **action只管中间处理，处理完我就给你，你怎么存我不管；**
  
  **Getter 我只管取，我不改的**
  
  * 一个叫 save 的 action。首先在里面发起了一个请求，这个请求是经过包装的（改了名字叫
    api.save），咱不管它改名字这茬儿。第一个
  
    then 就是成功的回调，通过
  
    res 拿到数据，拿到数据了就怎么存。。。第二个
  
    catch 就是失败的回调，通过
  
    err 拿到错误信息，拿到错误信息了怎么存。。。呵呵，跟if判断差不多嘛。。。其实异步的
  
    action 还可以发送第三个
  
    dispatch 的，在发起请求前先保存下原始数据，有时候有需求会用到的，比如官方 DEMO 的购物车。
  
    ```js
    export const save = function({ dispatch, state }) {
      const params = state
      api.save(params).then(function(res) {
        console.log(res)
        dispatch('SAVE_SUCCESS', res)
      }).catch((err) => {
        console.log(err)
        dispatch('SAVE_FAIL', err)
      })
    }
    ```
  
      * 直接使用方式 写在：**methods方法函数里面**
    
        （1）this.$store.dispatch（"action方法名字"，{传入数据}）负载模式
    
        （2）this.$store.dispatch({"action方法名字"，传入数据}) 对象方式
        
    * mapActions使用 写在：**methods方法里使用**
    
      （1）...mapActions（{定义名字："方法名"，定义名字："方法名"}）

#### 三、vuex模块化

```js
export default new Vue.store({
    state,
    getters,
    actions,
    mutations
})
```

#### 四、页面刷新vuex数据丢失初始化

```js
 1 created () {
 2     var store = require('store');
 3 
 4     //在页面加载时读取sessionStorage里的状态信息
 5     if (sessionStorage.getItem("storedata") ) {
 6         this.$store.replaceState(Object.assign({}, this.$store.state,JSON.parse(sessionStorage.getItem("storedata"))))
 7     }
 8     //在页面刷新时将vuex里的信息保存到sessionStorage里
 9     window.addEventListener("beforeunload",()=>{
10         sessionStorage.setItem("storedata",JSON.stringify(this.$store.state))
11     });
12     // 兼容iphone手机
13     window.addEventListener("pagehide",()=>{
14         sessionStorage.setItem("storedata",JSON.stringify(this.$store.state))
15     });
16   },
```

