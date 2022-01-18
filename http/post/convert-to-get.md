# Convert to GET

# 转换为 GET

A little convenience feature that could be suitable to mention in this context
is the `-G` or `--get` option, which takes all data you have specified with
the different `-d` variants and appends that data to the inputted URL
e.g. ```http://example.com``` separated with a '?' and then makes curl send a
GET instead.

在本文中可以提到的一个方便的特性是 `-G` 或 `--get` 选项，它获取您使用不同的 `-d` 变量指定的所有数据，并将这些数据附加到输入的 URL 中，例如```http://example.com``` 用 '?' 分隔然后让 curl 发送一个 GET 请求来代替。
 
This option makes it easy to switch between POSTing and GETing a form, for
example.

例如，此选项可以轻松地在 POST 和 GET 之间切换。
 
An example that adds an encoded piece of data as a query in the URL:

在 URL 中添加编码数据段作为查询的示例：

    curl -G --data-urlencode "name=daniel stenberg" https://example.com/
