## 概要

ab是用于对Apache超文本传输协议（HTTP）服务器进行基准测试的工具。它旨在使您对当前的Apache安装如何执行有一个印象。这尤其向您显示Apache安装每秒能够处理多少个请求。

```
ab [ -A auth-username:password ] [ -b windowsize ] [ -B local-address ] [ -c concurrency ] [ -C cookie-name=value ] [ -d ] [ -e csv-file ] [ -E client-certificate file ] [ -f protocol ] [ -g gnuplot-file ] [ -h ] [ -H custom-header ] [ -i ] [ -k ] [ -l ] [ -m HTTP-method ] [ -n requests ] [ -p POST-file ] [ -P proxy-auth-username:password ] [ -q ] [ -r ] [ -s timeout ] [ -S ] [ -t timelimit ] [ -T content-type ] [ -u PUT-file ] [ -v verbosity] [ -V ] [ -w ] [ -x <table>-attributes ] [ -X proxy[:port] ] [ -y <tr>-attributes ] [ -z <td>-attributes ] [ -Z ciphersuite ] [http[s]://]hostname[:port]/path
# 常用
ab -n 1000 -c 500 https://www.google.com/
```

## 参数

- `-A auth-username:password`

  向服务器提供BASIC身份验证凭据。用户名和密码之间用单个分隔，`:`并通过编码为base64的网络发送。无论服务器是否需要该字符串，都将发送该字符串（*即*，已发送所需的401身份验证）。

- `-b windowsize`

  TCP发送/接收缓冲区的大小，以字节为单位。

- `-B local-address`

  进行传出连接时要绑定的地址。

- `-c concurrency`

  一次执行的多个请求的数量。默认值为一次一个请求。

- `-C cookie-name=value`

  `Cookie:`在请求中添加一行。该参数通常为一 对形式。该字段是可重复的。`name=value`

- `-d`

  不要显示“ XX [ms]表中的投放百分比”。（旧版支持）。

- `-e csv-file`

  编写一个逗号分隔值（CSV）文件，其中包含每个百分比（从1％到100％）的服务时间（以毫秒为单位），该百分比用于处理该百分比的请求。通常，它比“ gnuplot”文件有用。因为结果已经“装箱”了。

- `-E client-certificate-file`

  连接到SSL网站时，请使用提供的PEM格式的客户端证书对服务器进行身份验证。该文件应包含客户端证书，然后是中间证书，然后是私钥。在2.4.36及更高版本中可用。

- `-f protocol`

  指定SSL / TLS协议（SSL2，SSL3，TLS1，TLS1.1，TLS1.2或ALL）。TLS1.1和TLS1.2支持在2.4.4及更高版本中提供。

- `-g gnuplot-file`

  将所有测量值写为“ gnuplot”或TSV（制表符单独值）文件。此文件可以轻松导入到Gnuplot，IDL，Mathematica，Igor甚至Excel等软件包中。标签位于文件的第一行。

- `-h`

  显示使用情况信息。

- `-H custom-header`

  将额外的标头添加到请求。该参数是典型地在一个有效报头线的形式，含有一个冒号分隔的字段值对（*即*，`"Accept-Encoding: zip/zop;8bit"`）。

- `-i`

  做`HEAD`请求，而不是`GET`。

- `-k`

  启用HTTP KeepAlive功能，*即*在一个HTTP会话中执行多个请求。默认为no KeepAlive。

- `-l`

  如果响应的长度不是恒定的，请不要报告错误。这对于动态页面很有用。在2.4.7及更高版本中可用。

- `-m HTTP-method`

  请求的自定义HTTP方法。在2.4.10及更高版本中可用。

- `-n requests`

  为基准测试会话执行的请求数。默认设置是仅执行一个请求，这通常会导致非代表性的基准测试结果。

- `-p POST-file`

  包含要发布的数据的文件。记住也要设置`-T`。

- `-P proxy-auth-username:password`

  在代理途中提供BASIC身份验证凭据。用户名和密码之间用单个分隔，`:`并通过编码为base64的网络发送。不管代理是否需要它都将发送该字符串（*即*，已发送所需的407代理身份验证）。

- `-q`

  当处理150个以上的请求时，每10％或100个左右的请求`ab`输出进度计数`stderr`。该 `-q`标志将禁止显示这些消息。

- `-r`

  不要退出套接字接收错误。

- `-s timeout`

  套接字超时之前要等待的最大秒数。默认值为30秒。在2.4.4及更高版本中可用。

- `-S`

  当平均值和中位数相距标准偏差的一倍或两倍以上时，请勿显示中位数和标准偏差值，也不会显示警告/错误消息。并默认为最小值/平均值/最大值。（旧版支持）。

- `-t timelimit`

  用于基准测试的最大秒数。这意味着 `-n 50000`内部。使用它在固定的总时间内对服务器进行基准测试。默认情况下没有时间限制。

- `-T content-type`

  用于POST / PUT数据的内容类型标头，例如 `application/x-www-form-urlencoded`。默认值为`text/plain`。

- `-u PUT-file`

  包含数据到PUT的文件。记住也要设置`-T`。

- `-v verbosity`

  设置详细级别- `4`上面将在标题上显示信息，`3`上面将在显示响应代码（404、200等）， `2`上面将显示警告和信息。

- `-V`

  显示版本号并退出。

- `-w`

  在HTML表格中打印出结果。默认表是两列宽，带有白色背景。

- `-x <table>-attributes`

  用作的属性的字符串`<table>`。插入属性。`<table here >`

- `-X proxy[:port]`

  使用代理服务器处理请求。

- `-y <tr>-attributes`

  用作的属性的字符串`<tr>`。

- `-z <td>-attributes`

  用作的属性的字符串`<td>`。

- `-Z ciphersuite`

  指定SSL / TLS密码套件（请参阅openssl密码）



## 输出

服务器软件

在第一个成功响应的服务器 HTTP标头中返回的值（如果有）。这包括从头到头的所有字符，从十进制值到32（最值得注意的是：空格或CR / LF）被检测到。

服务器主机名

命令行中提供的DNS或IP地址

服务器端口

ab连接到的端口。如果在命令行上未提供任何端口，则对于HTTP，默认为80；对于HTTP，默认为443。

SSL / TLS协议

客户端和服务器之间协商的协议参数。仅当使用SSL时才打印。

文件路径

从命令行字符串解析请求URI。

文件长度

这是第一个成功返回的文档的大小（以字节为单位）。如果在测试过程中文档长度发生变化，则将响应视为错误。

并发级别

测试期间使用的并发客户端数

测试时间

这是从创建第一个套接字连接到接收到最后一个响应的时间。

完成要求

收到成功回复的数量

请求失败

被视为失败的请求数。如果数字大于零，则将打印另一行，显示由于连接，读取，内容长度错误或异常而失败的请求数。

写错误

写入期间失败的错误数（管道断开）。

非2xx回应

不在200系列响应代码中的响应数。如果所有响应均为200，则不会打印此字段。

保持活动请求

导致保持活动请求的连接数

寄件总数

如果配置为在测试过程中发送数据，则这是测试期间发送的字节总数。如果测试未包含要发送的正文，则将忽略此字段。

转让总额

从服务器接收的字节总数。此数字本质上是通过网络发送的字节数。

HTML已转移

从服务器接收的文档字节总数。此数字不包括HTTP标头中接收的字节

每秒请求

这是每秒的请求数。该值是请求数除以总时间的结果

每个请求的时间

每个请求花费的平均时间。第一个值是用公式计算的，`concurrency * timetaken * 1000 / done` 而第二个值是用公式计算的 `timetaken * 1000 / done`

传输速率

通过公式计算得出的转移率 `totalread / 1024 / timetaken`