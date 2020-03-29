## vue生命周期

#### beforeCreate

*  vue实例的挂载元素$el和数据对象data都为undefined，还未初始化。

#### created

* vue实例的数据对象data有了，$el还没有，可以写http请求，但这时DOM还没有生成

#### beforeMount

* vue实例的$el和data都初始化了，但还是虚拟的dom节点，具体的data.filter还未替换。

#### mounted

* vue实例挂载完成，data.filter成功渲染，可以写http请求，和对dom的操作，这时DOM已经生成

#### beforeUpdate

* data更新时触发

####Update

* data更新后触发

####beforeDestroy（销毁前）

####destroyed（销毁完毕）



![](E:\总结知识\Imgs\vue生命周期.png)

##钩子函数

#### watch

监听函数，监听数据 路由的变化

####methods

方法：每当触发重新渲染时，调用方法将总会再次执行函数

####computed

计算属性：计算属性是基于它们的响应式依赖进行缓存的。只在相关响应式依赖发生改变时它们才会重新求值，多次访问 `getAge` 计算属性会立即返回之前的计算结果，而不必再次执行函数

**所以，对于任何复杂逻辑，你都应当使用计算属性**