# 《图解HTTP》

## 第 1 章 了解 Web 及网络基础
### 1.3.3 TCP/IP 通信传输流
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_tcp_1.png)
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_tcp_2.png)

### 1.4 与 HTTP 关系密切的协议 : IP、TCP 和DNS
#### 1.4.1 负责传输的 IP 协议
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_tcp_3.png)

#### 1.4.2 确保可靠性的 TCP 协议
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_tcp_4.png)

### 1.5 负责域名解析的 DNS 服务
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_dns.png)

### 1.6 各种协议与 HTTP 协议的关系
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_tcp_5.png)

## 第 2 章 简单的 HTTP 协议
### 2.7 持久连接节省通信量
#### 2.7.1 持久连接
HTTP keep-alive 或 HTTP connection reuse
持久连接的特点是， 只要任意一端没有明确提出断开连接， 则保持 TCP 连接状态
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_http_1.png)
#### 2.7.2 管线化
持久连接使得多数请求以管线化（pipelining） 方式发送成为可能。 从前发送请求后需等待并收到响应， 才能发送下一个请求。 管线化技术出现后， 不用等待响应亦可直接发送下一个请求。
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_http_2.png)

## 第 3 章 HTTP 报文内的 HTTP信息

## 3.1 HTTP 报文
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_msg_1.png)

## 3.2 请求报文及响应报文的结构
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_msg_2.png)
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_msg_3.png)

## 3.3 编码提升传输速率
### 3.3.1 报文主体和实体主体的差异
 
 - 报文（message）
	 - 是 HTTP 通信中的基本单位， 由 8 位组字节流（octet sequence，其中 octet 为 8 个比特） 组成， 通过 HTTP 通信传输。
 - 实体（entity）
	 - 作为请求或响应的有效载荷数据（补充项） 被传输， 其内容由实体首部和实体主体组成。

### 3.3.2 压缩传输的内容编码
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_msg_4.png)

常用的内容编码:

 - gzip（GNU zip）
 - compress（UNIX 系统的标准压缩）
 - deflate（zlib）
 - identity（不进行编码）

### 3.3.3 分割发送的分块传输编码
在 HTTP 通信过程中， 请求的编码实体资源尚未全部传输完成之前，浏览器无法显示请求页面。 在传输大容量数据时， 通过把数据分割成多块， 能够让浏览器逐步显示页面。这种把实体主体分块的功能称为分块传输编码（Chunked Transfer Coding） 。
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_msg_5.png)

## 3.4 发送多种数据的多部分对象集合
发送邮件时， 我们可以在邮件里写入文字并添加多份附件。 这是因为采用了 MIME（Multipurpose Internet Mail Extensions， 多用途因特网邮件扩展） 机制， 它允许邮件处理文本、 图片、 视频等多个不同类型的数据。 例如， 图片等二进制数据以 ASCII 码字符串编码的方式指明，就是利用 MIME 来描述标记数据类型。 而在 MIME 扩展中会使用一种称为多部分对象集合（Multipart） 的方法， 来容纳多份不同类型的数据。
相应地， HTTP 协议中也采纳了多部分对象集合， 发送的一份报文主体内可含有多类型实体。 通常是在图片或文本文件等上传时使用。

 - multipart/form-data
	 - 在 Web 表单文件上传时使用。
 - multipart/byteranges
	 - 状态码 206（Partial Content， 部分内容） 响应报文包含了多个范围的内容时使用。



## 第 4 章 返回结果的 HTTP 状态码
[https://tools.ietf.org/html/rfc7231#page-47](https://tools.ietf.org/html/rfc7231#page-47)

## 第 5 章 与 HTTP 协作的 Web 服务器
### 5.1 用单台虚拟主机实现多个域名
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_server_1.png)
### 5.2 通信数据转发程序 ： 代理、 网关、 隧道

 - 代理
	 - 代理是一种有转发功能的应用程序， 它扮演了位于服务器和客户端“中间人”的角色， 接收由客户端发送的请求并转发给服务器， 同时也接收服务器返回的响应并转发给客户端。
 - 网关
	 - 网关是转发其他服务器通信数据的服务器， 接收从客户端发送来的请求时， 它就像自己拥有资源的源服务器一样对请求进行处理。 有时客户端可能都不会察觉， 自己的通信目标是一个网关。
 - 隧道
	 - 隧道是在相隔甚远的客户端和服务器两者之间进行中转， 并保持双方通信连接的应用程序。

![](http://7xkcnd.com1.z0.glb.clouddn.com/http_server_2.png)
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_server_3.png)
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_server_4.png)

### 5.3 保存资源的缓存
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_server_5.png)

## 第 6 章 HTTP 首部
[https://tools.ietf.org/html/rfc7231#page-33](https://tools.ietf.org/html/rfc7231#page-33)
[https://tools.ietf.org/html/rfc7231#page-33](https://tools.ietf.org/html/rfc7232#page-5)
[https://tools.ietf.org/html/rfc7235#page-7](https://tools.ietf.org/html/rfc7235#page-7)

## 第 7 章 确保 Web 安全的HTTPS
[http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html](http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html)

### 7.2 HTTP+ 加密 + 认证 + 完整性保护 = HTTPS
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_https_1.png)
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_https_2.png)
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_https_3.png)
![](http://7xkcnd.com1.z0.glb.clouddn.com/http_https_4.png)

## 第 8 章 确认访问用户身份的认证
### HTTP 使用的认证方式
 - HTTP/1.1 使用的认证方式如下所示。
 - BASIC 认证（基本认证）
 - DIGEST 认证（摘要认证）
 - SSL 客户端认证：银行
 - FormBase 认证（基于表单认证）
	 - session
	 - hash salt

## 第 9 章 基于 HTTP 的功能追加协议

 - spdy
 - websocket
 - HTTP 2.0
 - webDAV


## 第 10 章 构建 Web 内容的技术

## 第 11 章 Web 的攻击技术
### 11.2 因输出值转义不完全引发的安全漏洞

 - 客户端的验证
 - Web 应用端（服务器端） 的验证
	 - 输入值验证
	 - 输出值转义

#### 11.2.1 跨站脚本攻击(XSS)
编码用户的输入 
http-only cookie

 - 利用虚假输入表单骗取用户个人信息。
 - 利用脚本窃取用户的 Cookie 值， 被害者在不知情的情况下，帮助攻击者发送恶意请求。
 - 显示伪造的文章或图片。

其他：
 - SQL 注入攻击
 - OS 命令注入攻击
 - 邮件首部注入攻击
 - 目录遍历攻击

![](http://7xkcnd.com1.z0.glb.clouddn.com/http_safe_1.png)

### 11.3 因设置或设计上的缺陷引发的安全漏洞
#### 11.3.1 强制浏览
强制浏览（Forced Browsing） 安全漏洞是指， 从安置在 Web 服务器的公开目录下的文件中， 浏览那些原本非自愿公开的文件。

 - 泄露顾客的个人信息等重要情报
 - 泄露原本需要具有访问权限的用户才可查阅的信息内容
 - 泄露未外连到外界的文件

#### 11.3.2 不正确的错误消息处
Web 应用抛出的错误消息
数据库等系统抛出的错误消息

### 11.3.3 开放重定向
钓鱼攻击的跳板

### 11.4 因会话管理疏忽引发的安全漏洞
会话劫持
会话固定攻击

#### 11.4.3 跨站点请求伪造(CSRF)
跨站点请求伪造（Cross-Site Request Forgeries， CSRF） 攻击是指攻击者通过设置好的陷阱， 强制对已完成认证的用户进行非预期的个人信息或设定信息等某些状态更新， 属于被动攻击。
跨站点请求伪造有可能会造成以下等影响。
 - 利用已通过认证的用户权限更新设定信息等
 - 利用已通过认证的用户权限购买商品
 - 利用已通过认证的用户权限在留言板上发表言论

![](http://7xkcnd.com1.z0.glb.clouddn.com/http_safe_2.png)

### 11.5 其他安全漏洞

 - 密码破解
	 - 穷举法
	 - 字典攻击
	 - 彩虹表
 - 点击劫持
 - DoS 攻击
 - 后门程序