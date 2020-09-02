## Drone + Github

### 创建一个Github OAuth APP

Settings > Developer settings > OAuth Apps > New OAuth App

![](https://www.lsaiah.cn/usr/uploads/2020/08/2703193624.png)

### 创建共享秘钥

```
openssl rand -hex 16
```

### 拉取Docker drone镜像

```
docker pull drone/drone:1
docker pull drone/drone-runner-docker:1
```

### 配置docker-compose.yml

Linux需安装Docker Compose, Windows和Mac集成了Compose.

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```

配置drone和drone-runner

```
version: '3'

services:
  drone-server:
    image: drone/drone:1
    ports:
      - 8081:80
    volumes:
      - ./drone:/data
    restart: always
    environment:
      - DRONE_SERVER_HOST=${DRONE_SERVER_HOST}
      - DRONE_SERVER_PROTO=${DRONE_SERVER_PROTO}
      - DRONE_RPC_SECRET=${DRONE_RPC_SECRET}

      # GitHub Config
      - DRONE_GITHUB_SERVER=https://github.com
      - DRONE_GITHUB_CLIENT_ID=${DRONE_GITHUB_CLIENT_ID}
      - DRONE_GITHUB_CLIENT_SECRET=${DRONE_GITHUB_CLIENT_SECRET}

      - DRONE_LOGS_PRETTY=true
      - DRONE_LOGS_COLOR=true

  # runner for docker version
  drone-runner:
    image: drone/drone-runner-docker:1
    restart: always
    depends_on:
      - drone-server
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - DRONE_RPC_HOST=${DRONE_RPC_HOST}
      - DRONE_RPC_PROTO=${DRONE_RPC_PROTO}
      - DRONE_RPC_SECRET=${DRONE_RPC_SECRET}
      - DRONE_RUNNER_CAPACITY=3
```

创建环境变量.env文件

```
DRONE_SERVER_HOST=drone.lsaiah.cn
DRONE_SERVER_PROTO=https
DRONE_RPC_SECRET=xxx

DRONE_GITHUB_CLIENT_ID=xxx
DRONE_GITHUB_CLIENT_SECRET=xxx

DRONE_RPC_HOST=drone-server
DRONE_RPC_PROTO=http
```

### 启动并访问

```
docker-compose -f ./docker-compose.yum up -d
```

### 外网域名访问

Nginx反向代理配置

```
server{
	listen 80;
	listen 443 ssl http2
	server name drone.lsaiah.cn
#PROXY-START/
location  ~* \.(php|jsp|cgi|asp|aspx)$
{
    proxy_pass http://127.0.0.1:8081;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
}
location /
{
    proxy_pass http://127.0.0.1:8081;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
  
    add_header X-Cache $upstream_cache_status;
  
    #Set Nginx Cache
  
    	add_header Cache-Control no-cache;
    expires 12h;
}
#PROXY-END/
}
```

### CentOS8 docker 容器网络配置

`sudo nano /etc/firewalld/firewalld.conf`
in config file change
`FirewallBackend=nftables`
on
`FirewallBackend=iptables`
save change and reload firewalld
`sudo systemctl restart firewalld.service`

### Docker Pipelines

1. Authenticate：login sync github repository
2. Enable repository：Settings > Activate repository add webhook notify drone when push code.
3. configure pipeline：Creat `.drone.yml` file to root of git repository.
   
   ```
   kind: pipeline # 定义对象种类为管道，其它种类是Secrets和signature（https://docs.drone.io/signature）
   type: docker # 定义管道的类型为docker，每个管道的步骤都在docker容器内执行，不支持不同类型的管道执行环境（docker,kubernetes,exec,ssh,digital ocean,macstadium）
   name: default # 定义管道的名称
   
   steps: # 定义一系列管道步骤，如果任何步骤失败将立刻退出。
   - name: greeting # 定义管道步骤的名称
     image: alpine # 定义其中执行shell命令的docker镜像
     commands: # 定义在docker容器内作为容器入口点执行shell命令列表，如果任何命令返回非0退出代码则步骤失败。
     - echo hello
     - echo world
   ```
   
   添加多个步骤：
   
   ```
   kind: pipeline
   type: docker
   name: greeting
   
   steps:
   - name: en
     image: alpine
     commands:
     - echo hello world
   
   - name: fr
     image: alpine
     commands:
     - echo bonjour monde
   ```
   
   根据分支或webhook事件限制步骤：
   
   ```
   kind: pipeline
   type: docker
   name: greeting
   
   steps:
   - name: en
     image: alpine
     commands:
     - echo hello world
   
   - name: fr
     image: alpine
     commands:
     - echo bonjour monde
     when:
       branch:
       - develop
   ```
   
   定义多个管道：
   
   ```
   kind: pipeline
   type: docker
   name: en
   
   steps:
   - name: greeting
     image: alpine
     commands:
     - echo hello world
   
   ---
   kind: pipeline
   type: docker
   name: fr
   
   steps:
   - name: greeting
     image: alpine
     commands:
     - echo bonjour monde
   ```
   
   在docker注册表中使用任何图像：
   
   ```
   kind: pipeline
   type: docker
   name: default
   
   steps:
   - name: test
     image: golang:1.13
     commands:
     - go build
     - go test -v
   ```
   
   定义用于集成测试的服务容器：
   
   ```
   kind: pipeline
   type: docker
   name: default
   
   steps:
   - name: test
     image: golang:1.13
     commands:
     - go build
     - go test -v
   
   services:
   - name: redis
     image: redis
   ```
   
   使用插件与第三方系统集成并执行常见的任务：如通知，发布，部署。
   
   ```
   kind: pipeline
   type: docker
   name: default
   
   steps:
   - name: test
     image: golang:1.13
     commands:
     - go build
     - go test -v
   
   - name: notify
     image: plugins/slack
     settings:
       channel: dev
       webhook: https://hooks.slack.com/services/...
   ```
   
   从Secrets中获取敏感参数：
   
   ```
   kind: pipeline
   type: docker
   name: default
   
   steps:
   - name: test
     image: golang:1.13
     commands:
     - go build
     - go test -v
   
   - name: notify
     image: plugins/slack
     settings:
       channel: dev
       webhook:
         from_secret: endpoint
   ```
4. Excute pipelines.

## Drone + Gitea/Gogs
