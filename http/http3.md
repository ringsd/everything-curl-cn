# HTTP/3

# HTTP/3

(This feature is marked **experimental** as of this time and needs to be
explicitly enabled in the build.)

（此时此功能被标记为**实验**，需要在构建中明确启用。）

## Drafts, not standard

## 草稿，而非标准

As of September 2020, **the HTTP/3 protocol has not yet been finalized**.
Everything and everyone that speaks HTTP/3 at this point does it with the
knowledge and awareness that it might change going forward.

截至 2020 年 9 月，**HTTP/3协议尚未最终确定**。
因此和 HTTP/3 相关的所有人，都知道并意识到 HTTP/3 未来可能会发生变化。

## QUIC

## QUIC

HTTP/3 is the HTTP version that is designed to communicate over QUIC. QUIC can
for all particular purposes to be considered as a TCP+TLS replacement.

HTTP/3 是设计用于通过 QUIC 进行通信的 HTTP 版本。QUIC 可以视为 TCP+TLS 的替代品。

All requests that do HTTP/3 will therefore not use TCP. They will use QUIC.
QUIC is a reliable transport protocol built over UDP. HTTP/3 implies use of
QUIC.

因此，所有 HTTP/3 的请求都不会使用 TCP 。他们将使用 QUIC。QUIC 是一种基于 UDP 的可靠传输协议。HTTP/3 就意味着使用 QUIC。

## HTTPS only

## HTTPS only

HTTP/3 is performed over QUIC which is always using TLS, so HTTP/3 is by
definition always encrypted and secure. Therefore, curl only uses HTTP/3 for
`HTTPS://` URLs.

HTTP/3 始终通过使用 TLS 的 QUIC 执行，因此根据定义 HTTP/3 始终是加密和安全的。因此，curl 也只针对 `HTTPS://` 开头的 URL 使用 HTTP/3。

## Enable

As a shortcut straight to HTTP/3, to make curl attempt a QUIC connect directly
to the given host name and port number, use `--http3`. Like this:

使用 `--http3` ，可以直接使用 HTTP/3，也就是可以让 curl 通过 QUIC 直接连接到给定的主机名和端口号。就像这样：

    curl --http3 https://example.com/

Normally, a `HTTPS://` URL implies that a client needs to connect to it using
TCP + TLS.

通常，`HTTPS://` URL 表示客户端需要使用 TCP+TLS 连接到它。

## Alt-svc:

The [alt-svc](altsvc.md) method of changing to HTTP/3 is the official way to
bootstrap into HTTP/3 for a server.

使用 [alt-svc](altsvc.md) 方法是将服务器引导到 HTTP/3 的正式方式。

Note that you need that feature built-in and that it does not switch to HTTP/3
for the *current* request unless the alt-svc cache is already populated, but
it will rather store the info for use in the *next* request to the host.

请注意，你需要内置该功能，并且 alt-svc 缓存已经填充，否则它不会为*当前*请求切换到 HTTP/3，但它会存储信息，以便在主机的*下一个*请求中使用。
 
## When QUIC is denied

## 当 QUIC 被拒绝时

A certain amount of QUIC connection attempts will fail, partly because many
networks and hosts block or throttle the traffic.

一定数量的QUIC连接尝试将失败，部分原因是许多网络和主机会阻止或限制流量。

Currently, curl features no fall-back logic but if an HTTP/3 (or QUIC rather)
connection fails it will be reported as, yes, a failure.

目前，curl 没有回退逻辑，但是如果 HTTP/3（或者更确切地说是 QUIC ）连接失败，它将被报告为失败。

Web browsers will upgrade to HTTP/3 in the background and only switch over
once they know it works, which is a smoother way that does not break things
for users as much.

Web浏览器将在后台升级到HTTP/3，只有在知道它可以工作后才能切换，这是一种更平滑的方式，不会对用户造成太多破坏。

Future curl versions will likely offer better fall-back and error handling for
this.

未来的 curl 版本可能会提供更好的回退和错误处理。
