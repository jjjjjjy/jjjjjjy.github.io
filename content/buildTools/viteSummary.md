+++
date = '2026-01-20T17:46:16+08:00'
draft = false
title = 'Vite总结'
+++

###### vite 本质
开发阶段利用浏览器原生 ESM 做按需编译，生产阶段基于Rollup做高质量打包。


###### vite配置文件为什么是vite.config.ts
1. 配置本身就是 ESM
2. 可以使用 TS
3. 区分生产环境和开发环境，可以写逻辑（if / env）

###### vite请求快的原因

