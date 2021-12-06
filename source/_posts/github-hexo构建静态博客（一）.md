---
title: github+hexo构建静态博客（一）
date: 2015-11-23 17:40:57
description: 静态建站 hexo github 免费建站 自己的博客
tags: [其他]
categories: [js]
---

## 一、GithubPages

#### 1.申请[github](https://github.com/)账号

#### 2.点击 new repository，建立一个仓库

![](/images/201511/1.png)

<!-- more -->

#### 3.输入文件名。必须是username.github.io的形式

![](/images/201511/2.png)

#### 4.成功以后进入该项目，点击右侧的seetings

![](/images/201511/3.png)

#### 5.点击Launch  automatic page generator

![](/images/201511/4.png)

#### 6.一直点击continue直至选择主题结束

完成后你就可以通过username.github.io来访问你的githubPages了

## 二、SSH设置

``` bash
$ ssh-keygen -t rsa -C "自己邮箱"    
```
(C一定要大写，接下来会有三次需要内容输入)
第一次：设置ssh生成位置，可以直接Enter；
第二次：输入密码；可以直接Enter；密码为空
第三次：确认密码；


完成！打开生成位置即可查看.ssh文件。也可再通过命令行验证是否成功，输入
$ ssh git@github.com

![](/images/201511/5.png)
显示如上则说明成功

---
接下来，打开github。进入settings页面，点击Deploy keys，
![](/images/201511/6.png)
点击add deploy key ,将.ssh文件中生成的id_rsa.pub中的内容复制粘贴到key框中；
至此，github部分就OK了


最后，使用ssh将git链接到远程仓库
``` bash
$ssh -T git@github.com   
```

输入就大功告成了！呼~



<p style="text-align:right;color:#f9a037">to be continued......</p>