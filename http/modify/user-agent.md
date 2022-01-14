# User-agent
# 用户代理

The User-Agent is a header that each client can set in the request to inform
the server which user-agent it is. Sometimes servers will look at this header
and determine how to act based on its contents.

每个客户端都可以在请求中设置用户代理头，用来通知服务器它是哪个用户代理。有时，服务器会查看此标头并根据其内容确定如何操作。

The default header value is 'curl/[version]', as in `User-Agent: curl/7.54.1`
for curl version 7.54.1.

默认请求头为 `curl/[version]`，如 curl 版本 7.54.1 的用户代理头为 `User-Agent: curl/7.54.1` 。
 
You can set any value you like, using the option `-A` or `--user-agent` plus
the string to use or, as it's just a header, `-H "User-Agent: foobar/2000"`.

你可以使用 `-A` 或 `--user-agent` 加上要使用的字符串，或者通过 `-H "User-Agent: foobar/2000"` 来设置任何你喜欢的值。

As comparison, a test version of Firefox on a Linux machine once sent this
User-Agent header:

作为比较，一个在 Linux 机器上运行的 Firefox 测试版本发送了以下用户代理头：

`User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:58.0) Gecko/20100101 Firefox/58.0`
