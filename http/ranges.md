## HTTP ranges
## HTTP 范围

What if the client only wants the first 200 bytes out of a remote resource or
perhaps 300 bytes somewhere in the middle? The HTTP protocol allows a client
to ask for only a specific data range. The client asks the server for the
specific range with a start offset and an end offset. It can even combine
things and ask for several ranges in the same request by just listing a bunch
of pieces next to each other. When a server sends back multiple independent
pieces to answer such a request, you will get them separated with mime
boundary strings and it will be up to the user application to handle that
accordingly. curl will not further separate such a response.

如果客户端只想从远程资源中取出前 200 个字节，或者中间某处的 300 字节应该怎么办？HTTP 协议允许客户端仅请求获取特定范围的数据。客户端将向服务器发送包括开始偏移和结束偏移这个特定范围的请求。它也可以将多个范围通过 `,` 分隔之后组合在一起成一个请求。服务器将会在同一个响应中包含多个独立数据，用户将会获取到一个用 mime boundary 分隔的字符串，接下来这部分将会由用户的应用程序来处理。针对这样的响应 curl 不会做进一步分割处理。

However, a byte range is only a request to the server. It does not have to
respect the request and in many cases, like when the server automatically
generates the contents on the fly when it is being asked, it will simply refuse
to do it and it then instead responds with the full contents anyway.
<!-- the above is duplicated at libcurl-http-ranges.md -->

然而，字节范围只是客户端对服务器发出请求。在许多情况下，服务器并不按照请求来处理，例如服务器在请求时才自动动态生成内容时，它会简单地拒绝请求，然后它会返回完整的内容进行响应。

<!-- the above is duplicated at libcurl-http-ranges.md -->

You can make curl ask for a range with `-r` or `--range`. If you want the
first 200 bytes out of something:

你可以让 curl 通过选项 `-r` 或 `--range` 来发送带范围的请求。例如，你想要获取前 200 个字节：

    curl -r 0-199 http://example.com

Or everything in the file starting from index 200:

或者从索引 200 开始的所有内容：

    curl -r 200- http://example.com

Get 200 bytes from index 0 *and* 200 bytes from index 1000:

从索引 0 获取 200 字节和从索引 1000 获取 200 字节：

    curl -r 0-199,1000-1199 http://example.com/
