# HTTP authentication
# HTTP 验证

Each HTTP request can be made authenticated. If a server or a proxy want the
user to provide proof that they have the correct credentials to access a URL
or perform an action, it can send an HTTP response code that informs the client
that it needs to provide a correct HTTP authentication header in the request
to be allowed.

任何 HTTP 请求都可以进行身份​​验证。如果服务器或代理希望用户提供他们拥有访问 URL 或执行操作的正确凭据，
它可以发送一个 HTTP 响应代码，通知客户端它需要在请求被允许。

A server that requires authentication sends back a 401 response code and an
associated `WWW-Authenticate:` header that lists all the authentication
methods that the server supports.

需要身份验证的服务器会发回 401 响应代码和对应的 `WWW-Authenticate:` 标头，其中列出了服务器支持的所有身份验证方法。

An HTTP proxy that requires authentication sends back a 407 response code and
an associated `Proxy-Authenticate:` header that lists all the authentication
methods that the proxy supports.

需要身份验证的 HTTP 代理会发回 407 响应代码和对应的 `Proxy-Authenticate:` 标头，其中列出了代理支持的所有身份验证方法。

It might be worth to note that most websites of today do not require HTTP
authentication for login etc, but they will instead ask users to login on web
pages and then the browser will issue a POST with the user and password etc,
and then subsequently maintain cookies for the session.

值得注意的是，现在大多数网站都不需要 HTTP 身份验证来登录，而是要求用户在网页上登录，浏览器会使用 `POST` 请求来发送用户和密码等，并且通过 cookie 来维护会话 。

To tell curl to do an authenticated HTTP request, you use the `-u, --user`
option to provide user name and password (separated with a colon). Like this:

要告诉 curl 执行经过身份验证的 HTTP 请求，请使用 `-u, --user` 选项提供用户名和密码（用冒号分隔）。 像这样：

    curl --user daniel:secret http://example.com/

This will make curl use the default "Basic" HTTP authentication method. Yes,
it is actually called Basic and it is truly basic. To explicitly ask for the
basic method, use `--basic`.

这将使 curl 默认使用 "Basic" HTTP 身份验证方法。是的，这种验证方法就被称为"Basic"，它是真正的基本。要明确要求基本方法，请使用 `--basic`。

The Basic authentication method sends the user name and password in clear text
over the network (base64 encoded) and should be avoided for HTTP transport.

"Basic" 身份验证方法通过网络以明文形式发送用户名和密码（采用 base64 编码），应该尽量避免用于 HTTP 传输。

When asking to do an HTTP transfer using a single (specified or implied),
authentication method, curl will insert the authentication header already in
the first request on the wire.

当要求使用单一（指定或隐含的）身份验证方法进行 HTTP 传输时，curl 将在线上的第一个请求中插入身份验证标头。

If you would rather have curl first *test* if the authentication is really
required, you can ask curl to figure that out and then automatically use the
most safe method it knows about with `--anyauth`. This makes curl try the
request unauthenticated, and then switch over to authentication if necessary:

如果你希望 curl 确认请求是否需要身份验证，并且需要选择最安全的身份认证，你可以使用 `--anyauth` 选项。这个选项将使得 curl 先尝试使用未经身份验证的请求，然后在必要时再切换到身份验证：

    curl --anyauth --user daniel:secret http://example.com/

and the same concept works for HTTP operations that may require
authentication:

相同的概念适用于可能需要身份验证针对代理的 HTTP 操作：

    curl --proxy-anyauth --proxy-user daniel:secret http://example.com/ \
         --proxy http://proxy.example.com:80/

curl typically (a little depending on how it was built) speaks several other
authentication methods as well, including Digest, Negotiate and NTLM. You can
ask for those methods too specifically:

curl 通常（取决于它是如何构建的）也使用其他几种身份验证方法，包括 Digest、Negotiate 和 NTLM。 你可以用以下方式来要求使用这些方法：

    curl --digest --user daniel:secret http://example.com/
    curl --negotiate --user daniel:secret http://example.com/
    curl --ntlm --user daniel:secret http://example.com/
