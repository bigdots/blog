# 第一次react-native项目实践要点总结

今天完成了我的第一个react-native项目的封包，当然其间各种环境各种坑，同时，成就感也是满满的。这里总结一下使用react-native的一些入门级重要点（不涉及环境）。**注意：阅读需要语法基础： ES6 、react 、JSX**

我对react-native的理解简而言之就是 ：**react的语法** ＋ **native的组件**

## 组件的创建声明
```
class HelloWorldApp extends Component {
  constructor(props) {
  	super(props);
    this.state = {
  
    };
  }
  render() {
    return (
      <Text>Hello world!</Text>
    );
  }
}
```
上面代码定义了一个“类”，可以看到里面有一个constructor方法，这就是构造方法，而this关键字则代表实例对象。当你在其他的组件中调用这个组件时，就会实例化这个“类”（即组件）。

**注意：组件名需要大写**

## 组件的导出、引用与注册
在ES6中，新增了import和export俩个关键字来导入导出模块。react－native的组件也是采用的这俩个关键字。

俩种方式：

第一种：

```
导出：
export default class HelloWorldApp extends Component{
	render() {
    return (
      <Text>Hello world!</Text>
    );
  }
｝

导入：
import HelloWorldApp from "../.."
```

第二种：

```
导出：
class HelloWorldApp extends Component {
  render() {
    return (
      <Text>Hello world!</Text>
    );
  }
}

export {HelloWorldApp}

导入：
import ｛ HelloWorldApp ｝ from "../.."

```

1. 后缀名自动获取（文件会获取拥有与之相应后缀名的文件）

	在组件模块的导入过程中，如果这个模块是分设备的，也就有俩个文件：xxx.android.js和xxx.ios.js，这些后缀（android和ios）是不需要的，在不同的设备环境中，它自动获取相应后缀名的文件，即ios包会自动获取xxx.ios，android包会自动获取xxx.android。

2. 后缀名自动忽略（文件会自动忽略拥有与之不相应后缀名的文件）

	一个ios和android的公共模块文件，即共用代码模块文件，命名不能加ios和android后缀，否则，ios包取不到有androis后缀的文件，android取不到有ios后缀的文件。


实例解释上述：
现在有以下五个文件：

index.ios.js 
 
index.android.js

say.android.js

say.ios.js

HelloWorldApp.android.js   

我们想要分别在index.ios.js 和 index.android.js引入其他三个模块。我们只要在index.ios.js 和 index.android.js文件中如下写法就行：

```
//这里，index.ios.js会自动获取say.ios.js的模块；index.android.js会自动获取say.android.js的模块

import 模块名 from "./say";

//这里，HelloWorldApp.android.js 是一个公共模块，index.android.js能成功获取到./HelloWorldApp；但是index.ios.js则无法获取到HelloWorldApp模块，因为index.ios.js会忽略android后缀名的模块文件

import 模块名 from "./HelloWorldApp"
```


## react组件的生命周期

![](https://raw.githubusercontent.com/bigdots/blog/master/images/201601/react-component.jpg)

项目中使用组件的时候，纠结于componentWillMount,componentDidMount...，直到看到这张图豁然开朗(so，图是盗的)。需要注意的是，这张图应该比较老了，其中的getDefaultProps和
getInitialState这俩个函数是ES5的写法了，ES6语法中，constructor方法中代替了getDefaultProps／getInitialState，我们可以在其内直接初始化props和state。

生命周期：

1. 实例化（初始化）
	- constructor

		设置默认的props->设置默认的state
	- componentWillMount
		
		完成渲染之前执行，此时可以设置state
		
	- render

		创建虚拟DOM，此时不能修改state
		
	- componentDidMount

		真实的DOM渲染完毕，此时可以更改组件props及state

2. 存在期：(这个时候的主要行为是状态的改变导致组件更新)
	- componentWillReceiveProps

		组件接收到新的props,此时可以更改组件props及state
		
	- shouldComponentUpdate

		操作组件是否应当渲染新的props或state，返回布尔值，首次渲染该方法不会被调用。
		
	- componentWillUpdate

		接收到新的props或者state后，进行渲染之前调用，此时不允许更新props或state。
		
	- render

		创建（更新）虚拟DOM
		
	- componentDidUpdate
	 组件真实的DOM更新完成
	 
3. 销毁期：
	- componentWillUnmount
	组件被移除之前，主要用于做一些清理工作，比如事件监听


## react 的 props 和 state

1. props（属性）

	当我们调用这些组件时，我们如果为每一个组件传递了不同的属性，这个属性就是props。比如下例中，我们调用了HelloWorldApp组件，并为其设置了一个date属性，则我们可以在HelloWorldApp的组件里，通过this.props.date来获取这一属性值。
	```
	<HelloWorldApp date = {2016}>
	```

2. state（状态）

	**state需要在constructor中初始化，然后通过调用setState方法修改。**
	通过上面的组件生命周期图，我们可以看出，state是一个状态机，state的改变会引起shouldcomponentupdate、componentwillupdate、rendner...一系列方法的执行，**视图会重新渲染**。所以，如果需要动态地改变组件的数据或试图，请操作state。


## react组件之间的通信  

1. 子组件接收父组件的改变信号

	简单：当父组件改变时，直接向子组件传递props
	
2. 父组件接收子组件的改变信号
	在父组件中定义一个方法，并通过props传递给子组件，子组件改变时，通过调用这个父组件传递过来的方法，从而实现在父组件中执行该方法。
	
3. 非父子关系组件之间的通信

	`RCTDeviceEventEmitter`模块：它有俩个方法：emit和addListener，一个发送，一个接收。
	
	RCTDeviceEventEmitter.emit(notifName,param);
	
	RCTDeviceEventEmitter.addListener(notifName,callback)
	

## native 事件对象

在项目中，遇到一个控制scrollview组件滚动的需求，需要获取当前滚动的坐标，当时找了好久的文档，没找到解决方案，后来发现可以通过这样来传入一个事件对象

```
<ScrollView ref='scrollView' onScroll = {(e) => {this.scrollhShow(e);}}>
```

然后在函数中读取：

```
scrollhShow(e) {
	console.log(e.nativeEvent)
}
```
当当当当，我要的滚动视图的坐标值就在里面了。
