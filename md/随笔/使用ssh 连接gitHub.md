# 使用ssh连接gitHub
github每次pull/push代码时要求推送代码的用户是合法的，所以每次推送时候都要输入账号密码用以验证用户是否为合法用户，而ssh是一种安全的传输模式，可以代替用户的这一“输入账号密码”的行为来验证用户。

##github的俩种操作方式
1. https

	可以随意克隆github上的项目，而不管是谁的；在pull/push的时候是需要验证用户名和密码的
2. ssh

	克隆者必须是拥者或管理员，且需要先添加 SSH key ，否则无法克隆。在pull/push的时候不再是验证用户名和密码，而是通过验证ssh的方式来验证用户。

## ssh（安全外壳协议）##
定义：
> SSH 为 Secure Shell 的缩写，由 IETF 的网络小组（Network Working Group）所制定；SSH 为建立在应用层和传输层基础上的安全协议。SSH 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。SSH最初是UNIX系统上的一个程序，后来又迅速扩展到其他操作平台。SSH在正确使用时可弥补网络中的漏洞。SSH客户端适用于多种平台。几乎所有UNIX平台—包括HP-UX、Linux、AIX、Solaris、Digital UNIX、Irix，以及其他平台，都可运行SSH。 --百度百科

功能： 

>传统的网络服务程序，如：ftp、pop和telnet在本质上都是不安全的，因为它们在网络上用明文传送口令和数据，别有用心的人非常容易就可以截获这些口令和数据。而且，这些服务程序的安全验证方式也是有其弱点的， 就是很容易受到“中间人”（man-in-the-middle）这种方式的攻击。所谓“中间人”的攻击方式， 就是“中间人”冒充真正的服务器接收你传给服务器的数据，然后再冒充你把数据传给真正的服务器。服务器和你之间的数据传送被“中间人”一转手做了手脚之后，就会出现很严重的问题。通过使用SSH，你可以把所有传输的数据进行加密，这样"中间人"这种攻击方式就不可能实现了，而且也能够防止DNS欺骗和IP欺骗。使用SSH，还有一个额外的好处就是传输的数据是经过压缩的，所以可以加快传输的速度。SSH有很多功能，它既可以代替Telnet，又可以为FTP、PoP、甚至为PPP提供一个安全的"通道"。 --百度百科

## 使用步骤
1. 查看是否已经存在ssh秘钥

	打开git bash，输入

		$ cd ~/.ssh
    	$ ls
如果，提示不存在此目录，则进行第二步操作，否则，你本机已经存在ssh公钥和私钥，可以略过第二步，直接进入第三步操作。


2. 生成ssh秘钥

		$ ssh-keygen -t rsa -C "your_email@example.com"

	代码参数含义：
	- -t 指定密钥类型，默认是 rsa ，可以省略。
	- -C 设置注释文字，比如邮箱。
	- -f 指定密钥文件存储文件名。
	
    根据提示，需要指定文件位置和密码，如果是你足够放心，其实都可以直接回车，不需要什么密码。执行完以后，可在/c/Users/you/.ssh/路径下看到刚生成的文件：id_rsa和id_rsa.pub。即公钥和私钥。
	
3. 在GitHub账户中添加公钥

	- 登录你的github，头像处下拉框选择settings。
	- 进入设置页后点击侧边栏的`SSH and GPG keys`按钮。
	- 点击`New SSH key`,title可以任意填，并且将上一步骤生成的id_rsa.pub的内容复制到这里的`key`输入框中。

4. 确认

		$ ssh -T git@github.com

	在这里我收到一个提示：

	*Warning: Permanently added the RSA host key for IP address '192.30.253.113' to the list of known hosts.*
	
    直接回车，最后看到这个就说明大功告成：
	
	*Hi username! You’ve successfully authenticated, but GitHub does not provide shell access.*

## 遇到的问题 ##

添加完公钥后，使用小乌龟（TortoiseGit）pull代码时报错：

*no supported authentication methods aviaible(server sent:publickey)*

查了一下，发现是因为TortoiseGit和Git的冲突 我们需要把TortoiseGit设置改正如下。

- 右键TortoiseGit -> Settings -> Network
- 将SSH client指向~\Git\usr\bin\ssh.exe(Git安装路径下)





