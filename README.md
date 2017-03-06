# 一个简单的vue2+webpack2+vue-router2的自环境搭建(新手向)
## 目录
- [前言](#前言)
- 准备工作
  - [安装node](#安装node)
- 正式开始
  - [初始化](#初始化)

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

[top](#目录)

## 准备工作
### 安装node
>为还没有安装node的小伙伴准备,如果已经安装那么请忽略\_(:3 \_| &nbsp;/\_ ) _

> 1.首先安装nodejs以获取npm包管理器（安装过程不介绍了,简单的下一步安装法即可）
>如果觉得npm安装速度慢可以尝试cnpm(淘宝镜像)或者yarn进行安装
>
>__淘宝镜像__ 安装方法传送门（http://npm.taobao.org/）
>
>__yarn__ 安装方法传送门（https://yarnpkg.com/zh-Hans/docs/install#windows-tab）
>
>npm安装包的使用方法
`npm i/install 包名@版本号 --save-dev`

[top](#目录)

##正式开始
### 初始化
