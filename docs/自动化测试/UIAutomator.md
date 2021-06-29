[github](https://github.com/Kwaiyu/UiAutomator)

[androidTest](https://developer.android.com/training/testing)

### 准备

java: jdk1.8, jre

Android SDK API 30

gradle 6.5,  gradle plugin 4.1.1

AVD: Pixel API 30

### 运行

1. 创建并运行 **Android Instrumented** 测试配置
   - 打开*运行*菜单 | *编辑配置*
   - 添加新的**Android Instrumented Tests**配置
   - 选择`app`模块
   - 连接设备或启动模拟器
   - 关闭动画。（在您的设备上，在 Settings->Developer options 下禁用以下 3 个设置：**“Window animation scale”、“Transition animation scale”和“Animator duration scale”**）
   - 运行新创建的配置
   - 应用程序将在设备/模拟器上启动，并自动执行一系列操作。
2. 创建并运行本地**Android Junit**测试配置
   - 打开运行菜单 | 编辑配置
   - 添加新的 **Android JUnit** 配置
   - 设置`Use classpath of module`为`app`
   - 设置`Class`为`ChangeTextBehaviorLocalTest`
   - 运行配置
   - 测试将在本地主机上运行

3. 使用自定义运行程序创建**Android APP**测试配置： 

   `androidx.test.runner.AndroidJUnitRunner`

   - 打开*运行*菜单 | *编辑配置*
   - 添加新的**Android APP**测试配置
   - 选择一个模块
   - 添加一个*特定的检测运行器*：`androidx.test.runner.AndroidJUnitRunner`


### 配置

build.gradle(project)

```java
// Top-level build file where you can add configuration options common to all sub-projects/modules.
buildscript {
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:4.1.1"

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        google()
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

build.gradle(app)

```java
plugins {
    id 'com.android.application'
}

android {
    compileSdkVersion 30
    buildToolsVersion "30.0.3"

    defaultConfig {
        applicationId "com.example.android.testing.uiautomator.BasicSample"
        minSdkVersion 23
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
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }

}

dependencies {
    implementation group: 'com.google.guava', name: 'guava', version: '30.0-android'

    implementation 'androidx.appcompat:appcompat:1.1.0'
    implementation 'com.google.android.material:material:1.1.0'
    testImplementation 'junit:junit:4.13.2'
    androidTestImplementation 'androidx.test.ext:junit:1.1.1'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'
    androidTestImplementation 'androidx.test.uiautomator:uiautomator:2.2.0'


}
```

AndroidMainfest.xml（main）

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example.uiautomator">

    <application
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >
        <activity
                android:name="com.example.uiautomator.MainActivity"
                android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name="com.example.uiautomator.ShowTextActivity"/>
    </application>

</manifest>
```

AndroidMainfest.xml（androidTest）

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:tools="http://schemas.android.com/tools"
      package="com.example.android.testing.uiautomator.BasicSample.test"
      android:versionCode="1"
      android:versionName="1.0">
    <uses-sdk android:minSdkVersion="18" android:targetSdkVersion="28" />

<!--    <instrumentation android:targetPackage="com.example.android.testing.uiautomator.BasicSample"-->
<!--                     android:name="androidx.test.runner.AndroidJUnitRunner"/>-->

    <application tools:replace="label" android:label="BasicSampleTest" />
</manifest>
```

