---
title: "JS模块化"
date: 2026-02-28T14:26:13+08:00
draft: false

# 访问路径（可选，不写则自动生成）
slug: "jsModule"

# 分类（建议 1～2 个）
categories:
  - 技术笔记

# 标签（可多个）
# tags:
#   - JavaScript
---

# 模块化
模块化（Module）指把代码拆分成独立、可复用、可维护的单元。每个模块能够：1️⃣ 独立作用域 2️⃣ 明确依赖 3️⃣ 对外暴露接口；**目的**：防止变量污染、提高代码复用、提升可维护性、支持大型工程

# JavaScript 模块化发展历史

## 阶段1：IIFE
IIFE = 立即执行函数

## 阶段2：CommonJS
CommonJS 是 Node.js 的模块规范。

特点：
1. 同步加载（Node 可以同步读文件。浏览器不行。）
2. 运行时加载
3. 适合服务器环境
4. 值拷贝（CommonJS 导出是 值拷贝）

**CommonJS 加载过程**： const module = require('./math');

## 阶段3：AMD / CMD

### AMD`异步模块定义`(Asynchronous Module Definition)

**使用 define 定义模块**

**使用 require 加载模块**


### CMD `公共模块定义`(Common Module Definition)
在 CMD 规范中，一个模块就是一个文件。
**使用 define 定义模块**
**使用 SeaJs 的 use 加载模块**


### UMD`通用模块定义`(Universal Module Definition)

UMD 是 AMD 和 CommonJS 的一个糅合。
AMD 是浏览器优先，异步加载；
CommonJS 是服务器优先，同步加载。
既然要通用，怎么办呢？
那就先判断是否支持 node 的模块，支持就使用 node；
再判断是否支持 AMD，支持则使用 AMD 的方式加载。
这就是所谓的 UMD。

### AMD 和 CMD 的区别
对于依赖的模块，AMD 是 提前执行，CMD 是 延迟就近执行。
AMD是立即执行，CMD是按需执行。

AMD 推崇 依赖前置，CMD 推崇 依赖就近。

AMD 的 API 默认是一个当多个用，CMD 的 API 严格区分，推崇职责单一。


## 阶段4：ES Module
ES6 模块的设计思想是尽量的 **静态化**，即在**编译阶段**就确定模块的依赖关系，以及输入和输出的变量。

｜ CommonJS 和 AMD 模块，都只能在运行时确定这些东西。

在 ES6 中，我们使用 export 关键字来导出模块，使用 import 关键字来引入模块。

🔥 优点：Tree Shaking 和 编译优化

ESM导出的是值引用。



## 💡ES6 模块与 CommonJS 模块的差异

｜ 区别
| 区别     | CommonJS | ESM     |
| :----:         |    :----:   |         :----:  |
| 加载方式      | 运行时加载       | 编译时输出接口   |
| 加载方式   | 同步加载        | 异步加载，有一个独立的模块依赖的解析阶段。   |
| 模块输出      | 值的拷贝       | 值的引用   |
| 语法   |  require()       | import   |
| 适用环境   | Node        | 浏览器+Node   |
|Tree Shaking| 不能，因为require是动态执行，编译期无法确定依赖      | 可以，因为依赖关系可以静态分析   |

🌼 引申
1. require和import的区别：require()是运行时加载，import是编译时输出接口。
2. treeShaking是在哪个阶段做的？
3. Node 为什么最早用 CommonJS？
- 因为node读取本地文件是同步的，而CJS同步加载没有问题
4. module.exports 和 exports 的区别？

