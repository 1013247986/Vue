## 脚手架创建

* 1、vue create <project_name> 创建项目
* 2、如果没有设置淘宝镜像，提示是否使用淘宝镜像：

```
?  Your connection to the default yarn registry seems to be slow.
   Use https://registry.npm.taobao.org for faster installation? (Y/n) 
```

* 3、选择默认配置：

```
? Please pick a preset: (Use arrow keys)
❯ default (babel, eslint)    # 默认配置
  Manually select features   # 自定义配置
```

* 4、选择包管理工具：

```
? Pick the package manager to use when installing dependencies: (Use arrow keys)
❯ Use Yarn 
  Use NPM 
```

* 5、**选择自定义配置流程如下：**

  通过按“空格”选择要安装的项：

```
Vue CLI v4.0.3
? Please pick a preset: Manually select features
? Check the features needed for your project: 
 ◉ Babel
 ◯ TypeScript
 ◯ Progressive Web App (PWA) Support # 支持渐进式网页应用程序
 ◉ Router # 路由管理器
 ◉ Vuex # 状态管理模式（构建一个中大型单页应用时）
 ◉ CSS Pre-processors # css预处理
❯◉ Linter / Formatter # 代码风格、格式校验
 ◯ Unit Testing # 单元测试
 ◯ E2E Testing # （End To End）即端对端测试
```

* 6、选择CSS预编译，这里我选择使用Less：

```
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules 
are supported by default): 
  Sass/SCSS (with dart-sass) 
  Sass/SCSS (with node-sass) 
❯ Less 
  Stylus 
```

* 7、语法检测工具，这里我选择ESLint + Prettier

```
? Pick a linter / formatter config: 
  ESLint with error prevention only  # 仅错误预防
  ESLint + Airbnb config  # Airbnb配置
  ESLint + Standard config  # 标准配置
❯ ESLint + Prettier 

? Pick additional lint features: (Press <space> to select, <a> to 
toggle all, <i> to invert selection)
❯◉ Lint on save # 保存时检查
 ◯ Lint and fix on commit # 提交时检查
```

* 8、选择Babel、PostCSS、ESLint等配置文件的放置位置：

  ```
  ? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? (Use arrow keys)
  ❯ In dedicated config files  # 在专用的配置文件中
    In package.json  # package.json
  ```

* 9、是否保存预设：

```
? Save this as a preset for future projects? (y/N) 
```

* 10、启动项目：

```
$ cd project_name
$ npm run serve
 App running at:
  - Local:   http://localhost:8080/ 
  - Network: http://10.2.1.2:8080/
```

**重点提示**

vue-cli 从3.x起 *webpack* 的配置已经被脚手架默认了，并不会显示。如果我们需要手动配置webpack的一些配置，可以手动创建配置文件。文件名为vue.config.js，此文件应该和package.json同级（创建之后会自动加载）,此文件需要按照JSON格式来撰写。

```
// vue.config.js
module.exports = {
  // 选项...
}
```

比如：有时候我们前后端是分离情况下，在开发模式下，我们需要在vue.config.js中配置了。

```
// vue config.js
module.exports = {
    devServer: {
        port: 8080,
        host: 'localhost',
        open: true,
        proxy: {
            '/api': {
                target: 'http://127.0.0.1:2019',
                secure: false,
                changeOrigin: true,
                pathRewrite: { '^/api': '' }
            }
        }
    }
}
```

