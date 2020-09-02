## Charles

charles是MacOS实用的抓包工具，支持 HTTP/HTTPS、socks5代理，可以实现拼装请求、重放请求以及重复请求。可对网络环境进行模拟，mock响应和请求进行修改，fake让 App 通过代理任意改变后端环境。

[下载地址](https://www.charlesproxy.com/)

[破解地址](https://www.zzzmode.com/mytools/charles/)

### http/socks5/https代理

![charles启用http/socks5并配置端口](https://qwq.lsaiah.cn/usr/uploads/Picture/20200629170907.png)

![charles启用https代理](https://qwq.lsaiah.cn/usr/uploads/Picture/20200629171000.png)

开启Windows Proxy并[下载证书](http://www.charlesproxy.com/getssl/)安装：Win+R运行`mmc` 为当前用户添加证书单元并导入到受信任的根证书颁发机构。

![安装证书](https://qwq.lsaiah.cn/usr/uploads/Picture/20200629171549.png)

通过 [浏览器代理扩展程序](https://github.com/FelisCatus/SwitchyOmega) 配置Charles代理服务器：

![](https://qwq.lsaiah.cn/usr/uploads/Picture/20200629172159.png)

启用代理服务器，`Start Recording`和`Start SSL Proxying`后在`Structure`中查看数据包，通过`Filter`过滤数据包，右边为请求和响应的数据包具体内容。右键数据包`Compose`可修改数据包内容并执行。

#### 移动端

手机与charles在同一个网段，设置WIFI代理HTTP Proxy， [下载HTTPS证书](chls.pro/ssl)并信任安装

#### 限速模拟

![](https://qwq.lsaiah.cn/usr/uploads/Picture/20200629173159.png)

#### MOCK断点调试

数据包右键`BreakPoints`开启断点调试或Proxy `Breakpoints Settings`批量添加，再次刷新数据包时将进入断点调试，可修改每一步接口的请求和响应。

![](https://qwq.lsaiah.cn/usr/uploads/Picture/20200629173527.png)

#### MOCK映射

Map Remote将指定的网络请求重定向到另一个网址请求地址

![](https://qwq.lsaiah.cn/usr/uploads/Picture/20200630135127.png)

Map Local是将指定的网络请求重定向到本地文件，修改本地Json文件达到返回内容的目的。

Reverse Proxies反向代理本地到外网

Repeat Advanced压力测试，选择好并发线程数和打压次数，对GET和POST请求进行并发性能测试

#### MOCK重写

Rewrite 功能功能适合对某一类网络请求进行一些正则替换，以达到修改结果的目的，适合批量和长期的替换。