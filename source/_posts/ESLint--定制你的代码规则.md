# ESLint--定制你的代码规则

## 简介
ESLint是一个开源的项目，由Nicholas C. Zakas（《JavaScript高级程序设计》作者）于2013年六月创建。它的目标是为JavaScript提供一个完全可配置的实用lint工具。

JavaScript是一种动态的、松散型的语言，是特别容易受到开发人员的错误使用。而ESLint可以在不执行JavaScript代码的情况下发现代码的问题。

ESLint由Node.js编写，通过NPM提供快速的运行环境，并且安装方便。

## 安装
1. 全局安装

	```
	$ npm install -g eslint
	```
2. 本地安装

	```
	$ npm install eslint --save-dev
	```

## 使用

### 生成配置文件
在你想要使用ESLint的工程根目录下执行：

```
eslint --init
```
这个命令的目的是创建一个eslint配置文件。如果你是全局安装的eslint，那么可以在任意文件中使用该命令，否则，你必须在使用该命令之前在该项目中安装eslint。

执行该命令后，一般会出现三个选项可供选择，他们分别是：

```
❯ Answer questions about your style
  Use a popular style guide
  Inspect your JavaScript file(s)
```
+ 通过询问你来定制你的配置文件；
+ 使用通用的配置文件；
+ 通过审查你写的JavaScript文件来生成一个配置文件；

在这之后， 在你的目录中会有一个.eslintrc文件，这个.eslintrc的存在形式也是可选择的，它可以是JavaScript、YAML、JSON、package.json等等。

### 配置文件
生成配置文件之后，打开.eslintrc文件（一般是隐藏的），可以看到以下格式的内容：

```
{
    "env": {
        "browser": true
    },
    "extends": "eslint:recommended",
    "rules": {
        "indent": [
            "error",
            "tab"
        ],
        "linebreak-style": [
            "error",
            "unix"
        ],
        "quotes": [
            "error",
            "double"
        ],
        "semi": [
            "error",
            "always"
        ]
    },
    Globals: {
    }
}
```

我这个是json格式的，这里解释一下这个文件里各个参数的意思：

1. env ：指定你的js代码在哪个运行环境中检测（每个运行环境都有一组预定义的全局变量）；

2. extends ：扩展配置规则（），我这里扩展的是eslint的推荐规则；

3. rules ：指定检测规则；

	这是最重要的部分，也是你的自定义js代码监测规则的地方，他的格式是：规则名: 规则。比如：
	```
	"indent": ["error","tab"]
	```
	这里`indent`就是规则名，它定义了缩进应该使用tab，规则内的第一个值`error`指的是错误等级，它有三个等级，分别是：

	| error level | 数值表示   | 涵义       |
	| ----------- |:---------:| ---------:|
	| error       | 2         | 作为错误    |
	| warn        | 1         | 作为提醒    |
	| off         | 0         | 关闭该规则  |
更多的规则可以参考[官网](http://eslint.org/docs/rules/)的rules。

4. Globals ：指定脚本执行过程中访问的附加全局变量（比如jquery）

**PS:**

您可以配置全局的.eslint文件，而不至于要每个工程中都创建一个独有的.eslint文件，烹制方法是将您的.eslintr文件放在当前用户的根目录下，类Unix系统的当前用户目录是`~`，而Windows系统的话则是类似于`C:\Windows\Users\Username`这样的地方

### 检测文件
在你的工程目录下执行：

```
eslint yourfile.js
```

它会在命令后输出你的所有报错信息。这样就ok了。个人感觉它的最大优势就是完全可配置，而且配置文件一次构建，可以通过粘贴复制的方式无数次使用。甚至整个团队可以通过使用一份配置文件来达到规范代码的作用，还是很强大的。

## 使用现有的通用规则
eslint官方提供了3种预安装包：

1. [eslint-config-google](https://www.npmjs.com/package/eslint-config-google)

	Google标准
	
	执行安装：
	
	```
	npm install eslint eslint-config-google -g
	```
	
2. [eslint-config-airbnb](https://www.npmjs.com/package/eslint-config-airbnb)

	Airbnb标准,它依赖eslint, eslint-plugin-import, eslint-plugin-react, and eslint-plugin-jsx-a11y等插件，并且对各个插件的版本有所要求。
	
	你可以执行以下命令查看所依赖的各个版本：

	```
	npm info "eslint-config-airbnb@latest" peerDependencies
	```
	你会看到以下输出信息，包含每个了每个plugins的版本要求
	
	```
	{ eslint: '^3.15.0',
	  'eslint-plugin-jsx-a11y': '^3.0.2 || ^4.0.0',
	  'eslint-plugin-import': '^2.2.0',
	  'eslint-plugin-react': '^6.9.0' }
	```
	知道了每个plugins的版本要求后，代入以下命令执行安装即可使用：
	
	```
	npm install eslint-config-airbnb eslint@^#.#.# eslint-plugin-jsx-a11y@^#.#.# eslint-plugin-import@^#.#.# eslint-plugin-react@^#.#.# -g
	```
	
3. [eslint-config-standard](https://www.npmjs.com/package/eslint-config-standard)

	Standard标准，它是一些前端工程师自定的标准。
	
	执行安装：
	
	```
	npm install eslint-config-standard eslint-plugin-standard eslint-plugin-promise -g
	```

目前来看，公认的最好的标准是Airbnb标准。建议全局安装这些标准，然后在你的.eslint配置文件中直接使用：

```
{
  "extends": "Airbnb/standard/Google/"
}

```

