---
title: "AbortController"
date: 2026-02-27T17:17:10+08:00
draft: false

# 访问路径（可选，不写则自动生成）
slug: "AbortController"

# 分类（建议 1～2 个）
<!--categories:
  - 技术笔记-->

# 标签（可多个）
<!--tags:
  - 如何保证页面最终展示的一定是“用户最后点击的那一次请求结果”？-->

# 列表页摘要（hugo-theme-stack 会优先使用）
summary: "如何保证页面最终展示的一定是用户最后点击的那一次请求结果?"
---

**🔥 如何保证页面最终展示的一定是用户最后点击的那一次请求结果？**

在前端开发中，我们经常会遇到这样的问题：用户快速点击多个按钮或者输入搜索关键词，发出了多次网络请求，但是由于网络传输、服务器处理、返回时间等原因，导致响应的顺序和请求的顺序不一定一致。

比如说用户点击了三个商品，顺序为ABC，但是响应的顺序为ACB，此时页面最终呈现的就是B商品的详情，但是按照道理来说，此时应该展示的是C商品。这个就是一个典型的竞态问题：即**多个请求并发时，返回顺序不可控，可能导致 UI 被旧请求的响应数据覆盖。**

## 1️⃣ AbortController（取消旧请求）

  `AbortController` 是 JavaScript 中的一个全局类，它可以用来终止任何**异步**操作。

  👉 AbortController是一个Web API，它提供了一个信号对象（AbortSignal），该对象可以用来取消与Fetch API相关的操作。当我们创建AbortController实例时，会自动生成一个与之关联的AbortSignal对象。我们可以将这个AbortSignal对象作为参数传递给fetch函数，从而实现对网络请求的取消控制。

  基本思路如下：
  1. 为每次请求创建一个 AbortController 实例。 `const controller = new AbortController();`
  2. 通过AbortController实例的signal属性，获取到AbortSignal对象，然后在调用fetch函数时，我们将signal对象作为选项对象的signal属性传递进去。`const signal = controller.signal;`
  3. 每次新请求前，取消上一次未完成的请求。 `controller.abort();`
  4. 只处理最后一次请求的响应，旧请求即使返回也不会影响页面显示。

  通过这种方式，无论用户点击多快，页面最终展示的总是 最后一次点击的商品详情。

💡 小提示：对于搜索框或者频繁触发的请求，还可以结合 防抖（debounce） 或 节流（throttle），进一步优化性能，减少无效请求。

👉 但是 AbortController 存在兼容性问题


## 2️⃣ 版本号校验（保证正确性）最稳的方案
思路：每次点击生成一个“请求版本号”。只有最后一次请求才允许更新 UI。

## **📍 为什么 AbortController 不能完全替代版本号？**

因为即使你调用：controller.abort()；也可能会出现以下几种情况
1. 请求已经返回，已经完成的请求调用 abort() 没用（因为 signal 只控制未完成的请求）
2. fetch 响应返回（resolve），abort 不能阻止 resolve 进入微任务队列
3. then 已经注册回调，abort 不会移除已注册回调，只能 reject fetch promise

所以：UI 仍然可能被旧数据覆盖，这就是为什么真正稳的方案是：AbortController + 版本号校验


