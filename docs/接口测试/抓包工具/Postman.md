# Postman中的脚本

Postman包含一个基于Node.js的强大运行时，该运行时可让您向请求和集合中添加动态行为。这样，您就可以编写测试套件，构建可以包含动态参数的请求，在请求之间传递数据等等。您可以添加JavaScript代码以在流中的2个事件期间执行：

1. 一个请求之前被发送到服务器，作为 [预请求脚本](https://learning.postman.com/docs/writing-scripts/pre-request-scripts/) 下 **预请求脚本** 标签。
2. 收到响应后，作为 “**测试”** 选项卡下的 **测试**[脚本](https://learning.postman.com/docs/writing-scripts/test-scripts/)。

键入时，Postman会提示您建议-选择其中一项以自动完成代码。

[![脚本自动完成](https://assets.postman.com/postman-docs/11-autocomplete.gif)](https://assets.postman.com/postman-docs/11-autocomplete.gif)

您可以将预请求和测试脚本添加到集合，文件夹，集合中的请求或未保存到集合的请求。

## 脚本的执行顺序

在Postman中，单个请求的脚本执行顺序如下所示：

- 与请求关联的预请求脚本将在发送请求之前执行
- 发送请求后，将执行与请求关联的测试脚本

[![单个请求的工作流程](https://assets.postman.com/postman-docs/req-resp.png)](https://assets.postman.com/postman-docs/req-resp.png)

对于集合中的每个请求，脚本将按以下顺序执行：

- 与集合关联的预请求脚本将在集合中的每个请求之前运行。
- 与文件夹关联的预请求脚本将在文件夹中的每个请求之前运行。
- 与集合关联的测试脚本将在集合中的每个请求之后运行。
- 与文件夹关联的测试脚本将在文件夹中请求后运行。

[![集合中的请求工作流程](https://assets.postman.com/postman-docs/execOrder.png)](https://assets.postman.com/postman-docs/execOrder.png)

对于集合中的每个请求，脚本将始终根据以下层次结构运行：集合级脚本（如果有），文件夹级脚本（如果有），请求级脚本（如果有）。请注意，此执行顺序适用于预请求脚本和测试脚本。

例如，假设您有以下集合，该集合由一个文件夹和该文件夹中的两个请求构成。

[![控制台日志语句](https://assets.postman.com/postman-docs/Test_script2.png)](https://assets.postman.com/postman-docs/WS-console-log-statement.png)

如果您在集合，文件夹和请求的请求前和测试脚本部分中创建了日志语句，那么您将在[Postman控制台中](https://learning.postman.com/docs/sending-requests/troubleshooting-api-requests/)清楚地看到执行顺序。

[![登录控制台](https://assets.postman.com/postman-docs/logs-in-console.png)](https://assets.postman.com/postman-docs/logs-in-console.png)

### 这是如何运作的？

这是魔法吗？不，这是 [Postman Sandbox](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/)。Postman Sandbox是一个JavaScript执行环境，您可以在编写请求的请求前脚本和测试脚本时使用（Postman和Newman中都使用）。您在这些部分中编写的任何代码都将在此沙箱中执行。  

## 调试脚本

可以在**Pre-request Script** 标签或 **Tests** 标签下编写调试脚本 ，并在[Postman Console中](https://learning.postman.com/docs/sending-requests/troubleshooting-api-requests/)记录有用的消息 。



# 编写请求前脚本

您可以在Postman中使用请求前脚本来在请求运行之前执行JavaScript。通过在“**请求前脚本”**选项卡中包含请求，集合或文件夹的代码，可以执行预处理，例如设置变量值，参数，标头和主体数据。您还可以使用请求前脚本来调试代码，例如通过将输出记录到控制台来进行调试。

请求前脚本的用法示例如下：

- 您在集合中有一系列请求，并按顺序运行它们，例如，使用[集合运行器](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)。
- 第二个请求取决于从第一个请求返回的值。
- 在将该值传递给第二个请求之前，需要对其进行处理。
- 第一个请求将数据值从响应字段设置为其**Tests**脚本中的变量。
- 第二个请求检索该值并在其**Pre-request脚本**中对其进行处理，然后将处理后的值设置为一个变量（在第二个请求中，例如在其参数中引用）。

## 在请求运行之前编写脚本

要包含要在Postman发送请求之前执行的代码，请打开请求并选择“**预请求脚本”**选项卡。

![预请求标签](https://assets.postman.com/postman-docs/pre-request-tab-empty.jpg)

输入在运行请求之前需要处理的JavaScript。

![预请求代码](https://assets.postman.com/postman-docs/pre-request-script.jpg)

当您点击**Send时**，代码将在Postman将请求发送到API之前执行。

## 重用请求前脚本

您可以将请求前脚本添加到整个集合以及集合中的文件夹。在这两种情况下，您的请求前脚本都将在集合或文件夹中的每个请求之前运行。这使您可以定义对多个请求执行所需的常用预处理或调试步骤。

为了预处理添加到组的请求，定位在收集或文件夹**集合**上留下Postman。点击**...**以**查看更多动作**，然后选择**编辑**。

![收款动作](https://assets.postman.com/postman-docs/edit-collection-action.jpg)

打开“**预请求脚本”**以输入将在集合或文件夹中的每个请求之前运行的代码。

![收集预请求脚本](https://assets.postman.com/postman-docs/edit-collection-pre-request.jpg)

> 您可以在首次创建集合或文件夹时或之后的任何时间定义预请求脚本。

# 编写测试

您可以使用JavaScript为Postman API请求编写测试脚本。通过测试，您可以确保您的API能够按预期运行，确定服务之间的集成可靠运行，并验证新开发是否没有破坏任何现有功能。当您的API项目出现问题时，您还可以使用测试代码来帮助调试过程。

> 例如，您可以编写测试以通过发送不完整数据的请求来验证API的错误处理。

您可以将测试添加到单个[请求](https://learning.postman.com/docs/sending-requests/requests/)，文件夹和[集合](https://learning.postman.com/docs/sending-requests/intro-to-collections/)。Postman提供的代码片段可以单击添加，然后根据需要进行修改以适合您的逻辑。

要将测试添加到请求中，请打开请求，然后在“测试”选项卡中输入代码。测试将在请求运行后执行。您将能够在**测试结果**选项卡中的响应数据旁边看到输出。

![要求测试标签](https://assets.postman.com/postman-docs/request-test-tab.jpg)

## 编写测试脚本

您的测试脚本可以使用动态变量，对响应数据执行测试断言，并在请求之间传递数据。在**测试**的请求选项卡，您可以手动输入您的JavaScript或使用**片段**，你会看到在代码编辑器的右侧。

测试将在收到响应后执行，因此，当您单击**Send时**，当响应数据从API返回时，Postman将运行您的测试脚本。

> 如果您需要在请求运行之前执行代码，请改用“[请求前脚本”](https://learning.postman.com/docs/writing-scripts/pre-request-scripts/)。有关在请求运行时脚本如何执行的更多信息，请参见[脚本简介](https://learning.postman.com/docs/writing-scripts/intro-to-scripts/)。

要执行测试以验证请求返回的数据，可以使用`pm.response`对象。您可以使用`pm.test`函数定义测试，并提供一个名称和函数，该函数返回一个布尔值（`true`或`false`）来指示测试是通过还是失败。您可以在声明中使用[ChaiJS BDD](https://www.chaijs.com/api/bdd/)语法`pm.expect`来测试响应详细信息。

该`.test`函数的第一个参数是文本字符串，它将出现在测试结果输出中，因此您可以使用它来识别测试，并将测试的目的传达给查看结果的任何人。

例如，在“**测试”**选项卡中为任何请求输入以下内容，以测试响应状态代码是否为`200`。

```js
pm.test("Status test", function () {
    pm.response.to.have.status(200);
});
```

![测试状态示例](https://assets.postman.com/postman-docs/example-test-status.jpg)

单击**发送**以运行您的请求，然后在响应部分中打开**测试结果**。选项卡标题显示通过了多少测试，总共运行了多少。您还可以在通过，跳过和失败的测试结果之间切换。

![试验结果](https://assets.postman.com/postman-docs/test-result-status.jpg)

如果请求返回了`200`状态码，则测试将通过，否则将失败。尝试在测试脚本中更改预期的状态代码，然后再次运行请求。

![测试结果失败](https://assets.postman.com/postman-docs/failed-test-status.jpg)

使用`pm.expect`语法可以为测试结果消息提供不同的格式-尝试使用其他方法来获得最有用的输出。

![测试结果失败](https://assets.postman.com/postman-docs/expect-test-syntax.jpg)

> 使用**“**[简介](https://documenter.getpostman.com/view/1559645/RzZFCGFR?version=latest)**”中**的**“在Postman**中**运行”**按钮[来编写测试集合，](https://documenter.getpostman.com/view/1559645/RzZFCGFR?version=latest)以将包含一些示例测试脚本的模板导入Postman并进行代码试验。

您的代码可以测试请求[环境](https://learning.postman.com/docs/sending-requests/managing-environments/)，如以下示例所示：

```js
pm.test("environment to be production", function () {
    pm.expect(pm.environment.get("env")).to.equal("production");
});
```

您可以使用不同的语法变体以您认为可读的方式编写测试，并且适合您的应用程序和测试逻辑。

```js
pm.test("response should be okay to process", function () {
    pm.response.to.not.be.error;
    pm.response.to.have.jsonBody("");
    pm.response.to.not.have.jsonBody("error");
});
```

测试可以使用适合于响应数据格式的语法来确定请求响应的有效性。

```js
pm.test("response must be valid and have a body", function () {
     pm.response.to.be.ok;
     pm.response.to.be.withBody;
     pm.response.to.be.json;
});
```

您的脚本可以包含您需要的许多测试，并且当您单击**Save**时，这些脚本将与其余的请求详细信息一起**保存**。如果您共享一个收藏集或发布文档/“在Postman中运行”按钮，则查看或导入模板的任何人都将包含您的测试代码。

### 使用摘要

你会看到一个选择常用的测试代码摘录的**片段**，以测试编辑器的右侧。单击添加一个，它将出现在编辑器中。代码段可以加快脚本入门的速度-您可以在添加代码段以满足自己的测试要求之后对其进行编辑。

![添加了代码段](https://assets.postman.com/postman-docs/added-test-snippet.jpg)

## 测试集合和文件夹

您可以将测试脚本添加到集合，文件夹或集合中的单个请求中。与集合关联的测试脚本将在集合中的每个请求之后运行。与文件夹关联的测试脚本将在文件夹中的每个请求之后运行。这样，您可以在每次请求后重用通常执行的测试。

> 将脚本添加到集合和文件夹后，您可以测试API项目中的工作流程。这有助于确保您的请求涵盖典型场景，从而为应用程序用户提供可靠的体验。

您可以通过单击集合或文件夹名称旁边的**查看更多操作**（...），然后选择**编辑**来更新集合和文件夹脚本。选择“**测试”**选项卡以添加或更新脚本。您也可以在首次创建集合时添加集合脚本。

![收集测试](https://assets.postman.com/postman-docs/collection-test-script.jpg)

当你[运行一个集合](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)，你将看到由集合亚军输出的测试结果。

![收集测试](https://assets.postman.com/postman-docs/collection-tests-run.jpg)

您可以编写脚本来使用[分支和循环](https://learning.postman.com/docs/running-collections/building-workflows/)控制请求运行的顺序。

# 测试脚本示例

您可以使用请求和集合中的“**测试”**选项卡来编写测试，这些测试将在Postman收到您向其发送请求的API的响应时执行。您可以为每个请求添加任意数量的测试。将测试添加到**Collection时**，它们将在其中的每个请求之后执行。

Postman在脚本区域的右侧显示代码段。您可以添加这些以试用常见脚本，并可以对其进行调整以适合您的需求和请求/响应详细信息。

## 内容

- [测试入门](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#getting-started-with-tests)
- [使用多个断言](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#using-multiple-assertions)
- [解析响应主体数据](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#parsing-response-body-data)
  - [处理未解析的响应](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#handling-responses-that-dont-parse)
- [对HTTP响应进行断言](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#making-assertions-on-the-http-response)
  - [测试响应主体](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#testing-response-body)
  - [测试状态码](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#testing-status-codes)
  - [测试头](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#testing-headers)
  - [测试cookie](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#testing-cookies)
  - [测试响应时间](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#testing-response-times)
- [常见断言示例](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#common-assertion-examples)
  - [针对变量声明响应值](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#asserting-a-response-value-against-a-variable)
  - [断言值类型](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#asserting-a-value-type)
  - [声明数组属性](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#asserting-array-properties)
  - [声明对象属性](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#asserting-object-properties)
  - [断言值在集合中](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#asserting-that-a-value-is-in-a-set)
  - [断言包含一个对象](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#asserting-that-an-object-is-contained)
  - [声明当前环境](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#asserting-the-current-environment)
- [解决常见的测试错误](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#troubleshooting-common-test-errors)
  - [断言深度相等错误](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#assertion-deep-equality-error)
  - [JSON未定义错误](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#json-not-defined-error)
  - [断言未定义错误](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#assertion-undefined-error)
  - [测试没有失败](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#test-not-failing)
- [验证响应结构](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#validating-response-structure)
- [发送异步请求](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#sending-an-asynchronous-request)
- [较旧的Postman写作风格（已弃用）](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#older-style-of-writing-postman-tests-deprecated)

## 测试入门

要尝试首次编写测试脚本，请在Postman应用程序中打开一个请求，然后打开“**测试”**选项卡。输入以下JavaScript代码：

```js
pm.test("Status code is 200", function () {
  pm.response.to.have.status(200);
});
```

此代码使用该`pm`库来运行该`test`方法。文本字符串将出现在测试输出中。测试中的函数表示一个断言。Postman测试可以使用[Chai断言库BDD](https://www.chaijs.com/api/bdd/)语法，该语法提供了一些选项来优化您和您的协作者对测试的可读性。在这种情况下，代码使用BDD链`to.have`来表达断言。

此测试检查API返回的响应代码。如果响应代码为`200`，则测试将通过，否则将失败。单击**发送，**然后在响应区域中检查**测试结果**输出。

[![测试输出](https://assets.postman.com/postman-docs/example-test-assertion-result.jpg)](https://assets.postman.com/postman-docs/example-test-assertion-result.jpg)

尝试更改断言代码中的状态代码并再次运行，以查看测试结果通过或失败时的外观如何。

您可以采用各种方式来构造测试断言，以根据您希望结果输出的方式来适应您的逻辑和偏好。以下代码是使用`expect`语法完成与上述测试相同的测试的另一种方式：

```js
pm.test("Status code is 200", () => {
  pm.expect(pm.response.code).to.eql(200);
});
```

> 请参阅[Chai Docs](https://www.chaijs.com/api/bdd/)，以获取断言语法选项的完整概述。

## 使用多个断言

您的测试可以在单个测试中包含多个断言，您可以使用此断言将相关断言分组在一起。

```js
pm.test("The response has all properties", () => {
    //parse the response json and test three properties
    const responseJson = pm.response.json();
    pm.expect(responseJson.type).to.eql('vip');
    pm.expect(responseJson.name).to.be.a('string');
    pm.expect(responseJson.id).to.have.lengthOf(1);
});
```

如果任何包含的断言失败，则整个测试将失败。所有断言必须成功才能通过测试。

## 解析响应主体数据

为了对响应执行断言，首先需要将数据解析为断言可以使用的JavaScript对象。

要解析JSON数据，请使用以下语法：

```js
const responseJson = pm.response.json();
```

要解析XML，请使用以下命令：

```js
const responseJson = xml2Json(pm.response.text());
```

> 如果您要处理复杂的XML响应，则可能会发现[控制台日志记录](https://learning.postman.com/docs/sending-requests/troubleshooting-api-requests/#using-the-console)很有用。

要解析CSV，请使用[CSV解析](https://github.com/adaltas/node-csv-parse)实用程序：

```js
const parse = require('csv-parse/lib/sync');
const responseJson = parse(pm.response.text());
```

要解析HTML，可以使用[cheerio](https://cheerio.js.org/)：

```js
const $ = cheerio.load(pm.response.text());
//output the html for testing
console.log($.html());
```

### 处理未解析的响应

如果您无法将响应主体解析为JavaScript，因为它没有被格式化为JSON，XML，HTML，CSV或任何其他可解析的数据格式，则您仍然可以对数据进行断言。

您可以测试响应正文是否包含字符串：

```js
pm.test("Body contains string",() => {
  pm.expect(pm.response.text()).to.include("customer_id");
});
```

这不会告诉您在哪里遇到了字符串，因为它在整个响应主体上执行测试。您还可以测试响应是否与字符串匹配（通常仅对简短响应有效）：

```js
pm.test("Body is string", function () {
  pm.response.to.have.body("whole-body-text");
});
```

## 对HTTP响应进行断言

您的测试可以检查请求响应的各个方面，包括[正文](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#testing-response-body)，[状态码](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#testing-status-codes)，[标头](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#testing-headers)，[cookie](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#testing-cookies)，[响应时间](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#testing-response-times)等。

### 测试响应主体

您可以在响应正文中检查特定值：

```js
pm.test("Person is Jane", () => {
  const responseJson = pm.response.json();
  pm.expect(responseJson.name).to.eql("Jane");
  pm.expect(responseJson.age).to.eql(23);
});
```

### 测试状态码

您可以测试响应状态代码：

```js
pm.test("Status code is 201", () => {
  pm.response.to.have.status(201);
});
```

如果要测试状态码是否为一组，可以将它们全部包含在数组中并使用`oneOf`：

```js
pm.test("Successful POST request", () => {
  pm.expect(pm.response.code).to.be.oneOf([201,202]);
});
```

您还可以检查状态代码文本：

```js
pm.test("Status code name has string", () => {
  pm.response.to.have.status("Created");
});
```

### 测试头

您可以检查响应头是否存在：

```js
pm.test("Content-Type header is present", () => {
  pm.response.to.have.header("Content-Type");
});
```

您还可以测试具有特定值的响应头：

```js
pm.test("Content-Type header is application/json", () => {
  pm.expect(pm.response.headers.get('Content-Type')).to.eql('application/json');
});
```

### 测试cookie

您可以测试响应中是否存在Cookie：

```js
pm.test("Cookie JSESSIONID is present", () => {
  pm.expect(pm.cookies.has('JSESSIONID')).to.be.true;
});
```

您还可以测试特定的Cookie值：

```js
pm.test("Cookie isLoggedIn has value 1", () => {
  pm.expect(pm.cookies.get('isLoggedIn')).to.eql('1');
});
```

### 测试响应时间

您可以测试响应时间是否在指定范围内：

```js
pm.test("Response time is less than 200ms", () => {
  pm.expect(pm.response.responseTime).to.be.below(200);
});
```

## 常见断言示例

继续阅读一些常见断言的示例，这些示例可能对您的脚本有用，如下面概述的那样，或者通过编辑详细信息以适合您自己的需求。

> 有关您可以在断言中包括的内容的更全面概述，请参阅[Chai Docs](https://www.chaijs.com/api/bdd/)。

### 针对变量声明响应值

您可以检查响应属性是否与变量（在这种情况下为环境变量）具有相同的值。

```js
pm.test("Response property matches environment variable", function () {
  pm.expect(pm.response.json().name).to.eql(pm.environment.get("name"));
});
```

> 有关可用于在脚本中操作变量的操作的概述，请参见[使用变量](https://learning.postman.com/docs/sending-requests/variables/)。

### 断言值类型

您可以测试响应的任何部分的类型。

```js
/* response has this structure:
{
  "name": "Jane",
  "age": 29,
  "hobbies": [
    "skating",
    "painting"
  ],
  "email": null
}
*/
const jsonData = pm.response.json();
pm.test("Test data type of the response", () => {
  pm.expect(jsonData).to.be.an("object");
  pm.expect(jsonData.name).to.be.a("string");
  pm.expect(jsonData.age).to.be.a("number");
  pm.expect(jsonData.hobbies).to.be.an("array");
  pm.expect(jsonData.website).to.be.undefined;
  pm.expect(jsonData.email).to.be.null;
});
```

### 声明数组属性

您可以检查数组是否为空，以及是否包含特定项目。

```js
/*
response has this structure:
{
  "errors": [],
  "areas": [ "goods", "services" ],
  "settings": [
    {
      "type": "notification",
      "detail": [ "email", "sms" ]
    },
    {
      "type": "visual",
      "detail": [ "light", "large" ]
    }
  ]
}
*/

const jsonData = pm.response.json();
pm.test("Test array properties", () => {
    //errors array is empty
  pm.expect(jsonData.errors).to.be.empty;
    //areas includes "goods"
  pm.expect(jsonData.areas).to.include("goods");
    //get the notification settings object
  const notificationSettings = jsonData.settings.find
      (m => m.type === "notification");
  pm.expect(notificationSettings)
    .to.be.an("object", "Could not find the setting");
    //detail array should include "sms"
  pm.expect(notificationSettings.detail).to.include("sms");
    //detail array should include all listed
  pm.expect(notificationSettings.detail)
    .to.have.members(["email", "sms"]);
});
```

> 顺序`.members`不影响测试。

### 声明对象属性

您可以断言对象包含键或属性。

```js
pm.expect({a: 1, b: 2}).to.have.all.keys('a', 'b');
pm.expect({a: 1, b: 2}).to.have.any.keys('a', 'b');
pm.expect({a: 1, b: 2}).to.not.have.any.keys('c', 'd');
pm.expect({a: 1}).to.have.property('a');
pm.expect({a: 1, b: 2}).to.be.an('object')
  .that.has.all.keys('a', 'b');
```

> 目标可以是`object`，`set`，`array`或`map`。如果`.keys`在不使用`.all`或`.any`的情况下运行，则表达式默认为`.all`。由于`.keys`行为会因目标而异`type`，因此建议`type`在`.keys`与结合使用之前进行检查`.a`。

### 断言值在集合中

您可以根据有效选项列表检查响应值。

```js
pm.test("Value is in valid list", () => {
  pm.expect(pm.response.json().type)
    .to.be.oneOf(["Subscriber", "Customer", "User"]);
});
```

### 断言包含一个对象

您可以检查对象是否是父对象的一部分。

```js
/*
response has the following structure:
{
  "id": "d8893057-3e91-4cdd-a36f-a0af460b6373",
  "created": true,
  "errors": []
}
*/

pm.test("Object is contained", () => {
  const expectedObject = {
    "created": true,
    "errors": []
  };
  pm.expect(pm.response.json()).to.deep.include(expectedObject);
});
```

> 使用`.deep`使所有`.equal`，`.include`，`.members`，`.keys`，并`.property`随后在使用深平等（松平等），而不是严格（===）平等链断言。虽然`.eql`还松散地进行了比较，但`.deep.equal`导致深度相等性比较也可用于链中后面的任何其他声明，而`.eql`不会。

### 声明当前环境

您可以在Postman中检查活动的（当前选择的）环境。

```js
pm.test("Check the active environment", () => {
  pm.expect(pm.environment.name).to.eql("Production");
});
```

## 解决常见的测试错误

当您在测试脚本中遇到错误或意外行为时，Postman [Console](https://learning.postman.com/docs/sending-requests/troubleshooting-api-requests/)可以帮助您识别源。通过将`console.log`调试语句与测试断言结合在一起，您可以检查HTTP请求和响应的内容以及Postman数据项（例如变量）。

[![Postman控制台](https://assets.postman.com/postman-docs/console-logs-in-pane.jpg)](https://assets.postman.com/postman-docs/console-logs-in-pane.jpg)

单击Postman左下方的**控制台**以将其打开。

您可以记录变量或响应属性的值：

```js
console.log(pm.collectionVariables.get("name"));
console.log(pm.response.json().name);
```

您可以记录变量或响应属性的类型：

```js
console.log(typeof pm.response.json().id);
```

通常，您可以使用控制台日志来标记代码的执行，有时也称为“跟踪语句”：

```js
if (pm.response.json().id) {
  console.log("id was found!");
  // do something
} else {
  console.log("no id ...");
  //do something else
}
```

### 断言深度相等错误

您可能会遇到`AssertionError: expected  to deeply equal ''`。例如，这将由以下代码引起：

```js
pm.expect(1).to.eql("1");
```

发生这种情况是因为测试正在将数字与字符串值进行比较。仅当类型和值相等时，测试才会返回true。

### JSON未定义错误

您可能会遇到`ReferenceError: jsonData is not defined`问题。当您尝试引用未声明的JSON对象或超出测试代码范围的JSON对象时，通常会发生这种情况。

```js
pm.test("Test 1", () => {
  const jsonData = pm.response.json();
  pm.expect(jsonData.name).to.eql("John");
});

pm.test("Test 2", () => {
  pm.expect(jsonData.age).to.eql(29); // jsonData is not defined
});
```

确保所有将测试数据设置为变量的代码都可用于所有测试代码，例如，在这种情况下，如果移至`const jsonData = pm.response.json();`第一个代码之前`pm.test`，则可用于两个测试功能。

### 断言未定义错误

您可能会遇到`AssertionError: expected undefined to deeply equal..`问题。通常，当您引用的属性不存在或超出范围时，就会发生这种情况。

```js
pm.expect(jsonData.name).to.eql("John");
```

在上面的示例中，如果看到`AssertionError: expected undefined to deeply equal 'John'`，则表明该`name`属性未在`jsonData`对象中定义。

### 测试没有失败

在某些情况下，您可能期望测试失败，但不会失败。

```js
//test function not properly defined - missing second parameter
pm.test("Not failing", function () {
    pm.expect(true).to.eql(false);
});
```

确保您的测试代码在语法上正确无误，然后尝试再次发送您的请求。

## 验证响应结构

您可以使用tv4进行JSON模式验证。

```js
const schema = {
 "items": {
 "type": "boolean"
 }
};
const data1 = [true, false];
const data2 = [true, 123];

pm.test('Schema is valid', function() {
  pm.expect(tv4.validate(data1, schema)).to.be.true;
  pm.expect(tv4.validate(data2, schema)).to.be.true;
});
```

您还可以默认使用ajv验证JSON模式。

```js
const schema = {
  "properties": {
    "alpha": {
      "type": "boolean"
    }
  }
};
pm.test('Schema is valid', function() {
  pm.response.to.have.jsonSchema(schema);
});
```

## 发送异步请求

您可以从测试代码发送请求并记录响应。

```js
pm.sendRequest("https://postman-echo.com/get", function (err, response) {
    console.log(response.json());
});
```

## 较旧的Postman写作风格（已弃用）

> **本节涉及在较早版本的Postman中使用的不赞成使用的脚本语法。如果您现在正在编写脚本，请使用上面的语法。**

Postman测试的较早编写风格依赖于为`tests`对象设置值。您可以为对象中的元素设置一个描述性键，然后断言它是对还是错。例如，以下将检查响应正文是否包含`user_id`字符串：

```js
tests["Body contains user_id"] = responsebody.has("user_id");
```

您可以根据需要添加多少键，具体取决于要测试的内容。您可以在“**测试”**选项卡下的响应查看器中查看测试结果。选项卡标题显示了通过了多少测试，并在其中列出了您在tests变量中设置的键。如果该值评估为true，则测试通过。

```js
//set an environment variable
postman.setEnvironmentVariable("key", "value");

//set a nested object as an environment variable
const array = [1, 2, 3, 4];
postman.setEnvironmentVariable("array", JSON.stringify(array, null, 2));
const obj = { a: [1, 2, 3, 4], b: { c: 'val' } };
postman.setEnvironmentVariable("obj", JSON.stringify(obj));

//get an environment variable
postman.getEnvironmentVariable("key");

//get an environment variable whose value is a stringified object
//(wrap in a try-catch block if the data is coming from an unknown source)
const array = JSON.parse(postman.getEnvironmentVariable("array"));
const obj = JSON.parse(postman.getEnvironmentVariable("obj"));

//clear an environment variable
postman.clearEnvironmentVariable("key");

//set a global variable
postman.setGlobalVariable("key", "value");

//get a global variable
postman.getGlobalVariable("key");

//clear a global variable
postman.clearGlobalVariable("key");

//check if response body contains a string
tests["Body matches string"] = responseBody.has("string_you_want_to_search");

//check if response body is equal to a string
tests["Body is correct"] = responseBody === "response_body_string";

//check for a JSON value
const data = JSON.parse(responseBody);
tests["Your test name"] = data.value === 100;

//Content-Type is present (Case-insensitive checking)
tests["Content-Type is present"] = postman.getResponseHeader("Content-Type");
tests["Content-Type is present"] = postman.getResponseHeader("Content-Type");
//getResponseHeader() method returns the header value, if it exists

//Content-Type is present (Case-sensitive)
tests["Content-Type is present"] = responseHeaders.hasOwnProperty("Content-Type");

//response time is less than 200ms
tests["Response time is less than 200ms"] = responseTime < 200;

//response time is within a specific range
//(lower bound inclusive, upper bound exclusive)
tests["Response time is acceptable"] = _.inRange(responseTime, 100, 1001);

//status code is 200
tests["Status code is 200"] = responseCode.code === 200;

//code name contains a string
tests["Status code name has string"] = responseCode.name.has("Created");

//successful POST request status code
tests["Successful POST request"] = responseCode.code === 201 || responseCode.code === 202;
```



# 动态变量

邮差使用[伪造者库](https://www.npmjs.com/package/faker)来生成伪数据。您可以生成随机名称，地址，电子邮件地址等。您可以多次使用这些预定义的变量来为每个请求返回不同的值。

您可以像在Postman中使用任何其他变量一样使用这些变量。它们的值在执行时生成，并且它们的名称以`$`符号开头，例如`$guid`，`$timestamp`等等。

以下是动态变量的列表，其动态值在请求/收集运行期间随机生成。

> 要在预请求或测试脚本中使用动态变量，您需要使用`pm.variables.replaceIn()`，例如`pm.variables.replaceIn('{{$randomFirstName}}')`。

### Common

| **Variable Name**   | **Description**                       | **Examples**                             |
| ------------------- | ------------------------------------- | ---------------------------------------- |
| **`$guid`**         | A `uuid-v4` style guid                | `"611c2e81-2ccb-42d8-9ddc-2d0bfa65c1b4"` |
|                     |                                       | `"3a721b7f-7dc9-4c45-9777-516942b98e0d"` |
|                     |                                       | `"22eca807-006b-47df-9511-e92e37f5071a"` |
| **`$timestamp`**    | The current UNIX timestamp in seconds | `1562757107`, `1562757108`, `1562757109` |
| **`$isoTimestamp`** | The current ISO timestamp at zero UTC | `2020-06-09T21:10:36.177Z`               |
|                     |                                       | `2019-10-21T06:05:50.000Z`               |
|                     |                                       | `2019-07-29T18:29:00.000Z`               |
| **`$randomUUID`**   | A random 36-character UUID            | `"6929bb52-3ab2-448a-9796-d6480ecad36b"` |
|                     |                                       | `"53151b27-034f-45a0-9f0a-d7b6075b67d0"` |
|                     |                                       | `"727131a2-2717-44ad-ab02-006587e947dc"` |

### Text, Numbers and Colors

| **Variable Name**         | **Description**                     | **Examples**                          |
| ------------------------- | ----------------------------------- | ------------------------------------- |
| **`$randomAlphaNumeric`** | A random alpha-numeric character    | `6`, `"y"`, `"z"`                     |
| **`$randomBoolean`**      | A random boolean value (true/false) | `true`, `false`, `false`, `true`      |
| **`$randomInt`**          | A random integer between 1 and 1000 | `802`, `494`, `200`                   |
| **`$randomColor`**        | A random color                      | `"red"`, `"fuchsia"`, `"grey"`        |
| **`$randomHexColor`**     | A random hex value                  | `"#47594a"`, `"#431e48"`, `"#106f21"` |
| **`$randomAbbreviation`** | A random abbreviation               | `SQL`, `PCI`, `JSON`                  |

### Internet and IP Addresses

| **Variable Name**       | **Description**                               | **Examples**                                                 |
| ----------------------- | --------------------------------------------- | ------------------------------------------------------------ |
| **`$randomIP`**         | A random IPv4 address                         | `241.102.234.100`, `216.7.27.38`                             |
| **`$randomIPV6`**       | A random IPv6 address                         | `dbe2:7ae6:119b:c161:1560:6dda:3a9b:90a9`                    |
|                         |                                               | `c482:23a4:ce4c:a668:7736:6cc5:b0b6:cc37`                    |
|                         |                                               | `c791:18d1:fbba:87d8:d929:22aa:5a0a:ac3d`                    |
| **`$randomMACAddress`** | A random MAC address                          | `33:d4:68:5f:b4:c7`, `1f:6e:db:3d:ed:fa`                     |
| **`$randomPassword`**   | A random 15-character alpha-numeric password  | `t9iXe7COoDKv8k3`, `QAzNFQtvR9cg2rq`                         |
| **`$randomLocale`**     | A random two-letter language code (ISO 639-1) | `"ny"`, `"sr"`, `"si"`                                       |
| **`$randomUserAgent`**  | A random user agent                           | `Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.9.8; rv:15.6) Gecko/20100101 Firefox/15.6.6` |
|                         |                                               | `Opera/10.27 (Windows NT 5.3; U; AB Presto/2.9.177 Version/10.00)` |
|                         |                                               | `Mozilla/5.0 (Windows NT 6.2; rv:13.5) Gecko/20100101 Firefox/13.5.6` |
| **`$randomProtocol`**   | A random internet protocol                    | `"http"`, `"https"`                                          |
| **`$randomSemver`**     | A random semantic version number              | `7.0.5`, `2.5.8`, `6.4.9`                                    |

### Names

| **Variable Name**       | **Description**              | **Examples**                                           |
| ----------------------- | ---------------------------- | ------------------------------------------------------ |
| **`$randomFirstName`**  | A random first name          | `Ethan`, `Chandler`, `Megane`                          |
| **`$randomLastName`**   | A random last name           | `Schaden`, `Schneider`, `Willms`                       |
| **`$randomFullName`**   | A random first and last name | `Connie Runolfsdottir`, `Sylvan Fay`, `Jonathon Kunze` |
| **`$randomNamePrefix`** | A random name prefix         | `Dr.`, `Ms.`, `Mr.`                                    |
| **`$randomNameSuffix`** | A random name suffix         | `I`, `MD`, `DDS`                                       |

### Profession

| **Variable Name**          | **Description**         | **Examples**                            |
| -------------------------- | ----------------------- | --------------------------------------- |
| **`$randomJobArea`**       | A random job area       | `Mobility`, `Intranet`, `Configuration` |
| **`$randomJobDescriptor`** | A random job descriptor | `Forward`, `Corporate`, `Senior`        |
| **`$randomJobTitle`**      | A random job title      | `International Creative Liaison`,       |
|                            |                         | `Product Factors Officer`,              |
|                            |                         | `Future Interactions Executive`         |
| **`$randomJobType`**       | A random job type       | `Supervisor`, `Manager`, `Coordinator`  |

### Phone, Address and Location

| **Variable Name**           | **Description**                                     | **Examples**                                                |
| --------------------------- | --------------------------------------------------- | ----------------------------------------------------------- |
| **`$randomPhoneNumber`**    | A random 10-digit phone number                      | `700-008-5275`, `494-261-3424`, `662-302-7817`              |
| **`$randomPhoneNumberExt`** | A random phone number with extension (12 digits)    | `27-199-983-3864`, `99-841-448-2775`                        |
| **`$randomCity`**           | A random city name                                  | `Spinkahaven`, `Korbinburgh`, `Lefflerport`                 |
| **`$randomStreetName`**     | A random street name                                | `Kuhic Island`, `General Street`, `Kendrick Springs`        |
| **`$randomStreetAddress`**  | A random street address                             | `5742 Harvey Streets`, `47906 Wilmer Orchard`               |
| **`$randomCountry`**        | A random country                                    | `Lao People's Democratic Republic`, `Kazakhstan`, `Austria` |
| **`$randomCountryCode`**    | A random 2-letter country code (ISO 3166-1 alpha-2) | `CV`, `MD`, `TD`                                            |
| **`$randomLatitude`**       | A random latitude coordinate                        | `55.2099`, `27.3644`, `-84.7514`                            |
| **`$randomLongitude`**      | A random longitude coordinate                       | `40.6609`, `171.7139`, `-159.9757`                          |

### Images

| **Variable Name**           | **Description**                         | **Examples**                                                 |
| --------------------------- | --------------------------------------- | ------------------------------------------------------------ |
| **`$randomImage`**          | A random image                          | `http://lorempixel.com/640/480/technics`                     |
|                             |                                         | `http://lorempixel.com/640/480/food`                         |
|                             |                                         | `http://lorempixel.com/640/480/business`                     |
| **`$randomAvatarImage`**    | A random avatar image                   | `https://s3.amazonaws.com/uifaces/faces/twitter/johnsmithagency/128.jpg` |
|                             |                                         | `https://s3.amazonaws.com/uifaces/faces/twitter/xadhix/128.jpg` |
|                             |                                         | `https://s3.amazonaws.com/uifaces/faces/twitter/martip07/128.jpg` |
| **`$randomImageUrl`**       | A URL for a random image                | `http://lorempixel.com/640/480`                              |
| **`$randomAbstractImage`**  | A URL for a random abstract image       | `http://lorempixel.com/640/480/abstract`                     |
| **`$randomAnimalsImage`**   | A URL for a random animal image         | `http://lorempixel.com/640/480/animals`                      |
| **`$randomBusinessImage`**  | A URL for a random stock business image | `http://lorempixel.com/640/480/business`                     |
| **`$randomCatsImage`**      | A URL for a random cat image            | `http://lorempixel.com/640/480/cats`                         |
| **`$randomCityImage`**      | A URL for a random city image           | `http://lorempixel.com/640/480/city`                         |
| **`$randomFoodImage`**      | A URL for a random food image           | `http://lorempixel.com/640/480/food`                         |
| **`$randomNightlifeImage`** | A URL for a random nightlife image      | `http://lorempixel.com/640/480/nightlife`                    |
| **`$randomFashionImage`**   | A URL for a random fashion image        | `http://lorempixel.com/640/480/fashion`                      |
| **`$randomPeopleImage`**    | A URL for a random image of a person    | `http://lorempixel.com/640/480/people`                       |
| **`$randomNatureImage`**    | A URL for a random nature image         | `http://lorempixel.com/640/480/nature`                       |
| **`$randomSportsImage`**    | A URL for a random sports image         | `http://lorempixel.com/640/480/sports`                       |
| **`$randomTechnicsImage`**  | A URL for a random tech image           | `http://lorempixel.com/640/480/technics`                     |
| **`$randomTransportImage`** | A URL for a random transportation image | `http://lorempixel.com/640/480/transport`                    |
| **`$randomImageDataUri`**   | A random image data URI                 | `data:image/svg+xml;charset=UTF-8,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20version%3D%221.1%22%20baseProfile%3D%22full%22%20width%3D%22undefined%22%20height%3D%22undefined%22%3E%20%3Crect%20width%3D%22100%25%22%20height%3D%22100%25%22%20fill%3D%22grey%22%2F%3E%20%20%3Ctext%20x%3D%220%22%20y%3D%2220%22%20font-size%3D%2220%22%20text-anchor%3D%22start%22%20fill%3D%22white%22%3Eundefinedxundefined%3C%2Ftext%3E%20%3C%2Fsvg%3E` |

### Finance

| **Variable Name**            | **Description**                                              | **Examples**                                                 |
| ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **`$randomBankAccount`**     | A random 8-digit bank account number                         | `09454073`, `65653440`, `75728757`                           |
| **`$randomBankAccountName`** | A random bank account name (e.g. savings account, checking account) | `Home Loan Account`, `Checking Account`, `Auto Loan Account` |
| **`$randomCreditCardMask`**  | A random masked credit card number                           | `3622`, `5815`, `6257`                                       |
| **`$randomBankAccountBic`**  | A random BIC (Bank Identifier Code)                          | `EZIAUGJ1`, `KXCUTVJ1`, `DIVIPLL1`                           |
| **`$randomBankAccountIban`** | A random 15-31 character IBAN (International Bank Account Number) | `MU20ZPUN3039684000618086155TKZ`                             |
|                              |                                                              | `BR7580569810060080800805730W2`                              |
|                              |                                                              | `XK241602002200395017`                                       |
| **`$randomTransactionType`** | A random transaction type (e.g. invoice, payment, deposit)   | `invoice`, `payment`, `deposit`                              |
| **`$randomCurrencyCode`**    | A random 3-letter currency code (ISO-4217)                   | `CDF`, `ZMK`, `GNF`                                          |
| **`$randomCurrencyName`**    | A random currency name                                       | `CFP Franc`, `Cordoba Oro`, `Pound Sterling`                 |
| **`$randomCurrencySymbol`**  | A random currency symbol                                     | `$`, `£`                                                     |
| **`$randomBitcoin`**         | A random bitcoin address                                     | `3VB8JGT7Y4Z63U68KGGKDXMLLH5`                                |
|                              |                                                              | `1GY5TL5NEX3D1EA0TCWPLGVPQF5EAF`                             |
|                              |                                                              | `14IIEXV2AKZAHSCY2KNYP213VRLD`                               |

### Business

| **Variable Name**          | **Description**                                | **Examples**                          |
| -------------------------- | ---------------------------------------------- | ------------------------------------- |
| **`$randomCompanyName`**   | A random company name                          | `Johns - Kassulke`, `Grady LLC`       |
| **`$randomCompanySuffix`** | A random company suffix (e.g. Inc, LLC, Group) | `Inc`, `LLC`, `Group`                 |
| **`$randomBs`**            | A random phrase of business speak              | `killer leverage schemas`,            |
|                            |                                                | `bricks-and-clicks deploy markets`,   |
|                            |                                                | `world-class unleash platforms`       |
| **`$randomBsAdjective`**   | A random business speak adjective              | `viral`, `24/7`, `24/365`             |
| **`$randomBsBuzz`**        | A random business speak buzzword               | `repurpose`, `harness`, `transition`  |
| **`$randomBsNoun`**        | A random business speak noun                   | `e-services`, `markets`, `interfaces` |

### Catchphrases

| **Variable Name**                  | **Description**                       | **Examples**                                        |
| ---------------------------------- | ------------------------------------- | --------------------------------------------------- |
| **`$randomCatchPhrase`**           | A random catchphrase                  | `Future-proofed heuristic open architecture`,       |
|                                    |                                       | `Quality-focused executive toolset`,                |
|                                    |                                       | `Grass-roots real-time definition`                  |
| **`$randomCatchPhraseAdjective`**  | A random catchphrase adjective        | `Self-enabling`, `Business-focused`, `Down-sized`   |
| **`$randomCatchPhraseDescriptor`** | A random catchphrase descriptor       | `bandwidth-monitored`, `needs-based`, `homogeneous` |
| **`$randomCatchPhraseNoun`**       | Randomly generates a catchphrase noun | `secured line`, `superstructure`,`installation`     |

### Databases

| **Variable Name**              | **Description**               | **Examples**                                         |
| ------------------------------ | ----------------------------- | ---------------------------------------------------- |
| **`$randomDatabaseColumn`**    | A random database column name | `updatedAt`, `token`, `group`                        |
| **`$randomDatabaseType`**      | A random database type        | `tinyint`, `text`                                    |
| **`$randomDatabaseCollation`** | A random database collation   | `cp1250_bin`, `utf8_general_ci`, `cp1250_general_ci` |
| **`$randomDatabaseEngine`**    | A random database engine      | `MyISAM`, `InnoDB`, `Memory`                         |

### Dates

| **Variable Name**       | **Description**          | **Examples**                                               |
| ----------------------- | ------------------------ | ---------------------------------------------------------- |
| **`$randomDateFuture`** | A random future datetime | `Tue Mar 17 2020 13:11:50 GMT+0530 (India Standard Time)`, |
|                         |                          | `Fri Sep 20 2019 23:51:18 GMT+0530 (India Standard Time)`, |
|                         |                          | `Thu Nov 07 2019 19:20:06 GMT+0530 (India Standard Time)`  |
| **`$randomDatePast`**   | A random past datetime   | `Sat Mar 02 2019 09:09:26 GMT+0530 (India Standard Time)`, |
|                         |                          | `Sat Feb 02 2019 00:12:17 GMT+0530 (India Standard Time)`, |
|                         |                          | `Thu Jun 13 2019 03:08:43 GMT+0530 (India Standard Time)`  |
| **`$randomDateRecent`** | A random recent datetime | `Tue Jul 09 2019 23:12:37 GMT+0530 (India Standard Time)`, |
|                         |                          | `Wed Jul 10 2019 15:27:11 GMT+0530 (India Standard Time)`, |
|                         |                          | `Wed Jul 10 2019 01:28:31 GMT+0530 (India Standard Time)`  |
| **`$randomWeekday`**    | A random weekday         | `Thursday`, `Friday`, `Monday`                             |
| **`$randomMonth`**      | A random month           | `February`, `May`, `January`                               |

### Domains, Emails and Usernames

| **Variable Name**         | **Description**                                 | **Examples**                                                 |
| ------------------------- | ----------------------------------------------- | ------------------------------------------------------------ |
| **`$randomDomainName`**   | A random domain name                            | `gracie.biz`, `armando.biz`, `trevor.info`                   |
| **`$randomDomainSuffix`** | A random domain suffix                          | `org`, `net`, `com`                                          |
| **`$randomDomainWord`**   | A random unqualified domain name                | `gwen`, `jaden`, `donnell`                                   |
| **`$randomEmail`**        | A random email address                          | `Pablo62@gmail.com`, `Ruthe42@hotmail.com`, `Iva.Kovacek61@hotmail.com` |
| **`$randomExampleEmail`** | A random email address from an “example” domain | `Talon28@example.com`, `Quinten_Kerluke45@example.net`, `Casey81@example.net` |
| **`$randomUserName`**     | A random username                               | `Jarrell.Gutkowski`, `Lottie.Smitham24`, `Alia99`            |
| **`$randomUrl`**          | A random URL                                    | `https://anais.net`, `https://tristin.net`, `http://jakob.name` |

### Files and Directories

| **Variable Name**           | **Description**                                        | **Examples**                                           |
| --------------------------- | ------------------------------------------------------ | ------------------------------------------------------ |
| **`$randomFileName`**       | A random file name (includes uncommon extensions)      | `neural_sri_lanka_rupee_gloves.gdoc`,                  |
|                             |                                                        | `plastic_awesome_garden.tif`,                          |
|                             |                                                        | `incredible_ivory_agent.lzh`                           |
| **`$randomFileType`**       | A random file type (includes uncommon file types)      | `model`, `application`, `video`                        |
| **`$randomFileExt`**        | A random file extension (includes uncommon extensions) | `war`, `book`, `fsc`                                   |
| **`$randomCommonFileName`** | A random file name                                     | `well_modulated.mpg4`,                                 |
|                             |                                                        | `rustic_plastic_tuna.gif`,                             |
|                             |                                                        | `checking_account_end_to_end_robust.wav`               |
| **`$randomCommonFileType`** | A random, common file type                             | `application`, `audio`                                 |
| **`$randomCommonFileExt`**  | A random, common file extension                        | `m2v`, `wav`, `png`                                    |
| **`$randomFilePath`**       | A random file path                                     | `/home/programming_chicken.cpio`,                      |
|                             |                                                        | `/usr/obj/fresh_bandwidth_monitored_beauty.onetoc`,    |
|                             |                                                        | `/dev/css_rustic.pm`                                   |
| **`$randomDirectoryPath`**  | A random directory path                                | `/usr/bin`, `/root`, `/usr/local/bin`                  |
| **`$randomMimeType`**       | A random MIME type                                     | `audio/vnd.vmx.cvsd`,                                  |
|                             |                                                        | `application/vnd.groove-identity-message`,             |
|                             |                                                        | `application/vnd.oasis.opendocument.graphics-template` |

### Stores

| **Variable Name**             | **Description**                          | **Examples**                                   |
| ----------------------------- | ---------------------------------------- | ---------------------------------------------- |
| **`$randomPrice`**            | A random price between 100.00 and 999.00 | `531.55`, `488.76`, `511.56`                   |
| **`$randomProduct`**          | A random product                         | `Towels`, `Pizza`, `Pants`                     |
| **`$randomProductAdjective`** | A random product adjective               | `Unbranded`, `Incredible`, `Tasty`             |
| **`$randomProductMaterial`**  | A random product material                | `Steel`, `Plastic`, `Frozen`                   |
| **`$randomProductName`**      | A random product name                    | `Handmade Concrete Tuna`, `Refined Rubber Hat` |
| **`$randomDepartment`**       | A random commerce category               | `Tools`, `Movies`, `Electronics`               |

### Grammar

| **Variable Name**      | **Description**                | **Examples**                                                 |
| ---------------------- | ------------------------------ | ------------------------------------------------------------ |
| **`$randomNoun`**      | A random noun                  | `matrix`, `bus`, `bandwidth`                                 |
| **`$randomVerb`**      | A random verb                  | `parse`, `quantify`, `navigate`                              |
| **`$randomIngverb`**   | A random verb ending in "-ing" | `synthesizing`, `navigating`, `backing up`                   |
| **`$randomAdjective`** | A random adjective             | `auxiliary`, `multi-byte`, `back-end`                        |
| **`$randomWord`**      | A random word                  | `withdrawal`, `infrastructures`, `IB`                        |
| **`$randomWords`**     | Some random words              | `Samoa Synergistic sticky copying Grocery`,                  |
|                        |                                | `Corporate Springs`,                                         |
|                        |                                | `Christmas Island Ghana Quality`                             |
| **`$randomPhrase`**    | A random phrase                | `You can't program the monitor without navigating the mobile XML program!`, |
|                        |                                | `overriding the capacitor won't do anything, we need to compress the optical SMS transmitter!`, |
|                        |                                | `I'll generate the virtual AI program, that should microchip the RAM monitor!` |

### Lorem Ipsum

| **Variable Name**            | **Description**                            | **Examples**                                                 |
| ---------------------------- | ------------------------------------------ | ------------------------------------------------------------ |
| **`$randomLoremWord`**       | A random word of lorem ipsum text          | `est`                                                        |
| **`$randomLoremWords`**      | Some random words of lorem ipsum text      | `vel repellat nobis`                                         |
| **`$randomLoremSentence`**   | A random sentence of lorem ipsum text      | `Molestias consequuntur nisi non quod.`                      |
| **`$randomLoremSentences`**  | A random 2-6 sentences of lorem ipsum text | `Et sint voluptas similique iure amet perspiciatis vero sequi atque. Ut porro sit et hic. Neque aspernatur vitae fugiat ut dolore et veritatis. Ab iusto ex delectus animi. Voluptates nisi iusto. Impedit quod quae voluptate qui.` |
| **`$randomLoremParagraph`**  | A random paragraph of lorem ipsum text     | `Ab aliquid odio iste quo voluptas voluptatem dignissimos velit. Recusandae facilis qui commodi ea magnam enim nostrum quia quis. Nihil est suscipit assumenda ut voluptatem sed. Esse ab voluptas odit qui molestiae. Rem est nesciunt est quis ipsam expedita consequuntur.` |
| **`$randomLoremParagraphs`** | 3 random paragraphs of lorem ipsum text    | `Voluptatem rem magnam aliquam ab id aut quaerat. Placeat provident possimus voluptatibus dicta velit non aut quasi. Mollitia et aliquam expedita sunt dolores nam consequuntur. Nam dolorum delectus ipsam repudiandae et ipsam ut voluptatum totam. Nobis labore labore recusandae ipsam quo.` |
|                              |                                            | `Voluptatem occaecati omnis debitis eum libero. Veniam et cum unde. Nisi facere repudiandae error aperiam expedita optio quae consequatur qui. Vel ut sit aliquid omnis. Est placeat ducimus. Libero voluptatem eius occaecati ad sint voluptatibus laborum provident iure.` |
|                              |                                            | `Autem est sequi ut tenetur omnis enim. Fuga nisi dolor expedita. Ea dolore ut et a nostrum quae ut reprehenderit iste. Numquam optio magnam omnis architecto non. Est cumque laboriosam quibusdam eos voluptatibus velit omnis. Voluptatem officiis nulla omnis ratione excepturi.` |
| **`$randomLoremText`**       | A random amount of lorem ipsum text        | `Quisquam asperiores exercitationem ut ipsum. Aut eius nesciunt. Et reiciendis aut alias eaque. Nihil amet laboriosam pariatur eligendi. Sunt ullam ut sint natus ducimus. Voluptas harum aspernatur soluta rem nam.` |
| **`$randomLoremSlug`**       | A random lorem ipsum URL slug              | `eos-aperiam-accusamus`, `beatae-id-molestiae`, `qui-est-repellat` |
| **`$randomLoremLines`**      | 1-5 random lines of lorem ipsum            | `Ducimus in ut mollitia.\nA itaque non.\nHarum temporibus nihil voluptas.\nIste in sed et nesciunt in quaerat sed.` |

# Postman JavaScript参考

Postman提供了可在请求脚本中使用的JavaScript API。该`pm`对象提供了用于测试您的请求和响应数据的大多数功能，并且该`postman`对象提供了一些附加的工作流控件。

## 内容

- [pm对象](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#the-pm-object)
  - [在脚本中使用变量](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#using-variables-in-scripts)
    - [环境变量](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#using-environment-variables-in-scripts)
    - [集合变量](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#using-collection-variables-in-scripts)
    - [全局变量](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#using-global-variables-in-scripts)
    - [资料变数](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#using-data-variables-in-scripts)
  - [使用请求和响应数据编写脚本](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-request-and-response-data)
    - [索取资料](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-request-data)
    - [响应数据](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-response-data)
    - [索取资料](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-request-info)
    - [饼干](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-request-cookies)
  - [从脚本发送请求](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#sending-requests-from-scripts)
- [脚本工作流程](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-workflows)
- [脚本可视化](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-visualizations)
- [将响应数据构建到可视化中](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#building-response-data-into-visualizations)
- [编写测试断言](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#writing-test-assertions)
- [使用外部库](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#using-external-libraries)

## pm对象

您将使用来执行大多数Postman JavaScript API功能`pm.*`，该功能提供对请求和响应数据以及变量的访问。

### 在脚本中使用变量

您可以使用API在Postman中的每个范围内访问和操作[变量](https://learning.postman.com/docs/sending-requests/variables/)`pm`。

> 您的请求运行时，可以使用[动态变量](https://learning.postman.com/docs/writing-scripts/script-references/variables-list/)生成值。

Postman支持各种可变[范围](https://learning.postman.com/docs/sending-requests/variables/#variable-scopes)。该`pm`对象专门提供用于访问全局变量，集合变量和环境变量的`pm.variables`方法，以及用于访问不同作用域的变量以及设置局部变量的方法。

- 检查当前作用域中是否存在Postman变量：

```js
pm.variables.has(variableName:String):function → Boolean
```

- 获取具有指定名称的Postman变量的值：

```js
pm.variables.get(variableName:String):function → *
```

- 设置具有指定名称和值的局部变量：

```js
pm.variables.set(variableName:String, variableValue:*):function
```

- 使用以下语法返回脚本内动态变量的解析值`{{$variableName}}`：

```js
pm.variables.replaceIn(variableName:String):function: → *
```

> 例如：

```js
const stringWithVars = pm.variables.replaceIn("Hi, my name is {{$randomFirstName}}");
console.log(stringWithVars);
```

- 返回一个对象，该对象包含所有变量及其在当前作用域中的值。根据优先顺序，它将包含来自多个作用域的变量。

```js
pm.variables.toObject():function → Object
```

变量作用域确定了Postman在引用变量时赋予变量的优先级，按照优先级递增的顺序：

- 全球
- 环境
- 采集
- 数据
- 本地

作用域最接近的变量将覆盖所有其他变量。例如，如果您`score`在当前集合和活动环境中都同时命名了变量，并调用`pm.variables.get('score')`，则Postman将返回环境变量的当前值。使用设置变量值时`pm.variables.set`，该值是本地值，仅在当前请求或收集运行时持久存在。

```js
//collection var 'score' = 1
//environment var 'score' = 2

//first request run
console.log(pm.variables.get('score'));//outputs 2
console.log(pm.collectionVariables.get('score'));//outputs 1
console.log(pm.environment.get('score'));//outputs 2

//second request run
pm.variables.set('score', 3);//local var
console.log(pm.variables.get('score'));//outputs 3

//third request run
console.log(pm.variables.get('score'));//outputs 2
```

> 有关更多详细信息，请参见[Postman Collection SDK变量参考](https://www.postmanlabs.com/postman-collection/Variable.html)。

您还可以通过[pm.environment](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-environment-variables)，[pm.collectionVariables](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-collection-variables)和[pm.globals](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-global-variables)访问在各个范围中定义的变量。

#### 在脚本中使用环境变量

您的脚本可以使用这些`pm.environment`方法在活动的（当前选择的）环境中访问和操作变量。

- 活动环境的名称：

```js
pm.environment.name:String
```

- 检查环境是否具有指定名称的变量：

```js
pm.environment.has(variableName:String):function → Boolean
```

- 在活动环境中获取具有指定名称的变量：

```js
pm.environment.get(variableName:String):function → *
```

- 在活动环境中使用指定的名称和值设置变量：

```js
pm.environment.set(variableName:String, variableValue:*):function
```

- 使用以下语法返回脚本内动态变量的解析值`{{$variableName}}`：

```js
pm.environment.replaceIn(variableName:String):function → *
```

> 例如：

```js
//environment has vars firstName and age
const stringWithVars = pm.environment.replaceIn("Hi, my name is {{firstName}} and I am {{age}}.");
console.log(stringWithVars);
```

- 在单个对象的活动环境中返回所有变量及其值：

```js
pm.environment.toObject():function → Object
```

- 从活动环境中删除变量，并通过名称指定变量：

```js
pm.environment.unset(variableName:String):function
```

- 清除活动环境中的所有变量：

```js
pm.environment.clear():function
```

> 请注意，您编辑变量的能力取决于您在工作空间中的[访问级别](https://learning.postman.com/docs/sending-requests/managing-environments/#working-with-environments-as-a-team)。

#### 在脚本中使用集合变量

您的脚本可以使用这些`pm.collectionVariables`方法来访问和操作集合中的变量。

- 检查集合中是否存在具有指定名称的变量：

```js
pm.collectionVariables.has(variableName:String):function → Boolean
```

- 返回具有指定名称的collection变量的值：

```js
pm.collectionVariables.get(variableName:String):function → *
```

- 设置具有指定名称和值的集合变量：

```js
pm.collectionVariables.set(variableName:String, variableValue:*):function
```

- 使用以下语法返回脚本内动态变量的解析值`{{$variableName}}`：

```js
pm.collectionVariables.replaceIn(variableName:String):function → *
```

> 例如：

```js
//collection has vars firstName and age
const stringWithVars = pm.collectionVariables.replaceIn("Hi, my name is {{firstName}} and I am {{age}}.");
console.log(stringWithVars);
```

- 在对象的集合中返回所有变量及其值：

```js
pm.collectionVariables.toObject():function → Object
```

- 从集合中删除指定的变量：

```js
pm.collectionVariables.unset(variableName:String):function
```

- 清除集合中的所有变量：

```js
pm.collectionVariables.clear():function
```

#### 在脚本中使用全局变量

您的脚本可以使用这些`pm.globals`方法在工作空间中的全局范围内访问和操作变量。

- 检查哪里有一个具有指定名称的全局变量：

```js
pm.globals.has(variableName:String):function → Boolean
```

- 返回具有指定名称的全局变量的值：

```js
pm.globals.get(variableName:String):function → *
```

- 设置具有指定名称和值的全局变量：

```js
pm.globals.set(variableName:String, variableValue:*):function
```

- 使用以下语法返回脚本内动态变量的解析值`{{$variableName}}`：

```js
pm.globals.replaceIn(variableName:String):function → String
```

> 例如：

```js
//globals include vars firstName and age
const stringWithVars = pm.globals.replaceIn("Hi, my name is {{firstName}} and I am {{age}}.");
console.log(stringWithVars);
```

- 返回一个对象中的所有全局变量及其值：

```js
pm.globals.toObject():function → Object
```

- 删除指定的全局变量：

```js
pm.globals.unset(variableName:String):function
```

- 清除工作空间中的所有全局变量：

```js
pm.globals.clear():function
```

> 请注意，您编辑变量的能力取决于您在工作空间中的[访问级别](https://learning.postman.com/docs/sending-requests/managing-environments/#working-with-environments-as-a-team)。

#### 在脚本中使用数据变量

您的脚本可以使用这些`pm.iterationData`方法[在收集运行期间](https://learning.postman.com/docs/running-collections/working-with-data-files/)访问和操作[数据文件中的](https://learning.postman.com/docs/running-collections/working-with-data-files/)变量。

- 检查当前迭代数据中是否存在具有指定名称的变量：

```js
pm.iterationData.has(variableName:String):function → boolean
```

- 从迭代数据中以指定名称返回变量：

```js
pm.iterationData.get(variableName:String):function → *
```

- 返回对象中的迭代数据变量：

```js
pm.iterationData.toObject():function → Object
```

- 将erationData对象转换为JSON格式：

```js
pm.iterationData.toJSON():function → *
```

- 删除指定的变量：

```js
pm.iterationData.unset(key:String):function
```

### 使用请求和响应数据编写脚本

有多种方法可以访问Postman脚本中的请求和响应数据，包括[pm.request](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-request-data)，[pm.response](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-response-data)，[pm.info](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-request-info)和[pm.cookies](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#scripting-with-request-cookies)。另外，您可以使用[pm.sendRequest](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#sending-requests-from-scripts)发送请求。

#### 使用请求数据编写脚本

该`pm.request`对象提供对脚本在其中运行的请求的数据的访问。对于**预请求脚本，**这是即将运行的请求，对于**测试**脚本，这是已运行的请求。

您可以使用`pm.request`对象预请求脚本在请求配置运行之前对其进行更改。

该`pm.request`对象提供以下属性和方法：

- 请求网址：

```js
pm.request.url:Url
```

- 该[标题的列表](https://www.postmanlabs.com/postman-collection/HeaderList.html)当前请求：

```js
pm.request.headers:HeaderList
```

- HTTP请求方法：

```js
pm.request.method:String
```

- [请求正文中](https://www.postmanlabs.com/postman-collection/RequestBody.html)的数据。该对象是不可变的，不能通过脚本进行修改：

```js
pm.request.body:RequestBody
```

- 为当前请求添加具有指定名称和值的标头：

```js
pm.request.headers.add(header:Header):function
```

例如：

```js
pm.request.headers.add({
  key: "client-id",
  value: "abcdef"
});
```

- 删除具有指定名称的请求标头：

```js
pm.request.headers.remove(headerName:String):function
```

- 插入指定的标头名称和值（如果标头不存在，否则已经存在的标头将更新为新值）：

```js
pm.request.headers.upsert({key: headerName:String, value: headerValue:String}):function)
```

> 有关更多详细信息，请参见Postman [Collection SDK请求参考](https://www.postmanlabs.com/postman-collection/Request.html)。

#### 用响应数据编写脚本

该`pm.response`对象提供对添加到**Tests**中的脚本的当前请求的响应中返回的数据的访问。

该`pm.response`对象提供以下属性和方法：

- 响应状态码：

```js
pm.response.code:Number
```

- 状态文本字符串：

```js
pm.response.status:String
```

- 该[响应头的列表](https://www.postmanlabs.com/postman-collection/HeaderList.html)：

```js
pm.response.headers:HeaderList
```

- 响应收到的时间（以毫秒为单位）：

```js
pm.response.responseTime:Number
```

- 收到的回复大小：

```js
pm.response.responseSize:Number
```

- 响应文本：

```js
pm.response.text():Function → String
```

- 响应JSON，可用于深入了解收到的属性：

```js
pm.response.json():Function → Object
```

> 有关更多详细信息，请参见Postman [Collection SDK响应参考](https://www.postmanlabs.com/postman-collection/Response.html)。

#### 使用请求信息编写脚本

该`pm.info`对象提供与请求和脚本本身相关的数据，包括名称，ID和迭代计数。

该`pm.info`对象提供以下属性和方法：

- 该事件将是“预请求”或“测试”，具体取决于脚本在请求中的执行位置：

```js
pm.info.eventName:String
```

- 当前[迭代](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)的值：

```js
pm.info.iteration:Number
```

- 计划运行的迭代总数：

```js
pm.info.iterationCount:Number
```

- 运行的请求的已保存名称：

```js
pm.info.requestName:String
```

- 标识正在运行的请求的唯一GUID：

```js
pm.info.requestId:String
```

#### 使用请求Cookie编写脚本

该`pm.cookies`对象提供对与请求关联的cookie列表的访问。

该`pm.cookies`对象提供以下属性和方法：

- 检查请求的域是否存在特定的cookie（按名称指定）：

```js
pm.cookies.has(cookieName:String):Function → Boolean
```

- 获取指定cookie的值：

```js
pm.cookies.get(cookieName:String):Function → String
```

- 获取对象中所有cookie及其值的副本。返回为请求域和路径定义的所有cookie：

```js
pm.cookies.toObject():Function → Object
```

> 有关更多详细信息，请参见Postman [Collection SDK Cookie列表参考](https://www.postmanlabs.com/postman-collection/CookieList.html)。

您还可以`pm.cookies.jar`用于指定访问请求Cookie的域。

要通过这些`pm.cookies.jar`方法启用编程访问，请先将Cookie URL[列入白名单](https://learning.postman.com/docs/sending-requests/cookies/)。

- 访问Cookie罐对象：

```js
pm.cookies.jar():Function → Object
```

例如：

```js
const jar = pm.cookies.jar();
//cookie methods...
```

- 使用名称和值设置Cookie：

```js
jar.set(URL:String, cookie name:String, cookie value:String, callback(error, cookie)):Function → Object
```

- 使用[PostmanCookie](https://www.postmanlabs.com/postman-collection/Cookie.html)或兼容对象设置cookie ：

```js
jar.set(URL:String, { name:String, value:String, httpOnly:Bool }, callback(error, cookie)):Function → Object
```

例如：

```js
const jar = pm.cookies.jar();
jar.set("httpbin.org", "session-id", "abc123", (error, cookie) => {
  if (error) {
    console.error(`An error occurred: ${error}`);
  } else {
    console.log(`Cookie saved: ${cookie}`);
  }
});
```

- 从饼干罐中获取饼干：

```js
jar.get(URL:String, cookieName:String, callback (error, value)):Function → Object
```

- 从饼干罐中获取所有饼干。cookie在回调函数中可用：

```js
jar.getAll(URL:String, callback (error, cookies)):Function
```

- 删除Cookie：

```js
jar.unset(URL:String, token:String, callback(error)):Function → Object
```

- 清除cookie罐中的所有cookie：

```js
jar.clear(URL:String, callback (error)):Function → Object
```

> 有关更多详细信息，请参见Postman [Collection SDK Cookie参考](https://www.postmanlabs.com/postman-collection/Cookie.html)。

### 从脚本发送请求

您可以使用该`pm.sendRequest`方法从“**预请求”**或“**测试”**脚本异步发送请求。如果您正在执行计算或同时发送多个请求，而不必等待每个请求完成，则可以在后台执行逻辑。您可以通过添加回调函数来避免阻塞问题，以便您的代码可以在Postman收到响应时做出响应。然后，您可以对响应数据执行所需的任何其他处理。

您可以向该`pm.sendRequest`方法传递URL字符串，也可以使用JSON提供完整的请求配置，包括标头，方法，主体[等](http://www.postmanlabs.com/postman-collection/Request.html#~definition)。

```js
// Example with a plain string URL
pm.sendRequest('https://postman-echo.com/get', (error, response) => {
  if (error) {
    console.log(error);
  } else {
  console.log(response);
  }
});

// Example with a full-fledged request
const postRequest = {
  url: 'https://postman-echo.com/post',
  method: 'POST',
  header: {
    'Content-Type': 'application/json',
    'X-Foo': 'bar'
  },
  body: {
    mode: 'raw',
    raw: JSON.stringify({ key: 'this is json' })
  }
};
pm.sendRequest(postRequest, (error, response) => {
  console.log(error ? error : response.json());
});

// Example containing a test
pm.sendRequest('https://postman-echo.com/get', (error, response) => {
  if (error) {
    console.log(error);
  }

  pm.test('response should be okay to process', () => {
    pm.expect(error).to.equal(null);
    pm.expect(response).to.have.property('code', 200);
    pm.expect(response).to.have.property('status', 'OK');
  });
});
```

有关更多详细信息，请参见[请求定义](http://www.postmanlabs.com/postman-collection/Request.html#~definition)和[响应结构](http://www.postmanlabs.com/postman-collection/Response.html)参考文档。

## 脚本工作流程

该`postman`对象提供了`setNextRequest`使用[收集运行器](https://learning.postman.com/docs/running-collections/building-workflows/)或[Newman](https://learning.postman.com/docs/running-collections/using-newman-cli/command-line-integration-with-newman/)时构建请求工作流的方法。

> 请注意，`setNextRequest`使用“**发送”**按钮运行请求时无效，仅当您运行集合时才生效。

运行集合（使用集合运行器或Newman）时，Postman将以默认顺序或设置运行时指定的顺序运行请求。但是，可以使用`postman.setNextRequest`指定执行下一个请求的方法来覆盖此执行顺序。

- 在此请求之后运行指定的请求（请求名称在集合中定义，例如“获取客户”）：

```js
postman.setNextRequest(requestName:String):Function
```

- 在此请求之后运行指定的请求（请求ID由返回`pm.info.requestId`）：

```js
postman.setNextRequest(requestId:String):Function
```

例如：

```js
//script in another request calls:
//pm.environment.set('next', pm.info.requestId)
postman.setNextRequest(pm.environment.get('next'));
```

## 脚本可视化

使用`pm.visualizer.set`指定模板[的可视化显示响应数据](https://learning.postman.com/docs/sending-requests/visualizer/)。

```js
pm.visualizer.set(layout:String, data:Object, options:Object):Function
```

- `layout` **需要**
  - [车把](https://handlebarsjs.com/)HTML模板字符串
- `data` *可选的*
  - 绑定到模板的数据，您可以在模板字符串内部访问
- `options` *可选的*
  - [选择对象](https://handlebarsjs.com/api-reference/compilation.html)为`Handlebars.compile()`

用法示例：

```js
var template = `<p>{{res.info}}</p>`;
pm.visualizer.set(template, {
    res: pm.response.json()
});
```

### 将响应数据构建到可视化中

用于`pm.getData`在可视化模板字符串中检索响应数据。

```js
pm.getData(callback):Function
```

回调函数接受两个参数：

- `error`
  - 任何错误详细信息
- `data`
  - 数据由[传递给模板](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/#pmvisualizerset)`pm.visualizer.set`

用法示例：

```js
pm.getData(function (error, data) {
  var value = data.res.info;
});
```

## 编写测试断言

- `pm.test(testName:String, specFunction:Function):Function`

您可以使用**Pre-request**或**Tests**脚本`pm.test`编写测试规范。测试包括名称和断言-Postman将输出测试结果作为响应的一部分。

该`pm.test`方法返回`pm`对象，使调用可链接。下面的示例测试检查响应是否有效才能继续。

```js
pm.test("response should be okay to process", function () {
  pm.response.to.not.be.error;
  pm.response.to.have.jsonBody('');
  pm.response.to.not.have.jsonBody('error');
});
```

`done`可以将可选的回调传递给`pm.test`，以测试异步功能。

```js
pm.test('async test', function (done) {
  setTimeout(() => {
    pm.expect(pm.response.code).to.equal(200);
    done();
  }, 1500);
});
```

- 在代码中获取从特定位置执行的测试总数：

```js
pm.test.index():Function → Number
```

该`pm.expect`方法允许您使用[ChaiJS Expect BDD](http://chaijs.com/api/bdd/)语法在响应数据上写断言。

```js
pm.expect(assertion:*):Function → Assertion
```

您还可以使用`pm.response.to.have.*`和`pm.response.to.be.*`建立您的断言。

有关更多声明，请参见[测试示例](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/)。

## 使用外部库

```js
require(moduleName:String):function → *
```

该`require`方法允许您使用沙盒内置库模块。可用库列表在下面列出，并带有相应文档的链接。

- [联合会](https://www.npmjs.com/package/ajv)
- [阿托布](https://www.npmjs.com/package/atob)
- [博托](https://www.npmjs.com/package/btoa)
- [柴](http://chaijs.com/)
- [欢乐](https://cheerio.js.org/)
- [加密js](https://www.npmjs.com/package/crypto-js)
- [csv-parse / lib / sync](http://csv.adaltas.com/parse)
- [Lodash](https://lodash.com/)
- [时刻](http://momentjs.com/docs/)
- [Postman收藏](http://www.postmanlabs.com/postman-collection/)
- [电视4](https://github.com/geraintluff/tv4)
- [uid](https://www.npmjs.com/package/uuid)
- [xml2js](https://www.npmjs.com/package/xml2js)

沙箱中还可以使用许多NodeJS模块：

- [路径](https://nodejs.org/api/path.html)
- [断言](https://nodejs.org/api/assert.html)
- [缓冲](https://nodejs.org/api/buffer.html)
- [实用程序](https://nodejs.org/api/util.html)
- [网址](https://nodejs.org/api/url.html)
- [punycode](https://nodejs.org/api/punycode.html)
- [请求参数](https://nodejs.org/api/querystring.html)
- [字符串解码器](https://nodejs.org/api/string_decoder.html)
- [流](https://nodejs.org/api/stream.html)
- [计时器](https://nodejs.org/api/timers.html)
- [大事记](https://nodejs.org/api/events.html)

为了使用库，请调用`require`方法，将模块名称作为参数传递，然后将方法中的返回对象分配给变量。

# UI测试

- 发送请求获取Html响应
- 解析Html标签，判断一些元素是否存在

使用[Cheerio库](https://cheerio.js.org) 在SandBox中用来代替Jquery。无法处理ajax请求，交互式能力一般。

```javascript
var cheerio = require('cheerio'),
    $ = cheerio.load('<h2 class = "title">hello world</h2>');
$('h2.title').text('hello there!');
$('h2').addclass('welcome');
$.html();
```

```javascript
var cheerio = require('cheerio');
$ = cheerio.load(responseBody);
pm.test("必须包含4门课程"， function(){
    pm.response.to.be.success;
    pm.expect($('.servive-block-in').length === 4);
});
```

