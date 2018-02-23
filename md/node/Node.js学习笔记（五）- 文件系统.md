# Node.js学习笔记（五）: 文件系统（fs）


Node.js 提供一组类似 UNIX（POSIX）标准的文件操作API。 要使用
这个模块需要require("fs")。fs 模块中所有的操作都提供了异步的和 同步的两个版本。

```
const fs = require("fs")
```

异步形式的API总是将回调函数作为它的最后一个参数，并且回调函数第一个参数总是为异常保留，如果没有发生错误，则第一个参数为`null`或`undefined`。

使用同步形式的API时，任何异常都会立即抛出，您可以使用try/catch来处理异常或允许它们冒泡。

强烈鼓励大家使用这些API的异步版本，同步版本会阻塞整个进程。

异步删除文件：

```
fs.unlink('/tmp/hello', (err) => {
  if (err) throw err;
  console.log('successfully deleted /tmp/hello');
});
```
这里传入了一个回调函数，这个回调函数会在删除文件操作完成后执行。打印'successfully deleted /tmp/hello'；

同步删除文件：

```
fs.unlinkSync('/tmp/hello');
console.log('successfully deleted /tmp/hello');
```
这里是同步函数，所以不需要传回调函数，打印字符串的操作会在删除文件操作开始的同时进行（不管文件删除操作是否已经完成）。

fs 模块提供了文件信息获取，文件读写，关闭文件，截取文件，删除文件，创建目录，读取目录，删除目录等文件系统操作。


### API一览：

+ 打开文件 

	- fs.open(path,flags, [mode], [callback(err, fd)])
	- fs.openSync(path, flags, [mode]) fd)])+ 关闭文件 

	- fs.close(fd, [callback(err)]) 
	- fs.closeSync(fd)
+ 读取文件(文件描述符） 

	- fs.read(fd,buffer,offset,length,position,[callback(err, bytesRead, buffer)])
	- fs.readSync(fd, buffer, offset, length, position)
+ 写入文件(文件描述符)

	- fs.write(fd,buffer,offset,length,position,  [callback(err, bytesWritten, buffer)])
	- fs.writeSync(fd, buffer, offset, length, position)

+ 读取文件内容 

	- fs.readFile(filename,[encoding],[callba ck(err, data)])
	- fs.readFileSync(filename,[encoding])
+ 写入文件内容 

	- fs.writeFile(filename, data,[encoding], [callback(err)])
	- fs.writeFileSync(filename, data, [encoding])
	+ 删除文件 

	- fs.unlink(path, [callback(err)])
	- fs.unlinkSync(path)
	+ 创建目录 

	- fs.mkdir(path, [mode], [callback(err)])
	- fs.mkdirSync(path, [mode])
	+ 删除目录 

	- fs.rmdir(path, [callback(err)])
	- fs.rmdirSync(path)
	+ 读取目录 

	- fs.readdir(path, [callback(err, files)])
	- fs.readdirSync(path)
	+ 获取真实路  

	- fs.realpath(path, [callback(err, resolvedPath)])
	- fs.realpathSync(path)
	+ 更名

	- fs.rename(path1, path2, [callback(err)])
	- fs.renameSync(path1, path2)

+ 截断

	- fs.truncate(fd, len, [callback(err)])
	- fs.truncateSync(fd, len)
	 + 更改所有权

	- fs.chown(path, uid, gid, [callback(err)])
	- fs.chownSync(path, uid, gid)
	+ 更改所有权(文件描述符）

	- fs.fchown(fd, uid, gid, [callback(err)])
	- fs.fchownSync(fd, uid, gid)
	+ 更改所有权(不解析符链接) 

	- fs.lchown(path, uid, gid, [callback(err)]) 
	- fs.lchownSync(path, uid, gid)
	
+ 更改权限

	- fs.chmod(path, mode, [callback(err)])
	- fs.chmodSync(path, mode)
	+ 更改权限(文件描述符)  

	- fs.fchmod(fd, mode, [callback(err)])
	- fs.fchmodSync(fd, mode)+ 获取文件信息

	- fs.stat(path, [callback(err, stats)]) 
	- fs.statSync(path)

+ 获取文件信息(文件描述符） 

	- fs.fstat(fd, [callback(err, stats)])
	- fs.fstatSync(fd)+ 创建硬链接 

	- fs.link(srcpath, dstpath, [callback(err)]) 
	- fs.linkSync(srcpath, dstpath)
	
+ 创建符号链接 

	- fs.symlink(linkdata, path, [type],[callback(err)])
	- fs.symlinkSync(linkdata, path,[type])+ 读取链接 

	- fs.readlink(path, [callback(err, linkString)])
	- fs.readlinkSync(path)
	+ 修改文件时间戳  

	- fs.utimes(path, atime, mtime, [callback (err)])
	- fs.utimesSync(path, atime, mtime)
	+ 修改文件时间戳（文件描述符）  

	- fs.futimes(fd, atime, mtime, [callback(err)])
	- fs.futimesSync(fd, atime, mtime)
+ 同步磁盘缓存

	- fs.fsync(fd, [callback(err)])
	- fs.fsyncSync(fd)