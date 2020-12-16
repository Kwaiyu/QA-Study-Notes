此文档适用于全新发布的 `HttpRunner 3.x` 版本

## 一、简介

### 1.1、介绍

 　　HttpRunner 是一款面向 HTTP(S) 协议的通用测试框架，只需编写维护一份YAML/JSON脚本，即可实现自动化测试、性能测试、线上监控、持续集成等多种测试需求。

### 1.2、框架设计理念

- 充分复用优秀的开源项目，不追求重复造轮子，而是将强大的轮子组装成战车
- 遵循 约定大于配置 的准则，在框架功能中融入自动化测试最佳工程实践
- 追求投入产出比，一份投入即可实现多种测试需求

### 1.3、核心特点

- 继承 [Requests](https://links.jianshu.com/go?to=http%3A%2F%2Fdocs.python-requests.org%2Fen%2Fmaster%2F) 的全部特性，轻松实现 HTTP(S) 的各种测试需求
- 采用 `YAML/JSON` 的形式描述测试场景，保障测试用例描述的统一性和可维护性
- 借助辅助函数（debugtalk.py），在测试脚本中轻松实现复杂的动态计算逻辑
- 支持完善的测试用例分层机制，充分实现测试用例的复用
- 测试前后支持完善的 hook 机制
- 响应结果支持丰富的校验机制
- 使用 [jmespath](https://links.jianshu.com/go?to=https%3A%2F%2Fjmespath.org%2F)，提取和验证 json 响应从未如此简单
- 使用 [pytest](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.pytest.org%2F)，数百个插件可供使用
- 基于 HAR 实现接口录制和用例生成功能（[har2case](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FHttpRunner%2Fhar2case)）
- 结合 [Locust](https://links.jianshu.com/go?to=http%3A%2F%2Flocust.io%2F) 框架，无需额外的工作即可实现分布式性能测试
- 执行方式采用 CLI 调用，可与 Jenkins 等持续集成工具完美结合
- 测试结果统计报告简洁清晰，附带详尽统计信息和日志记录
- 采用 [allure](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.qameta.io%2Fallure%2F)，可以生成美观且强大的测试报告

## 二、安装部署

　　HttpRunner 使用 Python 开发，它支持Python 3.6+ 和大多数操作系统。

### 2.1 安装

`　　HttpRunner` 已发布到 [`PyPI`](https://links.jianshu.com/go?to=https%3A%2F%2Fpypi.python.org%2Fpypi) 仓库，可以使用 `pip` 直接安装.

    pip3 install httprunner

　　如果要使用最新版本，可以使用 github 仓库的 URL 安装。

    pip3 install git+https://github.com/httprunner/httprunner.git@master

　　如果已经安装过，可以使用 `-U` 进行升级。

    pip3 install -U httprunner
    pip3 install -U git+https://github.com/httprunner/httprunner.git@master

### 2.2 检查是否安装成功

如果 HttpRunner 安装成功，有 4 个命令可用：

- `httprunner`: 核心命令，可以使用 HttpRunner 的所有命令
- `hrun`: `httprunner run` 命令的别名，运行 YAML/JSON/pytest 格式的测试用例
- `hmake`: `httprunner make` 命令的别名，将 YAML/JSON 格式的 testcases 转换成 pytest 格式的测试用例
- `har2case`: `httprunner har2case` 命令的别名，将 HAR 文件转换为 YAML/JSON 格式的测试用例

查看 `HttpRunner` 版本:

    httprunner -V  # hrun -V
    3.1.0

查看所有可用的命令:

    httprunner -h
    usage: httprunner [-h] [-V] {run,startproject,har2case,make} ...
    
    One-stop solution for HTTP(S) testing.
    
    positional arguments:
      {run,startproject,har2case,make}
                            sub-command help
        run                 Make HttpRunner testcases and run with pytest.
        startproject        Create a new project with template structure.
        har2case            Convert HAR(HTTP Archive) to YAML/JSON testcases for
                            HttpRunner.
        make                Convert YAML/JSON testcases to pytest cases.
    
    optional arguments:
      -h, --help            show this help message and exit
      -V, --version         show version

## 三、用脚手架快速搭建项目

### 3.1、快速生成项目

　　我们不妨先输入httprunner startproject -h，来看一下命令说明。

    httprunner startproject -h

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823220758520-2024761599.png)

　　可以看出，只需要在命令后面带上项目名称这个参数就好了，那就先来创建一个项目，名称叫httprunner_demo。

    httprunner startproject httprunner_demo

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823220821380-27953359.png)

　　项目生成完毕，也是非常的简单。
　　如果你输入的项目名称已经存在，httprunner会给出warning提示。

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823220856369-1841334247.png)
　　相信了解过django的童鞋能感觉到，httprunner startproject这个命令跟django里的django-admin.py startproject project_name 很像，没错，其实httprunner的想法正式来源于django，这就是httprunner作为一个优秀开源技术资源整合和复用的体现之一，后续还有很多，届时提点出来。

### 3.2、项目结构梳理

    我把生成出的项目丢到sublime里方便查看，可以看的生成的目录结构如下图，那么这些都是什么意思呢？

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823220957913-511098305.png)

- debugtalk.py 放置在项目根目录下（借鉴了pytest的conftest文件的设计）
- .env 放置在项目根目录下，可以用于存放一些环境变量
- reports 文件夹：存储 HTML 测试报告
- testcases 用于存放测试用例
- har 可以存放录制导出的.har文件
- 具体用法会在后续中细讲，本章不展开。我们可以点开生成的testcases文件夹下的测试用例，里面是提供了一个可运行的demo内容的，那先来运行一下看看。

 ![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823221026707-1867700611.png)

　　运行用例：

    hrun httprunner_demo

　　可以看的httprunner输出了运行过程中的调试信息

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823221224997-1716263102.png)

　　最后，运行结束，2个用例运行pass。

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823221301530-2070479496.png)

 

　　前期准备工作就算是结束了，接下来就可以进入到详细的学习中了。

## 四、录制生成测试用例

　　在正式手动编写case之前，我们可以先来熟悉下httprunner的录制生成用例功能。
　　用postman的童鞋都知道，里面有个功能可以将接口转换成代码，可以直接copy过来使用，提升case编写效率。
　　那httprunner的录制生成用例功能又是怎么回事呢？

### 4.1、har2case

　　其实，这都要依托于另一个独立的项目-har2case。
　　原理就是当前主流的抓包工具和浏览器都支持将抓取得到的数据包导出为标准通用的 HAR 格式（HTTP Archive），然后 HttpRunner 将 HAR 格式的数据包转换为YAML/JSON格式的测试用例文件。
　　比如，我现在用window系统上的fiddler去抓取一个百度首页的请求。（常见的Charles，Fiddler、包括浏览器自带的F12开发者工具都可以导出）

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823222411081-187583403.png)
　　选中这个请求，点击左上角的File——Export Sessions——（可以选择导出选中的也可以导出所有），这里我们选择导出选中的，导出HTTPArchive，文件名baidu_home.har，保存到了项目的har目录下。

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823222421583-389349840.png)

### 4.2 、转换为pytest文件

　　获取`.har`文件后，可以使用内置命令`har2case`将其转换为 HttpRunner 测试用例。

　　har2case 命令帮助：

    $ har2case -h
    usage: har2case har2case [-h] [-2y] [-2j] [--filter FILTER]
                             [--exclude EXCLUDE]
                             [har_source_file]
    
    positional arguments:
      har_source_file       指定 .har 源文件
    
    optional arguments:
      -h, --help            显示此帮助信息并退出
      -2y, --to-yml, --to-yaml
                            转换为 YAML 格式的用例，
                            如果你没有特殊指定，默认转化为 pytest 格式的用例
                            
      -2j, --to-json        转换为 JSON 格式的用例，
                            如果你没有特殊指定，默认转化为 pytest 格式的用例
                            
      --filter FILTER       指定过滤关键字，只有包含过滤字符串的 url 的 API 才会被转换
      
      --exclude EXCLUDE     指定忽略关键字，如果 url 包含该关键字，则会被忽略，
                            如果需要过滤多个关键字，使用`|`（竖线）分隔

　　运行命令将har文件转换成测试用例：

    har2case baidu_home.har

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823222926791-1973209653.png)

　　生成完毕，在har目录下可以看到生成出的python文件：

    # NOTE: Generated By HttpRunner v3.1.1
    # FROM: har\baidu_home.harfrom httprunner import HttpRunner, Config, Step, RunRequest, RunTestCase


​    
    class TestCaseBaiduHome(HttpRunner):
        config = Config("testcase description").verify(False)
    
        teststeps = [
            Step(
                RunRequest("/")
                .get("https://www.baidu.com/")
                .with_headers(
                    **{
                        "Host": "www.baidu.com",
                        "Connection": "keep-alive",
                        "Upgrade-Insecure-Requests": "1",
                        "User-Agent": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.80 Safari/537.36",
                        "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3",
                        "Accept-Encoding": "gzip, deflate, br",
                        "Accept-Language": "zh-CN,zh;q=0.9,en;q=0.8",
                        "Cookie": "PSTM=1582766561; BAIDUID=5F919C7A22A02E55FBC58E932E7495CD:FG=1; BD_UPN=12314353; BIDUPSID=B2A8970CF5106170D98A137A26C533F7; H_WISE_SIDS=143933_142621_143879_144883_139041_141744_145870_144419_144135_145271_136863_131247_144682_137745_138883_140259_141941_127969_144790_140593_143491_144376_131423_114552_142206_145910_144501_125695_107313_139909_145654_143477_144966_140367_145423_144535_145305_145399_143857_139914_110085; BDUSS=ldaMkhYZjEtLTJQbFhQUTJtU3pwTGhnRVJkeE9YUmNzU2tRRExoZDE3N2hvfmxlSVFBQUFBJCQAAAAAAAAAAAEAAABJufAYUTc2Nzc2MDMyNQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAOEW0l7hFtJeQ; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; BDRCVFR[feWj1Vr5u3D]=I67x6TjHwwYf0; delPer=0; BD_CK_SAM=1; PSINO=5; BD_HOME=1; __yjsv5_shitong=1.0_7_47374759f3e4680e7c7afb4a907cb9d2e37d_300_1593325350568_112.80.30.194_b17302a9; yjs_js_security_passport=db59f16989e33eb07a57bd9928ad960ce36495c4_1593325351_js; H_PS_645EC=8043uGVfepN6KMlbDns%2FO0qiazHvEYcE62vhF1luCcNa%2FgwzKQEQq2epcrqdNRvj4iby; H_PS_PSSID=32095_1445_31672_21106_32139_31660_32045_31321; BDSVRTM=0",
                    }
                )
                .with_cookies(
                    **{
                        "PSTM": "1582766561",
                        "BAIDUID": "5F919C7A22A02E55FBC58E932E7495CD:FG=1",
                        "BD_UPN": "12314353",
                        "BIDUPSID": "B2A8970CF5106170D98A137A26C533F7",
                        "H_WISE_SIDS": "143933_142621_143879_144883_139041_141744_145870_144419_144135_145271_136863_131247_144682_137745_138883_140259_141941_127969_144790_140593_143491_144376_131423_114552_142206_145910_144501_125695_107313_139909_145654_143477_144966_140367_145423_144535_145305_145399_143857_139914_110085",
                        "BDUSS": "ldaMkhYZjEtLTJQbFhQUTJtU3pwTGhnRVJkeE9YUmNzU2tRRExoZDE3N2hvfmxlSVFBQUFBJCQAAAAAAAAAAAEAAABJufAYUTc2Nzc2MDMyNQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAOEW0l7hFtJeQ",
                        "BDORZ": "B490B5EBF6F3CD402E515D22BCDA1598",
                        "BDRCVFR[feWj1Vr5u3D]": "I67x6TjHwwYf0",
                        "delPer": "0",
                        "BD_CK_SAM": "1",
                        "PSINO": "5",
                        "BD_HOME": "1",
                        "__yjsv5_shitong": "1.0_7_47374759f3e4680e7c7afb4a907cb9d2e37d_300_1593325350568_112.80.30.194_b17302a9",
                        "yjs_js_security_passport": "db59f16989e33eb07a57bd9928ad960ce36495c4_1593325351_js",
                        "H_PS_645EC": "8043uGVfepN6KMlbDns%2FO0qiazHvEYcE62vhF1luCcNa%2FgwzKQEQq2epcrqdNRvj4iby",
                        "H_PS_PSSID": "32095_1445_31672_21106_32139_31660_32045_31321",
                        "BDSVRTM": "0",
                    }
                )
                .validate()
                .assert_equal("status_code", 200)
                .assert_equal('headers."Content-Type"', "text/html;charset=utf-8")
            ),
        ]


​    
    if__name__ == "__main__":
        TestCaseBaiduHome().test_start()

　　因为httprunner封装了pytest，所有既可以用hrun去运行，也可以用pytest去运行。

 hrun

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823223032386-1160720855.png)

pytest

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823223131351-853683609.png)

### 4.3 转换为YAML/JSON

　　很简单，只要在命令后面多加对应的参数就行了。-2y/--to-yml   或者   -2j/--to-json
**　　转为YAML：**

    har2case baidu_home.har -2y

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823223358078-384966663.png)

　　可以查看到转换生成的yaml文件了。

    config:
        name: testcase description
        variables: {}
        verify: false
    teststeps:
    -   name: /
        request:
            cookies:
                BAIDUID: 5F919C7A22A02E55FBC58E932E7495CD:FG=1
                BDORZ: B490B5EBF6F3CD402E515D22BCDA1598
                BDRCVFR[feWj1Vr5u3D]: I67x6TjHwwYf0
                BDSVRTM: '0'
                BDUSS: ldaMkhYZjEtLTJQbFhQUTJtU3pwTGhnRVJkeE9YUmNzU2tRRExoZDE3N2hvfmxlSVFBQUFBJCQAAAAAAAAAAAEAAABJufAYUTc2Nzc2MDMyNQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAOEW0l7hFtJeQ
                BD_CK_SAM: '1'
                BD_HOME: '1'
                BD_UPN: '12314353'
                BIDUPSID: B2A8970CF5106170D98A137A26C533F7
                H_PS_645EC: 8043uGVfepN6KMlbDns%2FO0qiazHvEYcE62vhF1luCcNa%2FgwzKQEQq2epcrqdNRvj4iby
                H_PS_PSSID: '32095_1445_31672_21106_32139_31660_32045_31321'
                H_WISE_SIDS: '143933_142621_143879_144883_139041_141744_145870_144419_144135_145271_136863_131247_144682_137745_138883_140259_141941_127969_144790_140593_143491_144376_131423_114552_142206_145910_144501_125695_107313_139909_145654_143477_144966_140367_145423_144535_145305_145399_143857_139914_110085'
                PSINO: '5'
                PSTM: '1582766561'__yjsv5_shitong: 1.0_7_47374759f3e4680e7c7afb4a907cb9d2e37d_300_1593325350568_112.80.30.194_b17302a9
                delPer: '0'
                yjs_js_security_passport: db59f16989e33eb07a57bd9928ad960ce36495c4_1593325351_js
            headers:
                Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
                Accept-Encoding: gzip, deflate, br
                Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
                Connection: keep-alive
                Cookie: PSTM=1582766561; BAIDUID=5F919C7A22A02E55FBC58E932E7495CD:FG=1;
                    BD_UPN=12314353; BIDUPSID=B2A8970CF5106170D98A137A26C533F7; H_WISE_SIDS=143933_142621_143879_144883_139041_141744_145870_144419_144135_145271_136863_131247_144682_137745_138883_140259_141941_127969_144790_140593_143491_144376_131423_114552_142206_145910_144501_125695_107313_139909_145654_143477_144966_140367_145423_144535_145305_145399_143857_139914_110085;
                    BDUSS=ldaMkhYZjEtLTJQbFhQUTJtU3pwTGhnRVJkeE9YUmNzU2tRRExoZDE3N2hvfmxlSVFBQUFBJCQAAAAAAAAAAAEAAABJufAYUTc2Nzc2MDMyNQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAOEW0l7hFtJeQ;
                    BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; BDRCVFR[feWj1Vr5u3D]=I67x6TjHwwYf0;
                    delPer=0; BD_CK_SAM=1; PSINO=5; BD_HOME=1; __yjsv5_shitong=1.0_7_47374759f3e4680e7c7afb4a907cb9d2e37d_300_1593325350568_112.80.30.194_b17302a9;
                    yjs_js_security_passport=db59f16989e33eb07a57bd9928ad960ce36495c4_1593325351_js;
                    H_PS_645EC=8043uGVfepN6KMlbDns%2FO0qiazHvEYcE62vhF1luCcNa%2FgwzKQEQq2epcrqdNRvj4iby;
                    H_PS_PSSID=32095_1445_31672_21106_32139_31660_32045_31321; BDSVRTM=0
                Host: www.baidu.com
                Upgrade-Insecure-Requests: '1'
                User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML,
                    like Gecko) Chrome/75.0.3770.80 Safari/537.36
            method: GET
            url: https://www.baidu.com/
        validate:
        -   eq:
            - status_code
            - 200
        -   eq:
            - headers.Content-Type
            - text/html;charset=utf-8

**　　转换为JSON：**

    har2case baidu_home.har -2j

![](https://img2020.cnblogs.com/blog/1381066/202008/1381066-20200823223622280-511696977.png)

　　可以看的对应的json文件：

    {
        "config": {
            "name": "testcase description",
            "variables": {},
            "verify": false
        },
        "teststeps": [
            {
                "name": "/",
                "request": {
                    "url": "https://www.baidu.com/",
                    "method": "GET",
                    "cookies": {
                        "PSTM": "1582766561",
                        "BAIDUID": "5F919C7A22A02E55FBC58E932E7495CD:FG=1",
                        "BD_UPN": "12314353",
                        "BIDUPSID": "B2A8970CF5106170D98A137A26C533F7",
                        "H_WISE_SIDS": "143933_142621_143879_144883_139041_141744_145870_144419_144135_145271_136863_131247_144682_137745_138883_140259_141941_127969_144790_140593_143491_144376_131423_114552_142206_145910_144501_125695_107313_139909_145654_143477_144966_140367_145423_144535_145305_145399_143857_139914_110085",
                        "BDUSS": "ldaMkhYZjEtLTJQbFhQUTJtU3pwTGhnRVJkeE9YUmNzU2tRRExoZDE3N2hvfmxlSVFBQUFBJCQAAAAAAAAAAAEAAABJufAYUTc2Nzc2MDMyNQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAOEW0l7hFtJeQ",
                        "BDORZ": "B490B5EBF6F3CD402E515D22BCDA1598",
                        "BDRCVFR[feWj1Vr5u3D]": "I67x6TjHwwYf0",
                        "delPer": "0",
                        "BD_CK_SAM": "1",
                        "PSINO": "5",
                        "BD_HOME": "1",
                        "__yjsv5_shitong": "1.0_7_47374759f3e4680e7c7afb4a907cb9d2e37d_300_1593325350568_112.80.30.194_b17302a9",
                        "yjs_js_security_passport": "db59f16989e33eb07a57bd9928ad960ce36495c4_1593325351_js",
                        "H_PS_645EC": "8043uGVfepN6KMlbDns%2FO0qiazHvEYcE62vhF1luCcNa%2FgwzKQEQq2epcrqdNRvj4iby",
                        "H_PS_PSSID": "32095_1445_31672_21106_32139_31660_32045_31321",
                        "BDSVRTM": "0"
                    },
                    "headers": {
                        "Host": "www.baidu.com",
                        "Connection": "keep-alive",
                        "Upgrade-Insecure-Requests": "1",
                        "User-Agent": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.80 Safari/537.36",
                        "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3",
                        "Accept-Encoding": "gzip, deflate, br",
                        "Accept-Language": "zh-CN,zh;q=0.9,en;q=0.8",
                        "Cookie": "PSTM=1582766561; BAIDUID=5F919C7A22A02E55FBC58E932E7495CD:FG=1; BD_UPN=12314353; BIDUPSID=B2A8970CF5106170D98A137A26C533F7; H_WISE_SIDS=143933_142621_143879_144883_139041_141744_145870_144419_144135_145271_136863_131247_144682_137745_138883_140259_141941_127969_144790_140593_143491_144376_131423_114552_142206_145910_144501_125695_107313_139909_145654_143477_144966_140367_145423_144535_145305_145399_143857_139914_110085; BDUSS=ldaMkhYZjEtLTJQbFhQUTJtU3pwTGhnRVJkeE9YUmNzU2tRRExoZDE3N2hvfmxlSVFBQUFBJCQAAAAAAAAAAAEAAABJufAYUTc2Nzc2MDMyNQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAOEW0l7hFtJeQ; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; BDRCVFR[feWj1Vr5u3D]=I67x6TjHwwYf0; delPer=0; BD_CK_SAM=1; PSINO=5; BD_HOME=1; __yjsv5_shitong=1.0_7_47374759f3e4680e7c7afb4a907cb9d2e37d_300_1593325350568_112.80.30.194_b17302a9; yjs_js_security_passport=db59f16989e33eb07a57bd9928ad960ce36495c4_1593325351_js; H_PS_645EC=8043uGVfepN6KMlbDns%2FO0qiazHvEYcE62vhF1luCcNa%2FgwzKQEQq2epcrqdNRvj4iby; H_PS_PSSID=32095_1445_31672_21106_32139_31660_32045_31321; BDSVRTM=0"
                    }
                },
                "validate": [
                    {
                        "eq": [
                            "status_code",
                            200
                        ]
                    },
                    {
                        "eq": [
                            "headers.Content-Type",
                            "text/html;charset=utf-8"
                        ]
                    }
                ]
            }
        ]
    }

　　以上转换出的pytest、yaml、json这3种格式的文件效果都是一样的，用hrun都可以运行，但是用pytest执行的话只可以运行.py的文件了。

##  五、结构解析

　　HttpRunner v3.x 支持三种测试用例格式，即 pytest，YAML 和 JSON。

格式关系如下图所示：

 ![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200904012440011-1774193009.png)


　　上图是来自官方的用例格式关系图，可以看出来，httprunner再对于第三方导出的har文件进行了转换处理，有的人喜欢转换成json，有的人喜欢转换成yaml。但是最终，还是通过解析json格式的文件，生成pytest的python文件。
　　既然最后都是要生成pytest，那何不一步到位呢？哈哈，我想这就是官方推荐pytest格式的原因吧。
　　我还是挺喜欢的，因为我对于pytest使用的较多，那么接下来也是基于pytest格式的用例进行解析。

### 5.1 用例结构

每个测试用例都是 HttpRunner 的子类（一个类即为一个测试用例），并且必须具有两个类属性：`config` 和 `teststeps`。

- 
`config`：配置测试用例级别的设置，包括 base_url，verify，variables，export。

- 
`teststeps`：测试步骤的列表（`List [Step]`），每个步骤对应一个 API 请求或另一个测试用例的应用。此外，还支持 `variables`/`extract`/`validate`/`hooks` 来创建极其复杂的测试方案。

    from httprunner import HttpRunner, Config, Step, RunRequest, RunTestCase
  
  
    class TestCaseRequestWithFunctions(HttpRunner):
        config = (
            Config("request methods testcase with functions")
            .variables(
                **{
                    "foo1": "config_bar1",
                    "foo2": "config_bar2",
                    "expect_foo1": "config_bar1",
                    "expect_foo2": "config_bar2",
                }
            )
            .base_url("https://postman-echo.com")
            .verify(False)
            .export(*["foo3"])
        )
  
        teststeps = [
            Step(
                RunRequest("get with params")
                .with_variables(
                    **{"foo1": "bar11", "foo2": "bar21", "sum_v": "${sum_two(1, 2)}"}
                )
                .get("/get")
                .with_params(**{"foo1": "$foo1", "foo2": "$foo2", "sum_v": "$sum_v"})
                .with_headers(**{"User-Agent": "HttpRunner/${get_httprunner_version()}"})
                .extract()
                .with_jmespath("body.args.foo2", "foo3")
                .validate()
                .assert_equal("status_code", 200)
                .assert_equal("body.args.foo1", "bar11")
                .assert_equal("body.args.sum_v", "3")
                .assert_equal("body.args.foo2", "bar21")
            ),
            Step(
                RunRequest("post form data")
                .with_variables(**{"foo2": "bar23"})
                .post("/post")
                .with_headers(
                    **{
                        "User-Agent": "HttpRunner/${get_httprunner_version()}",
                        "Content-Type": "application/x-www-form-urlencoded",
                    }
                )
                .with_data("foo1=$foo1&foo2=$foo2&foo3=$foo3")
                .validate()
                .assert_equal("status_code", 200)
                .assert_equal("body.form.foo1", "$expect_foo1")
                .assert_equal("body.form.foo2", "bar23")
                .assert_equal("body.form.foo3", "bar21")
            ),
        ]
  
    
  
    if__name__ == "__main__":
        TestCaseRequestWithFunctions().test_start()

### 5.2 链式调用

　　HttpRunner v3.x 的最强大功能之一是链式调用，使用它无需记住任何测试用例格式的详细信息，并且在 IDE 中编写测试用例时可以智能完成。
![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200904013100157-1201413567.png)

 

 

 ![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200904013219368-1844953028.png)

### 5.3、httprunner的用例结构与我自己的用例

　　补一个官方完整的一个demo代码，并说说httprunner中的用例与我自己编写的测试用例之间的联系。

    from httprunner import HttpRunner, Config, Step, RunRequest, RunTestCase


​    
    class TestCaseRequestWithFunctions(HttpRunner):
        config = (
            Config("request methods testcase with functions")
            .variables(
                **{
                    "foo1": "config_bar1",
                    "foo2": "config_bar2",
                    "expect_foo1": "config_bar1",
                    "expect_foo2": "config_bar2",
                }
            )
            .base_url("http://demo.qa.com")
            .verify(False)
            .export(*["foo3"])
        )
    
        teststeps = [
            Step(
                RunRequest("get with params")
                .with_variables(
                    **{"foo1": "bar11", "foo2": "bar21", "sum_v": "${sum_two(1, 2)}"}
                )
                .get("/get")
                .with_params(**{"foo1": "$foo1", "foo2": "$foo2", "sum_v": "$sum_v"})
                .with_headers(**{"User-Agent": "HttpRunner/${get_httprunner_version()}"})
                .extract()
                .with_jmespath("body.args.foo2", "foo3")
                .validate()
                .assert_equal("status_code", 200)
                .assert_equal("body.args.foo1", "bar11")
                .assert_equal("body.args.sum_v", "3")
                .assert_equal("body.args.foo2", "bar21")
            ),
            Step(
                RunRequest("post form data")
                .with_variables(**{"foo2": "bar23"})
                .post("/post")
                .with_headers(
                    **{
                        "User-Agent": "HttpRunner/${get_httprunner_version()}",
                        "Content-Type": "application/x-www-form-urlencoded",
                    }
                )
                .with_data("foo1=$foo1&foo2=$foo2&foo3=$foo3")
                .validate()
                .assert_equal("status_code", 200)
                .assert_equal("body.form.foo1", "$expect_foo1")
                .assert_equal("body.form.foo2", "bar23")
                .assert_equal("body.form.foo3", "bar21")
            ),
        ]


​    
    if__name__ == "__main__":
        TestCaseRequestWithFunctions().test_start()

1. httprunner中的testcase，其实说的就是上面的这一整个Python文件。
2. teststeps列表中的Step，其实就是我自己编写case时候的一个个def test_xxx()：pass。
3. 而每一个Step内部，依然是按照 传参——调用接口——断言，这样的过程来的。

万变不离其宗，httprunner框架目前看起来，确实可以让编写更加的便捷、简洁，但是这只是目前从demo的过程中得到的结论，后面还需要落地实战才可以。

## 六、测试用例 - config

　　上一篇中，我们了解到了config，在配置中，我们可以配置测试用例级级别的一些设置，比如基础url、验证、变量、导出。
　　我们一起来看，官方给出的一个例子：

    from httprunner import HttpRunner, Config, Step, RunRequest, RunTestCase


​    
    class TestCaseRequestWithFunctions(HttpRunner):
        config = (
            Config("request methods testcase with functions")
            .variables(
                **{
                    "foo1": "config_bar1",
                    "foo2": "config_bar2",
                    "expect_foo1": "config_bar1",
                    "expect_foo2": "config_bar2",
                }
            )
            .base_url("http://demo.qa.com")
            .verify(False)
            .export(*["foo3"])
        )
    
        teststeps = [
            Step(
                RunRequest("get with params")
                .with_variables(
                    **{"foo1": "bar11", "foo2": "bar21", "sum_v": "${sum_two(1, 2)}"}
                )
                .get("/get")
                .with_params(**{"foo1": "$foo1", "foo2": "$foo2", "sum_v": "$sum_v"})
                .with_headers(**{"User-Agent": "HttpRunner/${get_httprunner_version()}"})
                .extract()
                .with_jmespath("body.args.foo2", "foo3")
                .validate()
                .assert_equal("status_code", 200)
                .assert_equal("body.args.foo1", "bar11")
                .assert_equal("body.args.sum_v", "3")
                .assert_equal("body.args.foo2", "bar21")
            ),
            Step(
                RunRequest("post form data")
                .with_variables(**{"foo2": "bar23"})
                .post("/post")
                .with_headers(
                    **{
                        "User-Agent": "HttpRunner/${get_httprunner_version()}",
                        "Content-Type": "application/x-www-form-urlencoded",
                    }
                )
                .with_data("foo1=$foo1&foo2=$foo2&foo3=$foo3")
                .validate()
                .assert_equal("status_code", 200)
                .assert_equal("body.form.foo1", "$expect_foo1")
                .assert_equal("body.form.foo2", "bar23")
                .assert_equal("body.form.foo3", "bar21")
            ),
        ]


​    
    if__name__ == "__main__":
        TestCaseRequestWithFunctions().test_start()

### 6.1、name（必填）

　　即用例名称，这是一个必填参数。测试用例名称，将显示在执行日志和测试报告中。比如，我在之前的百度搜索的case里，加入name。
![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200907001129537-516620068.png)
　　运行后，在debug日志里，可以看的用例名称被展示出来。

 ![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200907001553491-1226871461.png)

### 6.2、base_url（选填）

　　其实这个配置一般在多环境切换中最常用。
　　比如你的这套测试用例在qa环境，uat环境都要使用，那么就可以把基础地址（举例http://demo.qa.com），设置进去。在后面的teststep中，只需要填上接口的相对路径就好了（举例 /get）。
　　这样的话，切换环境运行，只需要修改base_url即可。

![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200907001707717-932400913.png)

### 6.3、variables（选填）

　　变量，这里可以存放一些公共的变量，可以在测试用例里引用。这里大家可以记住这个“公共”的词眼，因为在后面的Step中，还会有步骤变量。
　　比如说，我的接口有个传参是不变的，比如用户名username，而且后面的没个Step都会用到这个传参，那么username就可以放在config的公共变量里。
　　另外，Step里的变量优先级是比config里的变量要高的，如果有2个同名的变量的话，那么引用的时候，是优先引用步骤里的变量的。

### 6.4、verify（选填）

　　用来决定是否验证服务器TLS证书的开关。
　　通常设置为False，当请求https请求时，就会跳过验证。如果你运行时候发现抛错SSLError，可以检查一下是不是verify没传，或者设置了True。

    SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: self signed certificate in certificate chain (_ssl.c:1076)'))

### 6.5、export（选填）

　　导出的变量，主要是用于Step之间参数的传递。还是以上面的官方代码为例：

![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200907002251164-1234256908.png)

1. 在config中配置export“foo3”这个变量。
2. 在第一个Step中，.extract() 提取了"body.args.foo2"给变量“foo3”。
3. 在第二个Step中，引用变量"foo3"。

像参数传递，提取这些点，会放在后面单独讲解，前面还是以熟悉框架为主。

## 七、 测试用例-teststeps-RunRequest

　　之前我们了解了config里的各项参数，今天来了解另一个重要部分——teststeps，在这之前，先看看测试用例的分层模型。

### 7.1、测试用例分层模型

　　一个testcase里（就是一个pytest格式的Python文件）可以有一个或者多个测试步骤，就是teststeps[]列表里的Step。

　　我的理解每一个Step就可以类比成pytest框架下的def test_xxx()的用例函数，在Step里通常都会要请求API完成测试，也可以调用其他测试用例来完成更多的需求。

　　可以来看下官方的测试用例逻辑图（2.x版本不同，3.x弃用了2.x的API概念）：
![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200907002510772-699848636.png)
　　可以看到，testsuite包含了testcase，testcase1需要依赖testcase2才可以完成，那么就可以在teststep12对其进行引用；而testcase2又依赖于testcase3，那么也可以在teststep22进行引用。

　　但是在整个testsuite下，这3个testcase都是相互独立的，可以独自运行。如果需要相互调用，则是在testcase内部去完成处理。

　　可能看起来有点绕，其实官方想表达的就是测试用例分层的一个思想：

- 测试用例（testcase）应该是完整且独立的，每条测试用例应该是都可以独立运行的
- 测试用例是测试步骤（teststep）的有序集合
- 测试用例集（testsuite）是测试用例的无序集合，集合中的测试用例应该都是相互独立，不存在先后依赖关系的；如果确实存在先后依赖关系，那就需要在测试用例中完成依赖的处理

其实这一点，在我们自己使用pytest框架编写测试用例的时候同样贯彻到了。为了自动化测试的稳定性和可维护性，每个测试用例之间相互独立是非常有必要的。

### 7.2、teststeps-RunRequest

　　先上一段Step的代码，结合下面的点对照着看：

        teststeps = [
            Step(
                RunRequest("get with params")
                .with_variables(
                    **{"foo1": "bar11", "foo2": "bar21", "sum_v": "${sum_two(1, 2)}"}
                )
                .get("/get")
                .with_params(**{"foo1": "$foo1", "foo2": "$foo2", "sum_v": "$sum_v"})
                .with_headers(**{"User-Agent": "HttpRunner/${get_httprunner_version()}"})
                .extract()
                .with_jmespath("body.args.foo2", "foo3")
                .validate()
                .assert_equal("status_code", 200)
                .assert_equal("body.args.foo1", "bar11")
                .assert_equal("body.args.sum_v", "3")
                .assert_equal("body.args.foo2", "bar21")
            ),

　　从上面的代码可以看出，RunRequest的作用就是在测试步骤中请求API，并且可以对于API的响应结果进行提取、断言。

**1.RunRequest(name)**

　　RunRequest的参数名用于指定teststep名称，它将显示在执行日志和测试报告中。

![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200908015150600-1476033327.png)

**2. .with_variables**

　　用于存放变量，但是这里的是Step步骤里的变量，不同的Step的变量是相互独立的。所以对于多个Step都要使用的变量，我们可以放到config的变量里去。

　　另外，如果config和Step里有重名的变量，那么当你引用这个变量的时候，Step变量会覆盖config变量。

**3. .method(url)**

　　这里就是指定请求API的方法了，常用的get、post等等。如图所示，就是用的get方法，括号里的url就是要请求的地址了。

　　这里要注意的是，如果在config里设置了基础url，那么步骤里的url就只能设置相对路径了。
![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200908015424921-1153381482.png)


**4. .with_params**

　　这个就简单了，测接口不都得要传参么，对于params类型的传参，就放这就行了，key-value键值对的形式。对于body形式的传参，看后面。
![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200908015442503-123556976.png)


**5. .with_headers**

　　同样，有header要带上的就放这里。
![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200908015545255-1200023216.png)


**6. .with_cookies**

　　需要带cookie的，可以用.with_cookies方法。
![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200908015558452-58040175.png)


**7. .with_data**

　　对于body类型的传参，可以用.with_data。
![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200908015632552-964937935.png)


**8. .with_json**

　　如果是json类型的body请求体，可以用.with_json。
![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200908021626254-1858837764.png)


**9. .extract**

　　这里就是要做提取操作了，使用.with_jmespath(jmes_path: Text, var_name: Text)。

　　这里是采用了JMESPath语言，JMESPath是JSON的查询语言，可以便捷的提取json中你需要的元素。

　　第一个参数是你的目标元素的jmespath表达式，第二个元素则是用来存放这个元素的变量，供有需要的引用。

　　这里不展开，后面单讲。
![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200908021646261-220027930.png)


**10. .validate**

　　断言，我们测试最终就是要验证接口返回是否符合预期。

　　那在httprunner框架中，可以使用assert_XXX(jmes_path: Text, expected_value: Any)来进行提取和验证。

　　第一个参数还是jmespath表达式，第二个参数则是预期值。

　　assert_XXX这种方式相信用过自动化测试框架的都不会陌生，所以也非常容易上手。目前httprunner还是封装了非常丰富的断言方法的，相信可以满足绝大多数的需求了。
![](https://img2020.cnblogs.com/blog/1268169/202006/1268169-20200630162233641-31937324.png)

1. equals： 是否相等
2. less_than： 小于
3. less_than_or_equals： 小于等于
4. greater_than： 大于
5. greater_than_or_equals： 大于等于
6. not_equals： 不等于
7. string_equals： 字符串相等
8. length_equals： 长度相等
9. length_greater_than： 长度大于
10. length_greater_than_or_equals： 长度大于等于
11. length_less_than： 长度小于
12. length_less_than_or_equals： 长度小于等于
13. contains： 预期结果是否被包含在实际结果中
14. contained_by： 实际结果是否被包含在预期结果中
15. type_match： 类型是否匹配
16. regex_match： 正则表达式是否匹配
17. startswith： 字符串是否以什么开头
18. endswith： 字符串是否以什么结尾

## 
八、测试用例-teststeps-RunTestCase[https://www.cnblogs.com/pingguo-softwaretesting/p/13215007.html](https://www.cnblogs.com/pingguo-softwaretesting/p/13215007.html)

　　以前我在写接口自动化用例的时候，为了保证用例的独立性，需要在setUp里调用各种满足用例的一些前置条件，其中就不乏调用了其他测试用例中的方法。

　　而httprunner也是支持了这一项很重要的特性，通过RunTestCase对其他测试用例进行调用，并且还可以导出用例中你所需要的变量，来满足后续用例的的运行。

　　首先还是来看下RunTestCase的用法，然后再用实例去实践。

        teststeps = [
            Step(
                RunTestCase("request with functions")
                .with_variables(
                    **{"foo1": "testcase_ref_bar1", "expect_foo1": "testcase_ref_bar1"}
                )
                .call(RequestWithFunctions)
                .export(*["foo3"])
            ),
            Step(
                RunRequest("post form data")
                .with_variables(**{"foo1": "bar1"})
                .post("/post")
                .with_headers(
                    **{
                        "User-Agent": "HttpRunner/${get_httprunner_version()}",
                        "Content-Type": "application/x-www-form-urlencoded",
                    }
                )
                .with_data("foo1=$foo1&foo2=$foo3")
                .validate()
                .assert_equal("status_code", 200)
                .assert_equal("body.form.foo1", "bar1")
                .assert_equal("body.form.foo2", "bar21")
            ),

### 8.1 . RunTestCase(name)

　　这个参数呢还是一个名称，毕竟RunTestCase还是一个Step，这个名称同样会在日志和报告中显示。

 ![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200908022741389-269933809.png)
### 8.2. .with_variables

　　这个变量跟RunRequest里的用法一样。

### 8.3. .call

　　这里就是指定你要引用的testcase类名称了。

 ![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200908022821321-1801425843.png)
### 8.4. .export

　　可以指导出的变量，以供后续Step引用。
　　可以看的.export（）内部是一个列表[]，这里可以用来导出多个变量。
![](https://img2020.cnblogs.com/blog/1268169/202006/1268169-20200630173332348-767666696.png)

## 九、用例引入，变量传递

　　进行一些实践操作。

　　上一篇提到了RunTestCase，里面有2个重要的特征：

　　一个是在一个用例中引用另一个测试用例，另一个则是变量的导出与引用。

　　用flask快速写了2个接口，以供在本地调用：

    from flask import Flask
    from flask import request
    
    app = Flask(__name__)


​    
    @app.route('/')
    def hello_world():
        return'Hello World!'
    
    @app.route('/getUserName', methods=['GET'])
    def get_user_name():
        if request.method == 'GET':
            return {
            "username": "wesson",
            "age": "27",
            "from": "China",
        }
    
    @app.route('/joinStr', methods=['GET'])
    def str_join():
        if request.method == 'GET':
            str1 = request.args.get("str1")
            str2 = request.args.get("str2")
            after_join = str1 + "" + str2
            return {
                "result": after_join
            }
    
    if__name__ == '__main__':
        app.run()

一共有2个接口：

1. /getUserName，查询用户名，返回是我写死的字典。
2. /joinStr，两个字符串拼接，返回的是拼接后的结果。

### 9.1、编写测试用例

　　根据之前学习过的，直接编写case，因为这个接口没有传参，cookie之类的，就省掉了，只是demo用。

#### 1. 接口：/getUserName

    from httprunner import HttpRunner, Config, Step, RunRequest, RunTestCase


​    
    class TestCaseRequestWithGetUserName(HttpRunner):
        config = (
            Config("test /getUserName")
                .base_url("http://localhost:5000")
                .verify(False)
        )
    
        teststeps = [
            Step(
                RunRequest("getUserName")
                    .get("/getUserName")
                    .validate()
                    .assert_equal("body.username", "wesson")
            ),
    
        ]


​    
    if__name__ == "__main__":
        TestCaseRequestWithGetUserName().test_start()

　　这里呢，步骤都有了，断言是验证返回的username字段值是不是“wesson”，运行一下，可以看到测试通过。

![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200910012056900-1086486814.png)

#### 2. 接口：/joinStr

　　这个接口就需要2个传参了，那么在Step里通过.with_params()来传参。

 

    from httprunner import HttpRunner, Config, Step, RunRequest, RunTestCase


​    
    class TestCaseRequestWithJoinStr(HttpRunner):
        config = (
            Config("test /joinStr")
                .base_url("http://localhost:5000")
                .verify(False)
        )
    
        teststeps = [
            Step(
                RunRequest("joinStr")
                    .get("/joinStr")
                    .with_params(**{"str1": "hello", "str2": "wesson"})
                    .validate()
                    .assert_equal("body.result", "hello wesson")
            ),
    
        ]


​    
    if__name__ == "__main__":
        TestCaseRequestWithJoinStr().test_start()

　　这里传入的参数分别是“hello”、“wesson”,这个字符串在拼接的时候是加了一个空格的，所以断言的时候我预期的值是"hello wesson"。
　　运行测试，可以看的测试通过。

 ![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200910012203080-404250805.png)
### 9.2、testcase引用&变量传递

　　以上是2个分开的case，都可以分别正常运行。

　　假设，/joinStr接口的第二个参数，是依赖/getUserName接口的返回，那么现在这2个testcase之间就有了依赖关系。

　　那么在写/getUserName接口用例的时候，就需要去引用/joinStr的测试用例了，并且需要把/getUserName用例的变量导出来，/joinStr的测试用例传参时候使用。

#### 1. 首先，先修改/getUserName接口的case：

    from httprunner import HttpRunner, Config, Step, RunRequest


​    
    class TestCaseRequestWithGetUserName(HttpRunner):
        config = (
            Config("test /getUserName")
                .base_url("http://localhost:5000")
                .verify(False)
                .export(*["username"])#这里定义出要导出的变量
        )
    
        teststeps = [
            Step(
                RunRequest("getUserName")
                    .get("/getUserName")
                    .extract()
                    .with_jmespath("body.username", "username")#提取出目标值，赋值给username变量                .validate()
                    .assert_equal("body.username", "wesson")
            ),
    
        ]


​    
    if__name__ == "__main__":
        TestCaseRequestWithGetUserName().test_start()

　　关注注释部分的代码，一个是config里定义了这个要导出的变量，另一个是在Step中，讲目标值提取出来，赋值给这个变量。

#### 2. 接下来，修改/joinStr接口的测试用例：

    from httprunner import HttpRunner, Config, Step, RunRequest, RunTestCase
    from .get_user_name_test import TestCaseRequestWithGetUserName #记得要导入引用的类class TestCaseRequestWithJoinStr(HttpRunner):
        config = (
            Config("test /joinStr")
                .base_url("http://localhost:5000")
                .verify(False)
        )
    
        teststeps = [
            Step(
                RunTestCase("setUp getUserName")
                .call(TestCaseRequestWithGetUserName)#导入后就可以调用了
                .export(*["username"])#在RunTestCase步骤中定义这个变量的导出        ),
            Step(
                RunRequest("joinStr")
                    .get("/joinStr")
                    .with_params(**{"str1": "hello", "str2": "$username"})#在第二个传参中引用导出的变量                .validate()
                    .assert_equal("body.result", "hello $username")#断言的预期值也引用变量        ),
    
        ]


​    
    if__name__ == "__main__":
        TestCaseRequestWithJoinStr().test_start()

　　按照直接学习的内容，case已经修改好，现在运行/joinStr接口的测试用例，可以看到运行通过。

![](https://img2020.cnblogs.com/blog/1381066/202009/1381066-20200910012346824-1455643229.png)

## 10、报告生成

　　HTTPrunner 集成了 pytest，所以 HTTPrunner v3.x 可以使用 pytest 的所有插件，包括测试报告插件，例如`pytest-html` 和`alluer-pytest`。

### 10.1 HTML 测试报告

　　HTTPrunner 安装之后自带 `pytest-html`插件，当你想生成 HTML 测试报告时，可以添加命令参数`--html`。

    $ hrun /path/to/testcase --html=report.html

-

    --html=report.html中的report.html是测试报告的存放路径，没有带文件夹的时候会存放在命令运行的文件夹。

　　通过`--html`生成的 HTML 报告包含了多个文件夹，如果你要创建一个单独的 HTML 文件，可以添加另一个命令参数`--self-contained-html`。

    $ hrun /path/to/testcase --html=report.html --self-contained-html

　　你可以参考[pytest-html](https://links.jianshu.com/go?to=https%3A%2F%2Fpypi.org%2Fproject%2Fpytest-html%2F)，获取更多关于 pytest-html 测试报告的信息。

###  10.2 allure report

　　pytest 支持大名鼎鼎的 allure 测试报告，HTTPrunner 集成了 pytest，也天然支持 allure。

　　不过 HTTPrunner 默认并未安装 allure，你需要另外安装。

安装有两种方式：

- 安装 allure 的 pytest 依赖库 `allure-pytest`；
- 安装 HTTPrunner 的 allure 依赖库 `httprunner[allure]`。

**安装 `allure-pytest`:**

    $ pip3 install "allure-pytest"

**安装 `httprunner[allure]` (推荐)**

    pip3 install "httprunner[allure]"

一旦 allure-pytest 准备好，以下参数就可以与 hrun/pytest 命令一起使用：

`--alluredir=DIR`: 生成 allure 报告到指定目录
`--clean-alluredir`: 如果指定目录已存在则清理该文件夹
`--allure-no-capture`:不要将 pytest 捕获的日志记录（logging）、标准输出（stdout）、标准错误（stderr）附加到报告中

要使 Allure 侦听器能够在测试执行期间收集结果，只需添加`--alluredir`选项，并提供指向存储结果的文件夹路径。如：

    $ hrun /path/to/testcase --alluredir=/tmp/my_allure_results

> /tmp/my_allure_results 只会存储收集的测试结果（你将会看到一堆莫名其妙的文件，这些都是收集的测试结果），并非完成的报告，还需要通过命令生成。

要在测试完成后查看实际报告，您需要使用Allure命令行实用程序从结果中生成报告。 

    $ allure serve /tmp/my_allure_results

此命令将在默认浏览器中显示你生成的报告。

同时你也可以在 Jenkins 中安装 allure 报告插件，将结果与 Jenkins 集成。

你可以在 [allure-pytest](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.qameta.io%2Fallure%2F%23_pytest) 查看关于 allure 报告的更多信息。