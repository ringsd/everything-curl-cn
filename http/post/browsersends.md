# Figure out what a browser sends

# 了解浏览器发送的内容

A common shortcut is to simply fill in the form with your browser and submit
it and check in the browser's network development tools exactly what it sent.

一个常见的方式是，使用浏览器填写表单并提交表单，然后在浏览器的网络开发工具中检查表单发送的内容。（例如 Chrome 按 F12 即可调出开发工具）

A slightly different way is to save the HTML page containing the form, and
then edit that HTML page to redirect the 'action=' part of the form to your
own server or a test server that just outputs exactly what it gets. Completing
that form submission will then show you exactly how a browser sends it.

另一种稍有不同的方法是保存包含表单的 HTML 页面，然后编辑该 HTML 页面，将表单的 `action=` 部分重定向到你自己的服务器或只输出它所得到内容的测试服务器。完成表单提交后，你将清楚地看到浏览器是如何发送表单的。

A third option is, of course, to use a network capture tool such as Wireshark to
check exactly what is sent over the wire. If you are working with HTTPS, you
cannot see form submissions in clear text on the wire but instead you need to
make sure you can have Wireshark extract your TLS private key from your
browser. See the Wireshark documentation for details on doing that.

当然，第三种选择是使用网络捕获工具（如 Wireshark）精确检查通过网络发送的内容。如果你使用的是 HTTPS，则无法在网上看到明文形式的表单提交，但你需要确保 Wireshark 可以从浏览器中提取 TLS 私钥。有关执行此操作的详细信息，请参阅 Wireshark 文档。
