---
title: "Promise"
date: 2026-03-13T14:47:52+08:00
draft: false

# 访问路径（可选，不写则自动生成）
slug: "promise"

# 分类（建议 1～2 个）
categories:
  - 技术笔记

# 标签（可多个）
tags:
  - 微任务

# 列表页摘要（hugo-theme-stack 会优先使用）
summary: ""
---

#### Promise 定义
1. Promise 是 JS 提供的**异步**解决方案。
2. 内部存在三种状态：pending fulfilled rejected。状态一旦改变不可逆。
3. Promise 内部维护两个回调队列：onFulfilledCallbacks和onRejectedCallbacks。then 会把回调存入队列。当 resolve 或 reject 时，会执行对应回调。
4. then 返回新的 Promise，因此可以实现链式调用。


#### Promise 状态
pending 等待
fulfilled 成功
rejected 失败

状态变化
pending → fulfilled
pending → rejected

状态一旦改变就不能再变。【原因:避免竞态条件】

#### Promise 特点
Promise 最大的特点是 then会返回新的Promise。也就是Promise可以进行链式调用，因此可以解决回调地狱

#### Promise 本质
Promise的本质是 状态 + 回调队列
