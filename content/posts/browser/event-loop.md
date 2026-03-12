---
title: "浏览器事件循环"
date: 2026-03-12T21:13:47+08:00
draft: false

slug: "event-loop"

categories:
  - 技术笔记

tags:
  - 浏览器
  - JavaScript

## summary: "从浏览器运行结构到宏任务与微任务，系统理解 JavaScript 事件循环机制。"
---

## 谨记

**JavaScript 在浏览器中是单线程执行的。**

但需要注意：

JavaScript 本身是单线程执行的，但浏览器运行环境是多线程的。
异步任务由浏览器的其他线程处理，完成后通过 **事件循环（Event Loop）** 回调到 JS 主线程执行。
如果需要真正的并行计算，可以通过 **Web Worker** 创建新的 JS 线程。

#### 浏览器事件循环是什么

JavaScript 在浏览器中是 **单线程执行的**，但浏览器需要处理很多异步任务，例如：网络请求、定时器、用户交互事件、DOM 事件。

为了在不阻塞主线程的情况下处理这些异步任务，浏览器引入了 **事件循环（Event Loop）机制**。

🌟 **核心流程：**

1. **同步代码**进入调用栈 `Call Stack` 执行
2. **异步任务**交给浏览器的 `Web APIs` 处理（如定时器、网络请求）
3. 异步任务完成后，其回调函数进入 **任务队列 `Task Queue`**
4. **Event Loop** 持续检测调用栈是否为空
   如果为空，则把任务队列中的任务放入调用栈执行


#### 为什么需要事件循环

由于 JavaScript 是单线程的：同一时间只能执行一段代码，如果没有事件循环，setTimeout就需要等待对应的time才能继续执行，而这会导致：页面卡死。

因此**浏览器设计成 = 单线程 JS + 事件循环调度 + 浏览器异步线程**。从而实现：非阻塞异步

#### 浏览器运行 JS 的基本结构
1. 同步代码进入 调用栈`Call Stack` 执行
2. 异步任务交给浏览器的 Web APIs（例如定时器、网络请求）
3. 异步任务完成后，其回调函数会进入 任务队列`Task Queue`
4. Event Loop 会不断检查 调用栈是否为空，如果为空，就把任务队列中的任务放入调用栈执行。

#### 任务队列：宏任务 与 微任务
任务队列中的任务分为两类：Macro Task（宏任务）和 Micro Task（微任务）

#### 宏任务（Macro Task）

**定义：** 宏任务是由浏览器调度的任务，每次事件循环会执行 **一个宏任务**。

**常见宏任务：**
1. 整个 JS 文件（script）
2. setTimeout
3. setInterval
4. DOM 事件
5. postMessage
6. MessageChannel

#### 微任务（Micro Task）

微任务会在 **当前宏任务执行结束后立即执行**。

常见微任务：

1. **Promise的回调**[.then、.catch、.finally]
2. queueMicrotask
3. MutationObserver
4. await 后面的代码会进入微任务队列

#### 浏览器事件循环的执行顺序是
**🔥 执行一个宏任务 → 执行所有微任务 → 浏览器进行一次渲染 → 再执行下一个宏任务**

也就是说：**微任务的优先级高于宏任务**

#### 浏览器渲染时机

浏览器通常会在 宏任务执行完并清空微任务队列后 才进行页面渲染，所以如果微任务不断产生，可能会导致页面渲染被延迟。

### 常见误区

#### 误区一：同步任务是宏任务，异步任务是微任务 ❌

浏览器调度任务分为宏任务和微任务，从浏览器层面而言，没有同步任务和异步任务的说法。在浏览器层面，同步和异步只是执行方式。同步代码只是 **当前宏任务中的执行内容**。


#### 误区二：Promise 本身是微任务 ❌ / Promise.then是微任务 ❌
Promise **本身不是微任务**。Promise executor 是同步执行，Promise的then、catch、finally的**回调**才是微任务。

#### 误区三：async是微任务 ❌ / await 是微任务 ❌
这个误区其实和 “Promise 是微任务” 的误区本质类似。
async 本身并不是微任务。async 只是 JavaScript 的一个语法糖，它会让 函数返回一个 Promise。
await 也是语法糖，await = **暂停当前 async 函数** + 注册一个 Promise.then。await的后面的代码才是微任务。

#### 误区四：浏览器执行 JS 时，并不是每一行代码都是一个任务。 ❌
整个JS代码块是一个宏任务。


### 实战
```javascript
async function async1() {
  console.log(1);
  await async2();
  console.log(2);
}

async function async2() {
  console.log(3);
  await Promise.resolve();
  console.log(4);
}

async1();

console.log(5);
```

最后输出:
1 3 5 4 2

拆解:
1. 进入 script（一个宏任务）执行 async1()
2. 进入 async1()
3. 执行 console.log(1); 此时输出 1。
4. 执行 await async2();
5. 进入 async2()
6. 执行 console.log(3); 此时输出 3。
7. 执行 await Promise.resolve(); 由于 await = 暂停当前 async 函数 + 注册一个 Promise.then。也就是说 console.log(4); 会被注册成一个微任务。
8. 此时 async2() 被暂停，后续代码 console.log(4); 已经进入 微任务队列。
9. async2() 返回一个 pending 的 Promise。
10. 回到 async1() 中的 await async2(); 由于 async2() 返回的 Promise 还没有 resolve，所以 async1() 也会 被暂停。
11. async1() 后面的 console.log(2); 暂时不会执行，也不会进入微任务队列。
12. async1() 执行结束（暂停状态），回到 script。
13. 执行 console.log(5); 此时输出 5。
14. 当前 script（宏任务）执行完毕。
15. Event Loop 开始执行 微任务队列。
16. 执行微任务 console.log(4); 此时输出 4。
17. async2() 执行完成，其返回的 Promise resolve。
18. async1() 中的 await async2(); 得到结果，于是继续执行 async1() 剩余代码。
19. console.log(2); 被加入 微任务队列。
20. 执行该微任务 console.log(2); 此时输出 2。

