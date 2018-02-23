# 原生js对IE7/8的兼容性写法 #
  IE6的退出市场，让我们在浏览器兼容性这一问题上舒了一口气，但是IE7/8的存在，依旧需要我们在书写脚本时谨小慎微，不可大意。本文主要整理了js对现代浏览器与IE7/8浏览器之间所需要做的兼容，水平有限，如果有疏漏或者错误的话，还望指出。
## event对象 ##
- IE7/8不支持event对象，只支持window.event对象

		function(event){
			var e = event || window.event
		}

## 鼠标坐标 ##

- IE7/8 ： event.x   event.y

- 现代浏览器： event.pageX  event.pageY
	
		function(event){
			var e = event || window.event;
			var pageX = e.pageX || e.x;
			var pageY = e.pageY || e.y;
		}

## 目标节点 ##

- IE7/8: event.srcElement

- 现代浏览器: event.target

		function(event){
			var e = event || window.event;
			var srcTarget = e.target || e.srcElement;
		}

## 事件绑定 ##

- IE7/8:element.attachEvent(on+event,fn)

- 现代浏览器:element.addEventListener(event,fn,useCapture)

		function bind(element, type, handler){
		  if (element.addEventListener){
		    element.addEventListener(type, handler, false);
		  } else if (element.attachEvent){
		    element.attachEvent("on" + type, handler);
		  }
		}

## 取消事件绑定 ##

- IE7/8:element.detachEvent(on+event,fn)

- 现代浏览器:element.removeEventListener(event,fn,useCapture)
 
		function unbind(element, type, handler){
			if (element.removeEventListener){
				element.removeEventListener(type, handler, false);
			} else if (element.detachEvent){
				element.detachEvent("on" + type, handler);
			}
		}


## 阻止默认事件 ##

- IE7/8: event.preventDefault()

- 现代浏览器: event.returnValue = false

		function(event){
			var e = event || window.event;
			if (event.preventDefault){
				event.preventDefault();
			} else {
				event.returnValue = false;
			}
		}


## 取消事件冒泡/捕获 ##

- IE7/8: 不支持事件捕获，因而只能取消事件冒泡，event.cancelBubble 

- 现代浏览器: event.returnValue = false

		function(event){
			if (event.stopPropagation){
				event.stopPropagation();
			} else {
				event.cancelBubble = true;
			}
		}
