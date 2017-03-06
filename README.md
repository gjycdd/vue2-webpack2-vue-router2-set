# 一个简单的vue2+webpack2+vue-router2的自环境搭建(新手向)
## 目录
- [前言](#前言)
- 准备工作
  - [安装node](#安装node)
- 正式开始
  - [初始化](#初始化)
  - [安装基础包](#安装基础包)
    - [安装vue2](#安装vue2)
    - [安装vue-loader](#安装vue-loader)
    - [安装vue-router](#安装vue-router)
    - [安装webpack2](#安装webpack2)
    - [安装热加载模块](#安装热加载模块)
  - [构建目录结构](#构建目录结构)
  - [初步配置webpack](#初步配置webpack)
    - [配置出入口](#配置出入口)
    - [配置热加载](#配置热加载)
  - [搭建vue页面](#搭建vue页面)
  - [配置路由](#配置路由)
  - [配置入口文件](#配置入口文件)
  - [配置App.vue和index.html](#配置appvue和indexhtml)
    - [App.vue](#appvue)
    - [index.html](#indexhtml)
  - [配置loader](#配置loader)
    - [补全依赖](#补全依赖)
    - [.babelrc](#babelrc)
    - [webpack.config.js](#webpackconfigjs)
- [拓展](#拓展)
  - [加载静态资源](#加载静态资源)
  - [预编译css](#预编译css)
  - [提取图片](#提取图片)
- [后记](#后记)
- [特别鸣谢](#特别鸣谢)

## 前言
>上一部分简单介绍了下基于vue1+webpack1+vue-router1的环境搭建，但是还有很多不足，但是New is always the best ，所以我决定
在这篇新技术教程中顺便补齐所需。

>github传送门 (https://github.com/gjycdd/vue1-webpack1-set)

>码云    传送门 (https://git.oschina.net/gjycdd/vue1-webpack1-set)

>在之前的教程中我们可以看到要搭建一个环境真的很不容易，对熟练工来说可能事几分钟的事情，大部分时间还是花在安装依赖的过程中，
但是，对我们新手来说，这个时间就成倍增长，也许短的几个小时也就完成了，长的可能踩坑要踩好几天都不出不来，尤其是面对版本更新时
的各种迁移。对新手来说，老版本文档都没看全又何谈通过新版本文档比较差异，重新配置一个符合新版本要求的环境呢？

>所以这又是一篇新手向的环境搭建教程，希望能给同样在挣扎的新手同胞们一些小小的帮助。

[top](#)

## 准备工作
### 安装node
>为还没有安装node的小伙伴准备,如果已经安装那么请忽略\_(:3 \_| &nbsp;/\_ ) _

> 首先安装nodejs以获取npm包管理器（安装过程不介绍了,简单的下一步安装法即可）
>如果觉得npm安装速度慢可以尝试cnpm(淘宝镜像)或者yarn进行安装
>
>__淘宝镜像__ 安装方法传送门（http://npm.taobao.org/）
>
>__yarn__ 安装方法传送门（https://yarnpkg.com/zh-Hans/docs/install#windows-tab）
>
>npm安装包的使用方法
`npm i/install 包名@版本号 --save-dev`

[top](#)

## 正式开始
### 初始化
> 利用npm自动生成package.json，来管理安装的包

`npm init`

[top](#)

### 安装基础包

#### 安装vue2
`npm install vue --save-dev`
>直接 __install vue__ 会自动安装 __最新版本__ 的 __vue2__ ，但是由于还是测试版所以存在各种未知bug，希望运用在 __生产环境__ 的的小伙伴自行安装vue2的 __稳定版本__ ，__这里__ 我就偷懒使用vue2的 __测试版本做简介了__ 。

[top](#)

#### 安装vue-loader
`npm install vue-loader --save-dev`
>安装最新的vue-loader,这篇教程我想说的是——All is new!!

>__安装结束后可能会提示缺少某些包，没关系，按照提示全部安装一遍或者暂时忽略稍后安装也是可行的__

[top](#)

#### 安装vue-router
`npm install vue-router --save-dev `
>安装vue-router用以配置路由，vue-router2与1有着极大的差别，请各位注意

[top](#)

#### 安装webpack2
`npm install webpack --save-dev`
> 默认安装最新版本

[top](#)

#### 安装热加载模块
`npm install webpack-dev-server --save-dev`
>安装webpack热加载模块，修改文件实时刷新网页，免除修改一次代码就要重新打包刷新网页的麻烦。

[top](#)

### 构建目录结构
>| -- package.json&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//包管理文件 <br />
| -- index.html          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 启动页<br />
| -- .babelrc&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 配置es6 - > es5<br />
| -- webpack.config.js&nbsp;&nbsp;&nbsp;&nbsp;// webpack配置文件<br />
| -- src<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | -- App.vue&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//项目的入口页面<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | -- main.js&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//项目的入口js<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | -- router.js&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//项目的router配置文件<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | -- components&nbsp;&nbsp;&nbsp;&nbsp;//各个组件文件夹<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | -- views&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//各个vue存放的文件夹<br />

[top](#)

### 初步配置webpack
#### 配置出入口
    var path = require('path')
    module.exports = {
      entry: './src/main.js',
      output: {
          path: path.resolve(__dirname, './dist'),
          publicPath: '/dist/',
          filename: 'build.js'
      }
    }
>其中output的各配置项作用如下：

>- __path: './dist'__ 打包后js、css、image等存放的目录；

>- __publicPath: '/dist/'__ 可以不配置，如果不配置则取默认 __publicPath: '/'__ ，在实际项目中，静态资源一般集中放在一个文件夹下，比如static目录，那么这里就应该改成 __publicPath: '/static/'__ ，相应的index.html中引用的 JS 也要改成 __src="/static/build.js"__ ，publicPath 可以解释为最终发布的服务器上 build.js 所在的目录，其他静态资源也应当在这个目录下。

>- __filename: 'build.js'__ 打包的js文件名，index.html 引用的 JS 要和这里保持一致。

[top](#)

#### 配置热加载
>在package.json中进行配置

>新版本可能会提示你缺少 __vue-template-compiler__ 包依赖，那么只要安装就好了。

    "scripts": {
        "dev": "webpack-dev-server --inline --hot --open"
    }
>在以上配置保存后可以打开命令行（win+r）或者你的git工具到项目跟目录下运行`npm run dev`

>访问`localhost:8080`（提前在index.html中写入内容确认是否成功）

>各命令参数作用如下：
- `--inline` 一共两种模式，默认为iframe模式，inline和iframe模式最明显的区别就是访问路径的不同，iframe模式的访问路径是 `http://localhost:8080/webpack-dev-server/` ，实际上iframe模式页面嵌入的iframe标签的地址还是 `http://localhost:8080/` ，那岂不是可以直接访问，不禁想问为啥不直接访问呢？因为无论是哪种模式，都是为了做到修改代码后能自动刷新，其中iframe模式是在修改代码后，重新加载iframe，而--inline是刷新浏览器，本质上都是重新全部加载一遍
- `--hot` iframe不需要配置，配置了反而不能正常刷新了，所以只能配合--inline使用，作用是开启热替换，修改代码后，浏览器只会重新加载修改的组件代码，不会全部重新加载
- `--open` 自动打开浏览器

[top](#)

### 搭建vue页面
#### 在搭建vue页面之前我们来学习一下vue文件的结构(老司机请自动忽略)
    <template></template> //请务必记住template内一般只允许存在一个节点
    <script>
     export default {
          name: '',
          data () {  //当存在data时，必须有return
               return {}
          }
     }
    </script>
    <style>可以在这里写css,前提是已经安装css-loader</style>
#### 我们需要在views下搭建两个vue页面，为接下来router的配置做准备
>创建一个home.vue页面

    <template>
         <div>
              <h1>This is {{pageName}} page</h1>
        </div>
    </template>
    <script>
         export default {
              name: 'home',
              data () {
                   return {
                        pageName: 'home'
                   }
              }
         }
    </script>
    <style></style>
>相似的我们再创建一个test.vue页面

[top](#)

### 配置路由
>就如同vue1要使用vue-router1一般，vue2同样需要和vue-router2配套使用，值得一提的是在2.0版本中router的配置变得更加简洁可定制，舍弃了原本router.map的方法，转而使用更简单的方式。

>在router.js中我们通过import(ES6语法，使用require也可)引入我们需要的模块，并且配置router。

    import Vue from 'vue';
    import VueRouter from 'vue-router';
    import Index from './views/index.vue'; //将页面引入
    import Test from './views/test.vue'; //将页面引入
    Vue.use(VueRouter);

    let router = new VueRouter({
        routes: [
            {path:'/home', component: Index},
            {path:'/test', component: Test}
        ]
    })

    export default router
>上面的代码有几点需要注意：
> - `new VueRouter()`中必须传递一个`对象`
> - 请不要想当当然的写成routers: []，一定一定是`routes: []`
> - 一个页面中可以有许多个export将需要的内容暴露给全局，但是`一定只有一个`export default

[top](#)

### 配置入口文件
>由于router的版本和vue的版本影响，为main.js的配置也带来了很大的变化

    import Vue from 'vue';
    import router from './router';
    import App from './App.vue';
    new Vue({
         router,
         render: h => h(App)
    }).$mount('#app')
>上面对vue的初始化采用了手动挂载节点的方法，当然还有一种办法同样可以挂载节点。

    import Vue from 'vue';
    import router from './router';
    import App from './App.vue';
    new Vue({
         el: '#app',
         router,
         render: h => h(App)
    })

[top](#)

### 配置App.vue和index.html
>我们写了页面，配了路由那我们当然需要去展示我们的页面，那么该怎么展示呢

#### App.vue
    <template>
         <div>
    <!-- 请注意，vue2中使用router-link的标签来代替 v-link属性 -->
              <router-link to='/home'>Home</router-link>
              <router-link to='/test'>Home</router-link>
    <!-- 视图加载的挂载点 -->
              <router-view></router-view>
         </div>
    </template>
    <script></script>
    <style></style>
>对上面的代码来说<router-link></router-link>的路径是可定制的可以传递你想要的参数

    <router-link :to="{ path: '/home/'+pageId }"></router-link>
>那么只要在router.js中对路由进行如下配置

    { path: '/home/:id', components: Index }
>就能够使得 ‘/home’后任意的路径都能访问到 home页面

>具体的可以参考(http://router.vuejs.org/zh-cn/)

[top](#)

#### index.html
    <body>
        <div id='app'></div>
        <script src='dist/build.js'></script>
    </body >

[top](#)

### 配置loader

#### 补全依赖
>和vue1的环境搭建类似，在配置loader之前我们需要安装各种各样的包依赖。确保以下的包已经安装完成，以实现基础功能。

    npm install babel-core --save-dev
    npm install babel-loader --save-dev
    npm install babel-preset-es2015 --save-dev
    npm install vue-hot-reload-api --save-dev
    npm install vue-style-loader --save-dev
    npm install vue-template-compiler --save-dev
    npm install css-loader --save-dev

[top](#)

#### .babelrc
>由于我们需要识别es6，在vue2中不能像vue1那么配置config。因此，我们需要一个.babelrc文件进行配置。

    {
        "presets": ["es2015"],
        "comments": false
    }

[top](#)

#### webpack.config.js
    var path = require('path')
    module.exports = {
        entry: './src/main.js',
        output: {
            path: path.resolve(__dirname, './dist'),//js，css，以及页面文件存放位置
            publicPath: '/dist/',//静态文件所处文件夹
            filename: 'build.js'//通过build来统筹
        },
        module: {
            loaders: [
                {
                    test: /\.vue$/,
                    loader: 'vue-loader'
                },
                {
                    test: /\.css$/,   //加载外部css
                    loader: 'vue-style!css'
                },
                {
                    test: /\.js$/,
                    exclude: /node_modules/,
                    loader: 'babel-loader'
                }
            ]
        }
    }
> __注意__ :在webpack2中规定了，loader的配置必须将loader的名字补全

>经过以上步骤我们就可以顺利的运行`npm run dev`来预览基本的效果了

[top](#)

## 拓展

### 加载静态资源
>vue模板以及CSS中免不了使用图片，现在如果直接加，又会报找不到模块的错误，静态资源（图片、图标字体、音频、视频、svg文件等）对应的loader为url-loader

>因此首先我们需要安装包依赖:

    npm install url-loader --save-dev
    npm install file-loader --save-dev //url依赖file
>安装好依赖，那么就需要在webpack.config.js中配置:

    {
        test: /\.(jpe?g|png|gif|svg|mp3)$/,
        loader: "url-loader"
    }
> 配置好以上我们就可以在项目中加载我们需要的静态资源了。

[top](#)

### 预编译css
>同样的我们需要先安装一些包依赖

    {
      "less": "^2.3.1", // less-loader依赖less
      "less-loader": "^2.2.3",
      "node-sass": "^3.4.2", // sass-loader依赖node-sass
      "sass-loader": "^4.0.2",
      "stylus": "^0.54.5", // stylus-loader依赖stylus
      "stylus-loader": "^2.4.0"
    }
>在这里就和css一样了，如果我们不需要引入外部文件，只要在vue文件内部使用，那么不需要配置loader，但是考虑到项目的可拓展性，我们最好还是配置一下loader，引入的方式同css，只需要`require(指定目录/xxx.scss)`即可。

[top](#)

### 提取图片
>对于图片来说我们有时候也许会有许多的小图片，但是我们又不想反复的去加载这些图片该怎么办？

>提取图片就在在url-loader的配置中添加

    query:{
      name:'images/[name].[ext]',
      limit:10000// 单位：字节 <10kb的图片会被转换为DataUrl存储起来
    }

[top](#)

## 后记
>由于本人也是新手，所以配置的时候也多有缺漏还需要继续学习，希望新手们同胞们能通过我这些浅薄的方法有所收获。同时也希望能指出我错误或者不足的地方，让我能够及时修改和学习，谢谢~~~

[top](#)

## 特别鸣谢
>特别鸣谢 (http://www.qinshenxue.com/article/20161106163608.html)

[top](#)
