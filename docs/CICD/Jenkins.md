## 安装

```
docker pull jenkinsci/blueocean
docker run --name jenkins-blueocean -u root --rm -d -p 8080:8080 -p 50000:50000 -v jenkins-data:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkinsci/blueocean
docker logs jenkins-blueocean
docker exec -it jenkins-blueocean bash
cat /var/jenkins_home/secrets/initialAdminPassword
541d88021b5a4f0b85927cfb8e81311a
```

### 配置

配置插件代理，安装更新插件

Plugin Manager > Advanced > Update Site

```
https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```

创建管理员账号

### 使用credentials

通过Credentials Binding Plugin插件配置credentials使第三方网站和应用于Jenkins交互。

Manage Jenkins > Manage Credentials > Stores scoped to Jenkins   system or User > 全局凭据 > Add Credentials

**Kind:**

- **Secret text** - API token之类的token (如GitHub个人访问token),
- **Username and password** - 可以为独立的字段，也可以为冒号分隔的字符串：`username:password`(更多信息请参照 [处理 credentials](https://www.jenkins.io/zh/doc/book/pipeline/jenkinsfile#handling-credentials))
- **Secret file** - 保存在文件中的加密内容
- **SSH Username with private key** - 在对应字段中指定 Username,Private Key 和 可选的 Passphrase。[SSH 公钥/私钥对](http://www.snailbook.com/protocols.html)
- **Certificate** - 指定[PKCS#12 证书文件](https://tools.ietf.org/html/rfc7292) 和可选密码
- **Docker Host Certificate Authentication** credentials：将相应的详细信息复制到c Client Key, Client Certificate 和 Server CA Certificate 字段

**Scope:**

- **Global** - 如果要添加的credential用于管道项目/项目，选择此项将crendential应用于管道项目/项目对象及其所有子对象
- **System** - 如果要添加的credential用于Jenkins实例本身与系统管理功能（例如电子邮件认证，代理连接等）交互。 选择此选项会将crendential的应用于单个对象。

**ID:**

指定一个有意义的credential ID - 例如 `jenkins-user-for-xyz-artifact-repository`可以使用大写或小写字母作为凭证ID，也可以使用任何有效的分隔符，该字段可选未指定系统分配，设置后不可修改。

## pipeline

pipeline是一套持续集成的插件，好处：

- 自动地为所有分支创建流水线构建过程并拉取请求。
- 在流水线上代码复查/迭代 (以及剩余的源代码)。
- 对流水线进行审计跟踪。
- 该流水线的真正的源代码 [[3](https://www.jenkins.io/zh/doc/book/pipeline/#_footnotedef_3)], 可以被项目的多个成员查看和编辑。

特性：

- **Code**: 流水线是在代码中实现的，通常会检查到源代码控制, 使团队有编辑, 审查和迭代他们的交付流水线的能力。
- **Durable**: 流水线可以从Jenkins的主分支的计划内和计划外的重启中存活下来。
- **Pausable**: 流水线可以有选择的停止或等待人工输入或批准，然后才能继续运行流水线。
- **Versatile**: 流水线支持复杂的现实世界的 CD 需求, 包括fork/join, 循环, 并行执行工作的能力。
- **Extensible**:流水线插件支持扩展到它的DSL [[1](https://www.jenkins.io/zh/doc/book/pipeline/#_footnotedef_1)]的惯例和与其他插件集成的多个选项。

持续集成模型示例：

![](https://qwq.lsaiah.cn/usr/uploads/Picture/20200702101501.png)

**声明式Pipeline**

```
Jenkinsfile (Declarative Pipeline)
pipeline { //声明式流水线的一种特定语法,定义包含执行整个流水线的所有内容和指令块
    agent any //在任何可用的代理上，执行流水线或它的任何阶段
    stages {
        stage('Build') { //定义build阶段
            steps {	//执行build阶段相关步骤
                sh 'make' //执行shell命令
            }
        }
        stage('Test') { //定义Test阶段
            steps {	//执行Test阶段相关步骤
                sh 'make check'
                junit 'reports/**/*.xml' //聚合测试报告
            }
        }
        stage('Deploy') { //定义Deploy阶段
            steps {	//执行Deploy阶段相关步骤
                sh 'make publish'
            }
        }
    }
}
```

**脚本化Pipeline**

```
Jenkinsfile (Scripted Pipeline)
node {  //在任何可用的代理上，执行pipeline或它的任何阶段
    stage('Build') { //定义build阶段，可选，在Jenkins UI显示每个stage的任务子集
        sh 'make' //执行build阶段相关步骤
    }
    stage('Test') { //定义Test阶段
        sh 'make check' //执行Test阶段相关步骤
        junit 'reports/**/*.xml'
    }
    stage('Deploy') { //定义Deploy阶段
        sh 'make publish' //执行Deploy阶段相关步骤
    }
}
```

### 创建

- Blue Ocean
- New Item > Enter name不要使用空格 > 选择Pipeline> 键入Script  > 保存 > 立即构建 > Build History查看构建详细信息 > Console Output查看全部输出

  ```
  pipeline {
      agent any //指示 Jenkins 为整个流水线分配一个执行器（在 Jenkins 环境中的任何可用代理/节点上）和工作区。和node做了同样的事情。
      stages {
          stage('Stage 1') {
              steps {
                  echo 'Hello world!' // 写一个简单的字符串到控制台输出。
              }
          }
      }
  }
  ```
- Jenkinsfile

  可在IDE中编写提交应对复杂的Pipeline。

  1. 在键入Script的时候选择Pipeline script from SCM。
  2. SCM字段选择源代码管理系统的类型

     git+gogs为例：

     ```
     docker run --name=gogs -d  -p 10022:22 -p 10080:3000 -v /var/gogs:/data gogs/gogs
     docker run --name jenkins-blueocean -u root -d -p 8080:8080 -p 50000:50000 -v jenkins-data:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkinsci/blueocean

     ssh-keygen -t rsa -C "lsaiah@126.com"
     eval `ssh-agent -s`
     ssh-add ~/.ssh/id_rsa
     # 新建config
     Host 192.168.99.100
         PreferredAuthentications publickey
         IdentityFile ~/.ssh/id_rsa.pub
     ```
  3. 脚本路径 （IDE插入#!/usr/bin/env groovy高亮Groovy语法）

  #### 内置文档：流水线语法


  1. 片段生成器有助于为`stage` 中的 `steps` 创建代码段
  2. 全局变量参考和片段生成器不一样的是，只包含由流水线或插件提供的可用于流水线的变量文档。
  3. 声明式指令生成器（Declarative Directive Generator）可以声明流水线section（节段）和 directive（指令）

### Jenkinsfile

#### 创建

```
Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any //必需的，它指示 Jenkins 为流水线分配一个执行器和工作区。
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
```

高级创建：

```
node {
    checkout scm //checkout 步骤将会从源代码控制中检出代码；scm 是一个特殊的变量， 它指示 checkout 步骤克隆触发流水线运行的特定修订版本。
    /* .. snip .. */
}
```

#### 构建

Jenkinsfile 文件不能替代现有的构建工具，如 GNU/Make、Maven、Gradle 等，而应视其为一个将项目的开发生命周期的多个阶段（构建、测试、部署等）绑定在一起的粘合层。

```
Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'make' //sh 步骤调用 make 命令，只有命令返回的状态码为零时才会继续。任何非零的返回码都将使流水线失败。
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true //archiveArtifacts 捕获符合模式（``**/target/*.jar``）匹配的交付件并将其保存到 Jenkins master 节点以供后续获取。
            }
        }
    }
}
```

#### 测试

运行自动化测试是任何成功的持续交付过程的重要组成部分。因此，Jenkins 有许多测试记录，报告和可视化工具，这些都是由[各种插件](https://plugins.jenkins.io/?labels=report)提供的。最基本的，当测试失败时，让 Jenkins 记录这些失败以供汇报以及在 web UI 中可视化是很有用的。下面的例子使用由 [JUnit 插件](https://plugins.jenkins.io/junit)提供的 `junit` 步骤。

在下面的例子中，如果测试失败，流水线就会被标记为“不稳定”，这通过 web UI 中的黄色球表示。基于测试报告的记录，Jenkins 还可以提供历史趋势分析和可视化。

```
Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                /* `make check` 在测试失败后返回非零的退出码；
                * 使用 `true` 允许流水线继续进行
                */
                sh 'make check || true' //使用内联的 shell 条件（sh 'make || true'）确保 sh 步骤总是看到退出码是零，使 junit 步骤有机会捕获和处理测试报告。在下面处理故障一节中，对它的替代方法有更详细的介绍。
                junit '**/target/*.xml' //junit 捕获并关联与包含模式（\**/target/*.xml）匹配的 JUnit XML 文件。

            }
        }
    }
}
```

#### 部署

部署可以隐含许多步骤，这取决于项目或组织的要求，并且可能是从发布构建的交付件到 Artifactory 服务器，到将代码推送到生产系统的任何东西。 在示例流水线的这个阶段，“Build（构建）” 和 “Test（测试）” 阶段都已成功执行。从本质上讲，“Deploy（部署）” 阶段只有在之前的阶段都成功完成后才会进行，否则流水线会提前退出。

```
Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any

    stages {
        stage('Deploy') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' //流水线访问 currentBuild.result 变量确定是否有任何测试的失败。在这种情况下，值为 UNSTABLE。
              }
            }
            steps {
                sh 'make publish'
            }
        }
    }
}
```

假设在示例的 Jenkins 流水线中所有的操作都执行成功，那么每次流水线的成功运行都会在 Jenkins 中存档相关的交付件、上面报告的测试结果以及所有控制台输出。

#### 使用环境变量

```
Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            }
        }
    }
}
```

#### 设置环境变量

```
Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any
    environment { //用在最高层的 pipeline 块的 environment 指令适用于流水线的所有步骤
        CC = 'clang'
    }
    stages {
        stage('Example') {
            environment { //定义在 stage 中的 environment 指令只适用于 stage 中的步骤
                DEBUG_FLAGS = '-g'
            }
            steps {
                sh 'printenv'
            }
        }
    }
}
```

#### 动态设置环境变量

```
Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any //agent 必须设置在流水线的最高级。如果设置为 agent none 会失败。
    environment {
        // 使用 returnStdout
        CC = """${sh(
                returnStdout: true,
                script: 'echo "clang"'
            )}""" 
        // 使用 returnStatus
        EXIT_STATUS = """${sh(
                returnStatus: true,
                script: 'exit 1'
            )}""" //使用 returnStdout 时，返回的字符串末尾会追加一个空格。可以使用 .trim() 将其移除。
    }
    stages {
        stage('Example') {
            environment {
                DEBUG_FLAGS = '-g'
            }
            steps {
                sh 'printenv'
            }
        }
    }
}
```

### 分支合并

### 在流水线中使用Docker

Table of Contents

- 自定义执行环境
  - [容器的缓存数据](https://www.jenkins.io/zh/doc/book/pipeline/docker/#容器的缓存数据)
  - [使用多个容器](https://www.jenkins.io/zh/doc/book/pipeline/docker/#使用多个容器)
  - [使用Dockerfile](https://www.jenkins.io/zh/doc/book/pipeline/docker/#dockerfile)
  - [指定Docker标签](https://www.jenkins.io/zh/doc/book/pipeline/docker/#指定docker标签)
- 脚本化流水线的高级用法
  - [运行 "sidecar" 容器](https://www.jenkins.io/zh/doc/book/pipeline/docker/#运行-sidecar-容器)
  - [构建容器](https://www.jenkins.io/zh/doc/book/pipeline/docker/#构建容器)
  - [使用远程 Docker 服务器](https://www.jenkins.io/zh/doc/book/pipeline/docker/#使用远程-docker-服务器)
  - [使用自定义注册表](https://www.jenkins.io/zh/doc/book/pipeline/docker/#custom-registry)

许多组织使用 [Docker](https://www.docker.com/) 在机器之间统一构建和测试环境, 并为部署应用程序提供有效的机制。从流水线版本 2.5 或以上开始, 流水线内置了与`Jenkinsfile`中的Docker进行交互的的支持。

虽然本节将介绍基础知识在 `Jenkinsfile`中使用Docker的基础,但它不会涉及 Docker 的基本原理, 可以参考 [Docker入门指南](https://docs.docker.com/get-started/)。

#### 自定义执行环境

设计流水线的目的是更方便地使用 [Docker](https://docs.docker.com/)镜像作为单个 [Stage](https://www.jenkins.io/zh/doc/book/glossary/#stage)或整个流水线的执行环境。 这意味着用户可以定义流水线需要的工具，而无需手动配置代理。 实际上，只需对 `Jenkinsfile`进行少量编辑，任何 [packaged in a Docker container](http://hub.docker.com/)的工具， 都可轻松使用。

Jenkinsfile (Declarative Pipeline)

```groovy
pipeline {
    agent {
        docker { image 'node:7-alpine' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'
            }
        }
    }
}
```

[Toggle Scripted Pipeline](https://www.jenkins.io/zh/doc/book/pipeline/docker/#) *(Advanced)*

当流水线执行时, Jenkins 将会自动地启动指定的容器并在其中执行指定的步骤:

```
[Pipeline] stage
[Pipeline] { (Test)
[Pipeline] sh
[guided-tour] Running shell script
+ node --version
v7.4.0
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
```

#### 容器的缓存数据

许多构建工具都会下载外部依赖并将它们缓存到本地以便于将来的使用。 由于容器最初是由 "干净的" 文件系统构建的, 这导致流水线速度变慢, 因为它们不会利用后续流水线运行的磁盘缓存。 on-disk caches between subsequent Pipeline runs.

流水线支持 向Docker中添加自定义的参数, 允许用户指定自定义的 [Docker Volumes](https://docs.docker.com/engine/tutorials/dockervolumes/) 装在, 这可以用于在流水线运行之间的 [agent](https://www.jenkins.io/zh/doc/book/glossary/#agent)上缓存数据。下面的示例将会在 流水线运行期间使用 [`maven` container](https://hub.docker.com/_/maven/)缓存 `~/.m2`, 从而避免了在流水线的后续运行中重新下载依赖的需求。

Jenkinsfile (Declarative Pipeline)

```groovy
pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v $HOME/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B'
            }
        }
    }
}
```

[Toggle Scripted Pipeline](https://www.jenkins.io/zh/doc/book/pipeline/docker/#) *(Advanced)*

#### 使用多个容器

代码库依赖于多种不同的技术变得越来越容易。比如, 一个仓库既有基于Java的后端API 实现 *and* 有基于JavaScript的前端实现。 Docker和流水线的结合允许 `Jenkinsfile` 通过将 `agent {}` 指令和不同的阶段结合使用 **multiple** 技术类型。

Jenkinsfile (Declarative Pipeline)

```groovy
pipeline {
    agent none
    stages {
        stage('Back-end') {
            agent {
                docker { image 'maven:3-alpine' }
            }
            steps {
                sh 'mvn --version'
            }
        }
        stage('Front-end') {
            agent {
                docker { image 'node:7-alpine' }
            }
            steps {
                sh 'node --version'
            }
        }
    }
}
```

[Toggle Scripted Pipeline](https://www.jenkins.io/zh/doc/book/pipeline/docker/#) *(Advanced)*

#### 使用Dockerfile

对于更需要自定义执行环境的项目, 流水线还支持从源仓库的`Dockerfile` 中构建和运行容器。 与使用"现成" 容器的 [previous approach](https://www.jenkins.io/zh/doc/book/pipeline/docker/#execution-environment) 不同的是 , 使用 `agent { dockerfile true }` 语法从 `Dockerfile` 中构建一个新的镜像而不是从 [Docker Hub](https://hub.docker.com/)中拉取一个。

重复使用上面的示例, 使用一个更加自定义的 `Dockerfile`:

Dockerfile

```
FROM node:7-alpine

RUN apk add -U subversion
```

通过提交它到源仓库的根目录下, 可以更改 `Jenkinsfile` 文件，来构建一个基于该 `Dockerfile` 文件的容器然后使用该容器运行已定义的步骤:

Jenkinsfile (Declarative Pipeline)

```groovy
pipeline {
    agent { dockerfile true }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'
                sh 'svn --version'
            }
        }
    }
}
```

`agent { dockerfile true }` 语法支持大量的其它选项，这些选项的更详细的描述请参考 [流水线语法](https://www.jenkins.io/zh/doc/book/pipeline/syntax#agent) 部分。

#### 指定Docker标签

的了 [agent](https://www.jenkins.io/zh/doc/book/glossary/#agent) 都能够运行基于Docker的流水线。 对于有macOS, Windows, 或其他代理的Jenkins环境, 不能运行Docker守护进程, 这个默认设置可能会有问题。 流水线在 **Manage Jenkins** 页面和 [文件夹](https://www.jenkins.io/zh/doc/book/glossary/#folder)级别提供一个了全局选项,用来指定运行基于Docker的流水线的代理 (通过 [标签](https://www.jenkins.io/zh/doc/book/glossary/#label))。

![Configuring the Pipeline Docker Label](https://www.jenkins.io/zh/doc/book/resources/pipeline/configure-docker-label.png)

#### 脚本化流水线的高级用法

##### 运行 "sidecar" 容器

在流水线中使用Docker可能是运行构建或一组测试的所依赖的服务的有效方法。类似于 [sidecar 模式](https://docs.microsoft.com/en-us/azure/architecture/patterns/sidecar), Docker 流水线可以"在后台"运行一个容器 , 而在另外一个容器中工作。 利用这种sidecar 方式, 流水线可以为每个流水线运行 提供一个"干净的" 容器。

考虑一个假设的集成测试套件，它依赖于本地 MySQL 数据库来运行。使用 `withRun` 方法, 在 [Docker Pipeline](https://plugins.jenkins.io/docker-workflow) 插件中实现对脚本化流水线的支持, `Jenkinsfile` 文件可以运行 MySQL作为sidecar :

```
node {
    checkout scm
    /*
     * In order to communicate with the MySQL server, this Pipeline explicitly
     * maps the port (`3306`) to a known port on the host machine.
     */
    docker.image('mysql:5').withRun('-e "MYSQL_ROOT_PASSWORD=my-secret-pw" -p 3306:3306') { c ->
        /* Wait until mysql service is up */
        sh 'while ! mysqladmin ping -h0.0.0.0 --silent; do sleep 1; done'
        /* Run some tests which require MySQL */
        sh 'make check'
    }
}
```

该示例可以更进一步, 同时使用两个容器。 一个 "sidecar" 运行 MySQL, 另一个提供[执行环境](https://www.jenkins.io/zh/doc/book/pipeline/docker/#execution-environment), 通过使用Docker [容器链接](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/)。

```
node {
    checkout scm
    docker.image('mysql:5').withRun('-e "MYSQL_ROOT_PASSWORD=my-secret-pw"') { c ->
        docker.image('mysql:5').inside("--link ${c.id}:db") {
            /* Wait until mysql service is up */
            sh 'while ! mysqladmin ping -hdb --silent; do sleep 1; done'
        }
        docker.image('centos:7').inside("--link ${c.id}:db") {
            /*
             * Run some tests which require MySQL, and assume that it is
             * available on the host name `db`
             */
            sh 'make check'
        }
    }
}
```

上面的示例使用 `withRun`公开的项目, 它通过 `id` 属性具有可用的运行容器的ID。使用该容器的 ID, 流水线通过自定义 Docker 参数生成一个到`inside()` 方法的链。

The `id` property can also be useful for inspecting logs from a running Docker container before the Pipeline exits:

```
sh "docker logs ${c.id}"
```

#### 构建容器

为了构建 Docker 镜像,[Docker 流水线](https://plugins.jenkins.io/docker-workflow) 插件也提供了一个 `build()` 方法用于在流水线运行期间从存储库的`Dockerfile` 中创建一个新的镜像。

使用语法 `docker.build("my-image-name")` 的主要好处是， 脚本化的流水线能够使用后续 Docker流水线调用的返回值, 比如:

```
node {
    checkout scm

    def customImage = docker.build("my-image:${env.BUILD_ID}")

    customImage.inside {
        sh 'make test'
    }
}
```

该返回值也可以用于通过 `push()` 方法将Docker 镜像发布到 [Docker Hub](https://hub.docker.com/), 或 [custom Registry](https://www.jenkins.io/zh/doc/book/pipeline/docker/#custom-registry),比如:

```
node {
    checkout scm
    def customImage = docker.build("my-image:${env.BUILD_ID}")
    customImage.push()
}
```

镜像 "tags"的一个常见用法是 为最近的, 验证过的, Docker镜像的版本，指定 `latest` 标签。 `push()` 方法接受可选的 `tag` 参数, 允许流水线使用不同的标签 push `customImage` , 比如:

```
node {
    checkout scm
    def customImage = docker.build("my-image:${env.BUILD_ID}")
    customImage.push()

    customImage.push('latest')
}
```

在默认情况下， `build()` 方法在当前目录构建一个 `Dockerfile`。提供一个包含 `Dockerfile`文件的目录路径作为`build()` 方法的第二个参数 就可以覆盖该方法, 比如:

```
node {
    checkout scm
    def testImage = docker.build("test-image", "./dockerfiles/test") //从在 ./dockerfiles/test/Dockerfile`中发现的Dockerfile中构建`test-image
    testImage.inside {
        sh 'make test'
    }
}
```

通过添加其他参数到 `build()` 方法的第二个参数中，传递它们到 [docker build](https://docs.docker.com/engine/reference/commandline/build/)。 当使用这种方法传递参数时, 该字符串的最后一个值必须是Docker文件的路径。

该示例通过传递 `-f`标志覆盖了默认的 `Dockerfile` :

```
node {
    checkout scm
    def dockerfile = 'Dockerfile.test'
    def customImage = docker.build("my-image:${env.BUILD_ID}", "-f ${dockerfile} ./dockerfiles") //从在`./dockerfiles/Dockerfile.test`发现的Dockerfile构建 `my-image:${env.BUILD_ID}`。
}
```

#### 使用远程 Docker 服务器

默认情况下, [Docker Pipeline](https://plugins.jenkins.io/docker-workflow) 插件会与本地的Docker的守护进程通信, 通常通过 `/var/run/docker.sock`访问。

要选择一个非默认的Docker 服务器, 比如 [Docker 集群](https://docs.docker.com/swarm/), 应使用`withServer()` 方法。

通过传递一个URI, 在Jenkins中预先配置的 **Docker Server Certificate Authentication**的证书ID, 如下:

```
node {
    checkout scm

    docker.withServer('tcp://swarm.example.com:2376', 'swarm-certs') {
        docker.image('mysql:5').withRun('-p 3306:3306') {
            /* do things */
        }
    }
}
```

#### 使用自定义注册表

默认情况下， [Docker 流水线](https://plugins.jenkins.io/docker-workflow) 集成了 [Docker Hub](https://hub.docker.com/)默认的 Docker注册表。 .

为了使用自定义Docker 注册吧, 脚本化流水线的用户能够使用 `withRegistry()` 方法完成步骤，传入自定义注册表的URL, 比如:

```
node {
    checkout scm

    docker.withRegistry('https://registry.example.com') {

        docker.image('my-custom-image').inside {
            sh 'make test'
        }
    }
}
```

对于需要身份验证的Docker 注册表, 从Jenkins 主页添加一个 "Username/Password" 证书项， 并使用证书ID 作为 `withRegistry()`的第二个参数:

```
node {
    checkout scm

    docker.withRegistry('https://registry.example.com', 'credentials-id') {

        def customImage = docker.build("my-image:${env.BUILD_ID}")

        /* Push the container to the custom Registry */
        customImage.push()
    }
}
```

### 扩展Pipeline

### 开发工具

### 语法

### 规模

## Blue Ocean

## 管理

## 系统管理

## 规模