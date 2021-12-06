---
title: redux 初识
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
---

## 前言

> Redux 试图让 state 的变化变得可预测

react／react-native 将组件的更新交给了状态机（state），想要更新页面活着页面的某个组件就必须得通过改变state的方式。页面越复杂，组件越多，所需要的state就越多，并且随着页面的交互，state也需要不断得变化，而管理这些不断变化的 state 就变的非常困难。终有一刻，不计其数的 state 会让你觉得 state 的变化已然不受控制。


> "如果你不知道是否需要 Redux，那就是不需要它。"

Redux主要作用是让应用的 state 可以集中管理，从而达到清晰管理每个 state，所以当你的应用很简单时，完全不需要使用redux，它会增加你的工作量。

## 三大原则

Redux 主要是通过限制 state 更新发生的时间和方式来实现 state 的管理。而这些限制条件则反应在三大原则中：

### 单一数据源

整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中。


### State 只读

**惟一改变 state 的方法就是触发 action。**

确保视图和网络请求都不能直接修改 state，它们可以表达想要修改的意图（actio），然后通过这个触发意图（action）来修改 state。

### 使用纯函数来执行修改

**为了描述 action 如何改变 state tree ，你需要编写 reducers。**

Reducer 是纯函数，它接收先前的 state 和 action，并返回新的 state。


## Action、Reducer 和 Store

### Action

action是一个普通对象，用于指明用户的操作行为，它是把数据从应用传到 store 的有效载荷，是 store 数据的唯一来源。通常将新数据（state）传入action发送给store。

```
const EAT_APPLE = 'EAT_APPLE'

{
  type: EAT_APPLE,
  text: 'eat an apple'
}
```
这里定义了一个action对象，它有`type`和`text`俩个键，其中`type`是必需的，用于描述当前action；`text`是自定义的，作为承载数据的载体。

 **Action 创建函数**
 
 `Action创建函数`就是生成 action 的方法，，调用这个函数会创建action，通常只返回一个简单的action对象。它的作用主要是为了减少重复大量地创建action。

 ```
 function eat(text) {
  return {
    type: EAT_APPLE,
    text
  }
 }
 ```

### Reducer

reducer根据action操作来做出不同的数据响应，指明应用如何更新 state。它是一个纯函数，只做数据处理。

```
(previousState, action) => newState
```

它接收俩个参数：action和state，并return一个新的state。

纯函数(同样的输入，必定得到同样的输出)：

+ 修改传入参数；
+ 执行有副作用的操作，如 API 请求和路由跳转；
+ 调用非纯函数，如 Date.now() 或 Math.random()。


```
 //使用ES6参数默认值语法初始化state
function toEat(state = {behavior: ""}, action) {
  if (typeof state === 'EAT_APPLE') {
    return Object.assign({}, state,{
    	behavior: action.text
    }}
  }
  
  return state；
}
```

**注意：**

+ 不要修改 state，Object.assign() 新建了一个副本
+ 在无匹配action的情况下返回旧的 state

由于redux单一数据源，所以整个应用只有一个单一的 store，所以当需要拆分数据处理逻辑时，你应该使用 reducer 组合 而不是创建多个 store。

Redux 提供了一个combineReducers方法，用于 Reducer 的合并：

```
combineReducers({reducer,...})

```

### Store

 Redux 应用只有一个单一的 store。
 
 store有以下方法：
   
 - createStore(reducer,[preloadedState])  

 	根据传入的reducer创建一个store。这个函数的第二个参数是可选的，用于设置 state 初始状态。
 	
 - store.getState()

 	获取当前state的值。
 	
 - store.dispatch(action)
	
	向store派遣一个action。即向store传值。
 	
 - store.subscribe(listener)
	
	注册监听器，监听store，一旦store变化，会触发listener。该函数会返回一个函数用于注销该监听器。


### 工作流程

 一个清晰明了的流程图能帮我们更好的理解：
 ![](https://raw.githubusercontent.com/bigdots/blog/master/images/201601/redux.png)

从上图可以看出redux处理的是一个单向数据流：

+ 用户行为或者程序调用 store.dispatch(action)，向store派遣action；

+ store在接收到action后，会自动呼起reducer来处理action，并且会传入俩个参数（当前 State 和收到的 Action），这里reducer可以依据数据处理逻辑拆分成多个,但是数据源store只能是一个；

+ combineReducers函数会将多个多个子 reducer 输出合并成一个单一的 state 树

+ 生成新的UI


## 总结
1. 并不总是需要redux，如果你的应用没那么复杂，就没必要用它；
2. redux通过限制数据更新发生的时间和方式来达到管理state的目的；
3. redux三大原则：单一store、reducer纯函数、state只读
4. 在 createStore 和 reducer 函数中都可以初始化state。
5. redux通过dispatch、subscribe、getState实现数据的发送、监听、获取，从而实现单向数据流的流动