# curl 中的 HTTP

在 curl 的整个生命周期的用户调查中，HTTP 一直是 curl 支持的最重要和最常用的协议。本章将解释如何使用 curl 进行有效的 HTTP 传输。

HTTP 和 HTTPS 的工作方式基本相同，因为它们实际上是相同的东西，HTTPS 只是带有 TLS 层的 HTTP。关于 HTTPS 参见下面 [HTTPS](#https) 部分。

## HTTP 方法

在每个 HTTP 请求中，都有一个方法。有时也称为动作。最常用的方法有 GET、POST、HEAD 和 PUT。

通常不在命令行中指定方法，而是使用特定的参数来决定使用什么方法。GET 是默认方法，要使用 POST 方法请使用 “-d” 或 “-F” ，要使用 HEAD 请使用 “-I”，要使用 PUT 请使用 “-T”。

更多关于 HTTP 请求的信息，请参见[修改HTTP请求](HTTP/requests.md)部分。
 