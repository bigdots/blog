# redux

 Redux 应用只有一个单一的 store

## action

Action 是把数据从应用,传到 store 的有效载荷。它是 store 数据的唯一来源。

action是一个普通对象，用于指明是哪种操作，这样才能在reducers中进行识别。action是用户触发或程序触发的一个普通对象。

## store

维持应用的 state
提供 getState() 方法获取 state；
提供 dispatch(action) 方法更新 state；
通过 subscribe(listener) 注册监听器;
通过 subscribe(listener) 返回的函数注销监听器。

store的最终值就是由reducer的值来确定的。（一个store是一个对象, reducer会改变store中的某些值）


## reducer
指明应用如何更新 state。是纯函数，只做数据处理，它接收俩个参数：action和state，并return一个新的state。

reducer是负责返回新的state的函数。


reducer是根据action操作来做出不同的数据响应，返回一个新的state。


## 单向数据流

调用 store.dispatch(action)

调用 reducer 函数处理数据，返回新的state

把多个子 reducer 输出合并成一个单一的 state 树

保存根 reducer 返回的完整 state 树



