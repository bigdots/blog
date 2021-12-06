

# 如何处理输入框的emoji表情

在前端开发中，特别是移动端，我们常常会遇到这个用户输入表情后，数据库无法保存甚至是报错的问题，今天我们就一起来研究一下这个问题的产生以及如何解决它。

## 为什么输入表情后，数据库无法存储？

我先把答案抛出： emoji用到的是4字节的utf-16编码，数据库是采用的utf-8编码，并且最大只允许3字节的字符。

至此，我们的问题变成首先要搞懂UTF-8和UTF-16编码了。



检测四字节的Unicode



## Unicode


## 编码






ASCII 码

UTF-8


UTF-16



[字符编码笔记：ASCII，Unicode 和 UTF-8](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)



web

使用utf-8


所以我们只要检测utf-16



4字节的utf-16编码范围为U+010000到U+10FFFF
/[\u010000-\u10FFFF]/g



javascript采用的是是ucs-2编码；所以正则需要再转化为ucs-2