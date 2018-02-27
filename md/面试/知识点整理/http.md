## 分层

1. 应用层

    http／ssl／DNS

2. 传输层

    TCP

3. 网络层
    
    IP

4. 数据链路层
    
    硬件部分。

## TCP

### 三次握手

1. 客户端发送连接请求SYN报文段；

2. 服务器收到客户端的SYN报文段，并将SYN+ACK报文段发送回客户端

3. 客户端收到服务器的SYN+ACK报文段，向服务器发送ACK报文段


类似于：

1. C：喂，听得到吗？
2. S：我听的到，你听得到吗？
3. C：嗯嗯，我听得到


为什么要三次？

为了验证服务端和客户端都具备发送和接收信息包的能力


四次挥手

1. A: 我没话讲了，你挂电话吧
2. B: 好吧，我听到了你说要挂电话
3. B: 那我挂了啊？
4. A:挂吧

B会关闭连接，A在B关闭后，一段时间收不到应答也会关闭。


为什么需要四次？

因为 挥手= 断开连接+数据；即在2，3之间，B还可以说他没说完的话，确保自己的数据发送完毕



## HTTPS

弥补http的缺点

1. 通信使用明文(不加密)
2. 不进行身份验证
3. 无法保证报文完整性



HTTP+ 加密 + 认证 + 完整性保护 =HTTPS (HTTP Secure)


### 加密

1. 共享秘钥加密（对称加密）

2. 公共秘钥加密


### 认证

数字证书CAD

## 缓存

![image](https://raw.githubusercontent.com/bigdots/blog/master/images/201801/http%E7%BC%93%E5%AD%98.png)


### HTTP1.0

1. Expires

    启用缓存和定义缓存
    
    value为GMT（格林尼治时间），定义源缓存过期时间，-1则表示立即过期
    
    允许客户端在这个时间之前不去检查（发请求），等同max-age的效果。但是如果同时存在，则被Cache-Control的max-age覆盖。
    
2. Pragma

    禁用缓存，优先级高于 Expires。

### HTTP1.1

HTTP 1.1支持**长连接（PersistentConnection）**和请求的**流水线（Pipelining）**

1. Cache-Control

请求首部： 

```cmd
"Cache-Control" ":" no-cache / no-store / max-age
```

响应首部：

```cmd
"Cache-Control" ":" no-cache/ max-age / no-store / public / private[] 
```

一般使用：

服务器响应头中国使用，并根据 max-age 决定储存在浏览器的时间，过期后删除。浏览器再次请求时，有缓存读缓存，无缓存直接请求。

2. Last-Modified
    
    服务器将资源最后更改的时间以“Last-Modified: GMT”的形式加在实体首部上一起返回给客户端。

    ```cmd
    If-Modified-Since: Last-Modified-value
    ```
    
    ```cdm
     If-Unmodified-Since: Last-Modified-value
     ```

3. ETag
    
    给资源计算得出一个唯一标志符（比如md5标志）
    
    ```cmd
    If-None-Match: ETag-value
    ```
    
    ```cmd
    If-Match: ETag-value
    ```







Expires 和 max-age
都可以用来指定文档的过期时间，但是二者有一些细微差别
1. Expires在HTTP/1.0中已经定义，Cache-Control:max-age在HTTP/1.1中才有定义，为了向下兼容，仅使用max-age不够
2. Expires指定一个绝对的过期时间(GMT格式),这么做会导致至少2个问题：
    - 客户端和服务器时间不同步导致Expires的配置出现问题。 
    - 很容易在配置后忘记具体的过期时间，导致过期来临出现浪涌现象
3. max-age 指定的是从文档被访问后的存活时间，这个时间是个相对值(比如:3600s)，相对的是文档第一次被请求时服务器记录的Request_time(请求时间)
4. Expires 指定的时间可以是相对文件的最后访问时间(Atime)或者修改时间(MTime)，而max-age相对的是文档的请求时间(Atime)
5. 如果值为 no-cache,那么每次都会访问服务器。如果值为max-age，则在过期之前不会重复访问服务器。

## 网络安全

1. XSS

    (Cross Site Scripting) 跨站脚本攻击。指攻击者在网页中嵌入客户端脚本(js),当用户浏览此网页时，脚本就会在用户的浏览器上执行，从而达到攻击者的目的.  

2. sql注入

    攻击者把SQL命令插入到Web表单的输入域或页面请求的查询字符串，欺骗服务器执行恶意的SQL命令。
    
3. CSRF：
    Cross-site request forgery）跨站请求伪造。它的核心也就是请求伪造，通过伪造身份（利用cookie）提交请求来进行跨域的攻击。

    完成CSRF需要两个步骤：
    - 登陆受信任的网站A，在本地生成 COOKIE
    - 在不登出A的情况下，或者本地 COOKIE 没有过期的情况下，访问危险网站B。
    
    
# WebSocket


WebSocket是HTML5开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

```js
var ws = new WebSocket("ws://localhost:9998/echo");
ws.onopen = function(){
  // Web Socket 已连接上，使用 send() 方法发送数据
  ws.send("发送数据");
  alert("数据发送中...");
};

ws.onmessage = function (evt) 
{ 
  var received_msg = evt.data;
  alert("数据已接收...");
};

ws.onclose = function()
{ 
  // 关闭 websocket
  alert("连接已关闭..."); 
};
}

```


http存在自身的弊端：通信只能由客户端发起。

有些场景需要全双工通讯，最典型的就是聊天室。


解决方法：

1. Flash XMLSocket

2. ajax轮询

    顾名思义，客户端隔个几秒就发送一次请求，询问服务器是否有新信息。

3. long poll 长轮询
    采用轮询的方式，不过采取的是阻塞模型。客户端每一次请求，服务端暂时不返回，直到有消息才返回，占用服务端线程资源。包括iframe的隐藏script标签 、 基于长轮询的 XHR
    
4. WebSocket

    全双工通讯，先建立TCP连接，连接成功后才能相互通信
    
    实现原理：
    跟Nginx等服务器建立持久连接，后台有消息就通知nginx，没消息
    
    
## HTTP 2.0

[阅读](http://blog.csdn.net/g6U8W7p06dCO99fQ3/article/details/78906348)
    