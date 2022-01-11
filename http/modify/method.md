# 修改请求方法(Method)

HTTP 请求的第一行包括*方法* —— 有时也称为动词。一个简单 GET 请求，可以执行如下命令行：

    curl http://example.com/file

HTTP 请求如下所示：

    GET /file HTTP/1.1

你可以使用 `-X` 或 `--request` 命令行来告诉 curl 将该方法更改为其他方法，参数后面跟实际的方法名称即可。
例如，你可以用 `DELETE` 请求来替代 `GET` 请求，命令如下所示：

    curl http://example.com/file -X DELETE

请求被修改为：

    DELETE /file HTTP/1.1

此命令行选项仅更改传出请求中的文本，不更改任何行为。
这一点尤其重要，例如，如果你要求 curl 发送带有 `-X` 的 HEAD 请求，因为 HEAD 是类似于 GET 的请求，只不过返回的响应中*没有*具体的内容，主要用来用于获取报头。因此，将 `-X HEAD` 添加到本可以正常执行 GET 的命令里将导致 curl 被挂起，因为它会等待不会出现的响应体。


当要求 curl 执行 HTTP 传输时，它将根据选项选择正确的方法，因此你应该很少需要使用 `-X` 来显式地要求。还应该注意的是，当在 curl 命令中使用 `-L` 重定向的时候，后续的请求也都会使用上 `-X` 设置的请求方法。

[HTTP 请求参考]([versions.md](https://www.runoob.com/http/http-methods.html))。
