---
title: jquery对javascript事件的封装一览
date: 2015-10-16 14:22:21
tags: [web,javascript]
description: 事件绑定，事件封装
categories: [js]
---

javaScript 与HTML 之间的交互是通过事件实现的。事件，就是文档或浏览器窗口中发生的一些特定的交互瞬间。可以使用侦听器（或处理程序）来预订事件，以便事件发生时执行相应的代码。这种在传统软件工程中被称为观察员模式的模型，支持页面的行为（JavaScript 代码）与页面的外观（HTML 和CSS 代码）之间的松散耦合。

<!-- more -->

### 1.事件封装一览表

<table> <tbody><tr> <td>描述</td> <td>jquery</td> <td>javascript</td> </tr> <tr> <td>鼠标点击某个对象</td> <td>click()</td> <td>onclick</td> </tr> <tr> <td>鼠标双击某个对象</td> <td>dblclick()</td> <td>ondblclick</td> </tr> <tr> <td>元素获得焦点</td> <td>focus()<br/>focusin()[可以在父元素上检测子元素获取焦点的情况]</td> <td>onfocus<br/>onfocusout</td> </tr> <tr> <td>元素失去焦点</td> <td>blur()<br/>focusout()[可以在父元素上检测子元素失去焦点的情况]</td> <td>onblur<br/>onfocusout</td> </tr> <tr> <td>用户改变域的内容</td> <td>change()</td> <td>onchange</td> </tr> <tr> <td>某个键盘的键被按下</td> <td>keydown()</td> <td>onkeydown</td> </tr> <tr> <td>某个键盘的键被按下或按住</td> <td>keypress()</td> <td>onkeypress</td> </tr> <tr> <td>某个键盘的键被松开 </td> <td>keyup()</td> <td>onkeyup</td> </tr> <tr> <td>某个页面或图像被完成加载</td> <td>load()</td> <td>onload</td> </tr> <tr> <td>用户退出页面</td> <td>unload()</td> <td>onunload</td> </tr> <tr> <td>某个鼠标按键被按下</td> <td>mousedown()</td> <td>onmousedown</td> </tr> <tr> <td>鼠标被移动</td> <td>mousemove()</td> <td>onmousemove</td> </tr> <tr> <td>鼠标从某元素移开</td> <td>mouseout()</td> <td>onmouseout</td> </tr> <tr> <td>鼠标被移到某元素之上</td> <td>mouseover()/mouseout【触发子元素有效】<br/> mouseenter()/mouseleave()</td> <td>onmouseover</td> </tr> <tr> <td>某个鼠标按键被松开</td> <td>mouseup()</td> <td>onmouseup</td> </tr> <tr> <td>窗口或框架被调整尺寸</td> <td>resize()</td> <td>onresize</td> </tr> <tr> <td>文本被选定</td> <td>select()</td> <td>onselect</td> </tr> <tr> <td>提交按钮被点击</td> <td>submit()</td> <td>onsubmit</td> </tr> <tr> <td>元素滚动条在滚动</td> <td>scroll()</td> <td>onscroll</td> </tr> <tr> <td>当加载文档或图像时发生某个错误</td> <td>error()</td> <td>onerror</td> </tr> <tr> <td>提交按钮被点击</td> <td>submit()</td> <td>onsubmit</td> </tr> </tbody></table>

### 2.jquery源码(版本2.1.4)
	jQuery.each( ("blur focus focusin focusout load resize scroll unload click dblclick " +
		"mousedown mouseup mousemove mouseover mouseout mouseenter mouseleave " +
		"change select submit keydown keypress keyup error contextmenu").split(" "), function( i, name ) {

		// Handle event binding  事件绑定
		jQuery.fn[ name ] = function( data, fn ) {
			return arguments.length > 0 ?
				this.on( name, null, data, fn ) :
				this.trigger( name ); 
				//如果不带参数，则返回this.on( name, null, data, fn )，否则返回this.trigger( name )
		};
	});

split() 方法用于把一个字符串分割成字符串数组；
each()方法遍历这个数组，为每个匹配元素规定运行的函数；
jQuery.fn 指jquery的命名空间，加上fn上的方法及属性；
on方法：在选择元素上绑定一个或多个事件的事件处理函数

	on: function( types, selector, data, fn, /*INTERNAL*/ one ) {
			var origFn, type;

			// Types can be a map of types/handlers  判断types参数
			if ( typeof types === "object" ) {
				// ( types-Object, selector, data )
				if ( typeof selector !== "string" ) {
					// ( types-Object, data )
					data = data || selector;
					selector = undefined;
				}
				for ( type in types ) {
					this.on( type, selector, data, types[ type ], one );
				}
				return this;
			}
			
			//判断data参数
			if ( data == null && fn == null ) {  
				// ( types, fn )
				fn = selector;
				data = selector = undefined;
			} else if ( fn == null ) {
				if ( typeof selector === "string" ) {
					// ( types, selector, fn )
					fn = data;
					data = undefined;
				} else {
					// ( types, data, fn )
					fn = data;
					data = selector;
					selector = undefined;
				}
			}

			//判断fn参数
			if ( fn === false ) {  
				fn = returnFalse;
			} else if ( !fn ) {
				return this;
			}

			//判断fn参数
			if ( one === 1 ) {
				origFn = fn;
				fn = function( event ) {
					// Can use an empty set, since event contains the info
					jQuery().off( event );
					return origFn.apply( this, arguments );
				};
				// Use same guid so caller can remove using origFn
				fn.guid = origFn.guid || ( origFn.guid = jQuery.guid++ );
			}
			return this.each( function() {
				jQuery.event.add( this, types, fn, data, selector );
			});
		}

trigger方法：在每一个匹配的元素上触发某类事件。

	trigger: function( type, data ) {
		return this.each(function() {
			jQuery.event.trigger( type, data, this );
		});
	},


---
<p style="text-align:right">整理于2015-11-30 17:28:21</p>