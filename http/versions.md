# HTTP versions
# HTTP版本

As any other Internet protocol, the HTTP protocol has kept evolving over the
years and now there are clients and servers distributed over the world and
over time that speak different versions with varying levels of success. So in
order to get libcurl to work with the URLs you pass in libcurl offers ways for
you to specify which HTTP version that request and transfer should
use. libcurl is designed in a way so that it tries to use the most common, the
most sensible if you want, default values first but sometimes that is not
enough and then you may need to instruct libcurl on what to do.

与任何其他互联网协议一样，HTTP 协议多年来一直在发展，现在分布在世界各地的客户端和服务器，随着时间的推移，它们使用不同的版本，取得了不同程度的成功。因此，为了让 libcurl 处理在 libcurl 中传递的 URL ，libcurl 提供了一些方法来指定请求和传输应该使用那个 HTTP 版本。libcurl 的设计方式是，它尝试首先使用最常见、最合理的默认值，但有时这还不够，然后可能需要指示 libcurl 做什么。


curl defaults to HTTP/1.1 for HTTP servers but if you connect to HTTPS and you
have a curl that has HTTP/2 abilities built-in, it attempts to negotiate
HTTP/2 automatically or falls back to 1.1 in case the negotiation failed.
Non-HTTP/2 capable curls get 1.1 over HTTPS by default.

对于HTTP服务器，curl 默认为 HTTP/1.1，但是如果你连接到 HTTPS，并且您有一个具有内置 HTTP/2 功能的curl，它会尝试自动协商 HTTP/2，或者在协商失败时返回到 1.1。
默认情况下，不支持 HTTP/2 的 curl 通过 HTTPS 获得1.1。
 
| Option                              | Description |
|-------------------------------------|-------------|
| --http1.0                           | HTTP/1.0
| --http1.1                           | HTTP/1.1
| --http2                             | [HTTP/2](http2.md)
| --http2-prior-knowledge             | [HTTP/2](http2.md)
| --http3                             | [HTTP/3](http3.md)

| 选项                                | 描述 |
|-------------------------------------|-------------|
| --http1.0                           | HTTP/1.0
| --http1.1                           | HTTP/1.1
| --http2                             | [HTTP/2](http2.md)
| --http2-prior-knowledge             | [HTTP/2](http2.md)
| --http3                             | [HTTP/3](http3.md)


## Accepting HTTP/0.9
## 接受HTTP/0.9

The HTTP version used before HTTP/1.0 was made available is often referred to
as HTTP/0.9. Back in those days, HTTP responses had no headers as they would
only return a response body and then immediately close the connection.

HTTP/1.0 可用之前使用的 HTTP 版本通常称为 HTTP/0.9。在那些日子里，HTTP 响应没有头，因为它们只返回一个响应体，然后立即关闭连接。

curl can be told to support such responses but will by default not recognize
them, for security reasons. Almost anything bad will look like an HTTP/0.9
response to curl so the option needs to be used with caution.

可以告诉 curl 支持这些响应，但出于安全原因，默认情况下不会识别它们。几乎任何不好的东西都看起来像是对 curl 的 HTTP/0.9 响应，因此需要谨慎使用该选项。

The HTTP/0.9 option to curl is different than the other HTTP command line
options for HTTP versions mentioned above as this controls what response to
accept, while the others are about what HTTP protocol version to try to use.

curl 的 HTTP/0.9 选项不同于上面提到的 HTTP 版本的其他 HTTP 命令行选项，因为它控制要接受的响应，而其他选项则是关于要尝试使用的 HTTP 协议版本。

Tell curl to accept an HTTP/0.9 response like this:
告诉 curl 接受如下 HTTP/0.9 响应：

    curl --http0.9 https://example.com/
