# HTTP redirects

# HTTP 重定向

The “redirect” is a fundamental part of the HTTP protocol. The concept was
present and is documented already in the first spec (RFC 1945), published in
1996, and it has remained well-used ever since.

“重定向”是 HTTP 协议的基本部分。这一概念在 1996 年发布的第一个规范 [RFC 1945](https://datatracker.ietf.org/doc/html/rfc1945) 中就已经出现并记录在案，从那时开始它就被广泛使用。

A redirect is exactly what it sounds like. It is the server sending back an
instruction to the client instead of giving back the contents the client
wanted. The server says “go look over *here* instead for that thing you asked
for“.

如他字面意思重定向就是重新定向。由服务器向客户端发重定向指令，而不是直接返回客户端请求的内容。就像是服务器说“去这里看看”取代了直接返回请求的内容。

Redirects are not all alike. How permanent is the redirect? What request
method should the client use in the next request?

重定向并不都是一样的。重定向的持久性如何？客户端在下一个请求中应该使用什么请求方法？

All redirects also need to send back a `Location:` header with the new URI to
ask for, which can be absolute or relative.

所有重定向需要发回一个带有新 URI 的 `Location:` 头，该 URI 可以是绝对的，也可以是相对的。

## Permanent and temporary

## 永久和临时

Is the redirect meant to last or just remain valid for now? If you want a GET to
permanently redirect users to resource B with another GET, send
back a 301. It also means that the user-agent (browser) is meant to cache this
and keep going to the new URI from now on when the original URI is requested.

重定向是持续还是暂时有效？如果你希望一个 `GET` 请求需要通过另一个 `GET` 永久重定向到资源B，请返回 301。这意味着用户代理（浏览器）要缓存这个，并从现在开始所有请求原始 URI 的都直接访问访问新 URI。

The temporary alternative is 302. Right now the server wants the client to
send a GET request to B, but it should not cache this but keep trying the
original URI when directed to it next time.

临时的重定向请返回 302。现在，服务器希望客户端向资源 B 发送 `GET` 请求，但它不应该缓存该请求，而是在下次继续尝试原始 URI。

Note that both 301 and 302 will make browsers do a GET in the next request,
which possibly means changing the method if it started with a POST (and only if
POST). This changing of the HTTP method to GET for 301 and 302 responses is
said to be “for historical reasons”, but that’s still what browsers do so most
of the public web will behave this way.

请注意，301 和 302 都将使浏览器在下一个请求中执行 GET，这可能意味着如果该方法以 POST 开头（并且仅当 POST 开始时），就需要更改该方法。HTTP 方法的这种改变，以获取 301 和 302 响应，据说是由于"历史原因"，但这仍然是浏览器所做的，因此大多数公共 web 都会这样做。

In practice, the 303 code is similar to 302. It will not be cached and it will
make the client issue a GET in the next request. The differences between a 302
and 303 are subtle, but 303 seems to be more designed for an “indirect
response” to the original request rather than just a redirect.

实际上，303 代码类似于 302。它不会被缓存，并且会使客户端在下一个请求中发出 GET。302 和 303 之间的区别很微妙，但 303 似乎更适合于对原始请求的“间接响应”，而不仅仅是重定向。

These three codes were the only redirect codes in the HTTP/1.0 spec.

这三个代码是 HTTP/1.0 规范中规定的重定向代码。

curl however, does not remember or cache any redirects at all so to it,
there is really no difference between permanent and temporary redirects.

不过，curl 不会缓存任何重定向，所以对它来说，永久重定向和临时重定向实际上没有区别。

## Tell curl to follow redirects

## 告诉 curl 遵循重定向

In curl's tradition of only doing the basics unless you tell it differently,
it does not follow HTTP redirects by default. Use the `-L, --location` option
to tell it to do that.

curl 只做基本的事情，默认情况下它不会遵循 HTTP 重定向，除非你用不同的方式告诉它。你需要使用 `-L, --location` 选项来告诉它这样做。

When following redirects is enabled, curl will follow up to 50 redirects by
default. There is a maximum limit mostly to avoid the risk of getting caught in
endless loops. If 50 is not sufficient for you, you can change the maximum
number of redirects to follow with the `--max-redirs` option.

当启用重定向时，默认情况下 curl 支持多达 50 个重定向。有一个最大限度，主要是为了避免陷入无尽循环的风险。如果 50 个还不够，你可以使用 `-max-redirs` 选项更改重定向的最大数量。

## GET or POST?

## GET or POST?

All three of these response codes, 301 and 302/303, will assume that the
client sends a GET to get the new URI, even if the client might have sent a
POST in the first request. This is important, at least if you do something
that does not use GET.

这三个响应代码 301 和 302/303 都假定客户端发送 GET 以获取新的 URI，即使客户端可能在第一个请求中发送了 POST。这很重要，至少如果你做了一些不使用 GET 的事情。

If the server instead wants to redirect the client to a new URI and wants it
to send the same method in the second request as it did in the first, like if
it first sent POST it’d like it to send POST again in the next request, the
server would use different response codes.

如果服务器希望将客户端重定向到一个新的 URI，并希望它在第二个请求中发送与在第一个请求中相同的方法，比如如果它第一次发送 POST，则希望它在下一个请求中再次发送 POST，那么服务器将使用不同的响应代码。

To tell the client “the URI you sent a POST to, is permanently redirected to B
where you should instead send your POST now and in the future”, the server
responds with a 308. And to complicate matters, the 308 code is only recently
defined (the [spec](https://tools.ietf.org/html/rfc7238#section-3) was
published in June 2014) so older clients may not treat it correctly! If so,
then the only response code left for you is…

为了告诉客户端“你发送的 POST 的 URI 将永久重定向到资源 B，你以后的 POST 请求都应该使用新的 URI”，服务器用 308 进行响应。不过麻烦的是，308 代码是最近才定义的 [规范](https://tools.ietf.org/html/rfc7238#section-3) 2014年6月发布），因此老客户端可能无法正确对待它！如果是这样，那么留给你的唯一响应代码是…

The (older) response code to tell a client to send a POST also in the next
request but temporarily is 307. This redirect will not be cached by the client
though, so it’ll again post to A if requested to again. The 307 code was
introduced in HTTP/1.1.

通知客户端在下一个请求中仍然使用 POST 的（旧）响应代码为 307。客户端不会缓存此重定向，因此如果再次请求，它只能先重新发送 POST 到 A。307 代码是在 HTTP/1.1 中引入的。

Oh, and redirects work the same way in HTTP/2 as they do in HTTP/1.1.

哦，重定向在 HTTP/2 中的工作方式和在 HTTP/1.1 中的工作方式相同。

|                     |Permanent | Temporary   |
|---------------------|----------|-------------|
|Switch to GET        | 301      | 302 and 303 |
|Keep original method | 308      | 307         |

|                     |永久      | 临时         |
|---------------------|----------|-------------|
|切换到 GET            | 301      | 302 和 303 |
|保持原始请求          | 308      | 307         |

### Decide what method to use in redirects

### 决定在重定向中使用什么方法

It turns out that there are web services out there in the world that want a
POST sent to the original URL, but are responding with HTTP redirects that use
a 301, 302 or 303 response codes and *still* want the HTTP client to send the
next request as a POST. As explained above, browsers won’t do that and neither
will curl—by default.

事实证明，有一些 web 服务器希望将 POST 发送到原始 URL，但是使用 301、302 或 303 进行 HTTP 重定向响应，并且*仍然*希望 HTTP 客户端将下一个请求用 POST 发送。如上所述，浏览器不会这样做，默认情况下 curl 也不会这样做。

Since these setups exist, and they’re actually not terribly rare, curl offers
options to alter its behavior.

由于存在这些情况，而且它们实际上并不罕见，curl提供了改变其行为的选项。

You can tell curl to not change the non-GET request method to GET after a 30x
response by using the dedicated options for that: `--post301`, `--post302` and
`--post303`. If you are instead writing a libcurl based application, you
control that behavior with the `CURLOPT_POSTREDIR` option.

通过使用专用选项 `--post301`、`--post302` 和 `--post303`，你可以告诉 curl 在收到 30x 响应后，不要将非 GET 请求更改为 GET 请求。如果你正在编写一个基于 libcurl 的应用程序，那么可以使用 `CURLOPT_POSTREDIR` 选项来控制该行为。

## Redirecting to other host names

## 重定向到其他主机名

When you use curl you may provide credentials like user name and password for
a particular site, but since an HTTP redirect might move away to a different
host curl limits what it sends away to other hosts than the original within
the same transfer.

使用 curl 时，可以为特定站点提供用户名和密码等凭据，但由于 HTTP 重定向可能会转移到不同的主机，因此 curl 限制了它在同一传输中发送给其他主机而不是原始主机的内容。

So if you want the credentials to also get sent to the following host names
even though they are not the same as the original—presumably because you
trust them and know that there is no harm in doing that—you can tell curl that
it is fine to do so by using the `--location-trusted` option.

因此，如果你希望凭据也被发送到以下主机名，即使它们与原始主机名不同，这可能是因为你信任它们，并且知道这样做没有什么害处，那么你可以告诉 curl，使用 `--location-trusted` 选项可以这样做。

# Non-HTTP redirects

# 非 HTTP 重定向

Browsers support more ways to do redirects that sometimes make life
complicated to a curl user as these methods are not supported or recognized by
curl.

浏览器支持更多重定向方法，这有时会让 curl 用户的使用变得复杂，因为 curl 不支持或无法识别这些方法。

## HTML redirects

## HTML 重定向

If the above was not enough, the web world also provides a method to redirect
browsers by plain HTML. See the example `<meta>` tag below. This is somewhat
complicated with curl since curl never parses HTML and thus has no knowledge
of these kinds of redirects.

如果以上还不够，那么 web 世界还提供了一种通过普通 HTML 重定向浏览器的方法。请参见下面的 `<meta>` 标记示例。这对于 curl 来说有点复杂，因为 curl 从不解析 HTML，因此不知道这类重定向。

    <meta http-equiv="refresh" content="0; url=http://example.com/">

## JavaScript redirects

## JavaScript 重定向

The modern web is full of JavaScript and as you know, JavaScript is a
language and a full run-time that allows code to execute in the browser when
visiting websites.

现代网络充满了 JavaScript，正如你所知，JavaScript 是一种语言，允许在访问网站时在浏览器中执行的代码。

JavaScript also provides means for it to instruct the browser to move on to
another site—a redirect, if you will.

JavaScript 还为它提供了指示浏览器转到另一个站点的方法——如果愿意的话，这是一种重定向。
