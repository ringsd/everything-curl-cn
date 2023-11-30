# HTTP Strict Transport Security

# HTTP 严格的传输安全

HTTP Strict Transport Security, HSTS, is a protocol mechanism that helps to
protect HTTPS servers against man-in-the-middle attacks such as protocol
downgrade attacks and cookie hijacking. It allows an HTTPS server to declare
that clients should automatically interact with this host name using only
HTTPS connections going forward - and explicitly not use clear text protocols
with it.

HTTP 严格传输安全（HSTS）是一种协议机制，有助于保护 HTTPS 服务器免受中间人攻击，如协议降级攻击和 cookie 劫持。它允许 HTTPS 服务器声明客户端应仅使用 HTTPS 连接与该主机名交互，并且明确不使用明文协议。

## HSTS cache

## HSTS 缓存

The HSTS status for a certain server name is set in a response header and has
an expire time. The status for every HSTS host name needs to be saved
in a file for curl to pick it up and to update the status and expire time.

特定服务器名称的 HSTS 状态在响应头中设置，并具有过期时间。每个 HSTS 主机名的状态都需要保存在一个文件中，以便 curl 获取并更新状态和过期时间。

Invoke curl and tell it which file to use as a hsts cache:

调用 curl 并告诉它使用哪个文件作为 hsts 缓存：

    curl --hsts hsts.txt https://example.com

curl will only update the hsts info if the header is read over a secure
transfer, so not when done over a clear text protocol.

curl 只会在通过安全传输读取标题时更新 hsts 信息，因此在通过明文协议读取时不会更新。

## Use HSTS to update insecure protocols

## 使用 HSTS 更新不安全的协议

If the cache file now contains an entry for the given host name, it will
automatically switch over to a secure protocol even if you try to connect to
it with an insecure one:

如果缓存文件现在包含给定主机名的条目，它将自动切换到安全协议，即使你尝试使用不安全的协议连接到它：

    curl --hsts hsts.txt http://example.com
