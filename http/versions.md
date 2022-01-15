# HTTP versions
# HTTP 版本

As any other Internet protocol, the HTTP protocol has kept evolving over the
years and now there are clients and servers distributed over the world and
over time that speak different versions with varying levels of success. So in
order to get libcurl to work with the URLs you pass in libcurl offers ways for
you to specify which HTTP version that request and transfer should
use. libcurl is designed in a way so that it tries to use the most common, the
most sensible if you want, default values first but sometimes that is not
enough and then you may need to instruct libcurl on what to do.

和其他互联网协议一样，HTTP 协议也紧跟时代一直在发展，现在分布在世界各地的客户端和服务器可能使用不同的版本，不过他们都取得了不同程度的成功。因此，为了让 libcurl 正确处理你传入的 URL ，libcurl 也提供了一些方法来指定请求和传输应该使用那个 HTTP 版本。在 libcurl 的设计里，它首先尝试使用最常见、最合理的默认值，但有时这样是不够的，因此可能需要你去指示 libcurl 做什么。

curl defaults to HTTP/1.1 for HTTP servers but if you connect to HTTPS and you
have a curl that has HTTP/2 abilities built-in, it attempts to negotiate
HTTP/2 automatically or falls back to 1.1 in case the negotiation failed.
Non-HTTP/2 capable curls get 1.1 over HTTPS by default.

对于 HTTP 服务器，curl 默认使用 HTTP/1.1。如果你连接到 HTTPS，并且你使用的 curl 内置了 HTTP/2 的功能，它会尝试自动协商 HTTP/2，如果协商失败它会重新使用 1.1；如果你的 curl 没有 HTTP/2 的，那么连接到 HTTPS 默认就使用 1.1。
 
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
## 接受 HTTP/0.9

The HTTP version used before HTTP/1.0 was made available is often referred to
as HTTP/0.9. Back in those days, HTTP responses had no headers as they would
only return a response body and then immediately close the connection.

通常将 HTTP/1.0 之前的 HTTP 版本称为 HTTP/0.9。在那些版本里，HTTP 响应没有头，它们只返回一个响应体，然后关闭连接。

curl can be told to support such responses but will by default not recognize
them, for security reasons. Almost anything bad will look like an HTTP/0.9
response to curl so the option needs to be used with caution.

curl 可以支持这些响应，不过出于安全原因，默认情况下不会识别它们。对于 curl 来说几乎所有关于 HTTP/0.9 响应的内容看起来都不太好，因此需要谨慎使用这个选项。

The HTTP/0.9 option to curl is different than the other HTTP command line
options for HTTP versions mentioned above as this controls what response to
accept, while the others are about what HTTP protocol version to try to use.

curl 中的 HTTP/0.9 选项不同于上面提到的其他 HTTP 版本的选项，它控制的是要接受的响应，而其他选项只是尝试使用不同的 HTTP 协议版本。

Tell curl to accept an HTTP/0.9 response like this:
curl 使用以下命令支持 HTTP/0.9：

    curl --http0.9 https://example.com/

在 curl `7.66.0` 版本中 `--http0.9` 已经默认关闭了。
