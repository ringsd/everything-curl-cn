## HTTP/2

## HTTP/2

curl supports HTTP/2 for both HTTP:// and HTTPS:// URLs assuming that curl was
built with the proper prerequisites. It will even default to using HTTP/2 when
given an HTTPS URL since doing so implies no penalty and when curl is used with
sites that do not support HTTP/2 the request will instead negotiate HTTP/1.1.

以适当条件构建的 curl 可以通过 HTTP:// 和 HTTPS:// 开头的 URL 去使用 HTTP/2。当给定 HTTPS URL 时，curl 可以默认就使用 HTTP/2，当访问到的站点不支持 HTTP/2 时，请求将转而协商 HTTP/1.1。

With HTTP:// URLs however, the upgrade to HTTP/2 is done with an `Upgrade:`
header that may cause an extra round-trip and perhaps even more troublesome, a
sizable share of old servers will return a 400 response when seeing such a
header.

然而，对于 HTTP:// 开头的 URL，升级到 HTTP/2 需要一个 `Upgrade:` 头，但是这可能会导致额外沟通，甚至更麻烦，相当一部分的旧服务器在收到这样的头时会返回 400 响应。

It should also be noted that some (most?) servers that support HTTP/2 for
HTTP:// (which in itself is not all servers) will not acknowledge the
`Upgrade:` header on POST, for example.

还应注意的是一些（大多数？）通过 HTTP://（并非所有服务器）支持 HTTP/2 的服务器不会在 POST 请求里确认 `Upgrade:` 头。

To ask a server to use HTTP/2, just:

要让服务器使用 HTTP/2，只需执行以下操作：

    curl --http2 http://example.com/

If your curl does not support HTTP/2, that command line will return an error
saying so. Running `curl -V` will show if your version of curl supports it.

如果你使用的 curl 不支持 HTTP/2，那么该命令行将返回一个错误。运行 `curl -V` 将显示你的 curl 版本，然后确认他是否支持 HTTP/2。

If you by some chance already know that your server speaks HTTP/2 (for example,
within your own controlled environment where you know exactly what runs in
your machines) you can shortcut the HTTP/2 "negotiation" with
`--http2-prior-knowledge`.

如果你碰巧已经知道你的服务器支持 HTTP/2（例如，在你自己的受控环境中，你确切地知道在你的机器中运行什么），那么你可以使用 `--http2-prior-knowledge` 选项直接使用 HTTP/2 协商 。

### Multiplexing

### 多路复用

One of the primary features in the HTTP/2 protocol is the ability to multiplex
several logical stream over the same physical connection. When using the curl
command-line tool, you cannot take advantage of that cool feature since curl
is doing all its network requests in a strictly serial manner, one after the
next, with the second only ever starting once the previous one has ended.

HTTP/2 协议的主要功能之一是能够在同一物理连接上多路传输多个逻辑流。当使用 curl 命令行工具时，你无法利用这个很酷的功能，因为 curl 以严格的串行方式执行其所有网络请求，一个接一个，第二个请求只有在前一个请求结束后才会启动。

Preferably, a future curl version will be enhanced to allow the use of this
feature.

未来的 curl 版本将完善这部分，以允许使用此功能。
