---
title: jsonp 演示实例 —— 基于node
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
---

## 序

1. **同源策略**是浏览器处于安全考虑，为通信设置了“相同的域、相同的端口、相同的协议”这一限制。这让我们的ajax请求存在跨域无权限访问的问题。

2. 同时我们发现script标签引入脚本的行为并不受同源策略的限制，但是script引入的文件会被立即执行，如果其内容不符合js语法，则会报错；

## 操作原理

针对以上情况，诞生了jsonp：

1. 利用script标签的src属性来请求接口，并向接口传递一个回调函数（克服了同源问题）

2. 接口将数据以`回调函数参数`的形式同回调函数一同传回;此时传回则是这样形式的一个字符串：`回调函数名(数据)`，这样就符合js语法了（克服了script标签引入内容非js报错的问题）



## 实例操作

纸上得来总觉浅，绝知此事要躬行。jsonp的原理我早就倒背入流了，但是看着觉得明白，但总觉得少了点什么没抓住。所以，实际操刀试试吧。[点击下载源码](https://github.com/bigdots/some-code)

下载代码后，进入some-code/jsonp-demo文件夹,该文件夹的目录为：

app.js	 

package.json

views

1. 命令行进入当前目录，安装包依赖：

	```
	npm install
	```

2. 安装完毕后，运行程序：
	
	```
	node app.js
	```

	如果看到命令行输出“app is listening”则表示运行成功

3. 修改host

	因为需要模拟跨域，所以在host文件中创建俩个不同的域名，在host文件中添加以下内容：

	```
	127.0.0.1  www.a.com www.b.com
	```
	自此结束，在浏览器中输入http://www.a.com:3000/，如果访问成功则表示大功告成，页面中应该出现俩个按钮。
	
这个时候，我们打开浏览器的控制台，分别点击页面中的俩个按钮，就可以看到测试结果啦。

##代码分析


1. 入口文件：app.js

	- 设定模版引擎
	
		```
		app.set('views', path.join(__dirname, 'views'));
		var swig = new swig.Swig();
		app.engine('html', swig.renderFile);
		app.set('view engine', 'html');
		```
	- 设置路由和接口
	
		访问www.a.com时，渲染view/index.html页面
		
		```
		app.get("/",function(req,res){
    		res.render('index', {});
		})
		```
		请求www.b.com/index.json时，返回数据,这里服务器收到jsonp的回调函数名，并把它与数据拼接在一起返回给客户端
		
		```
		//模拟数据
		var data = {"brand":23}
		app.get("/index.json",function(req,res){
		//解析请求路径
    	var param = urlLib.parse(req.url,true);
    	var returnValue = param.query.callback+ '(' + 			JSON.stringify(data) +')';
    		res.send(returnValue)
		})
		```
	- 启动服务

		```
		app.listen(3000,function(){
	      console.log("app is listening")
		})
		```


2. 页面：view/index.html

	页面中有俩个按钮：`jsonp_button` 和 `ajax_button`,点击以后分别进行jsonp请求和ajax请求。
	
	- 绑定点击事件
	
		```
		jsonp_button.onclick = function(){
	        var url = "http://www.b.com:3000/index.json?callback=jsonp";
	        //向页面中添加script标签，进行jsonp请求
	        creatScript(url)
	    }
	
	    ajax_button.onclick = function(){
	    //ajax请求
	        var xhr = getXhr();
	        xhr.open("get","http://www.b.com:3000/index.json");
	        xhr.send();
	        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
	              console.log(xhr.responseText);
	          } else {
	              console.log("Request was unsuccessful: " + xhr.status);
	        }
	    }
	    function getXhr(){
            var xhr;
            if(window.XMLHttpRequest){      
                xhr = new XMLHttpRequest()
            }else{
                xhr = new ActiveXObject("Microsoft.XMLHTTP"); 
            }
            return xhr;
        }
	```

	- 动态创建script标签
	
		```
		function creatScript(url){
            var scriptTag = document.createElement('script');
            scriptTag.setAttribute('src', url);
            document.getElementsByTagName('head')[0].appendChild(scriptTag);
        }
        ```
	- jsonp回调函数
	
		```
		function jsonp(data) {
  			  //获取数据
            console.log(data);
        }
		```

由上面可以得出：jsonp中所有请求数据的后续操作应写在jsonp的回调函数中，它类似于ajax 的 success操作。

最后一句话概括jsonp：jsonp就是原本应该发送json数据给客户端的服务器，不再发送json，改为发送一段调用回调函数的js代码，而原本应该返回的数据则是该函数的参数。


