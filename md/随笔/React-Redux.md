# React-Redux

## 序
Redux的作者友情赞助，封装了一个 React 专用的库 `React-Redux`，为 React + Redux 提供了一种更科学的代码组织方式。

本人认为`React-Redux`的中心思想就是实现`Model`与`View`的分离。它将所有组件分成两大类：

+ UI组件（View层）

	- 只负责 UI 的呈现，不带有任何业务逻辑
	- 没有状态（即不使用this.state这个变量）
	- 所有数据都由参数（this.props）提供
	- 不使用任何 Redux 的 API

+ 容器组件（Model层）

	- 负责管理数据和业务逻辑，不负责 UI 的呈现
	- 带有内部状态
	- 使用 Redux 的 API

这样以来也实现了Redux和React分离，UI组件只要书写react的代码，容器组件只负责书写redux相关操作，易于书写与维护。

react-redux主要提供了倆个关键API：Provider组件 和 connect函数。


## connect()
connect函数的主要功能是连接 UI组件 与 容器组件。它会自动为您的UI组件生成一个容器组件，并且创立起它们之间的通信。

```
const ContainerComponent = connect(
	mapStateToProps,
	mapDispatchToProps
)(UIComponent);
```

上面代码中，UIComponent是 UI组件，containerComponent就是由 React-Redux 通过connect方法自动生成的容器组件。而 `mapStateToProps` 和 `mapDispatchToProps` 则建立起了倆种组件之间的通信机制。

### mapStateToProps（可选参数）

`mapStateToProps(state,[ownProps])`

+ state: state数据
+ ownProps: 可选参数，容器组件的props对象，使用ownProps作为参数后，如果容器组件的参数发生变化，也会引发 UI 组件重新渲染。


这个函数的主要功能是将`state`通过`props`属性传递给UI组件，它会订阅 Store，每当state更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染。

mapStateToProps函数返回一个对象，这个对象中的每一个键值对都会映射到UI组件的`props`上。

```
const mapStateToProps = (state) => {
  return {
    data: state
  }
}
```

上面的 mapStateToProps 函数将`state`传递给了UI组件的`props`属性，您可以在UI组件内通过this.props.data来访问`state`。这样就实现了容器组件向UI组件方向上的数据传递。

### mapDispatchToProps（可选参数）

mapDispatchToProps可以是一个函数，也可以是一个对象。

作为函数，mapDispatchToProps 会得到 `dispatch` 和 `ownProps` 俩个参数，并且返回一个对象。

```
const mapDispatchToProps = (
  dispatch,
  ownProps
) => {
  return {
    click: () => {
    	//	可以进行dispatch操作，发出action
    }
  };
}
```

上面的`mapDispatchToProps`函数会返回一个对象，这个对象的键值依旧会传递给UI组件的`props`，在UI组件内部，您可以通过`this.props.click`来调用这个函数。


## Provider 组件

Provider 组件主要作为整个应用的容器，用来传递store给connct所生成的容器组件。

```
<Provider store={store}>
    <ContainerComponent />
</Provider>
```

上面代码中，Provider在根组件外面包了一层，这样一来，App的所有子组件就默认都可以拿到state了。

源码：

```
class Provider extends Component {
  getChildContext() {
    return {
      store: this.props.store
    };
  }
  render() {
    return this.props.children;
  }
}

Provider.childContextTypes = {
  store: React.PropTypes.object
}
```

解释上面的代码之前先解释下 context 和 contextTypes，

+ context：同state、props一样都是 React 的数据载体，但它可以实现组件间跨级传递数据
+ contextTypes： 在组件上指定后指定该属性后，方可**访问**该组件context的属性
+ getChildContext： 在需要向下传递数据的父组件中使用，用于指定传递的数据，
+ childContextTypes： 在组件上指定后指定该属性后，方可**传递**该组件context的属性

也就是说：指定数据并要将数据传递下去的父组件要定义 `childContextTypes` 和 getChildContext() ；想要接收到数据的子组件 必须定义 `contextTypes` 来使用传递过来的 context 。

所以，上面代码的意思就是：`Provider`组件通过`getChildContext`方法指定向下传递的数据为store，并通过`childContextType`属性使该属性得以传递。`Provider`的子组件可以通过`this.context`取到数据。






