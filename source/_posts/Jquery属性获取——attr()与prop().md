---
title: Jquery属性获取——attr()与prop()
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
---

今天在项目中使用`<select></select>`下拉菜单时，使用juery操作，使页面加载完菜单默认选中的值为2,我一开始的操作如下：

	<!--html部分-->
    <select>
       <option value="1">1</option>
       <option value="2" id="second">2</option>
       <option value="3">3</option>
    </select>

	/**js部分**/
	$("#second").attr("selected","selected");

咋一看好完美，木问题，但是我发现在Safari浏览器中，根本不起作用！！仔细查看一番发现,在Safari浏览器中，属性确实是设置成功了，既value=2的那一项确实是`<option value="2" selected="selected">2</option>`。那问题出在哪呢？冷静，不要方，万能的stack说只要把`attr`改成`prop`就行了，卧槽还真行了，这是啥诡异事件。好吧，我们需要来研究研究了,不用想，肯定是需要祭出官方文档了。

1. attr() ： 获取匹配的元素集合中的第一个元素的属性的值  或 设置每一个匹配元素的一个或多个属性。
	- .attr( attributeName )
		- .attr( attributeName )
	- .attr( attributeName, value )
		- .attr( attributeName, value )
		- .attr( attributes )
		- .attr( attributeName, function(index, attr) )

1. prop() ： 获取匹配的元素集中第一个元素的属性（property）值或设置每一个匹配元素的一个或多个属性。
	- .prop( propertyName )
		- .prop( propertyName )
	- .prop( propertyName, value )
		- .prop( propertyName, value )
		- .prop( properties )
		- .prop( propertyName, function(index, oldPropertyValue) )

看出区别了吗，没错，是参数有区别，`attr()`传入的是`attributeName`，而`prop()`传入的是`propertyName`,现在我们的问题转移了，我们需要研究的是`attributeName`和`propertyName`之间的区别了。


## Attributes vs. Properties ##
在这里，我们可以将attribute理解为“特性”，property理解为“属性”从而来区分俩者的差异。
如果把DOM元素看成是一个普通的Object对象，这个对象在其定义时就具有一些属性（property），比如把select的option当做一个对象：

    var option = {
		selected:false，
		disabled:false，
		attributes:NamedNodeMap,
		...
	}


现在，我们一目了然了，attribute是一个特性节点，每个DOM元素都有一个对应的attributes属性来存放所有的attribute节点，它是一个叫做NameNodeMap的对象。attributes的每个数字索引以名值对(name=”value”)的形式存放了一个attribute节点。而property就是一个属性，是一个以名值对(name=”value”)的形式存放在Object中的属性。

**上例中`<option value='2' selected>2</option>`的`property`为`{0: value, 1: selected, length: 2}`**


回到一开始的问题，根据W3C的表单规范 ，在selected属性（property）是一个布尔属性， 这意味着,如果这个特性（attribute）存在， 即使该特性没有对应的值，或者被设置为空字符串值，或甚至是"false"，相应的属性（property）都还是为true。 selected特性（attribute）值不会因为复选框的状态而改变，而selected属性（property）会因为复选框的状态而改变。因此，跨浏览器兼容的检索和更改DOM属性,比如元素的checked, selected, 或 disabled状态，请使用.prop()方法。


## 为什么会搞混？ ##
之所以attribute和property容易混倄在一起的原因是，很多attribute节点还有一个相对应的property属性，比如DOM元素的id和class既是attribute，也有对应的property，不管使用哪种方法都可以访问和修改。