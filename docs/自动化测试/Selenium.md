# Selenium Java

## Selenium IDE

Selenium IDE是用于开发Selenium测试用例的工具。这是一个易于使用的浏览器扩展程序，通常是开发测试用例的最有效方法。它使用现有的Selenium命令以及由该元素的上下文定义的参数，在浏览器中记录用户的操作。这不仅是节省时间的方法，还是学习Selenium脚本语法的绝佳方法。

### 如何记录悬停？

鼠标悬停（aka悬停）操作很难作为记录周期的一部分自动捕获。

要在您的测试中添加鼠标悬停，需要进行一些手动干预。您可以通过两种不同的方式来做到这一点。

*选项1：在录制时添加*

1. 录制时，右键单击要悬停的元素
2. 在出现的菜单中，单击`Selenium IDE`，然后`Mouse Over`
3. 确认`Mouse Over`测试步骤在测试中的正确位置（如果需要，将其拖放到其他位置）

*选项2：在测试编辑器中手动添加*

1. 右键单击IDE中的测试步骤
2. 选择 `Insert new command`
3. 输入`mouse over`到`Command`输入字段
4. 在`Target`输入字段中输入要悬停的定位器（或单击`Select target in page`并选择要悬停的元素）

### 为什么在日期输入字段中键入的数字不能正确显示？

通过Selenium IDE的命令行运行器运行测试时，会出现此问题。

要绕开它，您将需要启用w3c模式，您可以通过`-c "chromeOptions.w3c=true"`在启动运行程序时传递来进行此操作。

启用w3c模式会影响Selenium Actions的性能（如果您的测试最终使用它们）是毫无价值的，因此仅当日期输入字段存在问题时才使用此模式。

### 如何让IDE等待特定条件成立才能继续进行？

在某些情况下，IDE中的内置等待策略还不够。发生这种情况时，可以使用可用的显式等待命令之一。

- `wait for element editable`
- `wait for element present`
- `wait for element visible`
- `wait for element not editable`
- `wait for element not present`
- `wait for element not visible`

### 如何在文本验证中使用正则表达式？

这是我们最终将要添加的功能（[有关详细信息，](https://github.com/SeleniumHQ/selenium-ide/issues/141)请参阅[问题141](https://github.com/SeleniumHQ/selenium-ide/issues/141)）。解决方法是，可以将XPath定位器与`starts-with`和`contains`关键字一起使用。

| 命令                 | 目标                                                         | 值   |
| -------------------- | ------------------------------------------------------------ | ---- |
| assertElementPresent | //a@[starts-with(.,'you are the'）and contains（。，'今天要登录的用户'）] |      |

### 如何滚动？

Selenium IDE中没有用于滚动的独特命令，因为Selenium中没有实现任何命令。相反，您可以使用`scrollTo`JavaScript中的命令通过指定`x`和`y`滚动到所需坐标来完成此任务。

| 命令          | 目标                      | 值   |
| ------------- | ------------------------- | ---- |
| executeScript | window.scrollTo（0,1000） |      |

### 保存文件

#### 为什么我保存SIDE项目的位置不记得了？

#### 为什么每次要保存项目时都需要逐步执行“另存为”流程？

#### 为什么需要覆盖以前保存的文件？

所有这些问题都是同一个问题的一部分-因为浏览器扩展Selenium IDE不能访问文件系统。提供“保存”功能的唯一方法是通过下载文件。当IDE移至本机应用程序时，将解决此问题。这将使IDE具有首要的文件系统访问权限，从而使它能够提供优美的“保存”体验。

如果要保持更新，可以按照[问题363进行操作](https://github.com/SeleniumHQ/selenium-ide/issues/363)。

### 如何在严格的代理/防火墙后面安装IDE？

在某些情况下，您可能没有完全的公共Internet访问权限（例如，在“公司代理或防火墙”后面）。在这些环境中，您将需要获取内置的Selenium IDE ZIP文件的副本，以便记录自动测试脚本。可以在GitHub的“发布”部分中找到：

https://github.com/SeleniumHQ/selenium-ide/releases

并非所有版本都包含“ selenium-ide.zip”，因为其中一些仅仅是“源代码”版本。查找具有此zip文件的最新版本。这意味着它是提交给Chrome和Firefox商店的最新版本。

#### 正式签署的版本

从项目发行页面下载zip文件可为您提供未签名的ZIP文件。或者，您可以从以下位置获取经过正式签名的安装程序，这些安装程序可以在“安全环境”中更好地发挥作用：

- [Firefox附加组件](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide/)
- [所需的“ .xpi”安装程序的下载说明](https://superuser.com/questions/646856/how-to-save-firefox-addons-for-offline-installation)

**注意：如果您已经安装了插件（例如，在便携式计算机上尝试获取安装程序的副本），则在尝试访问它们时只会看到“删除”按钮。因此，将它们删除一次，让安装程序移至另一台未连接的计算机，然后根据需要在主设备的浏览器中重新安装。**

- [Chrome商店](https://chrome.google.com/webstore/detail/selenium-ide/mooikfkahbdckldjjndioackbalphokd)
- [所需的“ .crx”安装程序的下载说明](https://stackoverflow.com/questions/25480912/how-to-download-a-chrome-extension-without-installing-it)

**注意：您不能直接从Chrome商店中获取“ .crx”文件。相反，您需要在本地安装一次，然后转到计算机上的安装目录以进行检索。**

### 附加了插件后，为什么没有保存对话框出现？

由于当前的[Chrome错误](https://bugs.chromium.org/p/chromium/issues/detail?id=922373)，如果您不答复Selenium IDE发出的消息，则不会进行进一步处理。为了解决此问题，请确保侦听`emit`该实体的操作`project`并使用进行回复`undefined`：

```javascript
chrome.runtime.onMessageExternal.addListener((message, sender, sendResponse) => {
  if (message.action === "emit" && message.entity === "project") {
    sendResponse(undefined);
  }
});
```

### API参考

https://www.selenium.dev/selenium-ide/docs/en/api/commands

## Selenium Client & WebDriver

使用WebDriver API自动化测试桌面网站或移动网站。使用浏览器供应商提供的浏览器自动化API来控制浏览器并运行测试。模拟真实用户正在操作浏览器一样。由于WebDriver不需要使用应用程序代码编译其API，因此它不是侵入式的。

### 安装WebDriver：

https://sites.google.com/a/chromium.org/chromedriver/

添加环境变量

```
# windows
setx /m path "%path%;C:\WebDriver\bin\"
# macos & linux
export PATH=$PATH:/opt/WebDriver/bin >> ~/.profile
```

```
# 设置路径
System.setProperty("webdriver.chrome.driver", "/path/to/chromedriver");
```

### 安装Selenium库:

Maven依赖pom.xml：

```xml
<dependency>
  <groupId>org.seleniumhq.selenium</groupId>
  <artifactId>selenium-java</artifactId>
  <version>3.X</version>
</dependency>

<dependency>
  <groupId>org.seleniumhq.selenium</groupId>
  <artifactId>selenium-chrome-driver</artifactId>
  <version>3.X</version>
</dependency>

<dependency>
  <groupId>org.seleniumhq.selenium</groupId>
  <artifactId>selenium-server</artifactId>
  <version>3.X</version>
</dependency>
```

实例化：

```
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
WebDriver driver = new ChromeDriver();
```

### 定位一个元素

#### id定位：

```java
WebElement cheese = driver.findElement(By.id("cheese")); 
// 定位元素的ID和子ID
WebElement cheese = driver.findElement(By.id("cheese"));
WebElement cheddar = cheese.findElement(By.id("cheddar"));
```

#### CSS定位：

```
driver.findElement(By.cssSelector("#cheese #cheddar"));
```

### 定位多个元素

```
List<WebElement> muchoCheese = driver.findElements(By.cssSelector("#cheese li"));
```

### 选择策略

| 定位器            | 描述                                                         |                        |
| :---------------- | :----------------------------------------------------------- | ---------------------- |
| class name        | 查找其类名包含搜索值的元素（不允许使用复合类名）             | 唯一                   |
| CSS选择器         | 找到与CSS选择器匹配的元素                                    |                        |
| ID                | 查找其ID属性与搜索值匹配的元素                               | 唯一                   |
| name              | 找到其NAME属性与搜索值匹配的元素                             |                        |
| link text         | 查找可见文本与搜索值匹配的锚元素                             | 仅适用链接元素，运行慢 |
| partial link text | 查找可见文本包含搜索值的锚元素。如果多个元素匹配，则只会选择第一个。 |                        |
| tag name          | 查找标签名称与搜索值匹配的元素                               | 标签不唯一             |
| xpath             | 找到与XPath表达式匹配的元素                                  | 语法复杂难调试，运行慢 |

### 相对定位

#### above

返回一个指定元素上方的元素

```java
//import static org.openqa.selenium.support.locators.RelativeLocator.withTagName;
WebElement passwordField = driver.findElement(By.id("password"));
WebElement emailAddressField = driver.findElement(withTagName("input").above(passwordField));
```

#### below

返回一个指定元素下方的元素

```
//import static org.openqa.selenium.support.locators.RelativeLocator.withTagName;
WebElement emailAddressField= driver.findElement(By.id("email"));
WebElement passwordField = driver.findElement(withTagName("input").below(emailAddressField));
```

#### toLeftOf

返回一个元素左侧的元素

```
//import static org.openqa.selenium.support.locators.RelativeLocator.withTagName;
WebElement submitButton= driver.findElement(By.id("submit"));
WebElement cancelButton= driver.findElement(withTagName("button").toLeftOf(submitButton));
```

#### toRightOf

返回一个元素右侧的元素

```
//import static org.openqa.selenium.support.locators.RelativeLocator.withTagName;
WebElement cancelButton= driver.findElement(By.id("cancel"));
WebElement submitButton= driver.findElement(withTagName("button").toRightOf(cancelButton));
```

#### near

返回一个指定元素距离50px像素的元素

```
//import static org.openqa.selenium.support.locators.RelativeLocator.withTagName;
WebElement emailAddressLabel= driver.findElement(By.id("lbl-email"));
WebElement emailAddressField = driver.findElement(withTagName("input").near(emailAddressLabel));
```

### 在被测应用上执行操作

#### 设置元素文本

```
String name = "Charles";
driver.findElement(By.name("name")).sendKeys(name);
```

#### 拖拽

```
WebElement source = driver.findElement(By.id("source"));
WebElement target = driver.findElement(By.id("target"));
new Actions(driver).dragAndDrop(source,target).build().perform();
```

#### 点击

```
driver.findElement(By.cssSelector("input[type='submit']")).click();
```

### 浏览器操作

#### 打开网站

```
driver.get("https://lsaiah.cn");
driver.navigate().to("https://lsaiah.cn");
// 获取当前URL
driver.getCurrentUrl();
// 后退
driver.navigate().back();
// 向前
driver.navigate().forward();
// 刷新
driver.navigate().refresh();
// 获取标题
driver.getTitle();
```

#### 窗口和标签

```
// 获取窗口handle
driver.getWindowHandle();
// 切换窗口或标签
//Store the ID of the original window
String originalWindow = driver.getWindowHandle();

//Check we don't have other windows open already
assert driver.getWindowHandles().size() == 1;

//Click the link which opens in a new window
driver.findElement(By.linkText("new window")).click();

//Wait for the new window or tab
wait.until(numberOfWindowsToBe(2));

//Loop through until we find a new window handle
for (String windowHandle : driver.getWindowHandles()) {
    if(!originalWindow.contentEquals(windowHandle)) {
        driver.switchTo().window(windowHandle);
        break;
    }
}
//Wait for the new tab to finish loading content
wait.until(titleIs("Selenium documentation"));
// 创建窗口或标签
// Opens a new tab and switches to new tab
driver.switchTo().newWindow(WindowType.TAB);
// Opens a new window and switches to new window
driver.switchTo().newWindow(WindowType.WINDOW);
// 关闭窗口或标签
//Close the tab or window
driver.close();
//Switch back to the old tab or window
driver.switchTo().window(originalWindow);
// 在会话结束时推出浏览器
@AfterAll
public static void tearDown() {
    driver.quit();
}
```

#### Frames和Iframes

Webdriver无法获取iframe中的元素，需要先切换到框架

- 使用WebElement
  
  ```
  //Store the web element
  WebElement iframe = driver.findElement(By.cssSelector("#modal>iframe"));
  //Switch to the frame
  driver.switchTo().frame(iframe);
  //Now we can click the button
  driver.findElement(By.tagName("button")).click();
  ```
- 使用名称或ID
  
  ```
  //Using the ID
  driver.switchTo().frame("buttonframe");
  //Or using the name instead
  driver.switchTo().frame("myframe");
  //Now we can click the button
  driver.findElement(By.tagName("button")).click();
  ```
- 使用索引
  
  ```
  // Switches to the second frame  
  driver.switchTo().frame(1);
  ```

```
// 离开框架
// Return to the top level
driver.switchTo().defaultContent();
```

#### 窗口管理

```
// 获取窗口大小（以像素为单位）
//Access each dimension individually
int width = driver.manage().window().getSize().getWidth();
int height = driver.manage().window().getSize().getHeight();
//Or store the dimensions and query them later
Dimension size = driver.manage().window().getSize();
int width1 = size.getWidth();
int height1 = size.getHeight();
// 设置窗口大小
driver.manage().window().setSize(new Dimension(1024, 768));
// 获取窗口位置
// Access each dimension individually
int x = driver.manage().window().getPosition().getX();
int y = driver.manage().window().getPosition().getY();
// Or store the dimensions and query them later
Point position = driver.manage().window().getPosition();
int x1 = position.getX();
int y1 = position.getY();
// 设定窗口位置
// Move the window to the top left of the primary monitor
driver.manage().window().setPosition(new Point(0, 0));
// 最大化窗口
driver.manage().window().maximize();
// 最小化窗口
driver.manage().window().minimize();
// 全屏窗口
driver.manage().window().fullscreen();
```

### 等待

#### 明确等待

```
WebDriver driver = new ChromeDriver();
driver.get("https://google.com/ncr");
driver.findElement(By.name("q")).sendKeys("cheese" + Keys.ENTER);
// Initialize and wait till element(link) became clickable - timeout in 10 seconds
WebElement firstResult = new WebDriverWait(driver, Duration.ofSeconds(10)).until(ExpectedConditions.elementToBeClickable(By.xpath("//a/h3")));
// Print the first result
System.out.println(firstResult.getText());
```

```
WebElement foo = new WebDriverWait(driver,Duration.ofSeconds(3)).until(driver -> driver.findElement(By.name("q")));
assertEquals(foo.getText(), "Hello from JavaScript!");
```

等待条件：

```
new WebDriverWait(driver, Duration.ofSeconds(3)).until(ExpectedConditions.elementToBeClickable(By.xpath("//a/h3")));
```

#### 隐式等待

```
WebDriver driver = new FirefoxDriver();
driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
driver.get("http://somedomain/url_that_delays_loading");
WebElement myDynamicElement = driver.findElement(By.id("myDynamicElement"));
```

#### 流利等待

FluentWait实例定义了等待条件的最长时间，以及检查条件的频率。

```
// Waiting 30 seconds for an element to be present on the page, checking
// for its presence once every 5 seconds.
Wait<WebDriver> wait = new FluentWait<WebDriver>(driver)
  .withTimeout(Duration.ofSeconds(30))
  .pollingEvery(Duration.ofSeconds(5))
  .ignoring(NoSuchElementException.class);

WebElement foo = wait.until(new Function<WebDriver, WebElement>() {
  public WebElement apply(WebDriver driver) {
    return driver.findElement(By.id("foo"));
  }
});
```

### 支持类

#### ThreadGuard

线程冲突：

```
public class DriverClash {
  //thread main (id 1) created this driver
  private WebDriver protectedDriver = ThreadGuard.protect(new ChromeDriver()); 

  static {
    System.setProperty("webdriver.chrome.driver", "<Set path to your Chromedriver>");
  }
  
  //Thread-1 (id 24) is calling the same driver causing the clash to happen
  Runnable r1 = () -> {protectedDriver.get("https://selenium.dev");};
  Thread thr1 = new Thread(r1);
   
  void runThreads(){
    thr1.start();
  }

  public static void main(String[] args) {
    new DriverClash().runThreads();
  }
}
```

- protectedDriver 将在主线程中创建
- 使用Java Runnable启动一个新的进程和一个新的线程来运行WebDriver
- 全部线程将会冲突，因为主线程在内存中没有protectedDriver
- ThreadGuard.protect将会抛出异常

### JavaScript弹框

#### Alerts

确定关闭

```
//Click the link to activate the alert
driver.findElement(By.linkText("See an example alert")).click();
//Wait for the alert to be displayed and store it in a variable
Alert alert = wait.until(ExpectedConditions.alertIsPresent());
//Store the alert text in a variable
String text = alert.getText();
//Press the OK button
alert.accept();
```

#### Confirm

可取消

```
//Click the link to activate the alert
driver.findElement(By.linkText("See a sample confirm")).click();
//Wait for the alert to be displayed
wait.until(ExpectedConditions.alertIsPresent());
//Store the alert in a variable
Alert alert = driver.switchTo().alert();
//Store the alert in a variable for reuse
String text = alert.getText();
//Press the Cancel button
alert.dismiss();
```

#### Prompt

包括文本输入

```
//Click the link to activate the alert
driver.findElement(By.linkText("See a sample prompt")).click();
//Wait for the alert to be displayed and store it in a variable
Alert alert = wait.until(ExpectedConditions.alertIsPresent());
//Type your message
alert.sendKeys("Selenium");
//Press the OK button
alert.accept();
```

Selenium Grid允许在不同平台上的不同机器上运行测试用例。触发测试用例的控制在本地，当触发测试用例时，它们将由远程端自动执行。开发完WebDriver测试后，可能需要在多个浏览器和操作系统组合上运行测试。

### HTTP代理

```
import org.openqa.selenium.Proxy;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;

public class proxyTest {
  public static void main(String[] args) {
    Proxy proxy = new Proxy();
    proxy.setHttpProxy("<HOST:PORT>");
    ChromeOptions options = new ChromeOptions();
    options.setCapability("proxy", proxy);
    WebDriver driver = new ChromeDriver(options);
    driver.get("https://www.google.com/");
    driver.manage().window().maximize();
    driver.quit();
  }
}
```

### 页面加载策略

#### normal

默认情况为normal，WebDriver将等待页面加载完成。

```
import org.openqa.selenium.PageLoadStrategy;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.chrome.ChromeDriver;

public class pageLoadStrategy {
    public static void main(String[] args) {
        ChromeOptions chromeOptions = new ChromeOptions();
        chromeOptions.setPageLoadStrategy(PageLoadStrategy.NORMAL);
        WebDriver driver = new ChromeDriver(chromeOptions);
        try {
            // Navigate to Url
            driver.get("https://google.com");
        } finally {
            driver.quit();
        }
    }
}
```

#### eager

设置为eager时，WebDriver等待DOMContentLoaded事件触发，忽略样式、图像的加载开始执行。

```
import org.openqa.selenium.PageLoadStrategy;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.chrome.ChromeDriver;

public class pageLoadStrategy {
    public static void main(String[] args) {
        ChromeOptions chromeOptions = new ChromeOptions();
        chromeOptions.setPageLoadStrategy(PageLoadStrategy.EAGER);
        WebDriver driver = new ChromeDriver(chromeOptions);
        try {
            // Navigate to Url
            driver.get("https://google.com");
        } finally {
            driver.quit();
        }
    }
}
```

#### none

设置为none时，WebDriver仅等待初始页面加载完成。

```
import org.openqa.selenium.PageLoadStrategy;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.chrome.ChromeDriver;

public class pageLoadStrategy {
    public static void main(String[] args) {
        ChromeOptions chromeOptions = new ChromeOptions();
        chromeOptions.setPageLoadStrategy(PageLoadStrategy.NONE);
        WebDriver driver = new ChromeDriver(chromeOptions);
        try {
            // Navigate to Url
            driver.get("https://google.com");
        } finally {
            driver.quit();
        }
    }
}
```

### Web元素

WebDriver API 提供通过不同的属性查找Web元素（如 ID, Name, Class, XPath, CSS Selectors, link Text）

#### 查找单个元素

```
WebDriver driver = new FirefoxDriver();
driver.get("http://www.google.com");
// Get search box element from webElement 'q' using Find Element
WebElement searchBox = driver.findElement(By.name("q"));
searchBox.sendKeys("webdriver");
```

#### 查找多个元素

```
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.firefox.FirefoxDriver;
import java.util.List;

public class findElementsExample {
    public static void main(String[] args) {
        WebDriver driver = new FirefoxDriver();
        try {
            driver.get("https://example.com");
            // Get all the elements available with tag name 'p'
            List<WebElement> elements = driver.findElements(By.tagName("p"));
            for (WebElement element : elements) {
                System.out.println("Paragraph text:" + element.getText());
            }
        } finally {
            driver.quit();
        }
    }
}
```

#### 在单个父元素中查找单个子元素

```
WebDriver driver = new FirefoxDriver();
driver.get("http://www.google.com");
WebElement searchForm = driver.findElement(By.tagName("form"));
WebElement searchBox = searchForm.findElement(By.name("q"));
searchBox.sendKeys("webdriver");
```

#### 在单个父元素中查找多个子元素

```
import org.openqa.selenium.By;
  import org.openqa.selenium.WebDriver;
  import org.openqa.selenium.WebElement;
  import org.openqa.selenium.chrome.ChromeDriver;
  import java.util.List;

  public class findElementsFromElement {
      public static void main(String[] args) {
          WebDriver driver = new ChromeDriver();
          try {
              driver.get("https://example.com");

              // Get element with tag name 'div'
              WebElement element = driver.findElement(By.tagName("div"));

              // Get all the elements available with tag name 'p'
              List<WebElement> elements = element.findElements(By.tagName("p"));
              for (WebElement e : elements) {
                  System.out.println(e.getText());
              }
          } finally {
              driver.quit();
          }
      }
  }
```

#### 获取活动的焦点元素

```
import org.openqa.selenium.*;
  import org.openqa.selenium.chrome.ChromeDriver;

  public class activeElementTest {
    public static void main(String[] args) {
      WebDriver driver = new ChromeDriver();
      try {
        driver.get("http://www.google.com");
        driver.findElement(By.cssSelector("[name='q']")).sendKeys("webElement");

        // Get attribute of current active element
        String attr = driver.switchTo().activeElement().getAttribute("title");
        System.out.println(attr);
      } finally {
        driver.quit();
      }
    }
  }
```

### 键盘操作

#### sendKey

按键表：

https://www.w3.org/TR/webdriver/#keyboard-actions

```
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;

public class HelloSelenium {
  public static void main(String[] args) {
    WebDriver driver = new FirefoxDriver();
    try {
      // Navigate to Url
      driver.get("https://google.com");

      // Enter text "q" and perform keyboard action "Enter"
      driver.findElement(By.name("q")).sendKeys("q" + Keys.ENTER);
    } finally {
      driver.quit();
    }
  }
}
```

#### keyDown

按下按键(CONTROL, SHIFT, ALT)

```
WebDriver driver = new ChromeDriver();
try {
  // Navigate to Url
  driver.get("https://google.com");

  // Enter "webdriver" text and perform "ENTER" keyboard action
  driver.findElement(By.name("q")).sendKeys("webdriver" + Keys.ENTER);

  Actions actionProvider = new Actions(driver);
  Action keydown = actionProvider.keyDown(Keys.CONTROL).sendKeys("a").build();
  keydown.perform();
} finally {
  driver.quit();
}
```

#### keyUp

释放按键(CONTROL, SHIFT, ALT)

```
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.interactions.Actions;

public class HelloSelenium {
  public static void main(String[] args) {
    WebDriver driver = new FirefoxDriver();
    try {
      // Navigate to Url
      driver.get("https://google.com");
      Actions action = new Actions(driver);
      // Store google search box WebElement
      WebElement search = driver.findElement(By.name("q"));
      // Enters text "qwerty" with keyDown SHIFT key and after keyUp SHIFT key (QWERTYqwerty)
    action.keyDown(Keys.SHIFT).sendKeys(search,"qwerty").keyUp(Keys.SHIFT).sendKeys("qwerty").perform();
    } finally {
      driver.quit();
    }
  }
}
```

#### clear

清除可编辑元素内容

```
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class clear {
  public static void main(String[] args) {
    WebDriver driver = new ChromeDriver();
    try {
      // Navigate to Url
      driver.get("https://www.google.com");
      // Store 'SearchInput' element
      WebElement searchInput = driver.findElement(By.name("q"));
      searchInput.sendKeys("selenium");
      // Clears the entered text
      searchInput.clear();
    } finally {
      driver.quit();
    }
  }
}
```

## Page object

Page Object Models是一种设计模式，可为Web UI元素创建对象存储库，当UI修改时不需要修改测试部分。减少了代码的重复，提高了测试用例的可读性，可维护性。

## Grid

###

## Tips

- 在测试环境中禁用验证码
- 添加一个hook以允许测试绕过验证码
- 不适合测试下载文件
- 不适合性能测试，网站爬虫

```
import org.apache.log4j.Logger;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.openqa.selenium.Cookie;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.slf4j.ILoggerFactory;

import java.util.Set;

class MainTest {
    private static final Logger logger = Logger.getLogger(MainTest.class);
    WebDriver driver = new ChromeDriver();
    @BeforeEach
    void setUp() {
        //获得cookie
        Set<Cookie> coo = driver.manage().getCookies();
        //打印Cookie
        logger.info(coo);
        //清除所有的缓存
//        driver.manage().deleteAllCookies();
        driver.manage().deleteCookie();
    }

    @AfterEach
    void tearDown() {
    }

    @Test
    void main() {

    }
}
```

# Selenium Python

[API文档](https://www.selenium.dev/selenium/docs/api/py/index.html)

安装Selenium

```python
pip install selenium
```

下载[chromedriver](https://chromedriver.chromium.org/downloads)，实例化：

```python
#Simple assignment
from selenium.webdriver import Chrome
driver = Chrome(executable_path='C:\\Users\\lsaiah\\seleniumdemo\\chromedriver.exe')
#Or use the context manager
from selenium.webdriver import Chrome
with Chrome(executable_path='C:\\Users\\lsaiah\\seleniumdemo\\chromedriver.exe') as driver:
    #your code inside this indent
#或者
from selenium import webdriver
driver = webdriver.Chrome()
```

### 浏览器操作

```python
# 打开网站
driver.get('http://127.0.0.1:9000')
# 获取当前URL
driver.current_url
# 后退
driver.back()
# 向前
driver.forward()
# 刷新
driver.refresh()
# 获取标题
driver.title
# 切换窗口或标签
from selenium.webdriver.support import expected_conditions as EC
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.wait import WebDriverWait
with webdriver.Chrome() as driver:
    driver.get("http://127.0.0.1:9000/")
    wait = WebDriverWait(driver, 20)
    # 定义当前打开的窗口
    original_window = driver.current_window_handle
    # 断言总窗口数为1
    assert len(driver.window_handles) == 1, '窗口不等于1'
    # 前提注册链接target="_blank"
    driver.find_element(By.LINK_TEXT, "注册").click()
    # 等待注册窗口
    wait.until(EC.number_of_windows_to_be(2))
    # 循环判断新窗口和原窗口不一致，切换到原窗口
    for window_handle in driver.window_handles:
        if window_handle != original_window:
            driver.switch_to.window(original_window)
            break
    # 关闭窗口
    driver.close()
    # 切换到新窗口
    driver.switch_to.window(window_handle)
    # 等待新窗口loading结束
    wait.until(EC.title_is("注册 - Python Web Demo"))
    # Selenium 4和更高版本创建新标签，新窗口
    # driver.switch_to.new_window('tab')
```

```python
    # 安全退出
    driver.quit()
    # unittest
def tearDown(self):
    self.driver.quit()
    # try/finally
    try:
    #WebDriver code here...
finally:
    driver.quit()
    # with关键字执行结束自动退出
```

**操作iframe**

使用WebElement

```python
# Store iframe web element
iframe = driver.find_element(By.CSS_SELECTOR, "#modal > iframe")

# switch to selected iframe
driver.switch_to.frame(iframe)

# Now click on button
driver.find_element(By.TAG_NAME, 'button').click()
```

使用name或id

```python
# Switch frame by id
driver.switch_to.frame('buttonframe')

# Now, Click on the button
driver.find_element(By.TAG_NAME, 'button').click()
```

使用索引

```python
# Switch to the second frame
driver.switch_to.frame(1)
```

离开iframe

```python
# switch back to default content
driver.switch_to.default_content()
```

**窗口大小**

获取窗口大小

```python
# Access each dimension individually
width = driver.get_window_size().get("width")
height = driver.get_window_size().get("height")

# Or store the dimensions and query them later
size = driver.get_window_size()
width1 = size.get("width")
height1 = size.get("height")
```

设定窗口大小

```python
driver.set_window_size(1024, 768)
```

**窗口位置**

获取窗口位置

```python
# Access each dimension individually
x = driver.get_window_position().get('x')
y = driver.get_window_position().get('y')

# Or store the dimensions and query them later
position = driver.get_window_position()
x1 = position.get('x')
y1 = position.get('y')
```

设定窗口位置

```python
# Move the window to the top left of the primary monitor
driver.set_window_position(0, 0)
```

最大化

```python
driver.maximize_window()
```

最小化

```python
driver.minimize_window()
```

全屏

```python
driver.fullscreen_window()
```

**截屏**

screenshot for current browsing context.

```python
from selenium import webdriver
driver = webdriver.Chrome()
# Navigate to url
driver.get("http://www.example.com")
# Returns and base64 encoded string into image
driver.save_screenshot('./image.png')
driver.quit()
```

TakeElementScreenshot

screenshot of an element for current browsing context.

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
driver = webdriver.Chrome()
# Navigate to url
driver.get("http://www.example.com")
ele = driver.find_element(By.CSS_SELECTOR, 'h1')
# Returns and base64 encoded string into image
ele.screenshot('./image.png')
driver.quit()
```

**执行脚本**

```python
driver.execute_script('return document.title;')
```

