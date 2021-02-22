[官方文档](https://jmeter.apache.org/usermanual/get-started.html)

[插件管理](https://jmeter-plugins.org/wiki/Start/)

### 录制回放

- 打开jmeter
- 工具栏点击**Templates...** ，选择**Recording**模版，填写参数
- 点击Test Plan节点下的**HTTP(S) Test Script Recorder**
- 点击**启动**按钮，如下图所示

![](https://qwq.lsaiah.cn/picgo/20210222111117.png)

![image-20210222111310315](https://qwq.lsaiah.cn/picgo/20210222111310.png)

启动代理后，导入证书，设置系统代理为Jmeter代理：localhost:8888。

- 打开chrome浏览器的隐私模式。这是因为非隐私模式下浏览器发送请求时可能带有cookie，在录制过程中不希望已经保存的cookie对录制过程产生影响；
- 在地址栏中输入`www.bing.com`
- 待页面加载完毕后，在搜索框中输入 `Jmeter`，点击搜索
- 关闭chrome浏览器，停止录制，关闭系统的http代理配置
- 在节点树的Thread Group下的Recording Controller查看结果

![image-20210222133223999](https://qwq.lsaiah.cn/picgo/20210222133224.png)

结果可能包含很多请求，当测试场景不是整体系统而是主要业务时可以删除一些请求，将请求保存为search.jmx运行，点击View Results Tree查看结果。

### 性能测试

#### 添加线程组

![image-20210222135438821](https://qwq.lsaiah.cn/picgo/20210222135438.png)

每个测试计划至少包含一个线程组，多个线程组并行执行。

线程组主要包含三个参数：

- **线程数：** 虚拟用户数。一个虚拟用户占用一个进程或线程。设置多少虚拟用户数在这里也就是设置多少个线程数。
- **准备时长：** 设置的虚拟用户数需要多长时间全部启动。如果线程数为20 ，准备时长为10 ，那么需要10秒钟启动20个线程。也就是每秒钟启动2个线程。
- **循环次数:** 每个线程发送请求的次数。如果线程数为20 ，循环次数为100 ，那么每个线程发送100次请求，总请求数为20*100=2000 。如果勾选了永远，那么所有线程会一直发送请求，直到选择停止运行脚本。也可以灵活的选择设定测试运行时间，勾选调度器配置。

#### 添加HTTP请求

![image-20210222140129437](https://qwq.lsaiah.cn/picgo/20210222140129.png)

![image-20210222140352426](https://qwq.lsaiah.cn/picgo/20210222140352.png)

**名称：** 本属性用于标识一个取样器（Sampler），建议使用一个有意义的名称。

**注释：** 对于测试没有任何作用，仅用户记录用户可读的注释信息。

**服务器名称或IP ：** HTTP请求发送的目标服务器名称或IP地址。

**端口号：** 目标服务器的端口号，默认值为80 。

**协议：** 向目标服务器发送HTTP请求时的协议，可以是http或者是https ，默认值为http 。

**方法：** 发送HTTP请求的方法，可用方法包括GET、POST、HEAD、PUT、OPTIONS、TRACE、DELETE等。

**Content encoding ：** 内容的编码方式，默认值为iso8859

**路径：** 目标URL路径（不包括服务器地址和端口）

**自动重定向：** 如果选中该选项，当发送HTTP请求后得到的响应是302/301时，JMeter 自动重定向到新的页面。

**Use keep Alive ：** 当该选项被选中时，jmeter 和目标服务器之间使用 Keep-Alive方式进行HTTP通信，默认选中。

**Use multipart/from-data for HTTP POST ：** 当发送HTTP POST 请求时，使用Use multipart/from-data方法发送，默认不选中。

#### 添加监听器

添加查看结果树，聚合报告监听，启动测试。

![image-20210222141724585](https://qwq.lsaiah.cn/picgo/20210222141724.png)

![image-20210222141652083](https://qwq.lsaiah.cn/picgo/20210222141652.png)

**Label：** 请求的名称，就是httprequest sampler名称

**样本Samples：** 总共发给服务器的请求数量

**平均值Average：** 单个请求的平均响应时间，单位是毫秒

**中位数Median：** 50%的请求的响应时间

**90%Line：** 90%的请求的响应时间

**95%Line：** 95%的请求的响应时间

**99%Line：** 99%的请求的响应时间

**最小值Min：** 最小的响应时间

**最大值Max：** 最大的响应时间

**异常Error%：** 错误率=错误的请求的数量/请求的总数

**吞吐量Throughput：** 吞吐量即表示每秒完成的请求数

**接收的Received KB/sec：** 每秒从服务器端接收到的数据量

**发送的Sent KB/Sec：** 每秒从发送到服务器端的数据量

### 基础元件

#### Threads(Users)

- setup thread group

一种特殊类型的ThreadGroup的，可用于执行预测试操作。这些线程的行为完全像一个正常的线程组元件。不同的是这些类型的线程执行 **测试前** 进行定期线程组的执行。

- teardown thread group

一种特殊类型的ThreadGroup的，可用于执行测试后动作。这些线程的行为完全像一个正常的线程组元件。不同的是这些类型的线程执行 **测试结束后** 进行定期的线程组。

- thread group(线程组)

一个线程组可以看做一个虚拟用户组，线程组中的每个线程都可以理解为一个虚拟用户。线程组中包含的线程数量在测试执行过程中是不会发生改变的。

#### 测试片段Test Fragment

测试片段元素是控制器上的一个种特殊的线程组，它在测试树上与线程组处于一个层级。它与线程组有所不同，因为它不被执行，除非它是一个模块控制器或者是被控制器所引用时才会被执行。

#### 配置元件（Config Element）

配置元件（config element）用于提供对静态数据配置的支持。CSV Data Set config 可以将本地数据文件形成数据池（Data Pool），而对应于HTTP Request Sampler和 TCP Request Sampler等类型的配制元件则可以修改Sampler的默认数据。（例如，HTTP Cookie Manager 可以用于对 HTTP Request Sampler 的 cookie 进行管理）

#### 定时器（Timer）

定时器（Timer）用于操作之间设置等待时间，等待时间是性能测试中常用的控制客户端QPS的手段。类似于LoadRunner里面的思考时间。JMeter 定义了Bean Shell Timer、Constant Throughput Timer、固定定时器等不同类型的Timer。

#### 前置处理器（Pre Processors）

用于在实际的请求发出之前对即将发出的请求进行特殊处理。例如，HTTP URL重写修饰符则可以实现URL重写，当RUL中有sessionID 一类的session信息时，可以通过该处理器填充发出请求的实际的SessionID 。

#### 后置处理器（Post Processors）

用于对 **Sampler** 发出请求后得到的服务器响应进行处理。一般用来提取响应中的特定数据（类似LoadRunner测试工具中的关联概念）。例如，XPath Extractor 则可以用于提取响应数据中通过给定XPath值获得的数据。

#### 断言（Assertions）

断言用于检查测试中得到的相应数据等是否符合预期，断言一般用来设置检查点，用以保证性能测试过程中的数据交互是否与预期一致。

#### 监听器（Listener）

用来对测试结果数据进行处理和可视化展示的一系列元件。 图形结果、察看结果树、聚合报告等都是我们经常用到的元件。

#### 取样器（Sampler）

取样器（Sample）是性能测试中向服务器发送请求，记录响应信息，记录响应时间的最小单元，JMeter原生支持多种不同的sampler ，如 **HTTP Request Sampler 、 FTP Request Sample 、TCP Request Sample 、JDBC Request Sampler** 等，每一种不同类型的 sampler 可以根据设置的参数向服务器发出不同类型的请求。（在JMeter的所有sampler中，Java Request Sampler 和 Beanshell Request Sampler 是两种特殊的可定制的 Sampler ）

#### 逻辑控制器（Logic Controller）

逻辑控制器，包括两类无件，一类是用于控制test plan 中 sampler 节点发送请求的逻辑顺序的控制器，常用的有 如果（If）控制器 、switch Controller 、Runtime Controller、循环控制器等。另一类是用来组织可控制 sampler 来节点的，如 事务控制器、吞吐量控制器。

### 作用域

- **配置元件（config elements）**

元件会影响其作用范围内的所有元件。

- **前置处理程序（Per-processors）**

元件在其作用范围内的每一个sampler元件之前执行。

- **定时器（timers ）**

元件对其作用范围内的每一个sampler有效。

- **后置处理程序（Post-processors）**

元件在其作用范围内的每一个sampler元件之后执行。

- **断言（Assertions）**

元件对其作用范围内的每一个sampler元件执行后的结果执行校验。

- **监听器（Listeners）**

元件收集其作用范围的每一个sampler元件的信息并呈现。
在JMeter中，元件的作用域是靠测试计划的的树型结构中元件的父子关系来确定的，作用域的原则是：

- 取样器（sampler）元件不和其它元件相互作用，因此不存在作用域的问题。
- 逻辑控制器（Logic Controller）元件只对其子节点中的取样器 和 逻辑控制器作用。
- 除取样器和逻辑控制器元件外，其他6类元件，如果是某个sampler的子节点，则该元件会对其父子节点起作用。
- 除取样器和逻辑控制器元件外，其他6类元件，如果其父节点不是sampler ，则其作用域是该元件父节点下的其他所有后代节点（包括子节点，子节点的子节点等）。

### 执行顺序

在同一作用域名范围内，测试计划中的元件按照如下顺序执行。

（1）配置元件（config elements ）

（2）前置处理程序（Per-processors）

（3）定时器（timers ）

（4）取样器（Sampler）

（5）后置处理程序（Post-processors）（除非Sampler 得到的返回结果为空）。

（6）断言（Assertions）（除非Sampler 得到的返回结果为空）。

（7）监听器（Listeners）（除非Sampler 得到的返回结果为空）。

关于执行顺序，有两点需要注意：

- 前置处理器、后置处理器和断言等元件只能对 取样器起作用，因此，如果在它们的作用域内没有任何取样器，则不会被执行。
- 如果在同一作用域范围内有多个同一类型的元件，则这些元件按照它们在测试计划中的上下顺序一次执行。

### 参数化

假如录制的登录脚本中有登录操作，需要输入用户密码，系统不允许相同用户名和密码同时登录，模拟多个用户登录系统。对用户名和密码进行参数化使每个虚拟用户都使用不同用户名和密码访问。



### 检查点

### 集合点