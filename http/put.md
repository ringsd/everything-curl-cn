## PUT

## PUT

The difference between a PUT and a POST is subtle. They are virtually identical
transmissions except for the different method strings. Where POST is meant to
pass on data to a remote resource, PUT is supposed to be the new version of
that resource.

PUT 和 POST 之间的区别很微妙。除了不同的方法字符串，它们实际上是相同的传输。POST 的目的是将数据传递到远程资源，PUT 应该是该资源的新版本。

In that aspect, PUT is similar to good old standard file upload found in other
protocols. You upload a new version of the resource with PUT. The URL
identifies the resource and you point out the local file to put there:

在这方面，PUT 类似于在其他协议中找到的良好的旧标准文件上传。你可以使用 PUT 上传资源的新版本。URL 标识资源，并指出要放在那里的本地文件：

    curl -T localfile http://example.com/new/resource/file

…so -T will imply a PUT and tell curl which file to send off. But the
similarities between POST and PUT also allows you to send a PUT with a string
by using the regular curl POST mechanism using `-d` but asking for it to use a
PUT instead:

…`-T` 参数将暗示 curl 通过 PUT 即发送哪个文件。但是 POST 和 PUT 之间的相似性还允许你使用常规的 curl POST 机制，使用 `-d` 发送带有字符串的请求，但要求它使用PUT：

    curl -d "data to PUT" -X PUT http://example.com/new/resource/file
