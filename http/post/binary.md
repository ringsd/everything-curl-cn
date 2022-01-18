# Posting binary
# POST 二进制文件

When reading data to post from a file, `-d` will strip out carriage return and
newlines. Use `--data-binary` if you want curl to read and use the given file
in binary exactly as given:

从文件中读取要发送的数据时，使用 `-d` 将去掉回车和换行。如果你想 curl 完全按照给定的二进制文件来读取和使用，请使用 `--data-binary`：

    curl --data-binary @filename http://example.com/
