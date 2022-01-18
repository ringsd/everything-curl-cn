# Chunked encoded POSTs
# 分块编码 POST

When talking to an HTTP 1.1 server, you can tell curl to send the request body
without a `Content-Length:` header upfront that specifies exactly how big the
POST is. By insisting on curl using chunked Transfer-Encoding, curl will send
the POST chunked piece by piece in a special style that also sends the size
for each such chunk as it goes along.

当与 HTTP 1.1 服务器通信时，你可以用 curl 发送不包含内容大小的 `Content-Length:` 头的 POST 请求。curl 将使用分块传输编码，curl 以 POST 请求一块一块地发送分块，并在发送过程中发送每个此类分块的大小。

You send a chunked POST with curl like this:

你可以用 curl 发送分块的 POST，如下所示：

    curl -H "Transfer-Encoding: chunked" -d @file http://example.com

## Caveats

## 警告

This assumes that you know you do this against an HTTP/1.1 server. Before 1.1,
there was no chunked encoding, and after version 1.1 chunked encoding has been
deprecated.

这假设您知道你是针对 HTTP/1.1 服务器执行此操作的。在 1.1 之前，没有分块编码，而在 1.1 版本之后，分块编码已被弃用。
