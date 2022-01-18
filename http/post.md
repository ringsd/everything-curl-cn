## HTTP POST

## HTTP POST

POST is the HTTP method that was invented to send data to a receiving web
application, and it is how most common HTML forms on the web
works. It usually sends a chunk of relatively small amounts of data to the
receiver.

POST 是一个将数据发送给正在接收的 Web 应用程序的 HTTP 方法，它是应用在 HTML 表单中一种最常见方式。它通常将一块相对少量的数据发送到接收端。

This section describes the simple posts, for multipart formposts done with
`-F`, check out [Multipart formposts](multipart.md).

本节介绍简易的 POST 使用方法，关于 multipart formposts 请使用 `-F` 参数，详细请查看 [Multipart formposts](multipart.md)。

* [简易 POST](post/simple.md)
* [内容类型(Content-Type)](post/content-type.md)
* [POST 二进制文件](post/binary.md)
* [URL 编码](post/url-encode.md)
* [转换为 GET](post/convert-to-get.md)
* [Expect 100-continue](post/expect100.md)
* [分块编码的 POST](post/chunked.md)
* [隐藏的表单域](post/hiddenfields.md)
* [弄清楚浏览器发送的内容](post/browsersends.md)
* [JavaScript 和表单](post/javascript.md)

参考资料：
https://imququ.com/post/four-ways-to-post-data-in-http.html
