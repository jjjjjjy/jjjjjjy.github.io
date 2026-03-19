---
title: "SSE"
date: 2026-03-13T16:39:13+08:00
draft: false

# 访问路径（可选，不写则自动生成）
slug: "sse"

# 分类（建议 1～2 个）
categories:
  - 技术笔记

# 标签（可多个）
# tags:
#   - JavaScript
---


####
SSE（Server-Sent Events）是一种基于 HTTP 的服务器推送技术，浏览器通过 EventSource 建立长连接，服务器可以持续向客户端发送 text/event-stream 数据流。它是单向通信，适合实时推送场景，例如 AI 流式输出、通知系统、日志流等。

特点：

基于 HTTP

单向通信（Server → Client）

浏览器通过 EventSource API 接收服务器不断推送的数据

连接建立后 服务器可以持续发送数据流


优点:
* 简单易用：SSE提供了一种在服务器和客户端之间建立单向连接的直接方法。客户端订阅了SSE端点，然后服务器可以通过此连接将数据推送到客户端，而不需要客户端不断发送请求
* 减少网络开销：与持续轮询技术（即每隔几秒钟让每个客户端从服务器请求数据）相比，SSE显著减少了网络开销。
* 标准化协议：SSE基于HTTP协议，使其易于部署并与现有的web基础设施兼容。

#### 为什么SSE适合AI流式输出
因为服务器可以持续push数据

#### SSE 为什么比 WebSocket 简单
因为SSE基于 HTTP，不需要协议升级