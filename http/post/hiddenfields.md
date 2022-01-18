# Hidden form fields

# 隐藏表单字段

Sending a post with `-d` is the equivalent of what a browser does when an HTML
form is filled in and submitted.

发送带有 `-d` 选项的 POST 相当于用浏览器填写和提交 HTML 表单时所做的操作。

Submitting such forms is a common operation with curl; effectively, to have
curl fill in a web form in an automated fashion.

用 curl 提交此类表单是常见的操作；让 curl 以自动化的方式填充 web 表单。

If you want to submit a form with curl and make it look as if it has been done
with a browser, it is important to provide all the input fields from the
form. A common method for web pages is to set a few hidden input fields to the
form and have them assigned values directly in the HTML. A successful form
submission, of course, also includes those fields and in order to do that
automatically you may be forced to first download the HTML page that holds the
form, parse it, and extract the hidden field values so that you can send them
off with curl.

如果你希望用 curl 提交表单，并使其看起来像是通过浏览器完成的，那么提供表单中的所有输入字段非常重要。web 页面的一种常见方法是为表单设置一些隐藏的输入字段，并直接在 HTML 中为它们赋值。因此，一个成功的表单提交还包括这些字段，为了自动完成这一任务，你可能会被迫首先下载保存表单的 HTML 页面，对其进行解析，并提取隐藏的字段值，以便使用 curl 发送它们。
