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

## 前言
>上一部分简单介绍了下基于vue1+webpack1+vue-router1的环境搭建，但是还有很多不足，但是New is always the best ，所以我决定
在这篇新技术教程中顺便补齐所需。

>github传送门 (https://github.com/gjycdd/vue1-webpack1-set)

>码云    传送门 (https://git.oschina.net/gjycdd/vue1-webpack1-set)

>特别鸣谢 (http://www.qinshenxue.com/article/20161106163608.html)

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
