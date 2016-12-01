# React-Native性能优化点

## shouldComponentUpdate

确保组件在渲染之后不需要再更新的，即静态组件，尽量在其中增加shouldComponentUpdate方法,防止二次消耗所产生的性能消耗

```
shouldComponentUpdate() {    //完全静态的组件,无需更新 
    return false;}
```


## key
key是react的一个特殊的属性，它是给React自己用的。如果我们动态地创建 React 元素，而且 React 元素内包含数量或顺序不确定的子元素时，我们就需要提供 key 这个特殊的属性。（包括ListView 和 ScrollView）。

我们知道，当组件的属性发生了变化，其 render 方法会被重新调用，组件会被重新渲染。但是在渲染之前，react都会进行diff，确保高效率的渲染，而这个唯一的key就很好的为每一个react组件确定了id。React会比较更新前后的元素 key 值，如果相同则更新，如果不同则销毁之前的，重新创建一个元素。

## 渲染性能低效

基于ScrollView和ListView两大容器，在渲染上，相当于web端的table布局，需要等整个大table渲染完成才显示页面，也就是说，当容器内有大量的子元素，其白屏时间会非常长。

采用异步渲染的方式，减少首屏加载的数据，实现数据懒加载，使用requestAnimationFrame 或 setTimeout 定时将单个组件push进ScrollView容器。


## setNativeProps

使用该方法修改 View 、 Text 等 RN自带的组件 ，则不会触发组件的componentWillReceiveProps 、 shouldComponentUpdate 、 componentWillUpdate 等组件生命周期中的方法。

建议频繁更新的操作，如tabs切换操作时,需要更新改tab的style，则可以使用该属性。


代码片段：

```
me.refs.tabView.setNativeProps({
    style : {
        height : 0,
        opacity : 0
    }
});
```

##  最小化DOM
类似于html，React Native里虚拟dom结构越复杂，则越低效。


## 不要使用阴影效果
React Native 里面的 shadow 相关的样式，是非常耗性能的css属性

参考文章：
[React Native 性能优化建议](http://www.tuicool.com/articles/biUNriA)

