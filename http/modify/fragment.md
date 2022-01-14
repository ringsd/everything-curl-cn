# Anchors or fragments
# 锚或片段

A URL may contain an "anchor", also known as a fragment, which is written with
a pound sign and string at the end of the URL. Like for example
`http://example.com/foo.html#here-it-is`. That fragment part, everything from
the pound/hash sign to the end of the URL, is only intended for local use and
will not be sent over the network. curl will simply strip that data off and
discard it.

URL 可能包含一个“锚”，也称为片段，在 URL 的末尾用 `#` 号和字符串写入。比如说 `http://example.com/foo.html#here-it-is`。该片段部分，从 `#` 到URL结尾的所有内容，仅用于本地使用，不会通过网络发送。curl 只是将简单的将数据剥离丢弃。
