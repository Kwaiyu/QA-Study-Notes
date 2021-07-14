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
  
  之前有些warn的包没装，现在全部装好了，记录一些踩坑的点：

![image-20210704192122222](https://qwq.lsaiah.cn//image-20210704192122222.png)

**opencv4nodejs**

自动构建：

```
$ npm install -g node-gyp
# npm install --global --production windows-build-tools 在安装完Python2.7后卡死，可以尝试下面命令
$ npm install --global --production windows-build-tools --vs2015
$ npm config set python python2.7
$ npm config set msvs_version 2015
$ npm config set python /path/to/executable/python2.7
$ npm config get python
# 将Python环境变量指向v2.7

# 安装最新发布的CMAKE：https://cmake.org/download 并添加到Path
$ npm install opencv4nodejs
# CMakeDownloadLog报download failed 将raw.githubusercontent.com加入代理
# 找不到boostdesc_lbgm.i .... 等文件时：
cd ./cache/xfeatures2d/
cd boostdesc
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_lbgm.i> 0ae0675534aa318d9668f2a179c2a052-boostdesc_lbgm.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_binboost_256.i> e6dcfa9f647779eb1ce446a8d759b6ea-boostdesc_binboost_256.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_binboost_128.i> 98ea99d399965c03d555cef3ea502a0b-boostdesc_binboost_128.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_binboost_064.i> 202e1b3e9fec871b04da31f7f016679f-boostdesc_binboost_064.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_bgm_hd.i> 324426a24fa56ad9c5b8e3e0b3e5303e-boostdesc_bgm_hd.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_bgm_bi.i> 232c966b13651bd0e46a1497b0852191-boostdesc_bgm_bi.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/34e4206aef44d50e6bbcd0ab06354b52e7466d26/boostdesc_bgm.i> 0ea90e7a8f3f7876d450e4149c97c74f-boostdesc_bgm.i

cd ../vgg
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_120.i> 151805e03568c9f490a5e3a872777b75-vgg_generated_120.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_64.i> 7126a5d9a8884ebca5aea5d63d677225-vgg_generated_64.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_48.i> e8d0dcd54d1bcfdc29203d011a797179-vgg_generated_48.i
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_80.i> 7cd47228edec52b6d82f46511af325c5-vgg_generated_80.i

# 注意文件大小如果是0，手动下载
# 如果缓存不可用，将这些文件添加到“npm\node_modules\opencv4nodejs\node_modules\opencv-build\opencv\opencv_contrib\modules\xfeatures2d\src”文件夹中

```

手动构建：

1. 安装opencv release，可以在[这里](https://opencv.org/releases/)找到

2. 禁用自动构建，添加环境变量，并SET OPENCV4NODEJS_DISABLE_AUTOBUILD=1

   ```
   OPENCV4NODEJS_DISABLE_AUTOBUILD=1
   ```

3. 设置系统环境变量

   ![image-20210704194516243](https://qwq.lsaiah.cn//image-20210704194516243.png)

   ```
   npm install opencv4nodejs
   ```

**bundletool.jar**

在https://github.com/google/bundletool/releases 下载bundletool.jar，改名为bundletool.jar

在android sdk目录下创建bundle-tool目录，把jar包放入，属性-安全-完全控制，然后添加到**用户环境变量**，并在系统环境变量`PATHEXT`中加入`;.jar`

### 驱动程序

- The [XCUITest Driver](https://github.com/Kwaiyu/appium/blob/master/docs/en/drivers/ios-xcuitest.md) (for iOS and tvOS apps)
- The [Espresso Driver](https://github.com/Kwaiyu/appium/blob/master/docs/en/drivers/android-espresso.md) (for Android apps)
- The [UiAutomator2 Driver](https://github.com/Kwaiyu/appium/blob/master/docs/en/drivers/android-uiautomator2.md) (for Android apps)
- The [Windows Driver](https://github.com/Kwaiyu/appium/blob/master/docs/en/drivers/windows.md) (for Windows Desktop apps)
- The [Mac Driver](https://github.com/Kwaiyu/appium/blob/master/docs/en/drivers/mac.md) (for Mac Desktop apps)

### 客户端

[客户端库列表](http://appium.io/docs/en/about-appium/appium-clients/index.html)

[mvnrepository](https://mvnrepository.com/artifact/io.appium/java-client)

Maven项目：

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

**java-client BUG**

```
//报错 1. More than one file was found with OS independent path 'META-INF/spring.tooling'
...
// 在build.gradle(app)中排除
android{
	    packagingOptions{
        exclude 'META-INF/spring.tooling'
        exclude 'META-INF/versions/9'
        exclude 'META-INF/spring.handlers'
        exclude 'META-INF/license.txt'
        exclude 'META-INF/DEPENDENCIES'
        exclude 'META-INF/spring.schemas'
        exclude 'META-INF/notice.txt'
    }
}
//报错 2. Duplicate class org.apache.commons.logging.Log found in modules jetified-commons-logging-1.2 (commons-logging:commons-logging:1.2) and jetified-spring-jcl-5.3.4 (org.springframework:spring-jcl:5.3.4)
...
// 
    compileOnly ('org.seleniumhq.selenium:selenium-java:3.141.59')
    androidTestImplementation('org.seleniumhq.selenium:selenium-java:3.141.59')
    testImplementation('org.seleniumhq.selenium:selenium-java:3.141.59')
    implementation (group: 'io.appium', name: 'java-client', version: '7.5.1') {
        ['org.apache.commons','commons-logging',
         'junit'].each {
            exclude group: "$it"
        }
        exclude group: "org.seleniumhq.selenium", module: "selenium-api"
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
    public void setUp() throws Exception {
        service = AppiumDriverLocalService.buildDefaultService();
        File classpathRoot = new File(System.getProperty("user.dir"));
        File appDir = new File(classpathRoot, "../Appium/apps");
        File app = new File(appDir.getCanonicalPath(), "ApiDemos-debug.apk");
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("automationName", "uiautomator2");
        capabilities.setCapability("platformName", "Android");
        capabilities.setCapability("platformVersion", "11");
        capabilities.setCapability("deviceName","emulator-5554");
//        capabilities.setCapability("noReset", true);
        capabilities.setCapability("app", app.getAbsolutePath());
        capabilities.setCapability("appPackage", "io.appium.android.apis");
        capabilities.setCapability("appActivity", "io.appium.android.apis.ApiDemos");
        service.start();
        driver = new AppiumDriver(new URL("http://127.0.0.1:4723/wd/hub"), capabilities);
//        driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
    }
```

adb获取App Activity和包名

```
adb shell dumpsys window | findstr mCurrentFocus
or
adb shell logcat|grep -i displayed
or
adb logcat ActivityManager:I *:s
{包名：io.appium.android.apis/Activity名:io.appium.android.apis.ApiDemos}
```

## Android

### 模拟器

```java
import io.appium.java_client.AppiumDriver;
import io.appium.java_client.MobileElement;
import io.appium.java_client.TouchAction;
import io.appium.java_client.service.local.AppiumDriverLocalService;
import io.appium.java_client.touch.offset.PointOption;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.interactions.touch.TouchActions;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.testng.Assert;
import org.testng.annotations.AfterSuite;
import org.testng.annotations.BeforeSuite;
import org.testng.annotations.Test;

import java.io.File;
import java.net.URL;
import java.util.List;
import java.util.concurrent.TimeUnit;


public class EmulatorUiTest {
    private AppiumDriver driver;
    public static AppiumDriverLocalService service=null;

    @BeforeSuite
    public void setUp() throws Exception {
        service = AppiumDriverLocalService.buildDefaultService();
        File classpathRoot = new File(System.getProperty("user.dir"));
        File appDir = new File(classpathRoot, "../Appium/apps");
        File app = new File(appDir.getCanonicalPath(), "ApiDemos-debug.apk");
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("automationName", "uiautomator2");
        capabilities.setCapability("platformName", "Android");
        capabilities.setCapability("platformVersion", "11");
        capabilities.setCapability("deviceName","emulator-5554");
        capabilities.setCapability("noReset", true);
        capabilities.setCapability("app", app.getAbsolutePath());
        capabilities.setCapability("appPackage", "io.appium.android.apis");
        capabilities.setCapability("appActivity", "io.appium.android.apis.ApiDemos");
        service.start();
        driver = new AppiumDriver(new URL("http://127.0.0.1:4723/wd/hub"), capabilities);
        driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
    }
    @AfterSuite
    public void tearDown() {
        service.stop();
//        driver.quit();
    }

    @Test
    public void testFindElementsByAccessibilityId() {
        List<WebElement> searchParametersElement = (List<WebElement>)
                driver.findElementsByAccessibilityId("Content");
        Assert.assertEquals(searchParametersElement.size(),1);
    }
    @Test
    public void testFindElementsById () {
        // Look for element by ID. In Android this is the "resource-id"
        List<WebElement> actionBarContainerElements = (List<WebElement>)
                driver.findElementsById("android:id/action_bar_container");
        Assert.assertEquals(actionBarContainerElements.size(), 1);
    }

    @Test
    public void testFindElementsByClassName () {
        // Look for elements by the class name. In Android this is the Java Class Name of the view.
        List<WebElement> linearLayoutElements = (List<WebElement>)
                driver.findElementsByClassName("android.widget.FrameLayout");
        Assert.assertTrue(linearLayoutElements.size() > 1);
    };

    @Test
    public void testFindElementsByXPath () {
        // Find elements by XPath
        List<WebElement> linearLayoutElements = (List<WebElement>)
                driver.findElementsByXPath("//*[@class=\"android.widget.FrameLayout\"]");
        Assert.assertTrue(linearLayoutElements.size() > 1);
    }

    @Test
    public void testTouchActionSwipe() {
        MobileElement el1 = (MobileElement) driver.findElementByAccessibilityId("App");
        el1.click();
        new TouchAction(driver).press(PointOption.point(993,1753))
                .moveTo(PointOption.point(982,1240)).release().perform();

        MobileElement el2 = (MobileElement) driver.findElementByAccessibilityId("Text-To-Speech");
        el2.click();
        MobileElement el3 = (MobileElement) driver.findElementByAccessibilityId("Again");
        el3.click();
    }
    @Test
    public void testSendKeys() throws InterruptedException {
        MobileElement el1 = (MobileElement) driver.findElementByAccessibilityId("App");
        el1.click();
        MobileElement el2 = (MobileElement) driver.findElementByAccessibilityId("Search");
        el2.click();
        MobileElement el3 = (MobileElement) driver.findElementByAccessibilityId("Invoke Search");
        el3.click();
        MobileElement el4 = (MobileElement) driver.findElementById("io.appium.android.apis:id/txt_query_prefill");
        el4.sendKeys("hello");
        MobileElement el5 = (MobileElement) driver.findElementById("io.appium.android.apis:id/txt_query_appdata");
        el5.sendKeys("world");
        MobileElement el6 = (MobileElement) driver.findElementByAccessibilityId("onSearchRequested()");
        el6.click();
        Thread.sleep(1000);
    }
    @Test
    public void testGetText(){
        MobileElement el1 = (MobileElement) driver.findElementByAccessibilityId("Text");
        el1.click();
        MobileElement el2 = (MobileElement) driver.findElementByAccessibilityId("LogTextBox");
        el2.click();
        MobileElement el3 = (MobileElement) driver.findElementByAccessibilityId("Add");
        el3.click();
        MobileElement el4 = (MobileElement) driver.findElement(By.id("io.appium.android.apis:id/text"));
//        el1.getText();
        Logger logger = LoggerFactory.getLogger(EmulatorUiTest.class);
        logger.info("文本内容是: " + el4.getText());
        Assert.assertEquals("This is a test\n", el4.getText());
    }
}
```

### 真机

```java
import io.appium.java_client.AppiumDriver;
import io.appium.java_client.MobileElement;
import io.appium.java_client.TouchAction;
import io.appium.java_client.android.AndroidDriver;
import io.appium.java_client.functions.ExpectedCondition;
import io.appium.java_client.service.local.AppiumDriverLocalService;
import io.appium.java_client.touch.offset.PointOption;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.testng.Assert;
import org.testng.annotations.AfterClass;
import org.testng.annotations.AfterSuite;
import org.testng.annotations.BeforeSuite;
import org.testng.annotations.Test;

import java.io.File;
import java.io.IOException;
import java.net.URL;
import java.util.List;
import java.util.concurrent.TimeUnit;

public class RealMachineTest {
    private AppiumDriver driver;
    public static AppiumDriverLocalService service=null;
    @BeforeSuite
    public void setUp() throws Exception {
        service = AppiumDriverLocalService.buildDefaultService();
        File classpathRoot = new File(System.getProperty("user.dir"));
        File appDir = new File(classpathRoot, "../Appium/apps");
        File app = new File(appDir.getCanonicalPath(), "ApiDemos-debug.apk");
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("automationName", "uiautomator2");
        capabilities.setCapability("platformName", "Android");
        capabilities.setCapability("platformVersion", "6");
        capabilities.setCapability("deviceName","6DSOTWGMPB6HSGZH");
        capabilities.setCapability("noReset", true);
        capabilities.setCapability("app", app.getAbsolutePath());
        capabilities.setCapability("appPackage", "io.appium.android.apis");
        capabilities.setCapability("appActivity", "io.appium.android.apis.ApiDemos");
        service.start();
        driver = new AppiumDriver(new URL("http://127.0.0.1:4723/wd/hub"), capabilities);
        driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
    }

    @AfterSuite
    public void tearDown() {
        service.stop();
//        driver.quit();
    }

    @Test
    public void testFindElementsByAccessibilityId() {
        List<WebElement> searchParametersElement = (List<WebElement>)
                driver.findElementsByAccessibilityId("Content");
        Assert.assertEquals(searchParametersElement.size(),1);
    }

    @Test
    public void testFindElementsById () {
        // Look for element by ID. In Android this is the "resource-id"
        List<WebElement> actionBarContainerElements = (List<WebElement>)
                driver.findElementsById("android:id/action_bar_container");
        Assert.assertEquals(actionBarContainerElements.size(), 1);
    }

    @Test
    public void testFindElementsByClassName () {
        // Look for elements by the class name. In Android this is the Java Class Name of the view.
        List<WebElement> linearLayoutElements = (List<WebElement>)
                driver.findElementsByClassName("android.widget.FrameLayout");
        Assert.assertTrue(linearLayoutElements.size() > 1);
    };

    @Test
    public void testFindElementsByXPath () {
        // Find elements by XPath
        List<WebElement> linearLayoutElements = (List<WebElement>)
                driver.findElementsByXPath("//*[@class=\"android.widget.FrameLayout\"]");
        Assert.assertTrue(linearLayoutElements.size() > 1);
    }

    @Test
    public void testTouchActionSwipe() {
        MobileElement el1 = (MobileElement) driver.findElementByAccessibilityId("App");
        el1.click();
        new TouchAction(driver).press(PointOption.point(993,1753))
                .moveTo(PointOption.point(982,1240)).release().perform();

            MobileElement el2 = (MobileElement) driver.findElementByAccessibilityId("Text-To-Speech");
            el2.click();
            MobileElement el3 = (MobileElement) driver.findElementByAccessibilityId("Again");
            el3.click();

    }
    @Test
    public void testSendKeys() throws InterruptedException {
        MobileElement el1 = (MobileElement) driver.findElementByAccessibilityId("App");
        el1.click();
        MobileElement el2 = (MobileElement) driver.findElementByAccessibilityId("Search");
        el2.click();
        MobileElement el3 = (MobileElement) driver.findElementByAccessibilityId("Invoke Search");
        el3.click();
        MobileElement el4 = (MobileElement) driver.findElementById("io.appium.android.apis:id/txt_query_prefill");
        el4.sendKeys("hello");
        MobileElement el5 = (MobileElement) driver.findElementById("io.appium.android.apis:id/txt_query_appdata");
        el5.sendKeys("world");
        MobileElement el6 = (MobileElement) driver.findElementByAccessibilityId("onSearchRequested()");
        el6.click();
        Thread.sleep(1000);
    }
    @Test
    public void testGetText(){
        MobileElement el1 = (MobileElement) driver.findElementByAccessibilityId("Text");
        el1.click();
        MobileElement el2 = (MobileElement) driver.findElementByAccessibilityId("LogTextBox");
        el2.click();
        MobileElement el3 = (MobileElement) driver.findElementByAccessibilityId("Add");
        el3.click();
        MobileElement el4 = (MobileElement) driver.findElement(By.id("io.appium.android.apis:id/text"));
//        el1.getText();
        Logger logger = LoggerFactory.getLogger(RealMachineTest.class);
        logger.info("文本内容是: " + el4.getText());
        Assert.assertEquals("This is a test\n", el4.getText());
    }

    @Test
    public void testWaitUntil() {
        MobileElement el1 = (MobileElement) new WebDriverWait(driver, 10)
          .until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//android.widget.TextView[@content-desc=\"App\"]")));
        el1.click();
    }
}

```

### 等待

1. 强制等待（1秒）

   ```java
   time.sleep(1)
   Thread.sleep(1000)
   driver.manage().timeouts().pageLoadTimeout(1, TimeUnit.SECONDS);
   ```

2. 隐式等待

   implicitlywait()方法用来等待页面加载完成，implicitly_wait(1)，1秒内一旦加载完成就执行下一条语句，如果11秒内页面都没有加载完，就超时抛出异常。但是隐式等待依然存在一个问题，那就是程序会一直等待整个页面加载完成，但有时候页面想要的元素已经加载完成，因为个别加载慢，仍然要等到页面全部完成才能执行下一步。

   ```java
   driver.manage().timeouts().implicitlyWait(30, TimeUnit.SECONDS);
   ```

3. 显示等待/动态等待

   1. 等待指定时间内加载完成；

   ```
   WebDriverWait(driver,timeout,poll_frequency=0.5,ignored_exceptions=None)
   ```

   - driver：浏览器驱动
   - timeout：最长超时时间，默认以秒为单位
   - poll_frequency：检测的间隔，默认为0.5s
   - ignored_exceptions：超时后的抛出的异常信息，默认抛出NoSuchElementExeception异常。
   2. 等待页面加载出现某个具体的元素后继续执行；
   
      ```java
      WebDriverWait(driver,10).until(method，message="")
      WebDriverWait(driver,10).until_not(method，message="")
      WebDriverWait(driver, 10).until(lambda x:x.find_element_by_id('elementid'))
      
      MobileElement el1 = (MobileElement) new WebDriverWait(driver, 10)             .until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//android.widget.TextView[@content-desc=\"App\"]")));
              el1.click();
      ```
   
      
   
   



### 基类

**JAVA**

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

**Python**

```python
import pytest
import datetime
import os

from helpers import ensure_dir


def pytest_configure(config):
    if not hasattr(config, 'input'):
        current_day = '{:%Y_%m_%d_%H_%S}'.format(datetime.datetime.now())
        ensure_dir(os.path.join(os.path.dirname(__file__), 'input', current_day))
        result_dir = os.path.join(os.path.dirname(__file__), 'results', current_day)
        ensure_dir(result_dir)
        result_dir_test_run = result_dir
        ensure_dir(os.path.join(result_dir_test_run, 'screenshots'))
        ensure_dir(os.path.join(result_dir_test_run, 'logcat'))
        config.screen_shot_dir = os.path.join(result_dir_test_run, 'screenshots')
        config.logcat_dir = os.path.join(result_dir_test_run, 'logcat')


class DeviceLogger:
    def __init__(self, logcat_dir, screenshot_dir):
        self.screenshot_dir = screenshot_dir
        self.logcat_dir = logcat_dir


@pytest.fixture(scope='function')
def device_logger(request):
    logcat_dir = request.config.logcat_dir
    screenshot_dir = request.config.screen_shot_dir
    return DeviceLogger(logcat_dir, screenshot_dir)

```

```python
import os
import sys
from selenium.common.exceptions import InvalidSessionIdException
from datetime import datetime
from sauceclient import SauceClient


ANDROID_BASE_CAPS = {
    'app': os.path.abspath('../apps/ApiDemos-debug.apk'),
    'automationName': 'UIAutomator2',
    'platformName': 'Android',
    'platformVersion': os.getenv('ANDROID_PLATFORM_VERSION') or '8.0',
    'deviceName': os.getenv('ANDROID_DEVICE_VERSION') or 'Android Emulator',
}

IOS_BASE_CAPS = {
    'app': os.path.abspath('../apps/TestApp.app.zip'),
    'automationName': 'xcuitest',
    'platformName': 'iOS',
    'platformVersion': os.getenv('IOS_PLATFORM_VERSION') or '12.2',
    'deviceName': os.getenv('IOS_DEVICE_NAME') or 'iPhone 8 Simulator',
    # 'showIOSLog': False,
}

if os.getenv('SAUCE_LABS') and os.getenv('SAUCE_USERNAME') and os.getenv('SAUCE_ACCESS_KEY'):
    build_id = os.getenv('TRAVIS_BUILD_ID') or datetime.now().strftime('%B %d, %Y %H:%M:%S')
    build_name = 'Python Sample Code %s' % build_id

    ANDROID_BASE_CAPS['build'] = build_name
    ANDROID_BASE_CAPS['tags'] = ['e2e', 'appium', 'sample-code', 'android', 'python']
    ANDROID_BASE_CAPS['app'] = 'http://appium.github.io/appium/assets/ApiDemos-debug.apk'

    IOS_BASE_CAPS['build'] = build_name
    IOS_BASE_CAPS['tags'] = ['e2e', 'appium', 'sample-code', 'ios', 'python']
    IOS_BASE_CAPS['app'] = 'http://appium.github.io/appium/assets/TestApp9.4.app.zip'

    EXECUTOR = 'http://{}:{}@ondemand.saucelabs.com:80/wd/hub'.format(
        os.getenv('SAUCE_USERNAME'), os.getenv('SAUCE_ACCESS_KEY'))

    sauce = SauceClient(os.getenv('SAUCE_USERNAME'), os.getenv('SAUCE_ACCESS_KEY'))
else:
    EXECUTOR = 'http://127.0.0.1:4723/wd/hub'


def ensure_dir(directory):
    if not os.path.exists(directory):
        os.makedirs(directory)


def take_screenshot_and_logcat(driver, device_logger, calling_request):
    __save_log_type(driver, device_logger, calling_request, 'logcat')


def take_screenshot_and_syslog(driver, device_logger, calling_request):
    __save_log_type(driver, device_logger, calling_request, 'syslog')


def __save_log_type(driver, device_logger, calling_request, type):
    logcat_dir = device_logger.logcat_dir
    screenshot_dir = device_logger.screenshot_dir

    try:
        driver.save_screenshot(os.path.join(screenshot_dir, calling_request + '.png'))
        logcat_data = driver.get_log(type)
    except InvalidSessionIdException:
        logcat_data = ''

    with open(os.path.join(logcat_dir, '{}_{}.log'.format(calling_request, type)), 'w') as logcat_file:
        for data in logcat_data:
            data_string = '%s:  %s\n' % (data['timestamp'], data['message'].encode('utf-8'))
            logcat_file.write(data_string)

def report_to_sauce(session_id):
    print("Link to your job: https://saucelabs.com/jobs/%s" % session_id)
    passed = str(sys.exc_info() == (None, None, None))
    sauce.jobs.update_job(session_id, passed=passed)

```



### TestNG+Appium+SLF4J+APP查找元素

**Java**

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

**Python**

```python
import pytest
import os
import copy

from appium import webdriver
from helpers import report_to_sauce, take_screenshot_and_logcat, ANDROID_BASE_CAPS, EXECUTOR


class TestAndroidSelectors():

    @pytest.fixture(scope='function')
    def driver(self, request, device_logger):
        calling_request = request._pyfuncitem.name

        caps = copy.copy(ANDROID_BASE_CAPS)
        caps['name'] = calling_request
        driver = webdriver.Remote(
            command_executor=EXECUTOR,
            desired_capabilities=caps
        )

        def fin():
            report_to_sauce(driver.session_id)
            take_screenshot_and_logcat(driver, device_logger, calling_request)
            driver.quit()

        request.addfinalizer(fin)

        driver.implicitly_wait(10)
        return driver

    def test_should_find_elements_by_accessibility_id(self, driver):
        search_parameters_element = driver.find_elements_by_accessibility_id('Content')
        assert 1 == len(search_parameters_element)

    def test_should_find_elements_by_id(self, driver):
        action_bar_container_elements = driver.find_elements_by_id('android:id/action_bar_container')
        assert 1 == len(action_bar_container_elements)

    def test_should_find_elements_by_class_name(self, driver):
        action_bar_container_elements = driver.find_elements_by_class_name('android.widget.FrameLayout')
        assert 3 == len(action_bar_container_elements)

    def test_should_find_elements_by_xpath(self, driver):
        action_bar_container_elements = driver.find_elements_by_xpath('//*[@class="android.widget.FrameLayout"]')
        assert 3 == len(action_bar_container_elements)

```



### TestNG+Appium+Logger+APP对话框

**Java**

```java
import android.content.res.Resources;
import io.appium.java_client.android.Activity;
import io.appium.java_client.android.AndroidDriver;
import io.appium.java_client.android.AndroidElement;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.Assert;
import org.testng.annotations.AfterClass;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;
import org.testng.log4testng.Logger;

import java.io.File;
import java.io.IOException;

public class AndroidBasicInteractionsTest extends BaseTest {
    private AndroidDriver<WebElement> driver;
    private final String SEARCH_ACTIVITY = ".app.SearchInvoke";
    private final String ALERT_DIALOG_ACTIVITY = ".app.AlertDialogSamples";
    private final String PACKAGE = "io.appium.android.apis";
    public static Logger logger = Logger.getLogger(AndroidBasicInteractionsTest.class);

    @BeforeClass
    public void setUp() throws IOException {
        File classpathRoot = new File(System.getProperty("user.dir"));
        File appDir = new File(classpathRoot, "../TestAppium/apps");
        File app = new File(appDir.getCanonicalPath(), "ApiDemos-debug.apk");
        DesiredCapabilities capabilities = new DesiredCapabilities();
        /*
        'deviceName' capability only affects device selection if you run the test in a cloud
        environment. It has no effect if the test is executed on a local machine.
        */
        capabilities.setCapability("deviceName", "Android Emulator");
        capabilities.setCapability("deviceName", "Android Emulator");
        capabilities.setCapability("platformVersion", "10");
        capabilities.setCapability("platformName", "Android");
        capabilities.setCapability("appPackage", "io.appium.android.apis");
        capabilities.setCapability("appActivity", ".ApiDemos");
        /*
        It makes sense to set device udid if there are multiple devices/emulators
        connected to the local machine. Run `adb devices -l` to check which devices
        are online and what are their identifiers.
        If only one device is connected then this capability might be omitted
        */
        // capabilities.setCapability("udid", "ABCD123456789");

        /*
        It is recommended to set a full path to the app being tested.
        Appium for Android supports application .apk and .apks bundles.
        If this capability is not set then your test starts on Dashboard view.
        It is also possible to provide an URL where the app is located.
        */
        capabilities.setCapability("app", app.getAbsolutePath());

        /*
        By default Appium tries to autodetect the main application activity,
        but if you app's very first activity is not the main one then
        it is necessary to provide its name explicitly. Check
        https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/android/activity-startup.md
        for more details on activities selection topic.
        */
        // capabilities.setCapability("appActivity", "com.myapp.SplashActivity"));
        // capabilities.setCapability("appPackage", "com.myapp"));
        // capabilities.setCapability("appWaitActivity", "com.myapp.MainActivity"));

        /*
        Appium for Android supports multiple automation backends, where
        each of them has its own pros and cons. The default one is UIAutomator2.
        */
        capabilities.setCapability("automationName", "UIAutomator2");
        // capabilities.setCapability("automationName", "Espresso");

        /*
        There are much more capabilities and driver settings, that allow
        you to customize and tune your test to achieve the best automation
        experience. Read http://appium.io/docs/en/writing-running-appium/caps/
        and http://appium.io/docs/en/advanced-concepts/settings/
        for more details.

        Feel free to visit our forum at https://discuss.appium.io/
        if you have more questions.
        */

        driver = new AndroidDriver<WebElement>(getServiceUrl(), capabilities);
    }

    @AfterClass
    public void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }


    @Test()
    public void testSendKeys() {
        driver.startActivity(new Activity(PACKAGE, SEARCH_ACTIVITY));
        AndroidElement searchBoxEl = (AndroidElement) driver.findElementById("txt_query_prefill");
        searchBoxEl.sendKeys("Hello world!");
        AndroidElement onSearchRequestedBtn = (AndroidElement) driver.findElementById("btn_start_search");
        onSearchRequestedBtn.click();
        AndroidElement searchText = (AndroidElement) new WebDriverWait(driver, 30)
                .until(ExpectedConditions.visibilityOfElementLocated(By.id("android:id/search_src_text")));
        String searchTextValue = searchText.getText();
        Assert.assertEquals(searchTextValue, "Hello world!");
    }

    @Test
    public void testOpensAlert() throws InterruptedException {
        logger.info("start test opens alert!!!!");
        // Open the "Alert Dialog" activity of the android app
        driver.startActivity(new Activity(PACKAGE, ALERT_DIALOG_ACTIVITY));

        // Click button that opens a dialog
        AndroidElement openDialogButton = (AndroidElement) driver.findElementById("io.appium.android.apis:id/two_buttons");
        Thread.sleep(200);
        openDialogButton.click();
        logger.info("找到并点击io.appium.android.apis:id/two_buttons");
        // Check that the dialog is there
//        AndroidElement alertElement = (AndroidElement) driver.findElementById("android:id/alertTitle");
        AndroidElement alertElement = (AndroidElement) driver.findElementById("android:id/alertTitle");
        String alertText = alertElement.getText();
        logger.info("对话框的内容是：" + alertText);
        Assert.assertEquals(alertText, "Lorem ipsum dolor sit aie consectetur adipiscing Plloaso mako nuto siwuf cakso dodtos anr koop.");
        AndroidElement closeDialogButton = (AndroidElement) driver.findElementById("android:id/button1");
        // Close the dialog
        closeDialogButton.click();
    }
}
```

**Python**

```python
import pytest
import os
import textwrap
import copy

from appium import webdriver
from helpers import report_to_sauce, take_screenshot_and_logcat, ANDROID_BASE_CAPS, EXECUTOR


class TestAndroidBasicInteractions():
    PACKAGE = 'io.appium.android.apis'
    SEARCH_ACTIVITY = '.app.SearchInvoke'
    ALERT_DIALOG_ACTIVITY = '.app.AlertDialogSamples'

    @pytest.fixture(scope='function')
    def driver(self, request, device_logger):
        calling_request = request._pyfuncitem.name

        caps = copy.copy(ANDROID_BASE_CAPS)
        caps['name'] = calling_request
        caps['appActivity'] = self.SEARCH_ACTIVITY

        driver = webdriver.Remote(
            command_executor=EXECUTOR,
            desired_capabilities=caps
        )

        def fin():
            report_to_sauce(driver.session_id)
            take_screenshot_and_logcat(driver, device_logger, calling_request)
            driver.quit()

        request.addfinalizer(fin)

        driver.implicitly_wait(10)
        return driver

    def test_should_send_keys_to_search_box_and_then_check_the_value(self, driver):
        search_box_element = driver.find_element_by_id('txt_query_prefill')
        search_box_element.send_keys('Hello world!')

        on_search_requested_button = driver.find_element_by_id('btn_start_search')
        on_search_requested_button.click()

        search_text = driver.find_element_by_id('android:id/search_src_text')
        search_text_value = search_text.text

        assert 'Hello world!' == search_text_value

    def test_should_click_a_button_that_opens_an_alert_and_then_dismisses_it(self, driver):
        driver.start_activity(self.PACKAGE, self.ALERT_DIALOG_ACTIVITY)

        open_dialog_button = driver.find_element_by_id('io.appium.android.apis:id/two_buttons')
        open_dialog_button.click()

        alert_element = driver.find_element_by_id('android:id/alertTitle')
        alert_text = alert_element.text

        assert textwrap.dedent('''\
        Lorem ipsum dolor sit aie consectetur adipiscing
        Plloaso mako nuto siwuf cakso dodtos anr koop.''') == alert_text

        close_dialog_button = driver.find_element_by_id('android:id/button1')
        close_dialog_button.click()

```



### TestNG+Appium+SLF4J+APP断言Activity和包名

**Java**

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

**Python**

```python
import unittest
import os
import copy
import sys

from time import sleep

from appium import webdriver
from helpers import report_to_sauce, ANDROID_BASE_CAPS, EXECUTOR
from selenium.common.exceptions import WebDriverException

# Run standard unittest base.


class TestAndroidCreateSession(unittest.TestCase):
    def tearDown(self):
        report_to_sauce(self.driver.session_id)

    def test_should_create_and_destroy_android_session(self):
        caps = copy.copy(ANDROID_BASE_CAPS)
        caps['name'] = 'test_should_create_and_destroy_android_session'

        self.driver = webdriver.Remote(
            command_executor=EXECUTOR,
            desired_capabilities=caps
        )
        self.driver.implicitly_wait(10)

        # make sure the right package and activity were started
        self.assertEquals('io.appium.android.apis', self.driver.current_package)
        self.assertEquals('.ApiDemos', self.driver.current_activity)

        self.driver.quit()

        sleep(5)

        # should not be able to use the driver anymore
        with self.assertRaises(WebDriverException) as excinfo:
            self.driver.title
        self.assertTrue('has already finished' in str(excinfo.exception.msg))

```



### TestNG+Appium+SLF4J+浏览器页面

**Java**

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
        capabilities.setCapability("chromedriverExecutable",
                "B:\\IdeaProject\\Appium\\apps\\chromedriver.exe");
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

**Python**

```python
import unittest
import os
import copy
import sys

from time import sleep

from appium import webdriver
from helpers import report_to_sauce, ANDROID_BASE_CAPS, EXECUTOR
from selenium.common.exceptions import WebDriverException


class TestAndroidCreateWebSession(unittest.TestCase):
    def tearDown(self):
        report_to_sauce(self.driver.session_id)

    def test_should_create_and_destroy_android_web_session(self):
        caps = copy.copy(ANDROID_BASE_CAPS)
        caps['name'] = 'test_should_create_and_destroy_android_web_session'
        # can only specify one of `app` and `browserName`
        caps['browserName'] = 'Chrome'
        caps.pop('app')

        self.driver = webdriver.Remote(
            command_executor=EXECUTOR,
            desired_capabilities=caps
        )
        self.driver.implicitly_wait(10)

        self.driver.get('https://www.google.com')

        assert 'Google' == self.driver.title

        self.driver.quit()

        sleep(5)

        with self.assertRaises(WebDriverException) as excinfo:
            self.driver.title
        self.assertTrue('has already finished' in str(excinfo.exception.msg))

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

## IOS

### IOSSelectorsTest

**Java**

```java
import io.appium.java_client.ios.IOSDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.testng.Assert;
import org.testng.annotations.AfterSuite;
import org.testng.annotations.BeforeSuite;
import org.testng.annotations.Test;

import java.io.File;
import java.util.List;

public class IOSSelectorsTest extends BaseTest {
    private IOSDriver<WebElement> driver;

    @BeforeSuite
    public void setUp() throws Exception {
        File classpathRoot = new File(System.getProperty("user.dir"));
        File appDir = new File(classpathRoot, "../apps");
        File app = new File(appDir.getCanonicalPath(), "TestApp.app.zip");
        String deviceName = System.getenv("IOS_DEVICE_NAME");
        String platformVersion = System.getenv("IOS_PLATFORM_VERSION");
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("deviceName", deviceName == null ? "iPhone 6s" : deviceName);
        capabilities.setCapability("platformVersion", platformVersion == null ? "11.1" : platformVersion);
        capabilities.setCapability("app", app.getAbsolutePath());
        capabilities.setCapability("automationName", "XCUITest");
        driver = new IOSDriver<WebElement>(getServiceUrl(), capabilities);
    }

    @AfterSuite
    public void tearDown() {
        driver.quit();
    }


    @Test
    public void testFindElementsByAccessibilityID () {
        // This finds elements by "accessibility id", which in the case of IOS is the "name" attribute of the element
        List<WebElement> computeSumButtons = driver.findElementsByAccessibilityId("ComputeSumButton");
        Assert.assertEquals(computeSumButtons.size(), 1);
        computeSumButtons.get(0).click();
    }

    @Test
    public void testFindElementsByClassName () {
        // Find element by name
        List<WebElement> windowElements = driver.findElementsByClassName("XCUIElementTypeWindow");
        Assert.assertTrue(windowElements.size() > 1);
    };

    @Test
    public void testFindElementsByNSPredicateString () {
        // This is an IOS-specific selector strategy. See https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Predicates/Articles/pSyntax.html for reference
        List<WebElement> allVisibleElements = driver.findElementsByIosNsPredicate("visible = true");
        Assert.assertTrue(allVisibleElements.size() > 1);
    };

    @Test
    public void testFindElementsByClassChain () {
        // This is also an IOS-specific selector strategy. Similar to XPath. This is recommended over XPath.
        List<WebElement> windowElements = driver.findElementsByIosClassChain("XCUIElementTypeWindow[1]/*[2]");
        Assert.assertEquals(windowElements.size(), 1);
    };

    @Test
    public void testFindElementsByXPath () {
        // Can find source xml by calling "driver.source()"
        // Note that XPath is not recommended due to major performance issues
        List<WebElement> buttons = driver.findElementsByXPath("//XCUIElementTypeWindow//XCUIElementTypeButton");
        Assert.assertTrue(buttons.size() > 1);
    };
}

```

**Python**

```python
import pytest
import os
import copy

from appium import webdriver
from helpers import report_to_sauce, take_screenshot_and_syslog, IOS_BASE_CAPS, EXECUTOR


class TestIOSSelectors():

    @pytest.fixture(scope='function')
    def driver(self, request, device_logger):
        calling_request = request._pyfuncitem.name

        caps = copy.copy(IOS_BASE_CAPS)
        caps['name'] = calling_request

        driver = webdriver.Remote(
            command_executor=EXECUTOR,
            desired_capabilities=caps
        )

        def fin():
            report_to_sauce(driver.session_id)
            take_screenshot_and_syslog(driver, device_logger, calling_request)
            driver.quit()

        request.addfinalizer(fin)

        driver.implicitly_wait(10)
        return driver

    def test_should_find_elements_by_accessibility_id(self, driver):
        search_parameters_element = driver.find_elements_by_accessibility_id('ComputeSumButton')
        assert 1 == len(search_parameters_element)

    def test_should_find_elements_by_class_name(self, driver):
        window_elements = driver.find_elements_by_class_name('XCUIElementTypeWindow')
        assert 2 == len(window_elements)

    def test_should_find_elements_by_nspredicate(self, driver):
        all_visible_elements = driver.find_elements_by_ios_predicate('visible = 1')
        assert 24 <= len(all_visible_elements)

    def test_should_find_elements_by_class_chain(self, driver):
        window_element = driver.find_elements_by_ios_class_chain('XCUIElementTypeWindow[1]/*')
        assert 1 == len(window_element)

    def test_should_find_elements_by_xpath(self, driver):
        action_bar_container_elements = driver.find_elements_by_xpath('//XCUIElementTypeWindow//XCUIElementTypeButton')
        assert 7 <= len(action_bar_container_elements) <= 8

```



### IOSCreateSessionTest

**Java**

```java
import io.appium.java_client.ios.IOSDriver;
import io.appium.java_client.ios.IOSElement;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.testng.Assert;
import org.testng.annotations.AfterSuite;
import org.testng.annotations.BeforeSuite;
import org.testng.annotations.Test;

import java.io.File;

public class IOSCreateSessionTest extends BaseTest {
    private IOSDriver<WebElement> driver;

    @BeforeSuite
    public void setUp() throws Exception {
        File classpathRoot = new File(System.getProperty("user.dir"));
        File appDir = new File(classpathRoot, "../apps");
        File app = new File(appDir.getCanonicalPath(), "TestApp.app.zip");

        String deviceName = System.getenv("IOS_DEVICE_NAME");
        String platformVersion = System.getenv("IOS_PLATFORM_VERSION");
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("deviceName", deviceName == null ? "iPhone 6s" : deviceName);
        capabilities.setCapability("platformVersion", platformVersion == null ? "11.1" : platformVersion);
        capabilities.setCapability("app", app.getAbsolutePath());
        capabilities.setCapability("automationName", "XCUITest");
        driver = new IOSDriver<WebElement>(getServiceUrl(), capabilities);
    }

    @AfterSuite
    public void tearDown() {
        driver.quit();
    }

    @Test
    public void testCreateSession () {
        // Check that the XCUIElementTypeApplication was what we expect it to be
        IOSElement applicationElement = (IOSElement) driver.findElementByClassName("XCUIElementTypeApplication");
        String applicationName = applicationElement.getAttribute("name");
        Assert.assertEquals(applicationName, "TestApp");
    }
}

```

**Python**

```python
import unittest
import os
import copy
import sys

from time import sleep

from appium import webdriver
from helpers import report_to_sauce, IOS_BASE_CAPS, EXECUTOR
from selenium.common.exceptions import WebDriverException


# Run standard unittest base.
class TestIOSCreateSession(unittest.TestCase):
    def tearDown(self):
        report_to_sauce(self.driver.session_id)

    def test_should_create_and_destroy_ios_session(self):
        caps = copy.copy(IOS_BASE_CAPS)
        caps['name'] = self.id()

        self.driver = webdriver.Remote(
            command_executor=EXECUTOR,
            desired_capabilities=caps
        )
        self.driver.implicitly_wait(10)

        app_element = self.driver.find_element_by_class_name('XCUIElementTypeApplication')
        self.assertEquals('TestApp', app_element.get_attribute('name'))

        self.driver.quit()

        # pause a moment because Sauce Labs takes a bit to stop accepting requests
        sleep(5)

        with self.assertRaises(WebDriverException) as excinfo:
            self.driver.find_element_by_class_name('XCUIElementTypeApplication')
        self.assertTrue(
            'has already finished' in str(excinfo.exception.msg) or
            'Unhandled endpoint' in str(excinfo.exception.msg)
        )

```



### IOSCreateWebSessionTest

**Java**

```java
import io.appium.java_client.ios.IOSDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.testng.Assert;
import org.testng.annotations.AfterSuite;
import org.testng.annotations.BeforeSuite;
import org.testng.annotations.Test;

import java.io.IOException;

public class IOSCreateWebSessionTest extends BaseTest {
    private IOSDriver<WebElement> driver;

    @BeforeSuite
    public void setUp() throws IOException {
        String deviceName = System.getenv("IOS_DEVICE_NAME");
        String platformVersion = System.getenv("IOS_PLATFORM_VERSION");
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability("deviceName", deviceName == null ? "iPhone 6s" : deviceName);
        capabilities.setCapability("platformVersion", platformVersion == null ? "11.1" : platformVersion);
        capabilities.setCapability("browserName", "Safari");
        capabilities.setCapability("automationName", "XCUITest");
        driver = new IOSDriver<WebElement>(getServiceUrl(), capabilities);
    }

    @AfterSuite
    public void tearDown() {
        driver.quit();
    }

    @Test()
    public void testCreateSafariSession() {
        // Navigate to google.com
        driver.get("https://www.google.com");

        // Test that it was successful by checking the document title
        String pageTitle = driver.getTitle();
        Assert.assertEquals(pageTitle, "Google");

    }
}

```

**Python**

```python
import unittest
import os
import copy
import sys

from time import sleep

from appium import webdriver
from helpers import report_to_sauce, IOS_BASE_CAPS, EXECUTOR
from selenium.common.exceptions import WebDriverException


class TestIOSCreateWebSession(unittest.TestCase):
    def tearDown(self):
        report_to_sauce(self.driver.session_id)

    def test_should_create_and_destroy_ios_web_session(self):
        caps = copy.copy(IOS_BASE_CAPS)
        caps['name'] = self.id()
        # can only specify one of `app` and `browserName`
        caps['browserName'] = 'Safari'
        caps.pop('app')

        self.driver = webdriver.Remote(
            command_executor=EXECUTOR,
            desired_capabilities=caps
        )

        self.driver.get('https://www.google.com')
        assert 'Google' == self.driver.title

        self.driver.quit()

        sleep(5)

        with self.assertRaises(WebDriverException) as excinfo:
            self.driver.title
        self.assertTrue(
            'has already finished' in str(excinfo.exception.msg) or
            'Unhandled endpoint' in str(excinfo.exception.msg)
        )

```



### IOSBrowserSaucelabsTest

```java
import java.net.MalformedURLException;
import java.net.URL;

import org.openqa.selenium.remote.DesiredCapabilities;
import org.testng.Assert;
import org.testng.annotations.AfterTest;
import org.testng.annotations.BeforeTest;
import org.testng.annotations.Test;

import io.appium.java_client.ios.IOSDriver;
import io.appium.java_client.remote.MobileCapabilityType;

/**
 * IOS Browser Sauce Labs Test.
 */
public class IOSBrowserSaucelabsTest extends BaseTest {
    public static final String USERNAME = "YOUR_USERNAME";
    public static final String ACCESS_KEY = "YOUR_ACESS_KEY";
    public static final String URL = "https://"+USERNAME+":" + ACCESS_KEY + "@ondemand.saucelabs.com:443/wd/hub";
    public static IOSDriver<?> mobiledriver;

    @BeforeTest
    public void beforeTest( ) throws MalformedURLException {
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability(MobileCapabilityType.PLATFORM_VERSION, "11.2.2");
        capabilities.setCapability(MobileCapabilityType.PLATFORM_NAME,"iOS");
        capabilities.setCapability(MobileCapabilityType.AUTOMATION_NAME,"XCUITest");
        capabilities.setCapability(MobileCapabilityType.DEVICE_NAME, "iPhone Simulator");
        capabilities.setCapability(MobileCapabilityType.BROWSER_NAME, "Safari");
        capabilities.setCapability("newCommandTimeout", 2000);
        mobiledriver = new IOSDriver<>(new URL(URL), capabilities);

    }

    @AfterTest
    public void afterTest( ) {
        mobiledriver.quit();
    }

    @Test
    public static void launchBrowser(){
        mobiledriver.get("http://appium.io/");
        Assert.assertEquals(mobiledriver.getCurrentUrl(), "http://appium.io/", "URL Mismatch");
        Assert.assertEquals(mobiledriver.getTitle(), "Appium: Mobile App Automation Made Awesome.", "Title Mismatch");
    }
}
```

### IOSBasicInteractionsTest

**Java**

```java
import io.appium.java_client.MobileBy;
import io.appium.java_client.ios.IOSDriver;
import io.appium.java_client.ios.IOSElement;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.Assert;
import org.testng.annotations.*;

import java.io.File;
import java.io.IOException;

public class IOSBasicInteractionsTest extends BaseTest {
    private IOSDriver<WebElement> driver;

    @BeforeTest
    public void setUp() throws IOException {
        File classpathRoot = new File(System.getProperty("user.dir"));
        File appDir = new File(classpathRoot, "../apps");
        File app = new File(appDir.getCanonicalPath(), "TestApp.app.zip");

        String deviceName = System.getenv("IOS_DEVICE_NAME");
        String platformVersion = System.getenv("IOS_PLATFORM_VERSION");
        DesiredCapabilities capabilities = new DesiredCapabilities();
        /*
        'deviceName' capability only affects device selection if you run the test in a cloud
        environment or your run your test on a Simulator device. This combination of this value
        plus `platformVersion` capability value
        is used to select a proper Simulator if it already exists. Use `xcrun simctl list` command
        to list available Simulator devices.
        */
        capabilities.setCapability("deviceName", deviceName == null ? "iPhone 6s" : deviceName);

        /*
        udid value must be set if you run your test on a real iOS device.
        The udid of your real device could be retrieved from Xcode->Windows->Devices and Simulators
        dialog.
        Usually, it is not enough to simply provide udid itself in order to automate apps
        on real iOS devices. You must also verify the target device is included into
        your Apple developer profile and the WebDriverAgent is signed with a proper signature.
        Refer https://github.com/appium/appium-xcuitest-driver/blob/master/docs/real-device-config.md
        for more details.
        */
        // capabilities.setCapability("udid", "ABCD123456789");

        /*
        Platform version is required to be set. Only the major and minor version numbers have effect.
        Check `xcrun simctl list` to see which platform versions are available if the test is going
        to run on a Simulator.
        */
        capabilities.setCapability("platformVersion", platformVersion == null ? "11.1" : platformVersion);

        /*
        It is recommended to set a full path to the app being tested.
        Appium for iOS supports .app and .ipa application bundles.
        It is also possible to pass zipped .app packages (they will be extracted automatically).

        Make sure the application is built for correct architecture (either
        real device or Simulator) before running your tests, as there are not interchangeable.
        If the test is going to run on a real device then make sure your app
        is signed with correct development signature (as described in the above
        Real Device Config document)

        If this capability is not set then your test starts on Springboard view.
        It is also possible to provide an URL where the app is located.
        */
        capabilities.setCapability("app", app.getAbsolutePath());

        /*
        This is the only supported automation backend for iOS
        */
        capabilities.setCapability("automationName", "XCUITest");

        /*
        There are much more capabilities and driver settings, that allow
        you to customize and tune your test to achieve the best automation
        experience. Read http://appium.io/docs/en/writing-running-appium/caps/
        and http://appium.io/docs/en/advanced-concepts/settings/
        for more details.

        Feel free to visit our forum at https://discuss.appium.io/
        if you have more questions.
        */

        driver = new IOSDriver<WebElement>(getServiceUrl(), capabilities);
    }

    @AfterTest
    public void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }

    @Test
    public void testSendKeysToInput () {
        // Find TextField input element
        String textInputId = "TextField1";
        IOSElement textViewsEl = (IOSElement) new WebDriverWait(driver, 30)
                .until(ExpectedConditions.visibilityOfElementLocated(MobileBy.AccessibilityId(textInputId)));

        // Check that it doesn"t have a value
        String value = textViewsEl.getAttribute("value");
        Assert.assertEquals(value, null);

        // Send keys to that input
        textViewsEl.sendKeys("Hello World!");

        // Check that the input has new value
        value = textViewsEl.getAttribute("value");
        Assert.assertEquals(value, "Hello World!");
    }

    @Test
    public void testOpenAlert () {
        // Find Button element and click on it
        String buttonElementId = "show alert";
        IOSElement buttonElement = (IOSElement) new WebDriverWait(driver, 30)
                .until(ExpectedConditions.visibilityOfElementLocated(MobileBy.AccessibilityId(buttonElementId)));
        buttonElement.click();

        // Wait for the alert to show up
        String alertTitleId = "Cool title";
        IOSElement alertTitleElement = (IOSElement) new WebDriverWait(driver, 30)
                .until(ExpectedConditions.visibilityOfElementLocated(MobileBy.AccessibilityId(alertTitleId)));

        // Check the text
        String alertTitle = alertTitleElement.getText();
        Assert.assertEquals(alertTitle, "Cool title");

        // Dismiss the alert
        IOSElement okButtonElement = (IOSElement) new WebDriverWait(driver, 30)
                .until(ExpectedConditions.visibilityOfElementLocated(MobileBy.AccessibilityId("OK")));
        okButtonElement.click();
    }
}

```

**Python**

```python
import pytest
import os
import copy

from appium import webdriver
from helpers import report_to_sauce, take_screenshot_and_syslog, IOS_BASE_CAPS, EXECUTOR


class TestIOSBasicInteractions():

    @pytest.fixture(scope='function')
    def driver(self, request, device_logger):
        calling_request = request._pyfuncitem.name

        caps = copy.copy(IOS_BASE_CAPS)
        caps['name'] = calling_request

        driver = webdriver.Remote(
            command_executor=EXECUTOR,
            desired_capabilities=caps
        )

        def fin():
            report_to_sauce(driver.session_id)
            take_screenshot_and_syslog(driver, device_logger, calling_request)
            driver.quit()

        request.addfinalizer(fin)

        driver.implicitly_wait(10)
        return driver

    def test_should_send_keys_to_inputs(self, driver):
        text_field_el = driver.find_element_by_id('TextField1')
        assert text_field_el.get_attribute('value') is None
        text_field_el.send_keys('Hello World!')
        assert 'Hello World!' == text_field_el.get_attribute('value')

    def test_should_click_a_button_that_opens_an_alert(self, driver):
        button_element_id = 'show alert'
        button_element = driver.find_element_by_accessibility_id(button_element_id)
        button_element.click()

        alert_title_element_id = 'Cool title'
        alert_title_element = driver.find_element_by_accessibility_id(alert_title_element_id)
        alert_title = alert_title_element.get_attribute('name')
        assert 'Cool title' == alert_title

```



## Windows

### WindowsDesktopAppTest

```java
import org.openqa.selenium.remote.DesiredCapabilities;
import org.testng.Assert;
import org.testng.annotations.AfterTest;
import org.testng.annotations.BeforeTest;
import org.testng.annotations.Test;

import io.appium.java_client.windows.WindowsDriver;

public class WindowsDesktopAppTest extends BaseTest {
	
    public static WindowsDriver<?> driver;

    @BeforeTest
    public void setup( ) {
        DesiredCapabilities caps = new DesiredCapabilities();
        caps.setCapability("platformVersion", "10");
        caps.setCapability("platformName", "Windows");
        caps.setCapability("deviceName", "WindowsPC");
        caps.setCapability("app", "Microsoft.WindowsCalculator_8wekyb3d8bbwe!App");
        caps.setCapability("newCommandTimeout", 2000);
        driver = new WindowsDriver<>(getServiceUrl(), caps);
    }

    @AfterTest
    public void tearDown( ) {
        driver.quit();
    }

    @Test
    public void test() {
        driver.findElementByName("One").click();
        driver.findElementByName("Plus").click();
        driver.findElementByName("Two").click();
        driver.findElementByName("Equals").click();
        Assert.assertEquals(driver.findElementByAccessibilityId("CalculatorResults").getText(), "Display is 3");
    }   
}
```

