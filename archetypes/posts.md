---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true

# 访问路径（可选，不写则自动生成）
slug: "{{ .Name }}"

# 分类（建议 1～2 个）
categories:
  - 技术笔记

# 标签（可多个）
tags:
  - 前端

# 列表页摘要（hugo-theme-stack 会优先使用）
summary: ""
---

## 背景

> 为什么要写这篇文章？

## 核心内容

### 1️⃣ 概念说明

### 2️⃣ 使用方式

### 3️⃣ 实际案例

```js
// 示例代码
console.log('Hello Vite');
