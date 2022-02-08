## Cookies

## Cookies

HTTP cookies are key/value pairs that a client stores on the behalf of a
server. They are sent back in subsequent requests to
allow clients to keep state between requests. Remember that the HTTP protocol
itself has no state but instead the client has to resend all data in subsequent
requests that it wants the server to be aware of.

HTTP Cookies 是由客户端存储服务器的密钥/值对。它们会在后续请求中发回，以允许客户端在请求之间保持状态。请记住，HTTP 协议本身没有状态，但客户端必须在后续请求中重新发送它希望服务器知道的所有数据。

Cookies are set by the server with the `Set-Cookie:` header and with each
cookie the server sends a bunch of extra properties that need to match for the
client to send the cookie back. Like domain name and path and perhaps most
important for how long the cookie should live on.

Cookies 由服务器使用 `Set-Cookie:` 头进行设置，服务器通过每个 Cookie 发送一组额外的属性，这些属性需要与客户端匹配才能将 Cookie 发回。比如域名和路径，也许最重要的是 cookie 的寿命。

The expiry of a cookie is either set to a fixed time in the future (or to live
a number of seconds) or it gets no expiry at all. A cookie without an expire
time is called a "session cookie" and is meant to live during the "session" but not longer. A session in this aspect is typically thought to be the life
time of the browser used to view a site. When you close the browser, you end
your session. Doing HTTP operations with a command-line client that supports
cookies begs the question of when a session really ends…

cookie 的到期时间要么设置为未来的固定时间（或生存数秒），要么根本没有到期时间。没有过期时间的 cookie 被称为 "session cookie"，它应该在整个会话期间一直存在，但不会更长。这方面的会话通常被认为是用于查看网站的浏览器的生命周期。关闭浏览器时，你将结束会话。使用支持 cookie 的命令行客户端执行 HTTP 操作时，会遇到一个问题：会话何时真正结束…

### Cookie engine

### Cookie 引擎

The general concept of curl only doing the bare minimum unless you tell it
differently makes it not acknowledge cookies by default. You need to switch on
"the cookie engine" to make curl keep track of cookies it receives and then
subsequently send them out on requests that have matching cookies.

curl 的一般概念是只做最简单的事情，除非你告诉它不同的地方，否则默认情况下它不会承认 cookie。你需要打开“cookie引擎”，让 curl 跟踪它接收到的cookie，然后在有匹配 cookie 的请求时发送它们。

You enable the cookie engine by asking curl to read or write cookies. If you
tell curl to read cookies from a non-existing file, you will only switch on
the engine but start off with an empty internal cookie store:

通过让 curl 读取或写入 cookie 将启用 cookie 引擎。如果你让 curl 从一个不存在的文件中读取 cookie，你将只打开引擎，然后从一个空的存储中开始 cookie：

    curl -b non-existing http://example.com

Just switching on the cookie engine, getting a single resource and then
quitting would be pointless as curl would have no chance to actually send any
cookies it received. Assuming the site in this example would set cookies and
then do a redirect we would do:

只打开 cookie 引擎，获取一个资源，然后退出是没有意义了。因为 curl 没有机会发送它收到的任何 cookie。假设本例中的站点设置 cookie，然后执行重定向，我们可以执行以下操作：

    curl -L -b non-existing http://example.com

### Reading cookies from file

### 从文件中读取 cookies

Starting off with a blank cookie store may not be desirable. Why not start off
with cookies you stored in a previous fetch or that you otherwise acquired?
The file format curl uses for cookies is called the Netscape cookie format
because it was once the file format used by browsers and then you could easily
tell curl to use the browser's cookies!

从一个空白的 cookie 存储开始可能并不可取。为什么不从之前获取的 cookie 开始呢？
curl 用于 cookie 的文件格式被称为 Netscape cookie 格式，因为它曾经是浏览器使用的文件格式，然后你可以很容易地告诉 curl 使用浏览器的 cookie!

As a convenience, curl also supports a cookie file being a set of HTTP
headers that set cookies. It's an inferior format but may be the only thing
you have.

为了方便起见，curl 还支持一个 cookie 文件，它是一组设置 cookie 的 HTTP 头的文件。这是一种较差的格式，但可能是你唯一拥有的。

Tell curl which file to read the initial cookies from:

告诉 curl 从哪个文件读取初始 cookie：

    curl -L -b cookies.txt http://example.com

Remember that this only *reads* from the file. If the server would update the
cookies in its response, curl would update that cookie in its in-memory store
but then eventually throw them all away when it exits and a subsequent invocation
of the same input file would use the original cookie contents again.

记住，这只会从文件中读取。如果服务器在其响应中更新 cookie，curl 将在其内存存储中更新 cookie，但最终在退出时将其全部丢弃，随后对同一输入文件的调用将再次使用原始 cookie 内容。

### Writing cookies to file

### 将 cookie 写入文件

The place where cookies are stored is sometimes referred to as the "cookie
jar". When you enable the cookie engine in curl and it has received cookies,
you can instruct curl to write down all its known cookies to a file, the
cookie jar, before it exits. It is important to remember that curl only
updates the output cookie jar on exit and not during its lifetime, no matter
how long the handling of the given input takes.

储存 cookie 的地方有时被称为“cookie 罐”。当你在 curl 中启用 cookie 引擎并且它已经收到 cookie 时，你可以指示 curl 在退出之前将其所有已知 cookie 写入一个文件 cookie 罐。重要的是要记住，curl 只在退出时更新输出 cookie 罐，而不是在其生命周期内更新，无论处理给定输入需要多长时间。

You point out the cookie jar output with `-c`:

用 `-c` 指定 cookie 罐的输出：

    curl -c cookie-jar.txt http://example.com

`-c` is the instruction to *write* cookies to a file, `-b` is the instruction
to *read* cookies from a file. Oftentimes you want both.

`-c` 是向文件*写入* cookies 的指令，`-b` 是从文件*读取* cookies 的指令。通常情况下两种你都需要。

When curl writes cookies to this file, it will save all known cookies
including those that are session cookies (without a given lifetime). curl
itself has no notion of a session and it does not know when a session ends so
it will not flush session cookies unless you tell it to.

当 curl 将 cookie 写入该文件时，它将保存所有已知的 cookie，包括会话 cookie（没有给定的生存期）。curl 本身没有会话的概念，也不知道会话何时结束，所以除非您告诉它，否则它不会刷新会话 cookie。

### New cookie session

### 新的 cookie 会话

Instead of telling curl when a session ends, curl features an option that lets
the user decide when a new session *begins*.

curl 没有告诉 curl 会话何时结束，而是提供了一个选项，让用户决定新会话*何时开始*。

A new cookie session means that all the old session cookies will be thrown
away. It is the equivalent of closing a browser and starting it up again.

新的 cookie 会话意味着所有旧的会话 cookie 都将被丢弃。这相当于关闭浏览器并再次启动它。

Tell curl a new cookie session starts by using `-j, --junk-session-cookies`:

告诉 curl 使用 `-j, --junk-session-cookies` 启动新的cookie会话：

    curl -j -b cookies.txt http://example.com/
