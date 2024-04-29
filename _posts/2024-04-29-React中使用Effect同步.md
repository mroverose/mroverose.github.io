---
layout: post
title: React中使用Effect与外部系统同步
description: "React"
date:   2024-04-29 9:44:05 +0800
categories: web技术 React
tags: Web前端 React
---
#### 使用Effect同步
> Effects会在渲染后运行一些代码，以便可以将组件与React之外的某些系统同步。

##### Effect与事件(event)有什么不同？
> React组件中主要有两种逻辑类型：
- 渲染逻辑代码。位于组件的顶层，在这里接收props和state，并对它们进行转换，最终返回在屏幕上看到的JSX。**渲染的代码必须是纯粹的**——它只应该“计算”结果，而不做其他任何事情。
- 事件处理程序。是嵌套在组件内部的函数，包含特定用户操作(列如按钮点击或键入)引起的“副作用”(它们改变了程序的状态)。

![pad image]({{ site.url }}/assets/image/hand-landscape.jpg)
<!- excerpt_separator ->
>Effect允许指定同渲染本身，而不是特定的事件引起的副作用，Effect在屏幕更新后的提交阶段运行，可以将React组件与某个处部系统同步。

##### 如何编写Effect
>编写Effect需要遵循以下三个规则：
  1. 声明Effect。默认情况下，Effect会在每次渲染后都会执行。
  2. 指定Effect依赖。可以指定Effect按需执行，而不是在每次渲染后都执行。
  3. 必要时添加清理(cleanup)函数。有时Effect需要指定如何停止、撤销，或者清除它的效果。

>##### 第一步：声明Effect

`import { useEffect } from 'react';`    //引入*useEffect Hook* ;
```
//在组件顶部调用它，并传入每次渲染时都需要执行的代码
function MyComponent(){
    useEffect(() =>{
        //...代码
    });
    return <div />;
}
```
useEffect会在屏幕更新渲染后执行代码。

>##### 第二步：指定Effect依赖

一般来说，Effect会在每次渲染时执行，但更多时候，并不需要每次渲染的时候都执行Effect。因为：
   - 有时这会拖慢运行速度，因为与外部系统的同步操作总是有一定时耗，在非必要时可能希望跳过它。
   - 有时这会导致程序逻辑错误。例如，组件的淡入动画只需要在第一轮渲染出现时播放一次，而不是每次触发新一轮渲染后都播放。

将**依赖数组**传入useEffect的每二个参数，以告诉React**跳过不必要地重新运行Effect**。
```
useEffect(() =>{
    //...
},[]);
```

> ##### 第三步：按需添加清理函数

如果Effect因为重新挂载而中断，那么需要实现一个清理函数。React将在**下次Effect运行之前**以及**卸载期间**这**两个时候**调用清理函数。


```
import { useState, useEffect } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    function onTick() {
      setCount(c => c + 1);
    }

    const intervalId = setInterval(onTick, 1000);
    return () => clearInterval(intervalId);  //清理函数，取消定时器
  }, []);

  return <h1>{count}</h1>;
}

```


 `来源：`[学习React](https://zh-hans.react.dev/learn/synchronizing-with-effects)