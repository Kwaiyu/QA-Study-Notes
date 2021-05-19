## 开始

- 安装Node.js通过NPM安装appium：
  
  ```
  npm install -g appium
  ```
- Appium Desktop
  
  [Release page](https://github.com/appium/appium-desktop/releases)
- 验证安装
  
  ```
  npm install -g appium-doctor
  appium-doctor --android
  ```

### 驱动程序

- 该[XCUITest驱动程序](http://appium.io/docs/en/drivers/ios-xcuitest/index.html)（iOS和tvOS应用程序）
- 在[咖啡驱动器](http://appium.io/docs/en/drivers/android-espresso/index.html)（适用于Android应用）
- 该[UiAutomator2驱动程序](http://appium.io/docs/en/drivers/android-uiautomator2/index.html)（适用于Android应用）
- 在[Windows驱动程序](http://appium.io/docs/en/drivers/windows/index.html)（用于Windows桌面应用程序）
- 在[Mac驱动](http://appium.io/docs/en/drivers/mac/index.html)（适用于Mac的桌面应用程序）

### 客户端

[客户端库列表](http://appium.io/docs/en/about-appium/appium-clients/index.html)

[mvnrepository](https://mvnrepository.com/artifact/io.appium/java-client)

Maven项目：

```xml
<dependency>
    <groupId>io.appium</groupId>
    <artifactId>java-client</artifactId>
    <version>7.3.0</version>
</dependency>
```

Gradle项目根目录build.gradle：

```xml
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
  
    repositories {
        maven { url 'https://maven.aliyun.com/repository/google'}
        maven { url 'https://maven.aliyun.com/repository/jcenter'}
        maven { url 'https://maven.aliyun.com/repository/central'}
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.6.3'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/google'}
        maven { url 'https://maven.aliyun.com/repository/jcenter'}
        maven { url 'https://maven.aliyun.com/repository/central'}
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

Gradle项目app build.gradle：

```xml
apply plugin: 'com.android.application'

android {
    compileSdkVersion 30
    buildToolsVersion "30.0.0"

    packagingOptions {
        exclude 'META-INF/spring.schemas'
        exclude 'META-INF/spring.handlers'
        exclude 'META-INF/versions/9'
        exclude 'META-INF/spring.tooling'
        exclude 'META-INF/DEPENDENCIES'
        exclude 'META-INF/LICENSE'
        exclude 'META-INF/LICENSE.txt'
        exclude 'META-INF/license.txt'
        exclude 'META-INF/NOTICE'
        exclude 'META-INF/NOTICE.txt'
        exclude 'META-INF/notice.txt'
        exclude 'META-INF/ASL2.0'
        exclude("META-INF/*.kotlin_module")
    }

    configurations {
//        all*.exclude module: 'selenium-api:3.141.59'
        all{
            exclude module: 'selenium-api:3.141.59'
        }
    }

    defaultConfig {
        applicationId "com.example.myapplication"
        minSdkVersion 29
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])

    implementation 'androidx.appcompat:appcompat:1.1.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'androidx.test.ext:junit:1.1.1'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'

    implementation (group: 'io.appium', name: 'java-client', version: '7.3.0'){
        exclude group: 'org.seleniumhq.selenium', module: 'selenium-api:3.141.59'
        exclude group: 'commons-logging', module: 'commons-logging'
    }
//    implementation files('libs/java-client-7.3.0.jar')
    implementation group: 'androidx.multidex', name: 'multidex', version: '2.0.1'
}
```

[JAVA client API](https://www.javadoc.io/doc/io.appium/java-client)

[Appium API](http://appium.io/docs/en/about-appium/api/)

[AM command](https://www.lsaiah.cn/admin/write-post.php?cid=41)

### 启动

- 启动Android Emulator（若不在Tools，Ctrl+Shift+A全局搜索AVD）或ADB连接真实设备
- 启动Appium Desktop（自带Appium Server）
- 启动检查器会话，配置DesiredCapabilities JSON（也可在WebDriver中编写脚本配置），启动会话
- 使用Appium元素检查或启动monitor.bat或Uiautomatorviewer或IDEA Android layout inspector。
  
  注意：Uiautomatorviewer需使用JDK 1.8版本配置与SDK swt.jar同位数，并配置ANDROID_HOME、ANDROID_SWT、JAVA_HOME、platform-tools、tools等环境变量，注释`call ..\lib\find_java.bat`并添加`set java_exe=C:\Program Files\Java\jdk1.8.0_251\bin\java.exe`

## 运行

### Desired Capabilities

[官方文档](https://appium.io/docs/en/writing-running-appium/caps/)

新建driver的时候必须要指定一个DesiredCapabilities 对象配置发送给Appium Server：

```
public class Test01 {
    public static void main(String[] args) throws MalformedURLException {
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("deviceName","Android Emulator");
        capabilities.setCapability("platformVersion", "10");
        capabilities.setCapability("platformName","Android");
        capabilities.setCapability("appPackage", "io.appium.android.apis");
        capabilities.setCapability("appActivity", ".ApiDemos");
        AppiumDriver driver = new AppiumDriver(new URL("http://127.0.0.1:4723"), capabilities);
    }
}
```

adb获取App Activity和包名

```
adb shell dumpsys window | findstr mCurrentFocus
or
adb shell logcat|grep -i displayed
```

### Maven pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.example</groupId>
    <artifactId>TestAppium</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/io.appium/java-client -->
        <dependency>
            <groupId>io.appium</groupId>
            <artifactId>java-client</artifactId>
            <version>7.5.1</version>
        </dependency>
<!--        &lt;!&ndash; https://mvnrepository.com/artifact/junit/junit &ndash;&gt;-->
<!--        <dependency>-->
<!--            <groupId>junit</groupId>-->
<!--            <artifactId>junit</artifactId>-->
<!--            <version>4.13.2</version>-->
<!--            <scope>test</scope>-->
<!--        </dependency>-->
        <!-- https://mvnrepository.com/artifact/org.testng/testng -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>7.4.0</version>
            <scope>test</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.30</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-simple -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.30</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

### 基类

```java
import io.appium.java_client.service.local.AppiumDriverLocalService;
import io.appium.java_client.service.local.AppiumServiceBuilder;
import org.testng.annotations.AfterSuite;
import org.testng.annotations.BeforeSuite;

import java.io.File;
import java.net.URL;

public abstract class BaseTest {
    private static AppiumDriverLocalService service;

    @BeforeSuite
    public void globalSetup () {
        AppiumServiceBuilder serviceBuilder = new AppiumServiceBuilder().withAppiumJS(new File("C:\\Program Files\\Appium\\resources\\app\\node_modules\\appium\\build\\lib\\main.js"));
        service = AppiumDriverLocalService.buildService(serviceBuilder);
        service.start();
    }
    @AfterSuite
    public void globalTearDown () {
        if (service != null) {
            service.stop();
        }
    }
    public URL getServiceUrl () {
        return service.getUrl();
    }
}
```

### TestNG+Appium+SLF4J+APP查找元素

```java
import io.appium.java_client.android.AndroidDriver;
import io.appium.java_client.service.local.AppiumDriverLocalService;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.testng.Assert;
import org.testng.annotations.AfterSuite;
import org.testng.annotations.BeforeSuite;
import org.testng.annotations.Test;

import java.io.File;
import java.util.List;

public class AndroidSelectorsTest extends BaseTest {
    private AndroidDriver<WebElement> driver;
    private static AppiumDriverLocalService service;
    private final String PACKAGE = "io.appium.android.apis";

    @BeforeSuite
    public void setUp() throws Exception {
        File classpathRoot = new File(System.getProperty("user.dir"));
        File appDir = new File(classpathRoot, "./apps");
        File app = new File(appDir.getCanonicalPath(), "ApiDemos-debug.apk");
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("deviceName", "Android Emulator");
        capabilities.setCapability("platformVersion", "10");
        capabilities.setCapability("platformName","Android");
        capabilities.setCapability("app", app.getAbsolutePath());
        capabilities.setCapability("appPackage", "io.appium.android.apis");
        capabilities.setCapability("appActivity", ".ApiDemos");
        driver = new AndroidDriver<WebElement>(getServiceUrl(), capabilities);
    }

    @AfterSuite
    public void tearDown() {
        driver.quit();
    }


    @Test
    public void testFindElementsByAccessibilityId () {
        // Look for element by accessibility. In Android this is the "content-desc"
        List<WebElement> searchParametersElement = (List<WebElement>) driver.findElementsByAccessibilityId("Content");
        Assert.assertEquals(searchParametersElement.size(), 1);
    }

    @Test
    public void testFindElementsById () {
        // Look for element by ID. In Android this is the "resource-id"
        List<WebElement> actionBarContainerElements = (List<WebElement>) driver.findElementsById("android:id/action_bar_container");
        Assert.assertEquals(actionBarContainerElements.size(), 1);
    }

    @Test
    public void testFindElementsByClassName () {
        // Look for elements by the class name. In Android this is the Java Class Name of the view.
        List<WebElement> linearLayoutElements = (List<WebElement>) driver.findElementsByClassName("android.widget.FrameLayout");
        Assert.assertTrue(linearLayoutElements.size() > 1);
    };

    @Test
    public void testFindElementsByXPath () {
        // Find elements by XPath
        List<WebElement> linearLayoutElements = (List<WebElement>) driver.findElementsByXPath("//*[@class=\"android.widget.FrameLayout\"]");
        Assert.assertTrue(linearLayoutElements.size() > 1);
    };
}
```

### TestNG+Appium+SLF4J+APP断言Activity和包名

```java
import io.appium.java_client.android.AndroidDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.testng.Assert;
import org.testng.annotations.*;

import java.io.File;

public class AndroidCreateSessionTest extends BaseTest {
    private AndroidDriver<WebElement> driver;

    @BeforeClass
    public void setUp() throws Exception {
        File classpathRoot = new File(System.getProperty("user.dir"));
        File appDir = new File(classpathRoot, "./apps");
        File app = new File(appDir.getCanonicalPath(), "ApiDemos-debug.apk");
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("deviceName", "Android Emulator");
        capabilities.setCapability("platformVersion", "10");
        capabilities.setCapability("platformName","Android");
        capabilities.setCapability("app", app.getAbsolutePath());
        capabilities.setCapability("appPackage", "io.appium.android.apis");
        capabilities.setCapability("appActivity", ".ApiDemos");
        driver = new AndroidDriver<WebElement>(getServiceUrl(), capabilities);
    }

    @AfterClass
    public void tearDown() {
        driver.quit();
    }

    @Test()
    public void testCreateSession() {
        String activity = driver.currentActivity();
        String pkg = driver.getCurrentPackage();
        Assert.assertEquals(activity, ".ApiDemos");
        Assert.assertEquals(pkg, "io.appium.android.apis");
    }
}
```

### TestNG+Appium+SLF4J+浏览器页面

```java
import io.appium.java_client.android.AndroidDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.testng.Assert;
import org.testng.annotations.*;

import java.net.URI;
import java.net.URISyntaxException;

public class AndroidCreateWebSessionTest extends BaseTest {
    private AndroidDriver<WebElement> driver;

    @BeforeClass
    public void setUp() {
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("deviceName", "Android Emulator");
        capabilities.setCapability("browserName", "Chrome");
        capabilities.setCapability("chromedriverExecutable", "C:\\Users\\lsaia\\node_modules\\appium\\node_modules\\appium-chromedriver\\chromedriver\\win\\chromedriver_win32_v74.0.3729.6.exe");
        driver = new AndroidDriver<WebElement>(getServiceUrl(), capabilities);
    }

    @AfterClass
    public void tearDown() {
        driver.quit();
    }

    @Test()
    public void testCreateWebSession() throws URISyntaxException {
        driver.get(new URI("http://www.baidu.com").toString());
        String title = driver.getTitle();
        Assert.assertEquals(title, "百度一下");
    }
}
```

### saucelabs+Appium+Windwos web

```java
import io.appium.java_client.android.AndroidDriver;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.testng.Assert;
import org.testng.annotations.AfterTest;
import org.testng.annotations.BeforeTest;
import org.testng.annotations.Test;
import java.net.MalformedURLException;
import java.net.URL;

/**
 * Android Browser Sauce Labs Test.
 */
public class AndroidBrowserSaucelabsTest {
    public static final String USERNAME = "lsaiah";
    public static final String ACCESS_KEY = "d23090b3-996c-46ef-ab7a-19dfdbf8cd7c";
    public static final String URL = "https://" + USERNAME + ":" + ACCESS_KEY + "@ondemand.apac-southeast-1.saucelabs.com:443/wd/hub";
    public static WebDriver driver;
    @BeforeTest
    public void beforeTest() throws MalformedURLException {
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("platformName", "Windows 10");
        capabilities.setCapability("platformVersion", "latest");
        capabilities.setCapability("browserName", "Chrome");
        capabilities.setCapability("newCommandTimeout", 2000);
        driver = new AndroidDriver<WebElement>(new URL(URL), capabilities);
    }

    @AfterTest
    public void afterTest( ){
        driver.quit();
    }

    @Test
    public static void launchBrowser(){
        driver.get("http://appium.io/");
        Assert.assertEquals(driver.getCurrentUrl(), "http://appium.io/", "URL Mismatch");
        Assert.assertEquals(driver.getTitle(), "Appium: Mobile App Automation Made Awesome.", "Title Mismatch");
    }
    @Test
    public static void testWebSession(){
        driver.get("http://www.baidu.com");
        String title = driver.getTitle();
        Assert.assertEquals(title, "百度一下，你就知道");
    }
}
```

