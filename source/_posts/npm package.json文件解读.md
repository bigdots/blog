# npm package.json文件解读

每个Nodejs项目的根目录下面，一般都会有一个package.json文件。该文件可以由`npm init`生成，定义了项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。

package.json文件内部就是一个JSON对象，该对象的每一个成员就是当前项目的一项设置。

## 基本字段

1. name

	项目名称（npm包名）。必需字段
	
2. version

	项目版本。必需字段。
	版本号的格式为"1.0.0",分别代表“主版本号.次版本号.修订号”。它的递增规则如下： 
	- 主版本号：当你做了不兼容的API 修改；
	- 次版本号：当你做了向下兼容的功能性新增；
	- 修订号：当你做了向下兼容的问题修正。
	
	在实际使用中，一般可以看到各种形式的版本号：
	
	```
	*: 任意版本
	1.1.0: 指定版本
	~1.1.0: 1.1.0 <= 版本 < 1.2.0
	^1.1.0: 1.1.0 <= 版本 < 2.0.0
	latest：安装最新版本。
	```
	
	- `*`表示任意版本；
	- `~ `前缀表示，安装大于指定的这个版本，并且匹配到 x.y.z 中 z 最新的版本；
	- `^ `前缀在 ^0.y.z 时的表现和 ~0.y.z 是一样的，然而 ^1.y.z 的时候，就会匹配到 y 和 z 都是最新的版本；
	- 特殊的是，当版本号为 ^0.0.z 或者 ~0.0.z 的时候，考虑到 0.0.z 是一个不稳定版本， 所以它们都相当于 =0.0.z。
	
3. author && contributors

	项目作者以及贡献者。author是字符串形式的作者名，contributors是一个项目贡献者数组。
	
4. description && keywords

	项目描述和项目关键字。帮助人们在使用npm search时找到这个包。
	
5. license
	
	许可证。
	

## 功能性字段

1. scripts

	定义脚本命令。它的每一个属性，对应一段脚本。并且可以在命令行下使用npm run命令执行这段脚本。
	
	npm 脚本的原理非常简单。每当执行npm run，就会自动新建一个 Shell，在这个 Shell 里面执行指定的脚本命令。因此，只要是 Shell（一般是 Bash）可以运行的命令，就可以写在 npm 脚本里面。
	
	比较特别的是，npm run新建的这个 Shell，会将当前目录的node_modules/.bin子目录加入PATH变量，执行结束后，再将PATH变量恢复原样。
	
	更多可参考阮老师的 [npm scripts 使用指南](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)
	
2. dependencies  &&  devDependencies

	`npm install`在安装node模块时，有两种命令参数可以把它们的信息写入package.json文件： `–save`以及`–save-dev`。`–save`会把依赖包名称添加到package.json文件`dependencies`键下，`–save-dev`则添加到package.json文件`devDependencies`键下。

	`dependencies`字段指定了项目生产环境中需要的依赖，即正常运行该包时所依赖的模块，`devDependencies`指定项目开发所需要的依赖项，像一些进行单元测试之类的包，比如grunt-contrib-uglify，我们用它混淆js文件，它们不会被部署到生产环境。
	
	它们都指向一个对象。该对象的各个成员，分别由模块名和对应的版本要求组成，表示依赖的模块及其版本范围。
	
3. bin

	用来指定各个内部命令对应的可执行文件的位置。
	
	例如：
	
	```
	"bin": {
  		"someTool": "./bin/someTool.js"
	}
	```
	
	上面代码指定，someTool 命令对应的可执行文件为 bin 子目录下的 someTool.js
	
4. main

	指定包的入口程序文件。这个字段的默认值是模块根目录下面的index.js。
	
5. config

	用于向环境变量输出值。
	
	比如：
	
	```
	{
		// ...
		"config" : { "port" : "8080" },
	}
	```
	
	然后通过`process.env.npm_package_config_port`读取该值。


## 其他字段

1. engines

	指定node的工作版本
	
2. man

	指定当前模块的man文档的位置。
	
3. preferGlobal

	布尔类型值，表示当用户不将该模块安装为全局模块时（即不用–global参数），要不要显示警告。
	
	...

	