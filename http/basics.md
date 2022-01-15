### HTTP protocol basics
### HTTP 协议基础
 
(This assumes you have read the [Network and protocols](../protocols.md)
section or are otherwise already familiar with protocols.)

（假设你已经阅读了[Network and protocols](../protocols.md)部分或已经熟悉协议。）

HTTP is a protocol that is easy to learn the basics of. A client connects to a
server—and it is always the client that takes the initiative—sends a
request and receives a response. Both the request and the response consist of
headers and a body. There can be little or a lot of information going in both
directions.

HTTP 是一种比较容易学习的协议。由客户端连接服务器，并且总是由客户端主动发送请求并接收响应。请求和响应都由头和数据组成。可以双向传输任意数量的信息。

An HTTP request sent by a client starts with a request line, followed by
headers and then optionally a body. The most common HTTP request is probably
the GET request which asks the server to return a specific resource, and this
request does not contain a body.

客户端发送的 HTTP 请求以请求行开始，然后是请求头，然后是可选的请求数据。最常见的 HTTP 请求可能是 GET 请求，它要求服务器返回特定的资源，而该请求不包含请求数据。

When a client connects to 'example.com' and asks for the '/' resource, it
sends a GET without a request body:

客户端连接到 'example.com' 并请求 '/' 资源，发送一个不带请求数据的 GET 请求：

    GET / HTTP/1.1
    User-agent: curl/2000
    Host: example.com

…the server could respond with something like below, with response headers
and a response body ('hello'). The first line in the response also contains
the response code and the specific version the server supports:

…服务器可以使用以下内容进行响应，包括响应头和响应数据（`hello`）。响应的第一行还包含响应代码和服务器支持的特定版本：

    HTTP/1.1 200 OK
    Server: example-server/1.1
    Content-Length: 5
    Content-Type: plain/text

    hello

If the client would instead send a request with a small request body
('hello'), it could look like this:

如果客户机改为发送一个带有请求数据（`hello`）的请求，则它可能如下所示：

    POST / HTTP/1.1
    Host: example.com
    User-agent: curl/2000
    Content-Length: 5

    hello

A server always responds to an HTTP request unless something is wrong.

除非出现问题了，否则服务器总是会响应 HTTP 请求。
 
### The URL converted to a request

### 转换 URL 为请求

So when an HTTP client is given a URL to operate on, that URL is then used,
picked apart and those parts are used in various places in the outgoing
request to the server. Let's take the an example URL:

因此，当给 HTTP 客户端一个 URL 进行操作时，该 URL 将被分割，分割的这些部分将放在请求中的不同位置然后发送给服务器使用。让我们以以下 URL 为例：

    https://www.example.com/path/to/file

 - **https** means that curl will use TLS to the remote port 443 (which is the
   default port number when no specified is used in the URL).

 - **https** 表示 curl 将使用 TLS 连接到远程端口 443（这是默认端口号，当 URL 中没有指定端口号时，会使用默认端口）。
 
 - **www.example.com** is the host name that curl will resolve to one or more IP
   address to connect to. This host name will also be used in the HTTP request in
   the `Host:` header.

 - **www.example.com** 将被 curl 解析为一个或多个要连接的 IP 地址的主机名。此主机名也将应用在 HTTP 请求中的 `Host:` 头中。

 - **/path/to/file** is used in the HTTP request to tell the server which exact
   document/resources curl wants to fetch

 - **/path/to/file** 将被 curl 放到 HTTP 请求中告诉服务器，它要获取的文档/资源
