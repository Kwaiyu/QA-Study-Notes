# 无限专业版

1. 从[Download Link](https://plugins.zhile.io/files/ide-eval-resetter-2.1.9.zip)下载并安装插件。
   - `https://plugins.zhile.io`手动添加“自定义插件存储库” （`Settings/Preferences`-> `Plugins`）
   - 搜索并安装插件： `IDE Eval Reset`
2. 单击`Help`或`Get Help`->`Eval Reset`菜单。
3. 单击`Reset`->`Yes`按钮。
4. 重新启动您的IDE。
5. 现在您还有30天的评估时间:)
6. 有关更多信息，请访问[此处](https://zhile.io/2020/11/18/jetbrains-eval-reset.html)。

# 调试运行

设置好断点，debug运行，然后 F8 单步调试，遇到想进入的函数 F7 进去，想出来在 shift + F8，跳过不想看的地方，直接设置下一个断点，然后 F9 过去。看这张图就行了（下面第6点有误，应该是运行到光标处，而不是下一断点处）

![image3](https://qwq.lsaiah.cn/picgo/20200823143211.png)

![](https://qwq.lsaiah.cn/picgo/20200823143535.png)

程序结束调试，修改变量，如调试正则表达式：

![image-20201223230113178](https://qwq.lsaiah.cn/picgo/image-20201223230113178.png)

![](https://qwq.lsaiah.cn/picgo/image-20201223230513639.png)

远程调试：

## 1. 新建一个项目

首先，要在Pycharm中新建一个空的项目，后面我们拉服务器上的项目代码就会放置在这个项目目录下。我这边的名字是 NOVA，你可以自己定义。

![image1](http://image.iswbm.com/20190113104817.png)

## 2. 配置连接服务器

Tools -> Deployment -> configuration

![image2](http://image.iswbm.com/20190113105512.png)

添加一个`Server`

- Name：填你的服务器的IP
- Type：设定为SFTP

![image3](http://image.iswbm.com/20190113105858.png)

点击`OK`后，进入如下界面，你可以按我的备注，填写信息：

- SFTP host：公网ip
- Port：服务器开放的ssh端口
- Root path：你要调试的项目代码目录
- Username：你登陆服务器所用的用户
- Auth type：登陆类型，若用密码登陆的就是Password
- Password：选密码登陆后，这边输入你的登陆密码，可以选择保存密码。

这里请注意，要确保你的电脑可以ssh连接到你的服务器，不管是密钥登陆还是密码登陆，如果开启了白名单限制要先解除。

![image4](http://image.iswbm.com/20190113105931.png)

填写完成后，切换到`Mappings`选项卡，在箭头位置，填写`\`

![image5](http://image.iswbm.com/20190113110928.png)

以上服务器信息配置，全部正确填写完成后，点击`OK`

接下来，我们要连接远程服务器了。 Tools -> Deployment -> Browse Remote Host

![image6](http://image.iswbm.com/20190113111042.png)

## 3. 下载项目代码

如果之前填写的服务器登陆信息准确无误的话，现在就可以看到远程的项目代码。

![image7](http://image.iswbm.com/20190113111151.png)

选择下载远程代码要本地。

![image8](http://image.iswbm.com/20190113111217.png)

下载完成提示。

![image9](http://image.iswbm.com/20190113111248.png)

现在的IDE界面应该是这样子的。

![image10](http://image.iswbm.com/20190113111307.png)

## 4. 下载远程解释器

为什么需要这步呢？

远程调试是在远端的服务器上运行的，它除了依赖其他组件之外，还会有一些很多Python依赖包我们本地并没有。

进入 File -> Settings 按图示，添加远程解释器。

![image11](http://image.iswbm.com/20190113111747.png)

填写远程服务器信息，跟之前的一样，不再赘述。

![image12](http://image.iswbm.com/20190113111828.png)

点击`OK`后，会自动下载远程解释器。如果你的项目比较大，这个时间可能会比较久，请耐心等待。

## 5. 添加程序入口

因为我们要在本地DEBUG，所以你一定要知道你的项目的入口程序。如果这个入口程序已经包含在你的项目代码中，那么请略过这一步。

如果没有，就请自己生成入口程序。

比如，我这边的项目，在服务器上是以一个服务运行的。而我们都知道服务的入口是`Service文件`。 `cat /usr/lib/systemd/system/openstack-nova-compute.service`

```
[Unit]
Description=OpenStack Nova Compute Server
After=syslog.target network.target libvirtd.service

[Service]
Environment=LIBGUESTFS_ATTACH_METHOD=appliance
Type=notify
NotifyAccess=all
TimeoutStartSec=0
Restart=always
User=nova
ExecStart=/usr/bin/nova-compute

[Install]
WantedBy=multi-user.target
```

看到那个`ExecStart`没有？那个就是我们程序的入口。 我们只要将其拷贝至我们的Pycharm中，并向远程同步该文件。

![image13](http://image.iswbm.com/20190113112004.png)

## 6. 调试前设置

开启代码自动同步，这样，我们对代码的修改Pycharm都能识别，并且为我们提交到远程服务器。

![image14](http://image.iswbm.com/20190113112055.png)

开启 `Gevent compatible`，如果不开启，在调试过程中，很可能出现无法调试，或者无法追踪/查看变量等问题。

![image15](http://image.iswbm.com/20190113113211.png)

## 7. 开始调试代码

在你的程序入口文件处，点击右键，选择Debug即可。

如果你的程序入口，需要引入参数，这是经常有的事，可以的这里配置。

![image16](http://image.iswbm.com/20190113112456.png)

配置完点击保存即可。

![image17](http://image.iswbm.com/20190113112649.png)



# 界面排版

# 代码编辑

# 快捷与效率

# 搜索与导航

# 版本与管理

# 插件与工具

# 常用技巧