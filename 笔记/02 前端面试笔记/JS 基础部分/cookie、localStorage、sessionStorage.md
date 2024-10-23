
要掌握 cookie，localStorage 和 sessionStorage。

##### cookie

Name、Value、Domain、Path、Expires/Max-age、Size、HttpOnly、Secure、SameSite和Priority

- 本身用于浏览器和 server 通讯。
- 被“借用”到本地存储来的。
- 可用 document.cookie = '...' 来修改。

其缺点：

- 存储大小限制为 4KB。
- http 请求时需要发送到服务端，增加请求数量。
- 只能用 document.cookie = '...' 来修改，太过简陋。

##### localStorage 和 sessionStorage

- HTML5 专门为存储来设计的，最大可存 5M。
- API 简单易用， setItem getItem。
- 不会随着 http 请求被发送到服务端。

它们的区别：

- localStorage 数据会永久存储，除非代码删除或手动删除。
- sessionStorage 数据只存在于当前会话，浏览器关闭则清空。
- 一般用 localStorage 会多一些。

