## 配置











## 常见问题

#### 本地刷新出现404

**有时候配置了路由的`history`模式后，在本地进行调试，手动刷新页面时也会出现404的情况，这个时候可以去`Vue`的配置文件中配置一下`devServer`**

```js
devServer: {
    historyApiFallback: {
      verbose: true,
      rewrites: [
        { from: /^\S*$/, to: '/index.html'}
      ]
    }
  }
```

