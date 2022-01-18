# Expect 100-continue

# Expect 100-continue

HTTP/1 has no proper way to stop an ongoing transfer (in any direction) and
still maintain the connection. So, if we figure out that the transfer had
better stop after the transfer has started, there are only two ways to
proceed: cut the connection and pay the price of reestablishing the connection
again for the next request, or keep the transfer going and waste bandwidth but
be able to reuse the connection next time.

HTTP/1 没有合适的方法来停止正在进行的传输（在任何方向上）并保持连接。因此，如果我们认为传输最好在传输开始后再停止，那么只有两种方法可以继续：切断连接并为下一个请求重新建立连接付出代价，或者继续传输并浪费带宽，但下次可以重新使用连接。

One example of when this can happen is when you send a large file over HTTP,
only to discover that the server requires authentication and immediately sends
back a 401 response code.

一个例子是，当你通过 HTTP 发送一个大文件时，却发现服务器需要身份验证，并且服务器立即发回 401 响应代码。
 
The mitigation that exists to make this scenario less frequent is to have
curl pass on an extra header, `Expect: 100-continue`, which gives the server a
chance to deny the request before a lot of data is sent off. curl sends this
Expect: header by default if the POST it will do is known or suspected to be
larger than just minuscule. curl also does this for PUT requests.

为了降低这种情况的发生频率，现有处理措施是请求里加上额外的头 `Expect:100 continue`，这将使服务器有机会在发送大量数据之前拒绝请求。默认情况下，如果已知或怀疑它将执行的 POST 大于极小值，curl 将发送 `Expect:` 头。同样 curl 也对 PUT 请求执行这个操作。

When a server gets a request with an 100-continue and deems the request fine,
it will respond with a 100 response that makes the client continue. If the
server does not like the request, it sends back response code for the error it
thinks it is.

当服务器收到一个 100-continue 的请求并认为该请求没有问题时，它将以 100 响应代码进行响应，从而使客户端继续。如果服务器不喜欢该请求，它会发回它认为是错误的响应代码。

Unfortunately, lots of servers in the world do not properly support the
Expect: header or do not handle it correctly, so curl will only wait 1000
milliseconds for that first response before it will continue anyway.

不幸的是，世界上很多服务器都不正确地支持 `Expect:` 头或者没有正确地处理它，所以 curl 只能在继续新的操作之前，等待第一个响应到来或者 1000 毫秒超时。

Those are 1000 wasted milliseconds. You can then remove the use of Expect:
from the request and avoid the waiting with `-H`:

这将浪费了 1000 毫秒。通过使用 `-H` 选项你可以从请求中删除 `Expect:` 头，这样就可以避免等待：

    curl -H Expect: -d "payload to send" http://example.com

In some situations, curl will inhibit the use of the Expect header if the
content it is about to send is small (like below one kilobyte), as having to
waste such a small chunk of data is not considered much of a problem.

在某些情况下，如果要发送的内容很小（比如低于 1KB），curl 将禁止使用 Expect 头，因为浪费这么小的数据块被认为不是什么大问题。

## HTTP/2 and later

## HTTP/2及更高版本

HTTP/2 and later versions of HTTP can stop an ongoing transfer without
shutting down the connection, which makes `Expect:` pointless.

HTTP/2 和更高版本的 HTTP 可以在不关闭连接的情况下停止正在进行的传输，这使得 `Expect:` 毫无意义。
 
