# Referer
# 来源

When a user clicks on a link on a web page and the browser takes the user away
to the next URL, it will send the new URL a `Referer:` header in the new
request telling it where it came from. That is the referer header. The
`Referer:` is misspelled but that is how it is supposed to be!

当用户单击网页上的链接时，浏览器会将用户带到下一个 URL ，它会在新请求中向新 URL 发送一个 `Referer:` 头，告诉用户该 URL 来自何处。这个就是referer 头的作用。`Referer:` 这个单词拼错了，但是它这样被支持的！

With curl you set the referer header with `-e` or `--referer`, like this:

在 curl 中可以通过 `-e` 或 `--referer` 来设置 referer 头，如下所示：

    curl --referer http://comes-from.example.com https://www.example.com/

Tips:

[`Referer:`拼错小故事](https://imququ.com/post/referrer-or-referer.html)
