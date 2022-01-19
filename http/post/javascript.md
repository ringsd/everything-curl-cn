# JavaScript and forms

# JavaScript 和表单

A common mitigation against automated agents or scripts using curl is to have
the page with the HTML form use JavaScript to set values of some input fields,
usually one of the hidden ones. Often, there is some JavaScript code that
executes on page load or when the submit button is pressed which sets a magic
value that the server then can verify before it considers the submission to be
valid.

针对使用 curl 的自动化代理或脚本，一种常见的保护措施是让带有 HTML 表单的页面使用 JavaScript 来设置一些隐藏字段的值。通常在页面加载或着按下提交按钮时会执行一些 JavaScript 代码，然后通过它设置一些值，服务器可以通过检查这些内容来决定提交是不是有效。

You can usually work around that by just reading the JavaScript code and
redoing that logic in your script. Using the tricks in [Figure out what a
browser sends](browsersends.md) to check exactly what a browser sends is then
also a good help.

你可以通过阅读 JavaScript 代码并在脚本中重做这部分逻辑来解决这个问题。不过使用 [了解浏览器发送的内容](browsersends.md) 中的技巧查看浏览器发送的内容也是一个好办法。
