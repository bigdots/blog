---
title: github+hexo构建静态博客（二）
date: 2015-11-23 18:46:49
description: 静态建站 hexo github 免费建站 自己的博客
tags: [其他]
categories: [js]
---

## 一、安装Hexo

在本机全局安装hexo
``` bash
$ npm install –g hexo
```
创建并初始化blog、文件
``` bash
$ hexo init blog   
```

进入blog文件夹
``` bash
$ cd blog    
```
<!-- more -->
安装npm模块
``` bash
$ npm install   
```
启动服务
``` bash
$ hexo server   
```

#### 目录结构
	_config.yml       配置文件
	node_modules      模块
	public            执行hexo -g后生成的最终文件
	scaffolds		  文章模版格式
	source  		  新文章生成，以及资源文件存放处
	themes			  主题
	db.json     
	package.json  
	README.md  

#### 配置文件 blog/_config.yml
	# Hexo Configuration
	## Docs: http://hexo.io/docs/configuration.html
	## Source: https://github.com/hexojs/hexo/

	# Site 站点
	title: bigdots
	subtitle:
	description:
	author: Brand
	language:
	timezone:

	# URL 设置根路径和网站地址
	## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
	url: http://bigdots.github.io
	root: /
	permalink: :year/:month/:day/:title/
	permalink_defaults:

	# Directory 设置各个文件路径地址
	source_dir: source
	public_dir: public
	tag_dir: tags
	archive_dir: archives
	category_dir: categories
	code_dir: downloads/code
	i18n_dir: :lang
	skip_render:

	# Writing 
	new_post_name: :title.md # File name of new posts
	default_layout: post
	titlecase: false # Transform title into titlecase
	external_link: true # Open external links in new tab
	filename_case: 0
	render_drafts: false
	post_asset_folder: false
	relative_link: false
	future: true
	highlight:
	  enable: true
	  line_number: true
	  auto_detect: true
	  tab_replace:

	# Category & Tag  分类和标签
	default_category: uncategorized
	category_map:
	tag_map:

	# Date / Time format  时间格式
	## Hexo uses Moment.js to parse and display date
	## You can customize the date format as defined in
	## http://momentjs.com/docs/#/displaying/format/
	date_format: YYYY-MM-DD
	time_format: HH:mm:ss

	# Pagination
	## Set per_page to 0 to disable pagination
	per_page: 10
	pagination_dir: page

	# Extensions  主题和插件
	## Plugins: http://hexo.io/plugins/
	## Themes: http://hexo.io/themes/
	theme: yilia

	# Deployment  部署
	## Docs: http://hexo.io/docs/deployment.html
	deploy: 
	  type: git
	  repository: git@github.com:bigdots/bigdots.github.io.git
	  branch: master

## 二、更换主题
*我这里使用的是[litten](http://litten.github.io/)的[yilia](https://github.com/litten/hexo-theme-yilia)主题，我们要对所有分享的人心怀感恩，没有他们就没有宇宙发展的今天!*

#### 安装
``` bash
$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```

修改blog根目录中的配置文件中的theme参数

#### 配置文件 blog/themes/yilia/_config.yml
	# Header
	menu:
	  主页: /
	  所有文章: /archives
	  随笔: /tags/随笔

	 # SubNav
	subnav:
	  github: https://github.com/bigdots
	  weibo: "#"
	  rss: "#"
	  zhihu: "#"
	  #douban: "#"
	  #mail: "#"
	  #facebook: "#"
	  #google: "#"
	  #twitter: "#"
	  #linkedin: "#"

	 rss: /atom.xml

	 # Content
	excerpt_link: more
	fancybox: true
	mathjax: true

	 #是否开启动画效果
	animate: true

	 #是否在新窗口打开链接
	open_in_new: false

	 #Miscellaneous 谷歌统计，考虑到谷歌被强我用的是百度统计
	google_analytics: ''
	favicon: /img/favicon.ico

	 #你的头像url  ‘/’为根目录
	avatar: /img/thumb.jpg
	#是否开启分享
	share: true
	#是否开启多说评论，填写你在多说申请的项目名称 duoshuo: duoshuo-key
	#若使用disqus，请在博客config文件中填写disqus_shortname，并关闭多说评论
	duoshuo: true
	#是否开启云标签
	tagcloud: true

	 #是否开启友情链接
	#不开启——
	#friends: false
	#开启——
	#friends:
	#奥巴马的博客: http://localhost:4000/
	#卡卡的美丽传说: http://localhost:4000/
	#本泽马的博客: http://localhost:4000/
	#吉格斯的博客: http://localhost:4000/
	#习大大大不同: http://localhost:4000/
	#托蒂的博客: http://localhost:4000/

	 #是否开启“关于我”。
	#不开启——
	#aboutme: false
	#开启——
	aboutme: 我是谁，我从哪里来，我到哪里去？我就是我，是颜色不一样的吃货…

## 最后一役

#### 1.修改配置文件中的Deployment一项
	# Deployment  
	deploy: 
	  type: git
	  repository: yoursshsite
	  branch: master

其中repository参数的值直接在github上复制即可，如图。
![](/images/201511/7.png)

#### 2.清除github上篇所建仓库的原文件
由于上篇（一）选过了主题，所以我们得先清除其原文件，将文件clone到本地，删除除readme之外的所有文件，然后将修改提交到github。

#### 3.部署
在blog根目录下执行：
`生成public文件`
``` bash
$ hexo g   
```
`部署到github`
``` bash
$ hexo d  
```

但我这里用git执行上面的命令行报了一个下面的错误
``` bash
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

最后我是用git shell来执行上诉命令行成功部署的。据网上说产生上面问题的原因是git版本过高，愿意的可以试试git1.9。

---
<b>That'all</b>