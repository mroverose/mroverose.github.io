---
layout: post
title: React中使用Context深层传递参数
description: "React"
date:   2024-04-26 10:17:05 +0800
categories: web技术 React
tags: Web前端 React
---
#### 使用Context传递参数
>如果希望通过许多中间件向下传递props,或是在应用中的许多组件都需要相同的信息，可以使用Context：

![pad image]({{ site.url }}/assets/image/ratingStar.jpg)
<!- excerpt_separator ->
##### 使用Context的一般步骤
1. 创建一个context。将其从一个文件中导出，这样组件才可以使用它。
2. 在需要数据的组件内使用刚刚创建的context。
3. 在指定数据的组件中提供这个context。context可以让父节点，甚至很远的父节点都可以为其内部的整个组件树提供数据。

##### Step 1：创建Context 
```
//创建一个context并导出，以便在组件中可以使用
import {crateContext } from 'react';
export const YourContext = createContext(1);
```
##### Step 2：使用Context
```
//在组件文件中导入useContext和创建的Context
import { useContext } from 'react';
import { YourContext } from './YourContext.js';

const level = useContext(YourContext);    //将yourContext值赋值给level来使用它
```

##### Step 3: 提供Context
```
//在父组件中用 context provider包就是裹子组件以提供context绘它们
import { YourContext } from './YourContext.js';


<YourContext.Provider value={level}>
    {children}
</YourContext.Provider>

```
#### Context的使用场景
- 主题：如果你的应用允许用户更改其外观，你可以在应用顶层放一个context provider，并在需要调整外观的组件中使用该context。
- 当前账户：许多组件可能需要知道当前登录的用户信息。将它们放到context中可以方便地在树中的任何位置读取它。某些应用还允许用户同时操作多个账户。在这些情况下，将UI的一部分包裹到具有一同账户数据的provider中会很方便。
- 路由：大多数路由的解决方案在其内部使用context来保存当前路由。这就是每个链接“知道”它是否处于活动状态的方式。
- 状态管理：随着你的应用的增长，最终在靠近应用顶部的位置可能会有很多state。许多遥远的下层组件可能想要修改它们。通常**将reducer与context搭配使用**来管理复杂的状态并将其传递给深层的组件来避免过多的麻烦。

   `来源：`[学习React](https://zh-hans.react.dev/learn/passing-data-deeply-with-context)