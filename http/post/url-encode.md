# URL encoding
# URL 编码

Percent-encoding, also known as URL encoding, is technically a mechanism for
encoding data so that it can appear in URLs. This encoding is typically used
when sending POSTs with the `application/x-www-form-urlencoded` content type,
such as the ones curl sends with `--data` and `--data-binary` etc.

百分号编码，也称为 URL 编码，是一种对数据进行编码的机制，以便数据可以显示在 URL 中。这种编码通常在发送带有 `application/x-www-form-urlencoded` 内容类型的 POST 请求中使用，例如 curl 发送带有 `--data` 和 `--data-binary` 等内容类型的 POST。

The command-line options mentioned above all require that you provide properly
encoded data, data you need to make sure already exists in the right format.
While that gives you a lot of freedom, it is also a bit inconvenient at times.

上面提到的命令行选项都要求你提供正确编码的数据，你需要确保数据已经以正确的格式存在。
虽然这给了你很多自由，但有时也有点不方便。

To help you send data you have not already encoded, curl offers the
`--data-urlencode` option. This option offers several different ways to URL
encode the data you give it.

为了帮助你发送尚未编码的数据，curl 提供了 `--data-urlencode` 选项。此选项提供了几种不同的方法对你提供的数据进行 URL 编码。

You use it like `--data-urlencode data` in the same style as the other --data
options. To be CGI-compliant, the **data** part should begin with a name
followed by a separator and a content specification. The **data** part can be
passed to curl using one of the following syntaxes:

你可以像使用其他 --data 选项一样来使用 `--data-urlencode data`。为了兼容 CGI，**data** 部分应以名称开头，后面跟着分隔符和符合规范的内容。可以使用以下语法之一将 **data** 部分传递给 curl：

 - `content`: This will make curl URL encode the content and pass that
   on. Just be careful so that the content does not contain any = or @ symbols,
   as that will then make the syntax match one of the other cases below!

 - `=content`: This will make curl URL encode the content and pass that
   on. The initial '=' symbol is not included in the data.

 - `name=content`: This will make curl URL encode the content part and pass
   that on. Note that the name part is expected to be URL encoded already.

 - `@filename`: This will make curl load data from the given file (including
   any newlines), URL encode that data and pass it on in the POST.

 - `name@filename`: This will make curl load data from the given file
   (including any newlines), URL encode that data and pass it on in the POST.
   The name part gets an equal sign appended, resulting in
   name=urlencoded-file-content. Note that the name is expected to be URL
   encoded already.

 - `content`：curl 对内容进行 URL 编码并发送。只是要小心，内容中不要包含任何 `=` 或 `@` 符号，因为这将它匹配到下面情况之一！

 - `=content`：curl 对内容进行 URL 编码并发送。最开始的 `=` 不被包括在数据中。

 - `name=content`：curl 对内容进行 URL 编码并发送。请注意，`name` 应该已经进行了 URL 编码。

 - `@filename`：curl 从给定文件中（包括任何换行符）加载数据，并对该数据进行 URL 编码然后通过 POST 请求传递。

 - `name@filename`：curl 从给定的文件中（包括任何换行符）加载数据，对该数据进行 URL 编码然后通过 POST 请求传递。`name` 部分附加了一个等号，编码后的结果是 `name=urlencoded-file-content`。请注意，`name` 应该已经进行了 URL 编码。

As an example, you could POST a name to have it encoded by curl:

例如，你可以通过 POST 请求发送一个 `name`，并使用 curl 对其进行编码：

    curl --data-urlencode "name=John Doe (Junior)" http://example.com

…which would send the following data in the actual request body:

…将在实际请求中发送以下数据：

    name=John%20Doe%20%28Junior%29

If you store the string `John Doe (Junior)` in a file named `contents.txt`,
you can tell curl to send that contents URL encoded using the field name
'user' like this:

如果将字符串 `John Doe（Junior）` 存储在名为 `contents.txt` 的文件中，你可以使用以下命令告诉 curl 发送使用 URL 编码并且字段名 `user` 的内容：

    curl --data-urlencode user@contents.txt http://example.com

In both these examples above, the field name is not URL encoded but is passed
on as-is. If you want to URL encode the field name as well, like if you want
to pass on a field name called `user name`, you can ask curl to encode the
entire string by prefixing it with an equals sign (that will not get sent):

在上述两个示例中，字段名不是 URL 编码的，而是按原样传递的。如果你还想对字段名进行 URL 编码，例如，如果你想传递名为 `user name` 的字段名，你可以通过在整个字符串前面加一个 `=` （不会被发送）让 curl 对其进行 URL 编码：

    curl --data-urlencode "=user name=John Doe (Junior)" http://example.com
