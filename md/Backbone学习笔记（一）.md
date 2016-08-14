title: Backbone学习笔记（一）
date: 2015-12-01 16:02:22
tags: [web,Backbone]
description: Backbone Backbone.js MVC

---

# ——Backbone中的MVC
[Backbone.js](http://backbonejs.org/)为复杂WEB应用程序提供模型(models)、集合(collections)、视图(views)的结构。其中models用于绑定键值数据和自定义事件；collections附有可枚举函数的丰富API； views可以声明事件处理函数，并通过RESRful JSON接口连接到应用程序。

下面通过实例来学习backbone的MVC。实例来自[http://dmyz.org/archives/598](http://dmyz.org/archives/598)。原文章虽然是入门范例，但大牛的入门对我这种菜逼来说还是看不懂，所以我在其原来的例子上进行了一些取舍并对一些代码进行了注释。

[Backbone.js中文文档:http://www.css88.com/doc/backbone/](http://www.css88.com/doc/backbone/)

![](/images/201511/1.svg)
<!-- more -->

---

HTML代码，只要把backbone的代码放入其<script></script>内运行即可：

	<!DOCTYPE html>
	<html>
	<head>
	<script type="text/javascript" src="https://code.jquery.com/jquery-1.11.1.js"></script>
	<script type="text/javascript" src="http://underscorejs.org/underscore-min.js"></script>
	<script type="text/javascript" src="http://backbonejs.org/backbone-min.js"></script>

	<link href="http://cdn.bootcss.com/bootstrap/3.1.1/css/bootstrap.min.css" rel="stylesheet">
	</head>
	<body>
	  <table id="js-id-gists" class="table">
	    <thead><th>description</th><th>URL</th><th>created_at</th></thead>
	    <tbody></tbody>
	  </table>

	  <script>
		//backbone....
	  </script>
	</body>
	</html>


## 首先是M:

	//创建一个自定义模型类
	var Gist = Backbone.Model.extend({
      url: 'https://api.github.com/gists/public',

      //backbone提供了一个parse方法,它会在 fetch及 save 时执行。 
      //传入本函数的为原始 response 对象,这个方法提供传入两个参数，并返回第一个参数。
      parse:function(response){
          return (response[0])
      },

      //默认属性值设置，如果属性中没有website（注意不是website值为空），会设置website值为dmyz。
      defaults:{
        website:'dmyz'
      },

      //验证事件，当website值为dmyz时触发
      validate:function(attrs){
        if(attrs.website == 'dmyz'){
          return 'website error'
        }
      }
    })

    gist = new Gist(); //实例化模块

    gist.on("validate",function(model,error){
      alert(error)
    })

    //get    collection.get(id) 通过一个id，一个cid，或者传递一个model来获得集合中的模型。

    gist.on('change',function(model){   //绑定一个change事件
      var tbody = document.getElementById('js-id-gists').children[1],
          tr = document.getElementById(model.get('id'));  

      if(!tr){
        tr = document.createElement('tr');  
        tr.setAttribute('id',model.get('id')); 
      }

      tr.innerHTML="<td>"+model.get('description')+"</td><td>"+model.get('url')+"</td><td>"+model.get('created_at') + '</td>';
      tbody.appendChild(tr);
    })

    //从远程获取数据，获到数据后会触发change事件
    // gist.fetch();
    
    // 将数据存储到数据库
    gist.save()

整个过程:
1. 创建自定义模型类，并实例化
2. gist.fetch()从远程拉取数据，触发了change事件
3. 绑定在gist上的change事件执行（model是其数据模型），通过model.get()来获取数据，渲染到页面
4. 在本例中，可以看出model是数据模型，起到操作数据的作用，
---

## M+V:

    var Gist = Backbone.Model.extend({
      url: 'https://api.github.com/gists/public',
      parse:function(response){
          return (response[0])
      }
    })

    //实例化模块
    gist = new Gist();
     
    //创建一个自定义视图类
    var GistRow = Backbone.View.extend({
      //所有的视图都拥有一个 DOM 元素（el 属性），即使该元素仍未插入页面中去。 视图可以在任何时候渲染，然后一次性插入 DOM 中去，这样能尽量减少 reflows 和 repaints 从而获得高性能的 UI 渲染。

      el:'tbody',
      
      //指定视图的数据模型
      Model:gist,

	  //initialize为初始化函数，创建视图时，它会立刻被调用。
      //initialize函数在this上监听Model的change事件，发生改变时时触发render事件
      initialize:function(){
        this.listenTo(this.Model,'change',this.render);
      },

      //渲染视图
      render:function(){
        var model =this.Model,
            tr = document.createElement('tr');
        tr.innerHTML='<td>' + model.get('description') + '</td><td>' + model.get('url') + '</td><td>' + model.get('created_at') + "</td>";
        this.el.innerHTML = tr.outerHTML;
        // console.log(this.el)
        return this;
      }
    });
    
    //实例化GistRow,立即执行initialize函数
    var tr = new GistRow();
    gist.fetch();
    
整个过程：
1. 创建自定义模型类，并实例化
2. 创建自定义视图类，并实例化，指定模型
3. 立即执行initialize函数，监听model的change事件，
4. fetch()事件获取远程数据，触发change事件，执行render事件

---

## M+V+C:
	var Gist = Backbone.Model.extend();

	//创建一个自定义集合类（collection）
    var Gists = Backbone.Collection.extend({
      //指定集合（collection）中包含的模型类
      model:Gist,
      url:'https://api.github.com/gists/public',
      parse:function(response){
          return response;
      }
    })

    gists = new Gists(); 

    //创建一个视图类，其作用是渲染每一行的视图
    var GistRow = Backbone.View.extend({
      //tagName, className, 或 id 为视图指定根元素
      tagName:'tr',
      render:function(model){
        this.el.id = model.cid;
        this.el.innerHTML =  '<td>' + model.get('description') + '</td><td>'+ model.get('url') + '</td><td>' + model.get('created_at')+"</td>"
        return this;
      }
    })

    var GistView = Backbone.View.extend({
      el:'tbody',
	  // 指定集合类
      collection:gists,

      //初始化函数，实例化时立即执行
      initialize:function(){
        this.listenTo(this.collection,'reset',this.render);
      },

      //渲染视图
      render:function(){
        var html = '';
        //collection.models 访问集合中模型的内置的JavaScript 数组
        //遍历所有models，设置他们的html
        //下划线 _ 是underscore.js的一个全局对象
        _.forEach(this.collection.models,function(model){
          var tr = new GistRow();
          html += tr.render(model).el.outerHTML;
        })
        this.el.innerHTML = html;
        return this;
      }
    });
    
    //实例化GistRow,调用initialize函数
    var gistsView = new GistView();
    gists.fetch({reset:true});

整个过程：
1. 创建一个自定义模型类和集合类，实例化集合类
2. 创建俩个自定义视图类，一个渲染每行的数据，一个渲染整个页面
3. 实例化视图类GistView，在集合类上绑定监听事件
4. fetch()触发监听事件，执行render函数，遍历所有models
5. 在render函数中实例化视图类GistRow,，渲染每行

