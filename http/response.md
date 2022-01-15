### HTTP responses

### HTTP 响应


When an HTTP client talks HTTP to a server, the server *will* respond with an
HTTP response message or curl will consider it an error and returns 52 with
the error message "Empty reply from server".

当 HTTP 客户端通过 HTTP 与服务器进行对话时，服务器将返回 HTTP 响应，否则 curl 会认为服务器出现错误，并且返回错误号 52， 52 对应的错误消息为 `Empty reply from server`。

#### Size of an HTTP response
#### HTTP 响应的大小

An HTTP response has a certain size and curl needs to figure it out. There are
several different ways to signal the end of an HTTP response but the most
basic way is to use the `Content-Length:` header in the response and with that
specify the exact number of bytes in the response body.

每个 HTTP 响应都有一个大小， curl 需要获取到这个大小。有几种不同的方法可以标记 HTTP 响应的结束，但最基本的方法是在响应中使用 `Content Length:` 头，用它来指定响应数据中的确切字节数。

Some early HTTP server implementations had problems with file sizes greater
than 2GB and wrongly managed to send Content-Length: headers with negative
sizes or otherwise just plain wrong data. curl can be told to ignore the
Content-Length: header completely with `--ignore-content-length`. Doing so may
have some other negative side-effects but should at least let you get the
data.

一些早期的 HTTP 服务器实现有这样一个问题：如果文件大小大于 2GB 的问题，就会错误地发送了数据长度，例如头文件大小为负数或者数据完全错误。curl 可以使用 `---ignore-content-length` 来忽略 `Content-Length:` 头。这样做可能会有其他一些问题，但至少能够获取到数据。

### HTTP response codes

### HTTP 响应代码

An HTTP transfer gets a 3 digit response code back in the first response line.
The response code is the server's way of giving the client a hint about how
the request was handled.

HTTP 会在响应的第一行中返回一个 3 位数的响应代码。响应代码是服务器告诉客户端请求是如何被处理的代号。

It is important to note that curl does not consider it an error even if the
response code would indicate that the requested document could not be
delivered (or similar). curl considers a successful sending and receiving of
HTTP to be good.

重要的是要注意，即使响应代码返回所请求的文档不能被处理（或类似问题），curl 也不认为它是错误的。只要成功的发送请求和接收响应，curl 就认为这个操作是正常的。

The first digit of the HTTP response code is a kind of "error class":

HTTP 响应代码的第一位数字是一种“错误类型”：

 - 1xx: transient response, more is coming
 - 2xx: success
 - 3xx: a redirect
 - 4xx: the client asked for something the server could not or would not deliver
 - 5xx: there is a problem in the server

 - 1xx：瞬态响应，更多信息即将到来
 - 2xx：成功
 - 3xx：重定向
 - 4xx：客户端请求的内容，服务器没有或者无法提供
 - 5xx：服务器存在问题

Remember that you can use curl's `--write-out` option to extract the response
code. See the [--write-out](../usingcurl/verbose/writeout.md) section.

你可以使用 curl 的 `--write-out` 选项获取响应代码。详细请参阅 [--write-out](../usingcurl/verbose/writeout.md) 部分。

To make curl return an error for response codes >= 400, you need to use
`--fail` or `--fail-with-body`. Then curl will exit with error code 22 for
such occurrences.

要使curl返回响应代码>=400的错误，需要使用“---fail”或“---fail with body”。然后curl将退出，错误代码为22。

### CONNECT response codes

### 连接响应代码

Since there can be an HTTP request and a separate CONNECT request in the same
curl transfer, we often separate the CONNECT response (from the proxy) from
the remote server's HTTP response.

因为在同一个curl传输中可以有一个HTTP请求和一个单独的CONNECT请求，所以我们通常将CONNECT响应（来自代理）与远程服务器的HTTP响应分开。
 

The CONNECT is also an HTTP request so it gets response codes in the same
numeric range and you can use `--write-out` to extract that code as well.

CONNECT也是一个HTTP请求，因此它获取相同数字范围内的响应代码，您也可以使用“---write out”提取该代码。
 

### Chunked transfer encoding

### 分块传输编码

An HTTP 1.1 server can decide to respond with a "chunked" encoded response, a
feature that was not present in HTTP 1.0.

HTTP 1.1服务器可以决定使用“分块”编码的响应进行响应，这是HTTP 1.0中不存在的特性。

When receiving a chunked response, there is no Content-Length: for the response
to indicate its size. Instead, there is a `Transfer-Encoding: chunked` header
that tells curl there is chunked data coming and then in the response body, the
data comes in a series of "chunks". Every individual chunk starts with the
size of that particular chunk (in hexadecimal), then a newline and then the
contents of the chunk. This is repeated over and over until the end of the
response, which is signalled with a zero sized chunk. The point of this
response encoding is for the client to be able to figure out when the
response has ended even though the server did not know the full size before
it started to send it. This is usually the case when the response is dynamic
and generated at the point when the request comes.

当接收分块响应时，没有内容长度：用于指示响应大小。相反，有一个“Transfer Encoding:chunked”头，告诉curl有分块数据出现，然后在响应体中，数据以一系列“chunks”的形式出现。每个块都以该块的大小（十六进制）开始，然后是换行符，然后是块的内容。这会一次又一次地重复，直到响应结束，并用一个大小为零的块发出信号。这种响应编码的目的是让客户机能够知道响应何时结束，即使服务器在开始发送之前不知道响应的完整大小。当响应是动态的并且在请求到来时生成时，通常就是这种情况。

Clients like curl will, of course, decode the chunks and not show the chunk
sizes to users.

当然，像curl这样的客户端将解码块，而不会向用户显示块大小。

### Gzipped transfers

### Gzip传输

Responses over HTTP can be sent in compressed format. This is most commonly
done by the server when it includes a `Content-Encoding: gzip` in the response
as a hint to the client. Compressed responses make a lot of sense when either
static resources are sent (that were compressed previously) or even in
run-time when there is more CPU power available than bandwidth. Sending a much
smaller amount of data is often preferred.

HTTP上的响应可以以压缩格式发送。当服务器在响应中包含一个“Content Encoding:gzip”作为对客户端的提示时，这通常由服务器完成。当发送静态资源（以前压缩过的资源）时，或者甚至在运行时，当可用CPU功率大于带宽时，压缩响应都很有意义。发送数量小得多的数据通常是首选。

You can ask curl to both ask for compressed content *and* automatically and
transparently uncompress gzipped data when receiving content encoded gzip (or
in fact any other compression algorithm that curl understands) by using
`--compressed`:

通过使用“---compressed”，您可以让curl在接收内容编码的gzip（或者实际上是curl理解的任何其他压缩算法）时，同时请求压缩内容*和*自动且透明地解压缩gzip数据：

    curl --compressed http://example.com/

### Transfer encoding

### 传输编码

A less common feature used with transfer encoding is compression.

传输编码使用的一个不太常见的特性是压缩。
 

Compression in itself is common. Over time the dominant and web compatible
way to do compression for HTTP has become to use `Content-Encoding` as
described in the section above. But HTTP was originally intended and specified
to allow transparent compression as a transfer encoding, and curl supports
this feature.

压缩本身很常见。随着时间的推移，对HTTP进行压缩的主要且与web兼容的方式已变成使用上文所述的“内容编码”。但是HTTP最初的目的是允许透明压缩作为传输编码，并且curl支持这个特性。

The client then simply asks the server to do compression transfer encoding and
if acceptable, it will respond with a header indicating that it will and curl
will then transparently uncompress that data on arrival. A user enables asking
for compressed transfer encoding with `--tr-encoding`:

然后，客户机简单地要求服务器进行压缩传输编码，如果可以接受，它将用一个报头响应，表明它将这样做，然后curl将在数据到达时透明地解压缩该数据。用户可以通过“--tr encoding”请求压缩传输编码：

    curl --tr-encoding http://example.com/

It should be noted that not many HTTP servers in the wild support this.

应该注意的是，野外没有多少HTTP服务器支持这一点。
 

### Pass on transfer encoding

### 传递传递编码

In some situations you may want to use curl as a proxy or other in-between
software. In those cases, curl's way to deal with transfer-encoding headers
and then decoding the actual data transparently may not be desired, if the end
receiver *also* expects to do the same.

在某些情况下，您可能希望使用curl作为代理或其他中间软件。在这些情况下，如果终端接收器*也*希望这样做，则可能不需要curl处理传输编码头然后透明地解码实际数据的方法。

You can then ask curl to pass on the received data, without decoding it. That
means passing on the sizes in the chunked encoding format or the compressed
format when compressed transfer encoding is used etc.

然后，您可以要求curl传递接收到的数据，而无需对其进行解码。这意味着在使用压缩传输编码时，以分块编码格式或压缩格式传递大小。

    curl --raw http://example.com/
