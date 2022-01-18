# 简易 POST

When the data is sent by a browser after data have been filled in a form, it
will send it URL encoded, as a serialized name=value pairs separated with
ampersand symbols (`&`). You send such data with curl's `-d` or `--data`
option like this:

当数据在表单中填充后由浏览器发送时，它将以 URL 编码的形式发送数据，以 `name=value` 这样的数据对来序列化，用符号(`&`)分隔。curl 中使用 `-d` 或 `--data` 选项发送此类数据，如下所示：

    curl -d 'name=admin&shoesize=12' http://example.com/

When specifying multiple `-d` options on the command line, curl will
concatenate them and insert ampersands in between, so the above example could
also be made like this:

在命令行上指定多个 `-d` 选项时，curl 会将它们连接起来，并在它们之间插入 `&` 符号，因此上面的示例也可以如下所示：

    curl -d name=admin -d shoesize=12 http://example.com/

If the amount of data to send is not really fit to put in a mere string on the
command line, you can also read it off a file name in standard curl style:

如果实际要发送的数据量不适合在命令行中放一个字符串，你也可以通过传入标准 curl 样式的文件名，从文件中读取数据：

    curl -d @filename http://example.com

While the server might assume that the data is encoded in some special way,
curl does not encode or change the data you tell it to send. **curl sends
exactly the bytes you give it**.

虽然服务器可能假定数据是以某种特殊方式编码的，但 curl 不会对需要发送的数据进行编码或更改。**curl 发送的字节数与你给的完全相同**。

To send a POST body that starts with a `@` symbol, to avoid that curl tries to
load that as a file name, use `--data-raw` instead. This option has no file
loading capability:

要发送以 `@` 符号开始的 POST 请求，避免 curl 试图将其作为文件名去加载，请使用 `--data-raw`。使用该选项就没有文件加载功能：

    curl --data-raw '@string' https://example.com
