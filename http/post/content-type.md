# Content-Type

# 内容类型(Content-Type)

POSTing with curl's `-d` option will make it include a default header that
looks like `Content-Type: application/x-www-form-urlencoded`. That is what
your typical browser will use for a plain POST.

使用 curl 的 `-d` 选项来发送 POST 请求将包含一个 `Content-Type: application/x-www-form-urlencoded` 的默认头。这就是您的典型浏览器用于普通帖子的方式。

Many receivers of POST data do not care about or check the Content-Type
header.

许多 POST 数据的接收者并不关心或检查内容类型(Content-Type)头。

If that header is not good enough for you, you should, of course, replace that
and instead provide the correct one. Such as if you POST JSON to a server and
want to more accurately tell the server about what the content is:

如果该头对你来说是不对的，你应该替换该头，并提供正确的头。例如，如果你将 JSON 数据通过 POST 请求发送服务器，并希望更准确地告诉服务器内容类型是什么：

    curl -d '{json}' -H 'Content-Type: application/json' https://example.com
