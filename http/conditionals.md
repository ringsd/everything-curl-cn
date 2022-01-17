# Conditionals

# 条件

Sometimes users want to avoid downloading a file again if the same file maybe
already has been downloaded the day before. This can be done by making the
HTTP transfer "conditioned" on something. curl supports two different
conditions: the file timestamp and etag.

有时用户希望避免重复下载前一天刚下载过的文件。可以通过让 HTTP 传输“条件”来实现这个功能。curl 支持两种不同的条件：文件时间戳和 etag。

## Check by modification date

## 检查修改日期

Download the file only if it is newer than a specific date with the use of the
`-z` or `--time-cond` option:

使用 `-z` 或 `--time-cond` 这两个选项之后，仅当文件比特定日期新时才下载：

    curl -z "Jan 10, 2017" https://example.com/file -O

Or the reverse, get the file only if it is older than the specific time by
prefixing the date with a dash:

反之亦然，在日期前面加上短划线，可以实现仅当文件早于特定时间时才获取的功能：

    curl --time-cond "-Jan 10, 2017" https://example.com/file -O

The date parser is liberal and accepts most formats you can write the date
with, and you can also specify it complete with a time:

日期解析器很自由，支持大多数格式，你可以只写日期，也可以指定包含完整时间的格式：

    curl --time-cond "Sun, 12 Sep 2004 15:05:58 -0700" https://www.example.org/file.html

The `-z` option can also extract and use the timestamp from a local file,
which is handy to only download a file if it has been updated remotely:

`-z` 选项还可以从本地文件中提取时间戳，如果文件已远程更新，则仅下载该文件就很方便：

    curl -z file.html https://example.com/file.html -O

It is often useful to combine the use of `-z` with the `--remote-time` flag,
which sets the time of the locally created file to the same timestamp as the
remote file had:

通常将 `-z` 和 `--remote-time` 结合起来使用很有用，通过它可以给本地文件设置一个和远端文件一样的时间戳：

    curl -z file.html -o file.html --remote-time https://example.com/file.html

## Check by modification of content

## 检查修改内容

HTTP servers can return a specific "ETag" for a given resource version. If the
resource at a given URL changes, a new Etag value must be generated, so a
client knows that as long as the ETag remains the same, the content has not
changed.

HTTP 服务器可以通过特定 `ETag` 标签返回资源的版本。如果给定 URL 上的资源发生更改，则必须生成新的 Etag 值，以便客户端知道只要 Etag 保持不变，内容就不会更改。

Using ETags, curl can check for remote updates without having to rely on times
or file dates. It also then makes the check able to detect sub-second changes,
which the timestamp based checks cannot.

通过使用 Etags，curl 可以检查远程文件是否更新，而不必依赖文件时间或日期。同时，还能够检测到亚秒级的变化，而基于时间戳的检查无法检测到亚秒级的变化。

Using curl you can download a remote file and save its ETag (if it provides
any) in a separate "cache" by using the `--etag-save` command line
option. Like this:

使用 curl，你可以下载一个远程文件，并使用 `--etag-save` 选项将 ETag（如果有）保存在单独的缓存中。就像这样：

    curl --etag-save etags.txt https://example.com/file -o output

A subsequent command line can then use that previously saved etag and make
sure to only download the file again if it has changed, like this:

以下命令行可以使用先前保存的 etag，并确保仅在文件发生更改时再次下载该文件，如下所示：

    curl --etag-compare etag.txt https://example.com/file -o output

The two etag options can also be combined in the same command line, so that if
the file actually was updated, curl would save the update ETag again in the
file:

这两个和 etag 相关的选项也可以组合在起使用，如果远程文件已更新，curl 将在保存 etag 的文件中重新保存已更新的 ETag：

    curl --etag-compare etag.txt --etag-save etag.txt \
      https://example.com/file -o output
