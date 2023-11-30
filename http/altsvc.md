# Alternative Services

# 替代服务

[RFC 7838](https://www.rfc-editor.org/rfc/rfc7838.txt) defines an HTTP header
which lets a server tell a client that there is one or more *alternatives* for
that server at "another place" with the use of the `Alt-Svc:` response header.

[RFC 7838](https://www.rfc-editor.org/rfc/rfc7838.txt) 定义了一个 HTTP 标头，它允许服务器告诉客户端该服务器在“另一个地方”有一个或多个 *替代方案*  " 使用 `Alt-Svc:` 响应标头。

The *alternatives* the server suggests can include a server running on another
port on the same host, on another completely different host name and it can
even perhaps offer the service over another protocol.

服务器建议的 *替代方案* 可以包括在同一主机上的另一个端口、另一个完全不同的主机名上运行的服务器，甚至可以通过另一个协议提供服务。

## Enable

## 使能

To make curl consider offered alternatives, tell curl to use a specific
alt-svc cache file like this:

为了让 curl 考虑提供的替代方案，请告诉 curl 使用一个特定的 alt-svc 缓存文件，如下所示：

    curl --alt-svc altcache.txt https://example.com/

then curl will load existing alternative service entries from the file at
start-up and consider those when doing HTTP requests, and if the servers sends
new or updated `Alt-Svc:` headers, curl will store those in the cache at exit.

然后，CURL 将在启动时从文件中加载现有的替代服务条目，并考虑在执行 HTTP 请求时的那些服务条目，并且如果服务器发送新的或更新的 `Alt-Svc:` 标题，CURL 将在退出时将这些存储在缓存中。 

## The alt-svc cache

## alt-svc 缓存

The alt-svc cache is similar to a cookie jar. It is a text based file that
stores one alternative per line and each entry also has an expiry time for
which duration that particular alternative is valid.

alt-svc 缓存类似于 cookie 罐。它是一个基于文本的文件，每行存储一个替代方案，每个条目也有一个过期时间标记该替代方案的有效期。

## HTTPS only

## HTTPS only

`Alt-Svc:` is only trusted and parsed from servers when connected to over
HTTPS.

`Alt Svc:`仅通过 HTTPS 连接到服务器时受信任并从服务器解析。

## HTTP/3

## HTTP/3

The use of `Alt-Svc:` headers is as of August 2019 the only defined way to
bootstrap a client and server into using HTTP/3. The server then hints to the
client over HTTP/1 or HTTP/2 that it also is available over HTTP/3 and then
curl can connect to it using HTTP/3 in the subsequent request if the alt-svc
cache says so.

从2019年8月起，使用 `Alt-Svc:` 头成为引导客户机和服务器使用 HTTP/3 的唯一方式。服务器通过 HTTP/1 或 HTTP/2 向客户端提示，它也可以使用 HTTP/3 ，然后 alt-svc 缓存说服务器支持 HTTP/3，curl 可以在后续请求中使用 HTTP/3 连接到服务器。
