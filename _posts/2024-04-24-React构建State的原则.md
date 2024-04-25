---
layout: post
title: React中State的构建原则
description: "React"
date:   2024-04-24 16:02:02 +0800
categories: Web技术 React
tags: web前端 React
---
#### React中构建state的原则
>当编写一个有state的组件时，你需要选择使用多少个state变量以及它们都是怎样的数据格式，下面几个原则有助于做出更好的决策:
![pad image]({{ site.url }}/assets/image/padWeb.jpg)
1. 合并关联的state。如果总是同时更新两个或更多的state变量，考虑将它们合并为一个单独的state变量。
2. 避免互相矛盾的state。当state结构中存在多个相互矛盾或“不一致”的state时，就可能存在隐患。就尽量避免这种情况。
3. 避免冗余的state。如果能在渲染期间从组件的props或其现有的state变量中计算出一些信息，则不应将这些信息放入该组件的state中。
4. 避免重复的state。当同一数据在多个state变量之间或在多个嵌套对象中重复时，这会很难保持它们同步。应尽可能减不重复。
5. 避免深度嵌套的state。深度分层的state更新起来不方便。如果可能的话，最好以扁平化方式构建state。
   这些原则背后的目标是**使state易于更新而不引入错误**。从state中删除冗余和重复数据有助于确保所有部分保持同步，更能够减少出现错误的机会。

   `来源：`[学习React](https://zh-hans.react.dev/learn/choosing-the-state-structure)