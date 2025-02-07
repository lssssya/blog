
###  状态码分类

- 1xx - 服务器收到请求。在`HTTP`升级为`WebSocket`的时候，如果服务器同意变更，就会发送状态码 101
- 2xx - 请求成功，如 200。
- 3xx - 重定向，如 302。
- 4xx - 客户端错误，如 404。
- 5xx - 服务端错误，如 500。

### 常见状态码

- 200 - 成功。
- 301 - 永久重定向（配合 location，浏览器自动处理）。
- 302 - 临时重定向（配合 location，浏览器自动处理）。
- 304 - 资源未被修改。
- 403 - 没权限。
- 404 - 资源未找到。
- 500 - 服务器错误。
- 504 - 网关超时。


### GET 和 POST 的区别

- 从**缓存**的角度，GET 请求会被浏览器主动缓存下来，留下历史记录，而 POST 默认不会。
- 从**编码**的角度，GET 只能进行 URL 编码，只能接收 ASCII 字符，而 POST 没有限制。
- 从**参数**的角度，GET 一般放在 URL 中，因此不安全，POST 放在请求体中，更适合传输敏感信息。
- 从**幂等性**的角度，GET 是幂等的，而 POST 不是。(幂等表示执行相同的操作，结果也是相同的)
- 从 **TCP** 的角度，GET 请求会把请求报文一次性发出去，而 POST 会分为两个 TCP 数据包，首先发 header 部分，如果服务器响应 100(continue)， 然后发 body 部分。(火狐浏览器除外，它的 POST 请求只发一个 TCP 包)

### HTTP 报文结构是怎样的

header + body 

起始行 + 头部 + 空行 + 实体

起始行包括 **方法 + 路径 + http版本** 、 **http版本 + 状态码 + 原因**
GET /home HTTP/1.1
HTTP/1.1 200 OK


### HTTP 的特点？HTTP 有哪些缺点？

特点
- 可靠传输。HTTP 基于 TCP/IP
- 灵活可扩展，传输多种数据
- 请求-应答
- 无状态的
缺点
- 无状态
- 明文传输
- 队头阻塞问题

关于无状态：服务器不会记忆http的状态，不需要额外的资源记录状态信息，减轻负担。但是无状态的话有关联性的操作就会比较麻烦，比如用户信息每次都要在请求中带上。用cookie，第一次带上了cookie，后面都带上服务器就认识了

### HTTP 如何处理大文件的传输

- 分块传输编码：在 HTTP/1.1 中，可以使用分块传输编码来处理未知大小的数据流。这种方法允许服务器在不知道整个响应大小的情况下逐块发送数据。客户端会接收到每一块数据，并在最后一块数据的长度为 0 时知道传输结束。（`Transfer-Encoding: chunked`）
- 流式传输：对于非常大的文件（如视频流媒体），可以使用流式传输技术。在这种情况下，服务器会一边读取一边发送文件，而不是等待整个文件加载完毕再发送。客户端可以一边接收一边处理数据，从而减少等待时间。（`Content-Length: （文件长度）`）
- 多部分下载：允许客户端请求资源的一部分。这在大文件传输时非常有用，尤其是当传输中断时，可以从中断处继续传输，而不需要重新开始。

```
GET /largefile HTTP/1.1
Host: www.example.com
Range: bytes=0-99999

HTTP/1.1 206 Partial Content
Content-Range: bytes 0-99999/123456789
Content-Length: 100000
```

- 文件压缩：（`Content-Encoding: gzip`）

### HTTP 中如何处理表单数据的提交

在 http 中，有两种主要的表单提交的方式，体现在两种不同的`Content-Type`取值:

- application/x-www-form-urlencoded
- multipart/form-data

由于表单提交一般是`POST`请求，很少考虑`GET`，因此这里我们将默认提交的数据放在请求体中。


对于`application/x-www-form-urlencoded`格式的表单内容，有以下特点:

- 其中的数据会被编码成以`&`分隔的键值对
- 字符以**URL编码方式**编码。

```
// 转换过程: {a: 1, b: 2} -> a=1&b=2 -> 如下(最终形式) 
"a%3D1%26b%3D2"
```

对于`multipart/form-data`而言:

- 请求头中的`Content-Type`字段会包含`boundary`，且`boundary`的值有浏览器默认指定。例: `Content-Type: multipart/form-data;boundary=----WebkitFormBoundaryRRJKeWfHPGrS4LKe`。
- 数据会分为多个部分，每两个部分之间通过分隔符来分隔，每部分表述均有 HTTP 头部描述子包体，如`Content-Type`，在最后的分隔符会加上`--`表示结束。

相应的`请求体`是下面这样:
```
Content-Disposition: form-data;name="data1";
Content-Type: text/plain 
data1 
----WebkitFormBoundaryRRJKeWfHPGrS4LKe 
Content-Disposition: form-data;name="data2"; 
Content-Type: text/plain 
data2 
----WebkitFormBoundaryRRJKeWfHPGrS4LKe--
```
  
值得一提的是，`multipart/form-data` 格式最大的特点在于:**每一个表单元素都是独立的资源表述**。另外，你可能在写业务的过程中，并没有注意到其中还有`boundary`的存在，如果你打开抓包工具，确实可以看到不同的表单元素被拆分开了，之所以在平时感觉不到，是以为浏览器和 HTTP 给你封装了这一系列操作。

而且，在实际的场景中，对于图片等文件的上传，基本采用`multipart/form-data`而不用`application/x-www-form-urlencoded`，因为没有必要做 URL 编码，带来巨大耗时的同时也占用了更多的空间。



### http 代理

我们知道在 HTTP 是基于`请求-响应`模型的协议，一般由客户端发请求，服务器来进行响应。

当然，也有特殊情况，就是代理服务器的情况。引入代理之后，作为代理的服务器相当于一个中间人的角色，对于客户端而言，表现为服务器进行响应；而对于源服务器，表现为客户端发起请求，具有**双重身份**。

- **负载均衡**。客户端的请求只会先到达代理服务器，后面到底有多少源服务器，IP 都是多少，客户端是不知道的。因此，这个代理服务器可以拿到这个请求之后，可以通过特定的算法分发给不同的源服务器，让各台源服务器的负载尽量平均。当然，这样的算法有很多，包括**随机算法**、**轮询**、**一致性hash**、**LRU**`(最近最少使用)`等等，不过这些算法并不是本文的重点，大家有兴趣自己可以研究一下。
- **保障安全**。利用**心跳**机制监控后台的服务器，一旦发现故障机就将其踢出集群。并且对于上下行的数据进行过滤，对非法 IP 限流，这些都是代理服务器的工作。
- **缓存代理**。将内容缓存到代理服务器，使得客户端可以直接从代理服务器获得而不用到源服务器那里。下一节详细拆解。

相关头部字段

- via：代理服务器需要标明自己的身份，在http传输中留痕 `Via: proxy_server1, proxy_server2`
- X-Real-IP：记录用户真实ip的方法，最初的那个客户端IP
- X-Forwarded-Host：同上，客户端的域名
- X-Forwarded-Proto：同上，客户端的协议名
- X-Forwarded-For：记录请求方的IP


### 跨域

浏览器遵循**同源政策**(`scheme(协议)`、`host(主机)`和`port(端口)`都相同则为`同源`)。非同源站点有这样一些限制:

- 不能读取和修改对方的 DOM
- 不读访问对方的 Cookie、IndexDB 和 LocalStorage
- 限制 XMLHttpRequest 请求。(后面的话题着重围绕这个)

CORS

请求会自动在请求头当中，添加一个`Origin`字段，用来说明请求来自哪个`源`。服务器拿到请求之后，在回应时对应地添加`Access-Control-Allow-Origin`字段，如果`Origin`不在这个字段的范围中，那么浏览器就会将响应拦截。

因此，`Access-Control-Allow-Origin`字段是服务器用来决定浏览器是否拦截这个响应，这是必需的字段。与此同时，其它一些可选的功能性的字段，用来描述如果不会拦截，这些字段将会发挥各自的作用。

Nginx 

是一种高性能的`反向代理`服务器，可以用来轻松解决跨域问题。
- 正向代理：帮助客户端**访问**客户端自己访问不到的服务器，然后将结果返回给客户端。
- 反向代理：拿到客户端的请求，将请求转发给其他的服务器，主要的场景是维持服务器集群的**负载均衡**，换句话说，反向代理帮**其它的服务器**拿到请求，然后选择一个合适的服务器，将请求转交给它。

因此，两者的区别就很明显了，正向代理服务器是帮**客户端**做事情，而反向代理服务器是帮其它的**服务器**做事情。


