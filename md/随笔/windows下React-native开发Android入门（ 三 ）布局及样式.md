# windows下React-native开发Android入门（ 三 ）布局及样式 #

## Flexbox布局 ##

### flexDirection ###

决定布局的主轴，子元素的排列方式。默认值是竖直轴(column)方向。

### justifyContent ###

决定子元素沿着主轴的排列方式。子元素是应该靠近主轴的起始端还是末尾段分布呢？亦或应该均匀分布？对应的这些可选项有：

- flex-start 靠近主轴的起始端分布
 
  ![](https://bigdots.github.io/blog/images/201601/flex-start.png)

- center 居中分布

  ![](https://bigdots.github.io/blog/images/201601/flex-center.png)

- flex-end 靠近末尾端的起始端

  ![](https://bigdots.github.io/blog/images/201601/flex-end.png)

- space-around  均匀分布，第一子元素不在容器最左边，最后一个子元素不在最右边

  ![](https://bigdots.github.io/blog/images/201601/flex-around.png)

- space-between 均匀分布，第一子元素在容器最左边，最后一个子元素在最右边。倆端对其

  ![](https://bigdots.github.io/blog/images/201601/flex-between.png)

### alignItems ###

决定其子元素沿着次轴（与主轴垂直的轴，比如若主轴方向为row，则次轴方向为column）的排列方式。

- flex-start
- center
- flex-end
- stretch。 项目被拉伸以适应容器，但是要使stretch选项生效的话，子元素在次轴方向上不能有固定的尺寸。

 ![](https://bigdots.github.io/blog/images/201601/align-items-stretch.png)

## CSS样式 ##

React Native基本遵循css的样式规则，但它是使用JavaScript来写样式的，所以所有样式属性必须按照JS的语法要求使用了驼峰命名法。

每一个组件都同html一样包含一个style属性，但不同的是，它接收的并不一个字符串，而是一个普通的JavaScript对象或者一个对象数组。实际开发中，我们可以直接在组件style属性中写样式对象，也可以传入一个通过`StyleSheet.create`集中定义的组件样式。

	class LotsOfStyles extends Component {
	  render() {
	    return (
	      <View>
	        <Text style={{color:red}}>just red</Text>
			<Text style={styles.red}>just red</Text>
	        <Text style={[styles.bigblue, styles.red]}>bigblue, then red</Text>
	      </View>
	    );
	  }
	}
	//集中定义样式组件
	const styles = StyleSheet.create({
	  bigblue: {
	    color: 'blue',
	    fontWeight: 'bold',
	    fontSize: 30,
	  },
	  red: {
	    color: 'red',
	  },
	});