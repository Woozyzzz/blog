# HTTP

## 1 GET 和 POST 的区别有哪些？

**记忆题**

- 要区分理论上的区别和实践中的区别
  - 根据技术规格文档，GET 和 POST 最大的区别是语义；
  - 面试通常问实践过程的区别，因此需要了解浏览器和服务器对 GET 和 POST 的常见实现方法。

### 1.1 区别 1 幂等性

- 可以使用相同参数重复执行，并能获得相同结果。

1.  由于 GET 是读，POST 是写，所以 GET 是幂等的，POST 不是幂等的。
2.  由于 GET 是读，POST 是写，所以用浏览器打开网页会发送 GET 请求，想要发送 POST 请求需要 AJAX 或 form 标签。
3.  由于 GET 是读，POST 是写，所以 GET 打开的页面刷新是无害的，POST 打开的页面刷新需要确认。
4.  由于 GET 是读，POST 是写，所以 GET 结果会被缓存，POST 结果不会被缓存。
5.  由于 GET 是读，POST 是写，所以 GET 打开的页面可被书签收藏，POST 打开的不行。

### 1.2 区别 2 请求参数

1.  通常 GET 请求参数放在 URL 里，POST 请求数据放在 body（消息体）里。
2.  GET 比 POST 更不安全，因为参数直接暴露在 URL 上。（瞎扯 POST 也是明文）。
3.  GET 请求参数放在 URL 上有长度限制（浏览器限制），而 POST 放到 body 的长度限制可以在服务器配置。

### 1.3 区别 3 TCP packet

- GET 产生一个 TCP 数据包，POST 产生两个或两个以上的 TCP 数据包。

## 2 HTTP 缓存有哪些方案？

**记忆题**

|          |                                                                          缓存（强缓存）                                                                           |                                                     内容协商（弱缓存）                                                     |
| :------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------: |
| HTTP 1.1 |                                   Cache-Control: max-age=3600 （缓存一小时，一小时内重复访问不发请求）<br> Etag: ABC （特征值）                                   |        If-None-Match: ABC （过期后浏览器拿特征值问服务器）<br> 响应状态码：304 (别删继续用) 或 200 （删了返回新的）        |
| HTTP 1.0 | Expires: Wed, 21 Oct 2015 02:30:00 GMT (过期时间，但受用户设备时间影响会时间错乱)<br> Last-Modified: Wed, 21 Oct 2016 01:00:00 GMT （特征值，最后一次更新的时间） | If-Modified-Since: Wed, 21 Oct 2015 07:28:00 GMT （过期后浏览器那特征值问服务器，但精度太粗糙）<br> 响应状态码：304 或 200 |

- Pragama MDN 不推荐在 HTTP1.1 使用

## 3 HTTP 和 HTTPS 的区别有哪些？

**记忆题**

- HTTPS = HTTP + SSL/TLS （安全层）

1. HTTP 是明文传输的，不安全；HTTPS 是加密传输的，非常安全。
2. HTTP 使用 80 端口；HTTPS 使用 443 端口。
3. HTTP 较快；HTTPS 较慢。
4. HTTP 不需要证书；HTTPS 证书通常需要购买。

## 4 TCP 三次握手和四次挥手是什么？

**记忆题**

1. 建立 TCP 连接时 client 与 server 会经历三次握手
   1. 浏览器向服务器发送 TCP 数据：SYN(seq=x)
   2. 服务器向浏览器发送 TCP 数据：ACK(seq=x+1) SYN(seq=y)
   3. 浏览器向服务器发送 TCP 数据：ACK(seq=y+1)
2. 关闭 TCP 连接时 client 与 server 会经历四次挥手
   1. 浏览器向服务器发送 TCP 数据：FIN(seq=x)
   2. 服务器向浏览器发送 TCP 数据：ACK(seq=x+1)
   3. 服务器向浏览器发送 TCP 数据：FIN(seq=y)
   4. 浏览器向服务器发送 TCP 数据：ACK(seq=y+1)

- 为什么关闭连接时的服务器要向浏览器发送两次数据而不是一次
- 因为在这两步之间服务器还有数据要发送，不能提前发送 FIN

## 5 HTTP/1.1 和 HTTP/2 的区别有哪些？

**记忆题**

1. HTTP/2 使用了二进制传输，而且将 head 和 body 分成帧来传输；HTTP/1.1 是字符串传输。
2. HTTP/2 支持多路复用，即浏览器与服务器之间的几百个请求都可以用一个 TCP 连接；HTTP/1 不支持。
3. HTTP/2 可以压缩 head；HTTP/1.1 不支持。
4. HTTP/2 支持服务器推送；HTTP/1.1 不支持。

## 6 说说同源策略和跨域

**概念题**

1. 是什么：
   - 如果两个 URL 的协议、端口、域名都完全一致，那么这两个 URL 是同源的。
2. 怎么做：
   - 只要在浏览器里打开页面，就会默认遵守同源策略。
3. 优点：
   - 保护用户隐私、数据安全。
4. 缺点：
   - 很多时候，前端需要访问另一个域名的后端接口，会被浏览器阻止其获取响应。
5. 怎么解决缺点：使用跨域手段
   1. JSONP
      1. A 站点利用 script 标签可以跨域的特性，向 B 站点发送 GET 请求；
      2. B 站点后端改造 JS 文件的内容，将数据传进回调函数；
      3. A 站点通过回调函数拿到 B 站点的数据。
      4. 优点：支持 IE、改动量小
      5. 缺点：缺失用户认证、只能发送 GET 请求不支持 POST
   2. Nginx 代理 或 Node.js 代理
   3. CORS
      1. 如果需要附带身份信息，JS 中需要在 AJAX 设置 `xhr.withCredentials = true`
      2. 对于简单请求 GET HEAD POST，B 站点在响应头中添加 `Access-Control-Allow-Origin: http://A站点` 即可。
      3. 对于复杂请求 PATCH 等，B 站点需要在响应头中添加：

```
Access-Control-Allow-Origin: http://A站点
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: Content-Type
```

## 7 Session、Cookie、LocalStorage、SessionStorage 的区别

**区分题**

- Cookie 与 LocalStorage
  - 主要区别是 Cookie 会被发送到服务器，而 LocalStorage 不会
  - Cookie 通常最大 4kb，LocalStorage 可以用 5mb 甚至 10mb（各个浏览器不同）
  - 防止 LocalStorage 超出用 try catch
  - 存储 LocalStorage 时加上时间戳可以用来判断其是否过期
  - 现在前端几乎没有场景需要操作 Cookie
- LocalStorage 与 SessionStorage
  - LocalStorage 一般不会自动过期，除非用户手动清除
  - SessionStorage 在会话结束时过期（如：关闭浏览器后，具体由浏览器决定）
- Cookie 与 Session
  - Cookie 存在浏览器的文件里，Session 存在服务器的文件里
  - Session 是基于 Cookie 实现的，具体做法是将 SessionID 存在 Cookie 里
