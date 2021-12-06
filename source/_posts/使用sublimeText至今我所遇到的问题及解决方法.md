---
title: 使用sublimeText至今我所遇到的问题及解决方法
date: 2015-05-04 22:13:01
tags: [web,工具]
description: sublimeText

---

### 1.汉化：
下载汉化包 、打开程序Preference下的浏览包文件夹、将解压的程序包粘贴进包文件夹

### 2.破解：标题栏上面有带（unregistered）表示还没有注册；
打开HELP→Enter license粘贴如下代码:
版本2：
<!-- more -->

	----- BEGIN LICENSE ----- 
	Andrew Weber 
	Single User License 
	EA7E-855605 
	813A03DD 5E4AD9E6 6C0EEB94 BC99798F 
	942194A6 02396E98 E62C9979 4BB979FE 
	91424C9D A45400BF F6747D88 2FB88078 
	90F5CC94 1CDC92DC 8457107A F151657B 
	1D22E383 A997F016 42397640 33F41CFC 
	E1D0AE85 A0BBD039 0E9C8D55 E1B89D5D 
	5CDB7036 E56DE1C0 EFCC0840 650CD3A6 
	B98FC99C 8FAC73EE D2B95564 DF450523 
	------ END LICENSE ------

版本3：

	—– BEGIN LICENSE —–
	Andrew Weber
	Single User License
	EA7E-855605
	813A03DD 5E4AD9E6 6C0EEB94 BC99798F
	942194A6 02396E98 E62C9979 4BB979FE
	91424C9D A45400BF F6747D88 2FB88078
	90F5CC94 1CDC92DC 8457107A F151657B
	1D22E383 A997F016 42397640 33F41CFC
	E1D0AE85 A0BBD039 0E9C8D55 E1B89D5D
	5CDB7036 E56DE1C0 EFCC0840 650CD3A6
	B98FC99C 8FAC73EE D2B95564 DF450523
	—— END LICENSE ——


### 3.安装包控制器(这个安装成功后就可以通过Preference-package control-Install package安装插件了)
首先按Ctrl+~打开控制台，输入以下代码：
版本2：

	import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print('Please restart Sublime Text to finish installation')

版本3：

	import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())

### 4.显示无法启动此程序，因为计算机丢失php_mbstring.dll：

修改php.ini配置。去掉“;extension=php_mbstring.dll”和“;extension=php_exif.dll”前的分号，并保证php_mbstring.dll在php_exif.dll之前加载，因为加载php_exif.dll需要用到php_mbstring.dll。然后保存php.ini，并重启web服务器。


### 5.弹出错误信息：
A plugin (SublimeCodeIntel) may be making Sublime Text unresponsive by taking too long (0.020000s) in its on_modified callback.

This message can be disabled via the detect_slow_plugins setting


解决方法：打开preference->setting_user
添加
“detect_slow_plugins”: false
这样以后就不会弹出类似提示了


### 6.弹出错误信息：
Error trying to parse settings: No data in ~/Library/Application Support/Sublime Text 2/Packages/User/JavaScript.sublime-settings:1:1


Stack Overflow的回答是：Most likely you or something has created an empty file in your User config directory.

The config files must be valid JSON. The file in the question is empty and is not JSON.

Try deleting the file or get a fixed version from somewhere (not sure for what the file is being used for)

最有可能你或者在您的用户配置目录中创建一个空文件。

配置文件必须是有效的JSON。 问题文件是空的,不是JSON。

试着从某个地方删除文件或得到一个固定的版本

---

<p style="text-align:right">整理于2015-11-27 16:28:24</p>