# -d vs -F

# -d vs -F

Previous chapters talked about [regular POST](post.md) and [multipart
formpost](multipart.md), and in your typical command lines you do them with
`-d` or `-F`.

前几章讨论了[常规 POST](POST.md)和[multipart formpost](multipart.md)，介绍了在 curl 的命令行中使用 `-d` 或 `-F` 来执行这些操作。

When do you use which of them?

不过什么时候用哪一种呢？

As described in the chapters mentioned above, both these options send the
specified data to the server. The difference is in how the data is
formatted over the wire. Most of the time, the receiving end is written to
expect a specific format and it expects that the sender formats and sends the
data correctly. A client cannot just pick a format of its own choice.

如上述章节所述，这两个选项都将指定的数据发送到服务器。不同之处在于数据是如何格式化的。大多数情况下，接收端被编写为期望特定的格式，它期望发送方正确地格式化和发送数据。客户机不能只选择自己的格式。

## HTML web forms

## HTML 网页表单

When we are talking browsers and HTML, the standard way is to offer a form to
the user that sends off data when the form has been filled in. The `<form>`
tag is what makes one of those appear on the web page. The tag instructs the
browser how to format its POST. If the form tag includes
`enctype=multipart/form-data`, it tells the browser to send the data as a
[multipart formpost](multipart.md) which you make with curl's `-F`
option. This method is typically used when the form includes a `<input
type=file>` tag, for file uploads.

当我们谈论浏览器和 HTML 时，标准的方法是向用户提供一个表单，在表单填写完毕后发送数据。`<form>` 标记使其出现在网页上。标记指示浏览器如何格式化其 POST 请求。如果表单标记包含 `enctype=multipart/form-data`，则它会告诉浏览器以[multipart formpost](multipart.md)的形式发送数据，curl 中则使用 `-F` 选项进行发送。通常当表单包含用于文件上传的 `<input type=file>` 标记时，会使用这种方式。

The default `enctype` used by forms, which is rarely spelled out in HTML since
it is default, is `application/x-www-form-urlencoded`. It makes the browser
"URL encode" the input as name=value pairs with the data encoded to avoid
unsafe characters. We often refer to that as a [regular POST](post.md),
and you perform one with curl's `-d` and friends.

表单使用的默认 `enctype` 是 `application/x-www-form-urlencoded`，因为它是默认的，所以 HTML 很少会写出来。这种标记下浏览器会将输入的`name=value` 对用“URL 编码”进行编码，以避免不安全的字符。我们通常将其称为[常规 POST](POST.md)，可以用 curl 的 `-d` 选项来实现。

## POST outside of HTML

## HTML 之外的 POST

POST is a regular HTTP method and there is no requirement that it be
triggered by HTML or involve a browser. Lots of services, APIs and other systems
allow you to pass in data these days in order to get things done.

POST 是一种常规的 HTTP 方法，它并不要求由 HTML 触发或着必须涉及到浏览器。现在许多的服务、API 和其他系统都允许通过 POST 来传递数据实现功能。

If these services expect plain "raw" data or perhaps data formatted as JSON or
similar, you want the [regular POST](post.md) approach. curl's `-d`
option does not alter or encode the data at all but will just send exactly
what you tell it to. Just pay attention to -d's default Content-Type as that
might not be what you want.

如果这些服务需要纯数据，或者是 JSON 格式或类似格式的数据，那么你需要参考[常规 POST](POST.md) 里面的方法。curl 的 `-d` 选项不会更改或编码数据，只会准确地发送你告诉它的内容。请注意 `-d` 的选项使用的是默认内容类型(Content-Type)，这可能不是你想要的。
