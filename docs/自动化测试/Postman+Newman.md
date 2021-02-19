## **Postman简介**

​    一般简单的接口测试我们可以直接在浏览器里面进行调试，但是涉及到一些权限设置就无法操作了，因此我们需要接口测试的相关工具：postman是一个接口测试和HTTP请求的工具。

postman的优点：

- 支持各种请求类型：get、post、put、patch、delete等
- 支持在线存储数据，通过账号就可以进行迁移数据
- 很方便的支持请求header和请求参数的设置
- 支持不同的认证机制，包括 Basic Auth、 Digest Auyh 、 OAuth1.0、 OAuth2.0等
- 响应数据时自动按照语法格式高亮的， 包括HTML、 JSON 和 XML


### **下载安装**

Postman有Windows，Mac，Liunx以及Chrome插件版本。这里主要介绍win平台版本的使用。

- 官网地址：https://www.getpostman.com/
- 下载地址：https://www.getpostman.com/apps
- 官方文档：https://www.getpostman.com/docs/v6/
- postman Api 文档：[https://docs.postman-echo.com](https://docs.postman-echo.com/)

### **Postman入门**

发送第一个请求：

1、启动软件后在引导界面点击Request，给Request命名，然后创建文件夹并把该Request归属到该文件夹。

2、在地址栏输入postman-echo.com/get然后点击send按钮，可以看到返回值。

![img](https://qwq.lsaiah.cn/picgo/20210219103451.png)

![img](https://qwq.lsaiah.cn/picgo/20210219103503.png)

![img](https://qwq.lsaiah.cn/picgo/20210219103518.png)

## **Postman工作原理**

如下图所示，当你在Postman中输入请求并单击Send按钮时，服务器将接收请求并返回Postman在接口中显示的响应

 ![img](https://qwq.lsaiah.cn/picgo/20210219103528.png)

**发送不同的HTTP请求**

**GET**

HTTP GET请求用于从服务器检索数据，数据由统一的URI（统一资源标识符）标识,GET请求可以使用Query String Parameters 将参数传递给服务器。

请求说明：

- Params下的Query Params是以键值对方式发送参数，
- 在URL后面加 ？可以添加发送参数，& 可以连接多个参数
- 例如：https://postman-echo.com/get?name=leesin&skill=qq2wrd

参数编辑：

- 点击params按钮，postman可以自动办公们解析出对应的参数
- 如果暂时不传参数，可以方便的通过不勾选方式去实现
- 如果想要批量编辑参数，可以点击右上角的Bulk Edit，实现批量编辑

响应数据：

- 在主页下方一栏菜单为响应菜单栏，可以查看响应内容，Cookie、Headers、响应状态码等信息

![img](https://qwq.lsaiah.cn/picgo/20210219103544.png)

**POST**

HTTP POST请求是将数据传输到服务器，返回 的数据取决于服务器的实现。

POST请求可以使用query String Parameters以及body将参数传递给服务器。

案例1：

在下面的请求中，使用Query String Parameters传递参数。

```
https://postman-echo.com/post?param=test
```

返回值

```json
{
    "args": {
        "param": "test"
    },
    "data": {},
    "files": {},
    "form": {},
    "headers": {
        "x-forwarded-proto": "https",
        "host": "postman-echo.com",
        "content-length": "0",
        "accept": "*/*",
        "accept-encoding": "gzip, deflate",
        "cache-control": "no-cache",
        "cookie": "sails.sid=s%3A57aLbjtudZ0eAUQPTGkyqZR-k148qAzN.tS52N8wbompQ8tzqpFZnu%2Bq4x5KLy1tR9g%2FhIn9Ss7s",
        "postman-token": "be4d5653-949f-4ea1-b63a-8572d1a8ffb5",
        "user-agent": "PostmanRuntime/7.13.0",
        "x-forwarded-port": "443"
    },
    "json": null,
    "url": "https://postman-echo.com/post?param=test"
} 
```

案例2：

发送一个Request，其中body为application/x-www-form-urlencoded类型，参数分别为param1=zed和param2=jiawen，请求URL如下：

```
https://postman-echo.com/post
```

![img](https://qwq.lsaiah.cn/picgo/20210219104027.png)

 **Postmam Body 数据类型数码：**

- form-data multipart/form-data是Web表单用于出书数据的默认编码。这模拟了在网站上填写表单并提交它，表单数据编辑器允许我们为数据设置键-值对。我们也可以为文件设置一个键，文件本身作为值进行设置。
- x-www-form-urlencoded该编码与URL参数中使用的编码相同。我们只需输入键-值对，postman会正确编码键和值，请注意，我们无法通过次编码模式上传文件。表单数据和urlencoded之间可能存在一些差异，因此请务必检查Api的编码实现，确认是否可以使用这种方式发送请求。
- raw请求可以包含任何内容，除了替换环境变量之外，Postman不触碰在编辑器中输入的字符串。无论你在编辑区输入什么内容，都会随请求一起发送到服务器。编辑器允许我们设置格式类型，以及使用原始主体发送的正确请求头。我们也可以手动设置Content-Type标题，这将覆盖Postman定义的设置
-  binary二进制数据可以让我们发送Postman我i发输入的内容，例如图像，音频或视频文件

**PUT**

HTTP PUT请求主要是从客户端向服务器传送的数据取代指定的文档的内容，PUT请求可以使用Query String Parameters以及body请求体将参数传递给服务器。

发送PUT请求，并传递字符参数“hello postman”

![img](https://img2018.cnblogs.com/blog/1522840/201905/1522840-20190526163005588-1825825637.png)

 **DELETE**

HTTP DELETE方法用于删除服务器上的资源，DELETE请求可以使用Query string parameters以及body请求体将参数传递给服务器

DELETE请求

```
https://postman-echo.com/delete
```

返回值

```json
{
    "args": {},
    "data": {},
    "files": {},
    "form": {},
    "headers": {
        "x-forwarded-proto": "https",
        "host": "postman-echo.com",
        "accept": "*/*",
        "accept-encoding": "gzip, deflate",
        "cache-control": "no-cache",
        "cookie": "sails.sid=s%3A-PlKnJ5cqYk6Uqz9tVwj-4o1lr5LZWrg.NRSWI4CcrBfKDAGgoUszOojVC%2F5v%2FY0YqZPFrRxaavg",
        "postman-token": "e8737025-ca4c-4b2c-91ef-338de8fd1f09",
        "user-agent": "PostmanRuntime/7.13.0",
        "x-forwarded-port": "443"
    },
    "json": null,
    "url": "https://postman-echo.com/delete"
}
```

## **Request Header**

请求头-用来说明服务器要使用的附加信息，比较重要的信息由Cookie、Referer、User-Agent等，在postman中可以在请求下方的Heafers栏目中设置，如下图所示

![img](https://qwq.lsaiah.cn/picgo/20210219104122.png)

## **Response Header**

响应头-其中包含了服务器对请求的应答信息，如Content-Type、Server、Set-Cookie等，在postman主界面下方Heerders或者Postman Console界面都可以查看Response Heaader信息

![img](https://qwq.lsaiah.cn/picgo/20210219104134.png)

![img](https://qwq.lsaiah.cn/picgo/20210219104146.png)

Tips： 通过控制台可以看到每次请求的Request Header详细信息

## **授权设置**

很多时候，出于安全考虑我们的接口并不希望公开。这是就需要使用授权（Authorization）机制，授权过程验证您是否具有访问服务器所需数据的权限。当您发送请求是，您通常必须包含参数，以确保请求具有访问和返回所需数据的权限。Postman提供授权类型，可以轻松的在Postman本地程序中处理生发验证协议。

Postman支持的授权协议类型如下：

- No Auth
- Bearer Token
- **Basic auth**
- **Digest Auth**
- **OAuth 1.0**
- OAuth 2.0
- **Hawk Authentication**
- AWS Signature
- NTLM Authentication [Beta]

这里主要介绍**加粗**的授权协议

### **Basic auth**

基本身份验证是一种比较简单的授权类型，需要经过验证的用户名和密码才能访问数据资源。这就需要我们输入用户名和应对 的密码。

案例：请求URL如下，授权账号为：

用户名：postman

密码：password

授权协议为：Basic auth

https://postman-echo.com/basic-auth

如果不输入用户名密码，直接用GET请求，则返回提示：Unauthorized

![img](https://qwq.lsaiah.cn/picgo/20210219104222.png)

如果输入用户密码，选择Basic auth授权类型，则返回如下结果

![img](https://qwq.lsaiah.cn/picgo/20210219104247.png)

### **Digest Auth**

Digest Auth是一个简单的认证机制，最初是为HTTP协议开发的，因此也常叫做HTTP摘要。其身份验证机制非常简单，它采用哈希加密方法，以避免铭文传输用户的口令。摘要认证就是要合适参与通信的两方都知道双方共享的口令。

当server想要查证用户的省份，它产生体个摘要盘问（Digest challenge），并发送给用户。典型的摘盘问用例如以下：

```
Digest realm = “iptel.org”, qop="auth,auth-int", nonce="dcd98b7102dd3f0e8b11d0f600bfb0c093",opaque=""

,algorithm=MD5
```

这里包含了一组参数，也要发送给用户。用户使用这些参数，来产生正确的摘要回答，并发送给server。再要盘问中的各个参数，其意义如下：

**realm（领域）**：领域参数是强制的，在全部的盘问中都不许。它的目的是鉴别SIP消息中的机密。在SIP实际引应用中，它通常设置为SIP代理所负责的域名。

**nonce（现时）**：这是由server规定的数据字符串，在server每次产生一个斩妖盘问时，这个参数都是不一样的（与前面所产生的不会雷同）。“现时”一般是由一些数据通过MD5杂凑运算构造的。这种数据通常包含时间标识和server的机密短语。这确保每一个“现时”都有一个有限的生命期（也就是过了一段时间会失效，并且以后不会使用），并且时独一无二的(即不论什么其他的server都不能产生一个同样的“现时”)。

**algorithm（算法）**：这是用来计算的算法。当前仅仅支持MD5算法。

**qop（保护的质量）**：这个参数规定server支持那种保护方案。client能够从列表中选择一个。值auth表示仅仅进行身份查验，auth-int表示进行查验外，另一些完整性保护。 

案例

请求URL如下

```
https://postman-echo.com/digest-auth
```

摘牌配置信息如下：用户密码和basic auth一样

```
Digest username=“postman”， realm=“Users”， nince=“ni1LiL0037PRRhofwdCLmwFsnEtH1lew”，uri=“？digest-auth”， opaque=“”
```

![img](https://qwq.lsaiah.cn/picgo/20210219104704.png)

执行结果如下

```
{
    "authenticated": true
}
```

### **Hawk Auth**

Hawk Auth是一个HTTP认证方案，使用MAC（message authentication code，消息认证算法），它提供了对请求进行部分加密的认证HTTP请求的方法。hawk方案要求提供一个共享对称密匙在服务器与客户端之间，通常这个共享的凭证在初试TLS（安全传输层协议）保护阶段建立的，或者是从客户端和服务器都可用的其他一些共享机密信息中获得的。

案例

请求URL如下：

```
https://postman-echo.com/auth/hawk
```

密匙信息如下：

- Hawk Auth ID：dh37fgj492je
- Hawk Auth Key：werxhqb98rpaxn39848xrunpaw3489ruxnpa98w4rxn
- Algorithm：sha256

![img](https://qwq.lsaiah.cn/picgo/20210219104739.png)

执行结果：

```
{
    "message": "Hawk Authentication Successful"
}
```

如果将Key改成其他任意的字符则返回如下结果：

```json
{
    "statusCode": 401,
    "error": "Unauthorized",
    "message": "Bad mac",
    "attributes": {
        "error": "Bad mac"
    }
}
```

### **OAuth 1.0**

OAuth(开放授权)是一个开放标准，允许客户让第三方应用访问该用户在某一网站上存储的私密的资源（如照片，视频，联系人列表），而无需将用户名和密码提供给第三方应用。

案例

请求URL如下：

```
https://postman-echo.com/oauth1
```

请求方式为GET，Add authorization data to 设置为：Request Headers

参数配置为：

```
Consumer Key：RKCGzna7bv9YD57c

Consumer Sceret：D+EdQ-gs$-%@2Nu7
```

![img](https://qwq.lsaiah.cn/picgo/20210219104811.png)

发送请求结果如下：

```json
{
    "status": "pass",
    "message": "OAuth-1.0a signature verification was successful"
}
```

如果Consumer Secret错误则返回如下结果：

```json
{
    "status": "fail",
    "message": "HMAC-SHA1 verification failed",
    "base_uri": "https://postman-echo.com/oauth1",
    "normalized_param_string": "oauth_consumer_key=RKCGzna7bv9YD57c&oauth_nonce=lBeuZSBkUw1&oauth_signature_method=HMAC-SHA1&oauth_timestamp=1558957675&oauth_version=1.0",
    "base_string": "GET&https%3A%2F%2Fpostman-echo.com%2Foauth1&oauth_consumer_key%3DRKCGzna7bv9YD57c%26oauth_nonce%3DlBeuZSBkUw1%26oauth_signature_method%3DHMAC-SHA1%26oauth_timestamp%3D1558957675%26oauth_version%3D1.0",
    "signing_key": "D%2BEdQ-gs%24-%25%402Nu7&"
} 
```

 扩展资料：[各个授权协议文档](https://www.jellythink.com/archives/169)

## **Cookie设置**

Cookie是存储在浏览器中的小片段信息，没次请求后都将其发送会服务器，以便在请求之间存储有用的信息。比如很多网站登录界面都有保留账号密码，以便下次登录。

由于HTTP是一种无状态的协议，服务器从网络连接上无从知道客户身份。怎么办呢？就给客户端们颁发一个通行证，没人一个，无论谁访问都必须携带自己通信证。这样服务器就能从通行证上确认客户身份了。这就是Cookie的工作原理。

Cookie是有服务端生成，储存在响应头中，返回客户端，客户端会将cookie存储下来，在客户端发送请求时，user-agent会自动获取本地存储的cookie，将cookie信息存储在请求头重，并发送给客户端。postman也可以设置、获取、删除cookie。

### **Set Cookies**

在send按钮下方点击Cookies文字菜单，弹出如下界面，然后可以设置Cookie。

![img](https://qwq.lsaiah.cn/picgo/20210219104904.png)

请求URL如下：

```
http://www.baidu.com/
```

请求方式为GET，添加Cookie值为hobby：rubikcube

 ![img](https://qwq.lsaiah.cn/picgo/20210219104919.png)

  打开Console找到Request Header可以看到自定义设置的Cookie内容

![img](https://qwq.lsaiah.cn/picgo/20210219104937.png)

### **Get Cookies**

Cookie获取比较简单，直接获取Response Headers里面的 set-cookie值即可，或者在主界面下方Cookie菜单栏里面也可以查看。

![img](https://qwq.lsaiah.cn/picgo/20210219104955.png)

**Delete Cookies**

点击Cookies文字菜单，然后可以根据需求去清除对应的Cookie。

![img](https://img2018.cnblogs.com/blog/1522840/201905/1522840-20190528183504998-309149762.png)

 **变量**

我们在开发不同阶段可能存在不同的环境，比如测试环境和是生产环境。

**测试环境API如下( 不是真实，用来玩的)：**

```
https://dev.postman.com/get
https://dev.postman.com/post
https://dev.postman.com/put
```

**生产环境如下：**

```
https://postman-echo.com/get
https://postman-echo.com/post
https://postman-echo.com/put
```

在这种情况下，按照常规思路要么维护两套环境的API，要么每次手动一个个去修改URL，两种方法都低效且麻烦，设置变量就可以很好的解决这个问题。

Postman变量类型

通过比较我们可以发现，以上两组API除了host不同之外其他都一样，其实吧host用变量替代，这样就可以灵活切换环境。

Postman提供了变量设置，有4种变量类型：

- 本地变量（Local Variable）
- 全局变量（Global Variable）
- 环境变量（Enviroment Variable）
- 数据变量（Data Variable）

Tips:数据变量结合Collection介绍，不在此处介绍

### **环境变量**

环境变量指在不同环境，同一个变量值随着环境不同而变化，比如我们上面举例场景就可以使用环境变量，当在测试环境时，host值为：dev.postman.com，当切换到生产环境时，host值变为：postman-echo.com。

 环境变量设置：在postman界面点击右上角眼睛图标，即可开始设置环境变量和全局变量。环境变量设置过程如下图所示：可以设置两种环境dev和release，dev是开发测试环境；release是正式生产的生产环境。host环境变量，根据不同的环境值而改变。

![img](https://qwq.lsaiah.cn/picgo/20210219105049.png)

![img](https://qwq.lsaiah.cn/picgo/20210219105103.png)

引用变量格式为{{varname}}，如下图所示：

![img](https://qwq.lsaiah.cn/picgo/20210219105120.png)

### **本地变量**

本地变量主要是针对单个URL请求设置的变量，作用域只局限在请求范围内。如请求URL：https://postman-echo.com/post，设置两个本地变量（user，passwd）作为参数。请求方式为post。

设置本地变量：

```
pm.variables.set("user","yi");
pm.variables.set("passwd","aeaqaaa");
```

![img](https://qwq.lsaiah.cn/picgo/20210219105142.png)

 引用本地变量格式：{{variable_name}}
![img](https://qwq.lsaiah.cn/picgo/20210219105154.png)

### **全局变量**

全局变量是指在所有环境里面，变量值都是一样的，全局变量的作用域是所有请求。

全局变量设置有两种方式：

- 页面设置
- 脚本设置

**页面设置**

点击右上角设置变量的图标，在Global选项菜单点击Edit菜单即可设置全局变量，如下图所示。全局变量的引用方式和环境变量一样。

**注意：当环境变量和全局变量名称一样，切换到某个环境时，环境变量会覆盖全局变量。**

![img](https://qwq.lsaiah.cn/picgo/20210219105209.png)

![img](https://qwq.lsaiah.cn/picgo/20210219105220.png)

**脚本设置**

在 Pre-request Script 编写如下代码可以设置全局变量：

```
pm.globals.set("variable_key","variable_value");
```

variable_key表示变量名称，variable_value代表变量值。

![img](https://qwq.lsaiah.cn/picgo/20210219105234.png)

案例实践

在实际接口测试中，接口经常会有关联。比如需要取上一个接口的某个返回值，然后作为参数传递到下一个接口作为参数。假设现在要获取A接口返回的userid值作为B接口的请求参数。

A、B接口请求URL如下：

```
A：https://postman-echo.com/postB:https://postman-echo.com/get
```

![img](https://qwq.lsaiah.cn/picgo/20210219105252.png)

![img](https://qwq.lsaiah.cn/picgo/20210219105302.png)

![img](https://qwq.lsaiah.cn/picgo/20210219105313.png)

 ![img](https://qwq.lsaiah.cn/picgo/20210219105324.png)

## **断言**

一般来说执行完测试，我们需要对测试结果来进行校验，判断结果是否符合我们的预期，也就是断言。在街口测试中一般会根据响应状态码或者返回的数据来进行断言。

postman提供一个测试沙盒（postman sandbox）测试沙盒是一个JavaScript执行环境，可以通过JS脚本来编写pre-request Script 和 test Script。

- pre-request Script（预置脚本）可以用来修改一些默认参数，在青丘发送之前执行。有点类似unittest框架中的seUP()方法。
- test Script（测试脚本）当接收到响应之后，在执行测试脚本。

**案例**

请求接口如下：

```
postman-echo.com/post
```

断言规则：

响应状态码：200

断言内容：返回的user参数值与定义的一致

响应时间：小于0.5s

测试脚本：

在pre-request Script定义变量user

```
pm.variables.set("user","zed");
```

![img](https://qwq.lsaiah.cn/picgo/20210219105341.png)

在Test编写断言脚本

```
//判断响应状态码
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});


//获取发送的参数值
username = pm.variables.get("user");
console.log(username)

//校验响应内容是否和请求一致
pm.test("Check username", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.json['user']).to.eql(username);
});

//检测响应时间是否小于0.5s
pm.test("Response time is less than 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});
```

![img](https://qwq.lsaiah.cn/picgo/20210219105442.png)

 引用本地变量

![img](https://qwq.lsaiah.cn/picgo/20210219105452.png)

**断言结果**

![img](https://qwq.lsaiah.cn/picgo/20210219105509.png)

## **批量执行**

### Collection

当我们想批量测试某个集合里面的各个API时，可以试用Collection Runner 来批量运行API，同时可以进行环境变量，迭代执行册数，延迟时间设置。

Collection入口

![img](https://qwq.lsaiah.cn/picgo/20210219105525.png)

 Collection执行前选项

![img](https://qwq.lsaiah.cn/picgo/20210219105538.png)

执行结果 

![img](https://qwq.lsaiah.cn/picgo/20210219105552.png)

###  **数据驱动**

有时我们针对一个接口需要测试很多不同的参数，如果每次一个个去修改参数值来进行测试这样效率肯定比较低下。因此需要每次迭代执行传入不同的参数进行测试，那么需要导入外部数据文件进行参数化，也就是数据驱动。

 **数据导入(此链接跳转Newman命令说明)**

如下图所示，data选择json数据文件：data.json，文件类型选择application/json Json数据内容如下：

```json
[{
    "username":"Jack",
    "passwd":"6666"
},{
    "username":"Bob",
    "passwd":"5555"
},{
    "username":"Marry",
    "passwd":"8888"
}]
```

![img](https://qwq.lsaiah.cn/picgo/20210219105632.png)

 执行结果

![img](https://qwq.lsaiah.cn/picgo/20210219105642.png)

**构建工作流**

再使用"Collection Runner"的时候，集合中的请求执行顺序就是在请求Collection中的显示排列顺序。但是，有的时候我们不希望按照这样的方式去执行，可能是执行完第个请求，再去执行第五个请求，然后再去执行第二个请求这样的顺序；那么在"Collection Runner"中如何去构建不同的执行顺序呢？

设置方法

最直接的方法就是在集合中拖动顺序，但是每次去拖动比较麻烦，特别是当请求比较多的时候。这时最高效的方法就是通过脚本设置。首先下载官方提供的案例文件：collection.json导入到postman，运行Collection结果如图所示： 

![img](https://qwq.lsaiah.cn/picgo/20210219105657.png)

接下看要调整执行顺序围为：Request1->Request3->Request2->Request4

首先在第一个请求Request1中Test代码栏中编辑如下代码，表示下一个请求为Request3

```
postman.setNextRequest('Request 3')
```

然后在Request3中Test代码栏中编辑如下代码，表示下一个请求为Request2

```
postman.setNextRequest('Request 2')
```

然后在Request2中Test代码栏中编辑如下代码，表示下一个请求为Request4

```
postman.setNextRequest('Request 4')
```

**注意：首个执行的请求要排在第一位**

**执行结果**

![img](https://qwq.lsaiah.cn/picgo/20210219105711.png)

相关资料：[collection runs 官方文档](https://learning.getpostman.com/docs/postman/collection_runs/intro_to_collection_runs/)

 **命令执行**

在之前我们都是在postman图形界面工具里面进行测试，但是有时候需要把测试脚本集成到CI平台，或者在非图形界面的系统环境下测试，此时就需要用到Newman。

### **Newman简介**

[Newman](https://www.npmjs.com/package/newman#using-newman-as-a-nodejs-module)时一款基于Node.js开发的可以运行postman的工具，使用Newman可以直接从命令行运行和测试postman集合。

**Newman应用**

**环境准备**

- Node.js
- cnpm或者npm

配置好环境后，执行如下命令安装newman

```
cnpm install newman -- global
```

输入如下命令检测安装是否成功

```
C:\Users\ccl>newman -v
4.4.1
```

**执行测试**

首先将postman的集合导出到桌面新建文件夹pmtest，如下图所示：

![img](https://qwq.lsaiah.cn/picgo/20210219105753.png)

打开cmd进入pmtest目录，输入如下命令：

```
newman run setNextRequest.postman_collection.json -d data.json -r html
```

命令说明

- run 代表要执行的postman脚本，即为导出的集合。
- -d 表示要执行的数据，也就是执行 [Collection数据驱动的data.json](https://www.cnblogs.com/youngleesin/p/10926189.html#datadriver) 的数据
- -r 生成测试报告类型，这里生成html格式报告

 更多方法请输入newman -h 即可查看

**报告查看**

在测试文件pmtest里面可以看到自动生成的newman文件夹，打开就可以看到生成的测试报告

Html报告样式：[newman-run-report](https://sutune.oss-cn-shenzhen.aliyuncs.com/interface_test/Postman/newman-run-report.html)

newman不仅支持生成html报告，还支持其他报告类型：

- JSON reporter
- JUNT/XML repoter
- Client report
- Html report

------

## **集成Jenkins**

### **Jenkins介绍**

jenkins是一个开源软件项目，是基于Java开发的一种[持续集成](https://baike.baidu.com/item/持续集成/6250744)工具，用于监控持续重复的工作，旨在一共一个开放易用的软件平台，使软件的持续集成变成可能。

### **下载与安装**

下载地址：https://jenkins.io/download/

下载后安装到指定的路径即可，默认启动也买你为localhost:8080，如果8080端口被占用无法打开，可以进入到jenkins安装目录，找到jenkins.xml配置文件，修改如下代码的端口号即可。

```
<arguments>-Xrs -Xmx256m -Dhudson.lifecycle=hudson.lifecycle.WindowsServiceLifecycle -jar "%BASE%\jenkins.war" --httpPort=8080 --webroot="%BASE%\war"</arguments>
```

### **集成步骤**

集成到jenkins的思路其实很简单，就把之前执行测试的cmd命令放到jenkins里面去执行，集成步骤也很简单：

- 首先新建一个项目：postman_api_test
- 然后再构建栏目下拉菜单选择 执行批处理命令（Execute Windows batch command）

```
cd C:\Users\ccl\Desktop\postman_api_test
newman run setNextRequest.postman_collection.json -d data.json -r html
```

Tips：我的文件夹中的文件与Newman应用的文件时一样的

![img](https://qwq.lsaiah.cn/picgo/20210219105831.png)

 

 如此保存后再jenkins主页进行构建任务即可，其他的设置如：定时执行，发送邮件报告等功能后期再Jenkin使用手册中详细介绍。

## **导出不同脚本语言**

 虽然Postman功能比较强大，但毕竟时一款商业工具，多少会有些限制。比如只支持JS脚本运行，如果享用自己熟悉的编程语言（如：python，java等）来做接口自动化测试就需要导出对应的语言脚本了。

**操作步骤**

Postman支持导出不同语言版本的脚本，到一个接口调试好之后，点击右侧的code字样即弹出图下界面可以选择语言。最后你需要选择语言版本即可生成对应的代码。

![img](https://qwq.lsaiah.cn/picgo/20210219105850.png)

 生成的代码片段可以点击Copy to Clipboard 复制。 

![img](https://qwq.lsaiah.cn/picgo/20210219105902.png)