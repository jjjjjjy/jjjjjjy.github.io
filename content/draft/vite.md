+++
date = '2026-01-07T11:33:39+08:00'
draft = false
title = 'Vite'
+++

# Vite 介绍
* Vite 发音 /viːt/
* vite官网: https://cn.vitejs.dev/
* vite源码: https://github.com/vitejs/vite

Vite是一个由原生ESM驱动的Web开发构建工具。

在开发环境下基于浏览器原生ESimports开发，在生产环境下基于Rollup打包。

vite是vue作者开发的一款意图取代webpack的工具，其实现原理是**利用ES6的import会发送请求去加载文件的特性，拦截这些请求，做一些预编译**，省去webpack冗长的打包时间。


# Vite 组成
vite主要由两部分组成：一个开发服务器 + 一套构建指令。
* 一个开发服务器，它基于 原生 ES 模块 提供了 丰富的内建功能，如速度快到惊人的 模块热更新（HMR）。
* 一套构建指令，它使用 Rollup 打包代码，并且是预配置的，可输出用于生产环境的高度优化过的静态资源。

**理解：Vite 的开发服务器 只在开发环境使用。Vite 的构建（build / Rollup）只在生产环境构建时使用。两者职责清晰、阶段完全不同。**

Vite 的精髓就是：
**开发阶段基于ESM，不打包，速度更快，生产阶段基于Rollup打包，打包到极致**

## 开发服务器
在开发期间，vite就是一个服务器。开发服务器只在开发阶段使用，本质上是“让浏览器直接跑你的源码”。


✔ 提供本地服务
✔ 利用 浏览器原生 ES Modules
✔ 实时转换源码（TS / JSX / Vue）
✔ HMR（模块热替换）
✔ 不做整体打包

你写的源码
   ↓
Vite Dev Server（即时转换）
   ↓
浏览器按需请求模块（ESM）
💡 特点：

没有 bundle

改一个文件，只重新加载这一个模块

启动速度 ≈ 0（项目再大也快）


## 构建指令
本质：把源码变成适合线上部署的静态文件

✔ 使用 Rollup
✔ 真正“打包”
✔ 代码分割、Tree-shaking
✔ 文件 hash
✔ 输出静态资源（dist）


# 🚧  Diff: Vite vs Webpack

## 开发环境
* Webpack 是在“浏览器不会模块化”的时代诞生的正确方案，【浏览器不支持ESModule和import和export】因此他的世界观是先打包再运行，因此开发服务器启动速度慢。
* Vite 是在“浏览器已经会模块化”之后，对构建工具的重构，因此他的世界观是先运行再打包，因此开发服务器启动速度极快。
|开发环境对比点|	Webpack|	Vite|
|----|----|----|
|是否打包|	✅ 是（内存 bundle）|	❌ 否|
|模块加载|	bundle|浏览器原生 ESM|
|启动时|全量构建依赖图|不扫描全量|
|改一个文件|	重新构建 chunk|	精确替换模块|
|HMR|	基于 bundle|	基于模块|
|项目越大|	越慢	|基本不变|




｜ 在开发环境，Vite 和 Webpack 的主要区别在于是否对代码进行打包。
1. webpack在启动开发服务器时，会将整个项目模块构建一个依赖图，打包为一个bundle.js文件，然后再运行这个文件。

   只是这个打包是 内存中的打包（in-memory bundle），不是写到磁盘而已。

2. vite则是利用浏览器的ES Module Imports特性，不进行预打包，只有在真正需要时才编译文件，按需加载模块。




* 在启动开发服务器时，Webpack 将整个项目所有模块构建一个依赖图，打包为一个bundle.js文件，然后再运行这个文件。对于大型项目，这个过程可能非常耗时，导致开发者需要等待较长时间才能开始调试代码。
* Vite：Vite 利用浏览器的ES Module Imports特性，不进行预打包，只有在真正需要时才编译文件，按需加载模块。
    * Vite直接启动了一个开发服务器devServer，然后劫持浏览器的HTTP请求，在后端进行相应的处理将项目中使用的文件通过简单的分解与整合，然后再返回给浏览器(整个过程没有对文件进行打包编译)。所以Vite 的开发服务器启动速度非常快，通常可以在几百毫秒内启动。
    * （在生产模式下，vite使用Rollup进行打包，提供更好的tree-shaking，代码压缩和性能优化。）



【对比: Webpack 在开发阶段「本质上也要打包」, 只是这个打包是 内存中的打包（in-memory bundle），不是写到磁盘而已。】



## 为什么 Vite 启动这么快？
因为vite是浏览器直接加载 ESM，不做 bundle，而是按需加载模块，只在请求到某个模块时才编译（on-demand）

## vite 的工作原理

1. 启动 dev server

2. 浏览器请求 index.html

3. 浏览器通过 <script type="module"> 加载模块

4. 遇到裸模块（vue / react）→ Vite 转成浏览器可识别路径

5. 按需编译、缓存结果




## Vite 中如何区分环境？
import.meta.env.MODE
import.meta.env.DEV
import.meta.env.PROD

只有 VITE_ 开头的环境变量才会注入到客户端


## Vite 如何处理 CSS？
原生支持：
CSS / Less / Sass

CSS Module：*.module.css

自动拆分 CSS

开发阶段 style 注入

生产阶段提取成文件