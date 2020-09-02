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

```
<dependency>
    <groupId>io.appium</groupId>
    <artifactId>java-client</artifactId>
    <version>7.3.0</version>
</dependency>
```

Gradle项目根目录build.gradle：

```
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

```
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

- 启动Android Emulator或ADB连接真实设备
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

### 获取App Activity和包名

```
adb shell dumpsys window | findstr mCurrentFocus
or
adb shell logcat|grep -i displayed
```

### 通过元素检查定位查找元素

```
findElementByAccessibilityId()
findElementsByAccessibilityId()
findElementByIosUIAutomation()
findElementsByIosUIAutomation()
findElementByAndroidUIAutomator()
findElementsByAndroidUIAutomator()
```