## 容器生命周期管理

![docker](https://qwq.lsaiah.cn/usr/uploads/Picture/docker.png)

### Docker run

```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
# 使用docker镜像nginx:latest以后台模式启动一个容器,并将容器命名为mynginx
docker run --name mynginx -d nginx:latest
# 使用镜像nginx:latest以后台模式启动一个容器,并将容器的80端口映射到主机随机端口
docker run -P -d nginx:latest
# 使用镜像 nginx:latest，以后台模式启动一个容器,将容器的 80 端口映射到主机的 80 端口,主机的目录 /data 映射到容器的 /data
docker run -p 80:80 -v /data:/data -d nginx:latest
# 绑定容器的 8080 端口，并将其映射到本地主机 127.0.0.1 的 80 端口上。
docker run -p 127.0.0.1:80:8080/tcp ubuntu bash
# 使用镜像nginx:latest以交互模式启动一个容器,在容器内执行/bin/bash命令
docker run -it nginx:latest /bin/bash
```

OPTIONS说明：

- **-a stdin:** 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；
- **-d:** 后台运行容器，并返回容器ID；
- **-i:** 以交互模式运行容器，通常与 -t 同时使用；
- **-P:** 随机端口映射，容器内部端口**随机**映射到主机的高端口
- **-p:** 指定端口映射，格式为：**主机(宿主)端口:容器端口**
- **-t:** 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
- **--name="nginx-lb":** 为容器指定一个名称；
- **--dns 8.8.8.8:** 指定容器使用的DNS服务器，默认和宿主一致；
- **--dns-search example.com:** 指定容器DNS搜索域名，默认和宿主一致；
- **-h "mars":** 指定容器的hostname；
- **-e username="ritchie":** 设置环境变量；
- **--env-file=[]:** 从指定文件读入环境变量；
- **--cpuset="0-2" or --cpuset="0,1,2":** 绑定容器到指定CPU运行；
- **-m :**设置容器使用内存最大值；
- **--net="bridge":** 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
- **--link=[]:** 添加链接到另一个容器；
- **--expose=[]:** 开放一个端口或一组端口；
- **--volume , -v:** 绑定一个卷

### Docker start/stop/restart

启动，停止，重启容器

```shell
docker start [OPTIONS] CONTAINER [CONTAINER...]
docker stop [OPTIONS] CONTAINER [CONTAINER...]
docker rm $(docker ps -a -q)
docker restart [OPTIONS] CONTAINER [CONTAINER...]
```

### Docker kill

杀掉一个运行中的容器

```shell
docker kill [OPTIONS] CONTAINER [CONTAINER...]
```

**-s :**向容器发送一个信号

### Docker rm

删除一个或多个容器

```
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

- **-f :**通过 SIGKILL 信号强制删除一个运行中的容器。
- **-l :**移除容器间的网络连接，而非容器本身。
- **-v :**删除与容器关联的卷。

### Docker pause/unpause

暂停，恢复容器中所有进程

```
docker pause [OPTIONS] CONTAINER [CONTAINER...]
docker unpause [OPTIONS] CONTAINER [CONTAINER...]
```

### Docker create

创建一个新的容器但不启动

```
docker create [OPTIONS] IMAGE [COMMAND] [ARG...]
```

### docker exec

在运行的容器中执行命令

```
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
# 在容器 mynginx 中以交互模式执行容器内 /root/runoob.sh 脚本
docker exec -it mynginx /bin/sh /root/runoob.sh
# 在容器 mynginx 中开启一个交互模式的终端
docker exec -i -t  mynginx /bin/bash
```

- **-d :**分离模式: 在后台运行
- **-i :**即使没有附加也保持STDIN 打开
- **-t :**分配一个伪终端

## 容器操作

### ps

列出容器

```
docker ps [OPTIONS]
```

- **-a :**显示所有的容器，包括未运行的。
- **-f :**根据条件过滤显示的内容。
- **--format :**指定返回值的模板文件。
- **-l :**显示最近创建的容器。
- **-n :**列出最近创建的n个容器。
- **--no-trunc :**不截断输出。
- **-q :**静默模式，只显示容器编号。
- **-s :**显示总的文件大小。

### inspect

获取容器/镜像的元数据

```
docker inspect [OPTIONS] NAME|ID [NAME|ID...]
```

- **-f :**指定返回值的模板文件。
- **-s :**显示总的文件大小。
- **--type :**为指定类型返回JSON。

### top

查看容器中运行的进程信息，支持ps命令参数

```
docker top [OPTIONS] CONTAINER [ps OPTIONS]
```

### attach

连接到正在运行中的容器，要attach上去的容器必须正在运行，可以同时连接上同一个container来共享屏幕（与screen命令的attach类似）

```
docker attach [OPTIONS] CONTAINER
# 容器mynginx将访问日志指到标准输出，连接到容器查看访问信息
```

### events

从服务器获取实时事件

```
docker events [OPTIONS]
```

- **-f ：**根据条件过滤事件；
- **--since ：**从指定的时间戳后显示所有事件;
- **--until ：**流水时间显示到指定的时间为止；

### logs

获取容器的日志

```
docker logs [OPTIONS] CONTAINER
```

- **-f :** 跟踪日志输出
- **--since :**显示某个开始时间的所有日志
- **-t :** 显示时间戳
- **--tail :**仅列出最新N条容器日志

### Wait

阻塞运行直到容器停止，然后打印出它的退出代码

```
docker wait [OPTIONS] CONTAINER [CONTAINER...]
```

### export

将文件系统作为一个tar归档文件导出到STDOUT

```
docker export [OPTIONS] CONTAINER
```

- **-o :**将输入内容写到文件

### port

列出指定的容器的端口映射，或者查找将PRIVATE_PORT NAT到面向公众的端口

```
docker port [OPTIONS] CONTAINER [PRIVATE_PORT[/PROTO]]
```

## 容器rootfs命令

### commit

```
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```

- **-a :**提交的镜像作者；
- **-c :**使用Dockerfile指令来创建镜像；
- **-m :**提交时的说明文字；
- **-p :**在commit时，将容器暂停。

### cp

用于容器与主机之间的数据拷贝

```
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH
```

**-L :**保持源目标中的链接

### diff

检查容器里文件结构的更改

```
docker diff [OPTIONS] CONTAINER
```

## 镜像仓库

### login/logout

登陆/登出到一个Docker镜像仓库，如果未指定镜像仓库地址，默认为官方仓库 Docker Hub

```
docker login [OPTIONS] [SERVER]
docker logout [OPTIONS] [SERVER]
```

- **-u :**登陆的用户名
- **-p :**登陆的密码

### pull

从镜像仓库中拉取或者更新指定镜像

```
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

- **-a :**拉取所有 tagged 镜像
- **--disable-content-trust :**忽略镜像的校验,默认开启

### push

将本地的镜像上传到镜像仓库,要先登陆到镜像仓库

```
docker push [OPTIONS] NAME[:TAG]
```

- **--disable-content-trust :**忽略镜像的校验,默认开启

### search

从Docker Hub查找镜像

```
docker search [OPTIONS] TERM
```

- **--automated :**只列出 automated build类型的镜像；
- **--no-trunc :**显示完整的镜像描述；
- **-s :**列出收藏数不小于指定值的镜像。

## 本地镜像管理

### images

列出本地镜像

```
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

- **-a :**列出本地所有的镜像（含中间映像层，默认情况下，过滤掉中间映像层）；
- **--digests :**显示镜像的摘要信息；
- **-f :**显示满足条件的镜像；
- **--format :**指定返回值的模板文件；
- **--no-trunc :**显示完整的镜像信息；
- **-q :**只显示镜像ID。

### rmi

删除本地一个或多个镜像

```
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

- **-f :**强制删除；
- **--no-prune :**不移除该镜像的过程镜像，默认移除；

### tag

标记本地镜像，将其归入某一仓库

```
docker tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]
```

### build

用于使用 Dockerfile 创建镜像

```
docker build [OPTIONS] PATH | URL | -
# 使用当前目录的 Dockerfile 创建镜像，标签为 runoob/ubuntu:v1
docker build -t runoob/ubuntu:v1 . 
# 使用URL github.com/creack/docker-firefox 的 Dockerfile 创建镜像
docker build github.com/creack/docker-firefox
# 也可以通过 -f Dockerfile 文件的位置
docker build -f /path/to/a/Dockerfile .
```

- **--build-arg=[] :**设置镜像创建时的变量；
- **--cpu-shares :**设置 cpu 使用权重；
- **--cpu-period :**限制 CPU CFS周期；
- **--cpu-quota :**限制 CPU CFS配额；
- **--cpuset-cpus :**指定使用的CPU id；
- **--cpuset-mems :**指定使用的内存 id；
- **--disable-content-trust :**忽略校验，默认开启；
- **-f :**指定要使用的Dockerfile路径；
- **--force-rm :**设置镜像过程中删除中间容器；
- **--isolation :**使用容器隔离技术；
- **--label=[] :**设置镜像使用的元数据；
- **-m :**设置内存最大值；
- **--memory-swap :**设置Swap的最大值为内存+swap，"-1"表示不限swap；
- **--no-cache :**创建镜像的过程不使用缓存；
- **--pull :**尝试去更新镜像的新版本；
- **--quiet, -q :**安静模式，成功后只输出镜像 ID；
- **--rm :**设置镜像成功后删除中间容器；
- **--shm-size :**设置/dev/shm的大小，默认值是64M；
- **--ulimit :**Ulimit配置。
- **--tag, -t:** 镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签。
- **--network:** 默认 default。在构建期间设置RUN指令的网络模式

### history

查看指定镜像的创建历史

```
docker history [OPTIONS] IMAGE
```

- **-H :**以可读的格式打印镜像大小和日期，默认为true；
- **--no-trunc :**显示完整的提交记录；
- **-q :**仅列出提交记录ID。

### save

将指定镜像保存成 tar 归档文件

```
docker save [OPTIONS] IMAGE [IMAGE...]
```

- **-o :**输出到的文件

### load

导入docker save命令导出的镜像

```
docker load [OPTIONS]
```

- **--input , -i :** 指定导入的文件，代替 STDIN。
- **--quiet , -q :** 精简输出信息。

### import

从归档文件中创建镜像

```
docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]
```

- **-c :**应用docker 指令创建镜像；
- **-m :**提交时的说明文字。

### info/version

显示 Docker 系统信息，包括镜像和容器数

显示Docker版本信息

```
docker info [OPTIONS]
docker version [OPTIONS]
```

## 客户端命令

### 选项

- `--config=""`：指定客户端配置文件，默认为 `~/.docker`；
- `-D=true|false`：是否使用 debug 模式。默认不开启；
- `-H, --host=[]`：指定命令对应 Docker 守护进程的监听接口，可以为 unix 套接字 `unix:///path/to/socket`，文件句柄 `fd://socketfd` 或 tcp 套接字 `tcp://[host[:port]]`，默认为 `unix:///var/run/docker.sock`；
- `-l, --log-level="debug|info|warn|error|fatal"`：指定日志输出级别；
- `--tls=true|false`：是否对 Docker 守护进程启用 TLS 安全机制，默认为否；
- `--tlscacert=/.docker/ca.pem`：TLS CA 签名的可信证书文件路径；
- `--tlscert=/.docker/cert.pem`：TLS 可信证书文件路径；
- `--tlscert=/.docker/key.pem`：TLS 密钥文件路径；
- `--tlsverify=true|false`：启用 TLS 校验，默认为否

### 命令

可以通过 `docker COMMAND --help` 来查看这些命令的具体用法。

- `attach`：依附到一个正在运行的容器中；
- `build`：从一个 Dockerfile 创建一个镜像；
- `commit`：从一个容器的修改中创建一个新的镜像；
- `cp`：在容器和本地宿主系统之间复制文件中；
- `create`：创建一个新容器，但并不运行它；
- `diff`：检查一个容器内文件系统的修改，包括修改和增加；
- `events`：从服务端获取实时的事件；
- `exec`：在运行的容器内执行命令；
- `export`：导出容器内容为一个 `tar` 包；
- `history`：显示一个镜像的历史信息；
- `images`：列出存在的镜像；
- `import`：导入一个文件（典型为 `tar` 包）路径或目录来创建一个本地镜像；
- `info`：显示一些相关的系统信息；
- `inspect`：显示一个容器的具体配置信息；
- `kill`：关闭一个运行中的容器 (包括进程和所有相关资源)；
- `load`：从一个 tar 包中加载一个镜像；
- `login`：注册或登录到一个 Docker 的仓库服务器；
- `logout`：从 Docker 的仓库服务器登出；
- `logs`：获取容器的 log 信息；
- `network`：管理 Docker 的网络，包括查看、创建、删除、挂载、卸载等；
- `node`：管理 swarm 集群中的节点，包括查看、更新、删除、提升/取消管理节点等；
- `pause`：暂停一个容器中的所有进程；
- `port`：查找一个 nat 到一个私有网口的公共口；
- `ps`：列出主机上的容器；
- `pull`：从一个Docker的仓库服务器下拉一个镜像或仓库；
- `push`：将一个镜像或者仓库推送到一个 Docker 的注册服务器；
- `rename`：重命名一个容器；
- `restart`：重启一个运行中的容器；
- `rm`：删除给定的若干个容器；
- `rmi`：删除给定的若干个镜像；
- `run`：创建一个新容器，并在其中运行给定命令；
- `save`：保存一个镜像为 tar 包文件；
- `search`：在 Docker index 中搜索一个镜像；
- `service`：管理 Docker 所启动的应用服务，包括创建、更新、删除等；
- `start`：启动一个容器；
- `stats`：输出（一个或多个）容器的资源使用统计信息；
- `stop`：终止一个运行中的容器；
- `swarm`：管理 Docker swarm 集群，包括创建、加入、退出、更新等；
- `tag`：为一个镜像打标签；
- `top`：查看一个容器中的正在运行的进程信息；
- `unpause`：将一个容器内所有的进程从暂停状态中恢复；
- `update`：更新指定的若干容器的配置信息；
- `version`：输出 Docker 的版本信息；
- `volume`：管理 Docker volume，包括查看、创建、删除等；
- `wait`：阻塞直到一个容器终止，然后输出它的退出符。

## 服务端命令

### 选项

- `--api-cors-header=""`：CORS 头部域，默认不允许 CORS，要允许任意的跨域访问，可以指定为 "*"；
- `--authorization-plugin=""`：载入认证的插件；
- `-b=""`：将容器挂载到一个已存在的网桥上。指定为 `none` 时则禁用容器的网络，与 `--bip` 选项互斥；
- `--bip=""`：让动态创建的 `docker0` 网桥采用给定的 CIDR 地址; 与 `-b` 选项互斥；
- `--cgroup-parent=""`：指定 cgroup 的父组，默认 fs cgroup 驱动为 `/docker`，systemd cgroup 驱动为 `system.slice`；
- `--cluster-store=""`：构成集群（如 `Swarm`）时，集群键值数据库服务地址；
- `--cluster-advertise=""`：构成集群时，自身的被访问地址，可以为 `host:port` 或 `interface:port`；
- `--cluster-store-opt=""`：构成集群时，键值数据库的配置选项；
- `--config-file="/etc/docker/daemon.json"`：daemon 配置文件路径；
- `--containerd=""`：containerd 文件的路径；
- `-D, --debug=true|false`：是否使用 Debug 模式。缺省为 false；
- `--default-gateway=""`：容器的 IPv4 网关地址，必须在网桥的子网段内；
- `--default-gateway-v6=""`：容器的 IPv6 网关地址；
- `--default-ulimit=[]`：默认的 ulimit 值；
- `--disable-legacy-registry=true|false`：是否允许访问旧版本的镜像仓库服务器；
- `--dns=""`：指定容器使用的 DNS 服务器地址；
- `--dns-opt=""`：DNS 选项；
- `--dns-search=[]`：DNS 搜索域；
- `--exec-opt=[]`：运行时的执行选项；
- `--exec-root=""`：容器执行状态文件的根路径，默认为 `/var/run/docker`；
- `--fixed-cidr=""`：限定分配 IPv4 地址范围；
- `--fixed-cidr-v6=""`：限定分配 IPv6 地址范围；
- `-G, --group=""`：分配给 unix 套接字的组，默认为 `docker`；
- `-g, --graph=""`：Docker 运行时的根路径，默认为 `/var/lib/docker`；
- `-H, --host=[]`：指定命令对应 Docker daemon 的监听接口，可以为 unix 套接字 `unix:///path/to/socket`，文件句柄 `fd://socketfd` 或 tcp 套接字 `tcp://[host[:port]]`，默认为 `unix:///var/run/docker.sock`；
- `--icc=true|false`：是否启用容器间以及跟 daemon 所在主机的通信。默认为 true。
- `--insecure-registry=[]`：允许访问给定的非安全仓库服务；
- `--ip=""`：绑定容器端口时候的默认 IP 地址。缺省为 `0.0.0.0`；
- `--ip-forward=true|false`：是否检查启动在 Docker 主机上的启用 IP 转发服务，默认开启。注意关闭该选项将不对系统转发能力进行任何检查修改；
- `--ip-masq=true|false`：是否进行地址伪装，用于容器访问外部网络，默认开启；
- `--iptables=true|false`：是否允许 Docker 添加 iptables 规则。缺省为 true；
- `--ipv6=true|false`：是否启用 IPv6 支持，默认关闭；
- `-l, --log-level="debug|info|warn|error|fatal"`：指定日志输出级别；
- `--label="[]"`：添加指定的键值对标注；
- `--log-driver="json-file|syslog|journald|gelf|fluentd|awslogs|splunk|etwlogs|gcplogs|none"`：指定日志后端驱动，默认为 `json-file`；
- `--log-opt=[]`：日志后端的选项；
- `--mtu=VALUE`：指定容器网络的 `mtu`；
- `-p=""`：指定 daemon 的 PID 文件路径。缺省为 `/var/run/docker.pid`；
- `--raw-logs`：输出原始，未加色彩的日志信息；
- `--registry-mirror=<scheme>://<host>`：指定 `docker pull` 时使用的注册服务器镜像地址；
- `-s, --storage-driver=""`：指定使用给定的存储后端；
- `--selinux-enabled=true|false`：是否启用 SELinux 支持。缺省值为 false。SELinux 目前尚不支持 overlay 存储驱动；
- `--storage-opt=[]`：驱动后端选项；
- `--tls=true|false`：是否对 Docker daemon 启用 TLS 安全机制，默认为否；
- `--tlscacert=/.docker/ca.pem`：TLS CA 签名的可信证书文件路径；
- `--tlscert=/.docker/cert.pem`：TLS 可信证书文件路径；
- `--tlscert=/.docker/key.pem`：TLS 密钥文件路径；
- `--tlsverify=true|false`：启用 TLS 校验，默认为否；
- `--userland-proxy=true|false`：是否使用用户态代理来实现容器间和出容器的回环通信，默认为 true；
- `--userns-remap=default|uid:gid|user:group|user|uid`：指定容器的用户命名空间，默认是创建新的 UID 和 GID 映射到容器内进程

## 镜像加速

### Docker Toolbox（win10以下）

```
docker-machine ssh default
sudo vi /var/lib/boot2docker/profile
# 在–label provider=virtualbox下添加
--registry-mirror https://e7zsfk4b.mirror.aliyuncs.com
docker-machine restart default
docker info
或者
docker-machine create --engine-registry-mirror=https://e7zsfk4b.mirror.aliyuncs.com -d virtualbox default
查看配置到本地访问服务
docker-machine env default
eval "$(docker-machine env default)"
docker info
```

### Docker for Windows（win10）

```
Settings Docker Daemon添加JSON字符串：
{
  "registry-mirrors": ["https://e7zsfk4b.mirror.aliyuncs.com"]
}
```

### CentOS、Ubuntu

```
过修改daemon配置文件/etc/docker/daemon.json:
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://e7zsfk4b.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 安装卸载

### CentOS 7

```
sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo (腾讯云镜像源：https://download.docker.com/linux/centos/docker-ce.repo)
或下载RHEL镜像文件
wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo
替换仓库地址
sudo sed -i 's+download.docker.com+mirrors.cloud.tencent.com/docker-ce+' /etc/yum.repos.d/docker-ce.repo
安装docker
yum install docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
卸载
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### CentOS 8

RHEL8弃用了yum，使用dnf包管理器

```
dnf update
dnf install yum-utils
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo(https://download.docker.com/linux/centos/docker-ce.repo)
安装依赖
dnf install https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm
dnf install docker-ce docker-ce-cli
systemctl enable --now docker containerd
卸载
dnf remove docker*
```

## 资源链接

### 官方网站

- Docker 官方主页：https://www.docker.com
- Docker 官方博客：https://www.docker.com/blog/
- Docker 官方文档：https://docs.docker.com/
- Docker Hub：https://hub.docker.com
- Docker 的源代码仓库：https://github.com/moby/moby
- Docker 路线图 https://github.com/docker/roadmap/projects
- Docker 发布版本历史：https://docs.docker.com/release-notes/
- Docker 常见问题：https://docs.docker.com/engine/faq/
- Docker 远端应用 API：https://docs.docker.com/develop/sdk/

### 实践参考

- Dockerfile 参考：https://docs.docker.com/engine/reference/builder/
- Dockerfile 最佳实践：https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/

### 技术交流

- Docker 邮件列表： https://groups.google.com/forum/#!forum/docker-user
- Docker 的 IRC 频道：https://chat.freenode.net#docker
- Docker 的 Twitter 主页：https://twitter.com/docker

### 其它

- Docker 的 StackOverflow 问答主页：https://stackoverflow.com/search?q=docker