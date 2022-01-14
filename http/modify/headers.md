# Customize headers

# 自定义头（Header）

In an HTTP request, after the initial request-line, there will typically
follow a number of request headers. That is a set of `name: value` pairs that
ends with a blank line that separates the headers from the following request
body (that sometimes is empty).

在一个 HTTP 请求的初始请求行之后，通常会有许多请求头。这是一系列的 `name:value` 对，通常以一个空行结束，该空行将头与后续的请求体（有时为空）分隔开。

curl will by default and on its own account pass a few headers in requests,
like for example `Host:`, `Accept:`, `User-Agent:` and a few others that may
depend on what the user asks curl to do.

curl 会有一些默认的请求头添加到请求里，例如`Host:`、`Accept:`、`User Agent:` ，其他就取决于用户要求 curl 添加什么样的请求头。
 
All headers set by curl itself can be overridden, replaced if you will, by the
user. You just then tell curl's `-H` or `--header` the new header to use and
it will then replace the internal one if the header field matches one of those
headers, or it will add the specified header to the list of headers to send in
the request.

用户可以覆盖 curl 本身设置的所有请求头。你只需要通过 curl 的 `-H` 或 `--header` 来告诉 curl 要使用新的请求头，如果新字段与其中的请求头匹配，它将替换内部请求头，如果没有匹配的它会将指定的请头添加到请求要发送的头列表中。

To change the `Host:` header, do this:

要更改 `Host:` ，请执行以下操作：

    curl -H "Host: test.example" http://example.com/

To add a `Elevator: floor-9` header, do this:

要添加 `Elevator: floor-9` ，请执行以下操作：

    curl -H "Elevator: floor-9" http://example.com/

If you just want to delete an internally generated header, just give it to
curl without a value, just nothing on the right side of the colon.

如果您只想删除一个内部生成的请求头，只需将一个不带值得请求头给 curl ，也就是在冒号的右侧没有任何内容。

To switch off the `User-Agent:` header, do this:

要删除 `User-Agent:` ，请执行以下操作：

    curl -H "User-Agent:" http://example.com/

Finally, if you then truly want to add a header with no contents on the right
side of the colon (which is a rare thing), the magic marker for that is to
instead end the header field name with a *semicolon*. Like this:

最后，如果您真的想添加一个在冒号右侧没有内容的请求头（这是一件罕见的事情），那么可以用*分号*这个神奇的标记来结束标题字段名。就像这样：

    curl -H "Empty;" http://example.com

