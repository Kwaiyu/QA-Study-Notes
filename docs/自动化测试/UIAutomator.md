[github](https://github.com/Kwaiyu/UiAutomator)

[androidTest](https://developer.android.com/training/testing)

[uiautomator api](https://developer.android.com/reference/androidx/test/uiautomator/package-summary)

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

### adb

path:	B:\Android\Sdk\platform-tools

USB连接 开发者选项 USB调试 动画关

通过 Wi-Fi 配置 ADB 连接的步骤：

- 将设备和电脑连接到同一个 Wi-Fi 网络
- 使用 USB 数据线将设备插入计算机以配置连接
- 在计算机命令行输入： `adb tcpip 5555`
- 在电脑命令行输入：`adb shell ip addr show wlan0`复制“inet”后的IP地址
- 在计算机命令行输入： `adb connect ip-address-of-device:5555`
- 断开 USB 电缆与设备的连接，并检查`adb devices`设备是否仍被检测到。

### uiautomatorviewer

`uiautomatorviewer` 工具提供了一个方便的 GUI，用于扫描和分析 Android 设备上当前显示的界面组件。您可以使用此工具来检查布局层次结构并查看设备前台显示的界面组件的属性。利用此信息，您可以使用 UI Automator 创建更精细的测试。例如，通过创建与特定可见属性匹配的界面选择器以做到这一点。

`uiautomatorviewer` 工具位于 `<android-sdk>/tools/bin` 目录中。

**访问设备状态**

UI Automator 测试框架提供了一个 [`UiDevice`](https://developer.android.com/reference/androidx/test/uiautomator/UiDevice) 类，用于在运行目标应用的设备上访问和执行操作。您可以调用其方法以访问设备属性，如当前屏幕方向或显示屏尺寸。[`UiDevice`](https://developer.android.com/reference/androidx/test/uiautomator/UiDevice) 类还可用于执行以下操作：

- 改变设备的旋转。
- 按硬件键，如“音量调高按钮”。
- 按返回、主屏幕或菜单按钮。
- 打开通知栏。
- 截取当前窗口的屏幕截图。

例如，如需模拟按下“主屏幕”按钮，请调用 `UiDevice.pressHome()` 方法。

**UI Automator API**

通过 UI Automator API，您可以编写可靠的测试，而无需了解目标应用的实现细节。您可以使用这些 API 在多个应用间捕获和操纵界面组件：

- [`UiCollection`](https://developer.android.com/reference/androidx/test/uiautomator/UiCollection)：枚举容器的界面元素，目的是为了计数，或者按可见文本或内容说明属性来定位子元素。
- [`UiObject`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject)：表示设备上可见的界面元素。
- [`UiScrollable`](https://developer.android.com/reference/androidx/test/uiautomator/UiScrollable)：支持搜索可滚动界面容器中的项目。
- [`UiSelector`](https://developer.android.com/reference/androidx/test/uiautomator/UiSelector)：表示对设备上的一个或多个目标界面元素的查询。
- [`Configurator`](https://developer.android.com/reference/androidx/test/uiautomator/Configurator)：可让您设置用于运行 UI Automator 测试的关键参数。

例如，以下代码展示了如何编写测试脚本以显示设备中的默认应用启动器：

```java
    device = UiDevice.getInstance(getInstrumentation());
    device.pressHome();

    // Bring up the default launcher by searching for a UI component
    // that matches the content description for the launcher button.
    UiObject allAppsButton = device
            .findObject(new UiSelector().description("Apps"));

    // Perform a click on the button to load the launcher.
    allAppsButton.clickAndWaitForNewWindow();
    
```

### 测单个应用界面

测试单个应用内的用户交互有助于确保用户在与应用交互时不会遇到意外结果或体验不佳的情况。如果您需要验证应用的界面是否正常运行，应养成创建界面测试的习惯。

由 [AndroidX Test](https://developer.android.com/training/testing) 提供的 Espresso 测试框架提供了一些 API，用于编写界面测试以模拟单个目标应用内的用户交互。Espresso 测试可以在搭载 Android 2.3.3（API 级别 10）及更高版本的设备上运行。使用 Espresso 的主要好处在于，它可以自动同步测试操作与您正在测试的应用的界面。Espresso 会检测主线程何时处于空闲状态，以便可以在适当的时间运行测试命令，从而提高测试的可靠性。此外，借助该功能，您不必在测试代码中添加任何计时解决方法，如 `Thread.sleep()`。

Espresso 测试框架是基于插桩的 API，可与 [`AndroidJUnitRunner`](https://developer.android.com/reference/androidx/test/runner/AndroidJUnitRunner) 测试运行程序一起使用。

**设置 Espresso** 

在使用 Espresso 构建界面测试之前，请务必设置对 Espresso 库的依赖项引用：

```groovy
    dependencies {
        androidTestImplementation 'androidx.test.espresso:espresso-core:3.1.0'
    }
    
```

在测试设备上关闭动画 - 如果让系统动画在测试设备上保持开启状态，可能会导致意外结果或导致测试失败。通过以下方式关闭动画：在“设置”中打开“开发者选项”，然后关闭以下所有选项：

- **窗口动画缩放**
- **过渡动画缩放**
- **Animator 时长缩放**

如果您希望设置项目以使用除核心 API 所提供功能之外的 Espresso 功能，请参阅 [Espresso](https://developer.android.com/training/testing/espresso) 专用指南。

**创建 Espresso 测试类** 

如需创建 Espresso 测试，请遵循以下编程模型：

1. 通过调用 [`onView()`](https://developer.android.com/reference/androidx/test/espresso/Espresso#onView(org.hamcrest.Matcher)) 方法或 `AdapterView` 控件的 [`onData()`](https://developer.android.com/reference/androidx/test/espresso/Espresso#onData(org.hamcrest.Matcher)) 方法，在 `Activity` 中找到要测试的界面组件（例如，应用中的登录按钮）。
2. 通过调用 [`ViewInteraction.perform()`](https://developer.android.com/reference/androidx/test/espresso/ViewInteraction#perform(androidx.test.espresso.ViewAction...)) 或 [`DataInteraction.perform()`](https://developer.android.com/reference/androidx/test/espresso/DataInteraction#perform(androidx.test.espresso.ViewAction...))方法并传入用户操作（例如，点击登录按钮），模拟要在该界面组件上执行的特定用户交互。如需对同一界面组件上的多项操作进行排序，请在方法参数中使用逗号分隔列表将它们链接起来。
3. 根据需要重复上述步骤，以模拟目标应用中跨多个 Activity 的用户流。
4. 执行这些用户交互后，使用 [`ViewAssertions`](https://developer.android.com/reference/androidx/test/espresso/assertion/ViewAssertions) 方法检查界面是否反映了预期的状态或行为。

下面几部分更详细地介绍了这些步骤。

以下代码段展示了测试类如何调用此基本工作流程：

```java
    onView(withId(R.id.my_view))            // withId(R.id.my_view) is a ViewMatcher
            .perform(click())               // click() is a ViewAction
            .check(matches(isDisplayed())); // matches(isDisplayed()) is a ViewAssertion
    
```

**将 Espresso 与 ActivityTestRule 一起使用** 

下文介绍如何创建新的 JUnit 4 型 Espresso 测试，并使用 [`ActivityTestRule`](https://developer.android.com/reference/androidx/test/rule/ActivityTestRule) 减少您需要编写的样板代码量。通过使用 [`ActivityTestRule`](https://developer.android.com/reference/androidx/test/rule/ActivityTestRule)，测试框架会在带有 `@Test` 注释的每个测试方法运行之前以及带有 `@Before` 注释的所有方法运行之前启动被测 Activity。该框架将在测试完成并且带有 `@After` 注释的所有方法都运行后关闭该 Activity。

```java
    package com.example.android.testing.espresso.BasicSample;

    import org.junit.Before;
    import org.junit.Rule;
    import org.junit.Test;
    import org.junit.runner.RunWith;

    import androidx.test.rule.ActivityTestRule;
    import androidx.test.runner.AndroidJUnit4;

    @RunWith(AndroidJUnit4.class)
    @LargeTest
    public class ChangeTextBehaviorTest {

        private String stringToBetyped;

        @Rule
        public ActivityTestRule<MainActivity> activityRule
                = new ActivityTestRule<>(MainActivity.class);

        @Before
        public void initValidString() {
            // Specify a valid string.
            stringToBetyped = "Espresso";
        }

        @Test
        public void changeText_sameActivity() {
            // Type text and then press the button.
            onView(withId(R.id.editTextUserInput))
                    .perform(typeText(stringToBetyped), closeSoftKeyboard());
            onView(withId(R.id.changeTextBt)).perform(click());

            // Check that the text was changed.
            onView(withId(R.id.textToBeChanged))
                    .check(matches(withText(stringToBetyped)));
        }
    }
    
```

**访问界面组件** 

您必须先指定界面组件或视图，然后 Espresso 才能与被测应用进行交互。Espresso 支持使用 [Hamcrest 匹配器](http://hamcrest.org/)指定应用中的视图和适配器。

如需查看视图，请调用 [`onView()`](https://developer.android.com/reference/androidx/test/espresso/Espresso#onView(org.hamcrest.Matcher)) 方法并传入用于指定目标视图的视图匹配器。[指定视图匹配器](https://developer.android.com/training/testing/ui-testing/espresso-testing#specifying-view-matcher)部分对此进行了更详细的说明。[`onView()`](https://developer.android.com/reference/androidx/test/espresso/Espresso#onView(org.hamcrest.Matcher)) 方法将返回一个 [`ViewInteraction`](https://developer.android.com/reference/androidx/test/espresso/ViewInteraction) 对象，该对象允许测试与视图进行交互。但是，如果希望在 `RecyclerView` 布局中查找视图，调用 [`onView()`](https://developer.android.com/reference/androidx/test/espresso/Espresso#onView(org.hamcrest.Matcher)) 方法可能不起作用。在这种情况下，请按照[在 AdapterView 中查找视图](https://developer.android.com/training/testing/ui-testing/espresso-testing#locating-adpeterview-view)中的说明进行操作。

**注意**：[`onView()`](https://developer.android.com/reference/androidx/test/espresso/Espresso#onView(org.hamcrest.Matcher)) 方法不检查您指定的视图是否有效。相反，Espresso 根据提供的匹配器仅搜索当前视图层次结构。如果未找到匹配项，该方法会抛出 [`NoMatchingViewException`](https://developer.android.com/reference/androidx/test/espresso/NoMatchingViewException)。

以下代码段展示了如何编写一个先访问 `EditText` 字段，再输入文本字符串，接着关闭虚拟键盘，然后执行按钮点击操作的测试。

```java
    public void testChangeText_sameActivity() {
        // Type text and then press the button.
        onView(withId(R.id.editTextUserInput))
                .perform(typeText(STRING_TO_BE_TYPED), closeSoftKeyboard());
        onView(withId(R.id.changeTextButton)).perform(click());

        // Check that the text was changed.
        ...
    }
    
```

**指定视图匹配器** 

您可以使用以下方法指定视图匹配器：

- 调用`ViewMatchers`

  类中的方法。例如，如需通过查找视图显示的文本字符串查找视图，您可以调用以下方法：

  ```java
      onView(withText("Sign-in"));
  ```

  同样，您可以调用 [`withId()`](https://developer.android.com/reference/androidx/test/espresso/matcher/ViewMatchers#withId(int)) 并提供视图的资源 ID (`R.id`)，如以下示例所示：

  ```java
      onView(withId(R.id.button_signin));
  ```

  不能保证 Android 资源 ID 是唯一的。如果测试尝试匹配由多个视图使用的某个资源 ID，Espresso 会抛出 [`AmbiguousViewMatcherException`](https://developer.android.com/reference/androidx/test/espresso/AmbiguousViewMatcherException)。

- 使用`Matchers`类。您可以使用``allOf()``方法组合多个匹配器，例如``containsString()``和``instanceOf()``此方法可让您更精细地过滤匹配结果，如以下示例所示：

  ```java
      onView(allOf(withId(R.id.button_signin), withText("Sign-in")));
  ```

  您可以使用 `not` 关键字过滤与匹配器不对应的视图，如以下示例所示：

  ```java
      onView(allOf(withId(R.id.button_signin), not(withText("Sign-out"))));
  ```

  如需在测试中使用这些方法，请导入 `org.hamcrest.Matchers` 软件包。如需详细了解 Hamcrest 匹配，请访问 [Hamcrest 网站](http://hamcrest.org/)。

  如需提高 Espresso 测试的性能，请指定查找目标视图所需的最少匹配信息。例如，如果某个视图可通过其描述性文本进行唯一标识，您无需指定该视图也可从 `TextView` 实例分配。

**在 AdapterView 中查找视图** 

在 `AdapterView` 微件中，视图会在运行时由子视图动态填充。如果您要测试的目标视图位于 `AdapterView`（例如 `ListView`、`GridView` 或 `Spinner`）内，则 [`onView()`](https://developer.android.com/reference/androidx/test/espresso/Espresso#onView(org.hamcrest.Matcher)) 方法可能不起作用，因为只能将一部分视图加载到当前视图层次结构中。

应改为调用 [`onData()`](https://developer.android.com/reference/androidx/test/espresso/Espresso#onData(org.hamcrest.Matcher))方法获取 [`DataInteraction`](https://developer.android.com/reference/androidx/test/espresso/DataInteraction) 对象，以访问目标视图元素。Espresso 负责将目标视图元素加载到当前视图层次结构中。Espresso 还负责滚动到目标元素，并将该元素置于焦点上。

**注意**：[`onData()`](https://developer.android.com/reference/androidx/test/espresso/Espresso#onData(org.hamcrest.Matcher)) 方法不检查您指定的项是否与视图对应。Espresso 仅搜索当前视图层次结构。如果未找到匹配项，该方法会抛出 [`NoMatchingViewException`](https://developer.android.com/reference/androidx/test/espresso/NoMatchingViewException)。

以下代码段展示了如何结合使用 [`onData()`](https://developer.android.com/reference/androidx/test/espresso/Espresso#onData(org.hamcrest.Matcher)) 方法和 Hamcrest 匹配搜索列表中包含给定字符串的特定行。在本例中，`LongListActivity` 类包含通过 `SimpleAdapter` 公开的字符串列表。

```java
    onData(allOf(is(instanceOf(Map.class)),
            hasEntry(equalTo(LongListActivity.ROW_TEXT), is("test input"))));
    
```

**执行操作**

调用 [`ViewInteraction.perform()`](https://developer.android.com/reference/androidx/test/espresso/ViewInteraction#perform(androidx.test.espresso.ViewAction...)) 或 [`DataInteraction.perform()`](https://developer.android.com/reference/androidx/test/espresso/DataInteraction#perform(androidx.test.espresso.ViewAction...)) 方法，以模拟界面组件上的用户交互。您必须将一个或多个 [`ViewAction`](https://developer.android.com/reference/androidx/test/espresso/ViewAction) 对象作为参数传入。Espresso 将按照给定的顺序依次触发每项操作，并在主线程中执行这些操作。

[`ViewActions`](https://developer.android.com/reference/androidx/test/espresso/action/ViewActions) 类提供了用于指定常见操作的辅助程序方法的列表。您可以将这些方法用作方便的快捷方式，而不是创建和配置单个 [`ViewAction`](https://developer.android.com/reference/androidx/test/espresso/ViewAction) 对象。您可以指定以下操作：

- [`ViewActions.click()`](https://developer.android.com/reference/androidx/test/espresso/action/ViewActions#click())：点击视图。
- [`ViewActions.typeText()`](https://developer.android.com/reference/androidx/test/espresso/action/ViewActions#typeText(java.lang.String))：点击视图并输入指定的字符串。
- [`ViewActions.scrollTo()`](https://developer.android.com/reference/androidx/test/espresso/action/ViewActions#scrollTo())：滚动到视图。目标视图必须是由 `ScrollView` 派生的子类，并且其 [`android:visibility`](https://developer.android.com/reference/android/view/View.html#attr_android:visibility) 属性的值必须为 `VISIBLE`。对于扩展 `AdapterView` 的视图（例如 `ListView`），[`onData()`](https://developer.android.com/reference/androidx/test/espresso/Espresso#onData(org.hamcrest.Matcher)) 方法将负责为您滚动。
- [`ViewActions.pressKey()`](https://developer.android.com/reference/androidx/test/espresso/action/ViewActions#pressKey(int))：使用指定的键码执行按键操作。
- [`ViewActions.clearText()`](https://developer.android.com/reference/androidx/test/espresso/action/ViewActions#clearText())：清除目标视图中的文本。

如果目标视图位于 `ScrollView` 内，请先执行 [`ViewActions.scrollTo()`](https://developer.android.com/reference/androidx/test/espresso/action/ViewActions#scrollTo()) 操作以在屏幕中显示该视图，然后再继续执行其他操作。如果已显示该视图，则 [`ViewActions.scrollTo()`](https://developer.android.com/reference/androidx/test/espresso/action/ViewActions#scrollTo()) 操作将不起作用。

**使用 Espresso Intent 单独测试 Activity** 

[Espresso Intent](https://android.github.io/android-test/docs/espresso/intents/index.html) 支持对应用发出的 intent 进行验证和打桩。使用 Espresso Intent，您可以通过以下方式单独测试应用、Activity 或服务：拦截传出 intent，对结果进行打桩，然后将其发送回被测组件。

如需开始使用 Espresso Intent 进行测试，您需要将以下代码行添加到应用的 build.gradle 文件中：

```groovy
    dependencies {
      androidTestImplementation 'androidx.test.espresso:espresso-intents:3.1.0'
    }
    
```

如需测试 intent，您需要创建 [IntentsTestRule](https://developer.android.com/reference/androidx/test/espresso/intent/rule/IntentsTestRule) 类（与 [ActivityTestRule](https://developer.android.com/reference/androidx/test/rule/ActivityTestRule) 类非常相似）的实例。[IntentsTestRule](https://developer.android.com/reference/androidx/test/espresso/intent/rule/IntentsTestRule) 类会在每次测试前初始化 Espresso Intent，终止托管 Activity，并在每次测试后释放 Espresso Intent。

以下代码段中显示的测试类提供了显式 intent 的简单测试。它会测试在[构建首个应用](https://developer.android.com/training/basics/firstapp)教程中创建的 Activity 和 intent。

```java
    @Large
    @RunWith(AndroidJUnit4.class)
    public class SimpleIntentTest {

        private static final String MESSAGE = "This is a test";
        private static final String PACKAGE_NAME = "com.example.myfirstapp";

        /* Instantiate an IntentsTestRule object. */
        @Rule
        public IntentsTestRule<MainActivity> intentsRule =
                new IntentsTestRule<>(MainActivity.class);

        @Test
        public void verifyMessageSentToMessageActivity() {

            // Types a message into a EditText element.
            onView(withId(R.id.edit_message))
                    .perform(typeText(MESSAGE), closeSoftKeyboard());

            // Clicks a button to send the message to another
            // activity through an explicit intent.
            onView(withId(R.id.send_message)).perform(click());

            // Verifies that the DisplayMessageActivity received an intent
            // with the correct package name and message.
            intended(allOf(
                    hasComponent(hasShortClassName(".DisplayMessageActivity")),
                    toPackage(PACKAGE_NAME),
                    hasExtra(MainActivity.EXTRA_MESSAGE, MESSAGE)));

        }
    }
    
```

如需详细了解 Espresso Intent，请参阅 [AndroidX Test 网站上的 Espresso Intent 文档](https://android.github.io/android-test/docs/espresso/intents/index.html)。您还可以下载 [IntentsBasicSample](https://github.com/android/testing-samples/tree/master/ui/espresso/IntentsBasicSample) 和 [IntentsAdvancedSample](https://github.com/android/testing-samples/tree/master/ui/espresso/IntentsAdvancedSample) 代码示例。

**使用 Espresso Web 测试 WebView** 

使用 Espresso Web，您可以测试包含在 Activity 中的 `WebView` 组件。它使用 [WebDriver API](http://docs.seleniumhq.org/docs/03_webdriver.jsp) 检查和控制 `WebView` 的行为。

如需开始使用 Espresso Web 进行测试，您需要将以下代码行添加到应用的 build.gradle 文件中：

```groovy
    dependencies {
      androidTestImplementation 'androidx.test.espresso:espresso-web:3.1.0'
    }
    
```

在使用 Espresso Web 创建测试的过程中，当您实例化 [ActivityTestRule](https://developer.android.com/reference/androidx/test/rule/ActivityTestRule) 对象以测试 Activity 时，需要在 `WebView` 上启用 JavaScript。在测试中，您可以选择 `WebView` 中显示的 HTML 元素并模拟用户交互，例如在文本框中输入文本，然后点击某个按钮。完成这些操作后，您可以验证网页上的结果是否与预期结果一致。

以下代码段中，该类测试被测 Activity 中 ID 值为“webview”的 `WebView` 组件。`typeTextInInput_clickButton_SubmitsForm()` 测试先选择网页上的一个 `<input>` 元素，再输入一些文本，然后检查出现在另一个元素中的文本。

```java
    @LargeTest
    @RunWith(AndroidJUnit4.class)
    public class WebViewActivityTest {

        private static final String MACCHIATO = "Macchiato";
        private static final String DOPPIO = "Doppio";

        @Rule
        public ActivityTestRule<WebViewActivity> activityRule =
            new ActivityTestRule<WebViewActivity>(WebViewActivity.class,
                false /* Initial touch mode */, false /*  launch activity */) {

            @Override
            protected void afterActivityLaunched() {
                // Enable JavaScript.
                onWebView().forceJavascriptEnabled();
            }
        }

        @Test
        public void typeTextInInput_clickButton_SubmitsForm() {
           // Lazily launch the Activity with a custom start Intent per test
           activityRule.launchActivity(withWebFormIntent());

           // Selects the WebView in your layout.
           // If you have multiple WebViews you can also use a
           // matcher to select a given WebView, onWebView(withId(R.id.web_view)).
           onWebView()
               // Find the input element by ID
               .withElement(findElement(Locator.ID, "text_input"))
               // Clear previous input
               .perform(clearElement())
               // Enter text into the input element
               .perform(DriverAtoms.webKeys(MACCHIATO))
               // Find the submit button
               .withElement(findElement(Locator.ID, "submitBtn"))
               // Simulate a click via JavaScript
               .perform(webClick())
               // Find the response element by ID
               .withElement(findElement(Locator.ID, "response"))
               // Verify that the response page contains the entered text
               .check(webMatches(getText(), containsString(MACCHIATO)));
        }
    }
    
```

如需详细了解 Espresso Web，请参阅 [AndroidX Test 网站上的 Espresso Web 文档](https://android.github.io/android-test/docs/espresso/web/index.html)。您还可以将此代码段作为 [Espresso Web 代码示例](https://github.com/android/testing-samples/tree/master/ui/espresso/WebBasicSample)的一部分下载。

**验证结果** 

调用 [`ViewInteraction.check()`](https://developer.android.com/reference/androidx/test/espresso/ViewInteraction#check(androidx.test.espresso.ViewAssertion)) 或 [`DataInteraction.check()`](https://developer.android.com/reference/androidx/test/espresso/DataInteraction#check(androidx.test.espresso.ViewAssertion)) 方法以断言界面中的视图与某种预期状态匹配。您必须将 [`ViewAssertion`](https://developer.android.com/reference/androidx/test/espresso/ViewAssertion) 对象作为参数传入。如果断言失败，Espresso 会抛出 `AssertionFailedError`。

[`ViewAssertions`](https://developer.android.com/reference/androidx/test/espresso/assertion/ViewAssertions) 类提供了用于指定常见断言的辅助程序方法的列表。可以使用的断言包括：

- [`doesNotExist`](https://developer.android.com/reference/androidx/test/espresso/assertion/ViewAssertions#doesNotExist())：断言当前视图层次结构中没有符合指定条件的视图。
- [`matches`](https://developer.android.com/reference/androidx/test/espresso/assertion/ViewAssertions#matches(org.hamcrest.Matcher))：断言当前视图层次结构中存在指定的视图，并且其状态与某个给定的 Hamcrst 匹配器匹配。
- [`selectedDescendentsMatch`](https://developer.android.com/reference/androidx/test/espresso/assertion/ViewAssertions#selectedDescendantsMatch(org.hamcrest.Matcher, org.hamcrest.Matcher))：断言存在父视图的指定子视图，并且其状态与某个给定的 Hamcrst 匹配器匹配。

以下代码段展示了如何检查界面中显示的文本与先前在 `EditText` 字段中输入的文本是否具有相同的值。

```java
    public void testChangeText_sameActivity() {
        // Type text and then press the button.
        ...

        // Check that the text was changed.
        onView(withId(R.id.textToBeChanged))
                .check(matches(withText(STRING_TO_BE_TYPED)));
    }
    
```

**在设备或模拟器上运行 Espresso 测试：**

您可以通过 [Android Studio](https://developer.android.com/studio) 或命令行运行 Espresso 测试。请务必在项目中将 [`AndroidJUnitRunner`](https://developer.android.com/reference/androidx/test/runner/AndroidJUnitRunner) 指定为默认插桩测试运行程序。

如需运行 Espresso 测试，请按照[测试入门](https://developer.android.com/training/testing/start#run-instrumented-tests)中所述的插桩测试运行步骤进行操作。

您还应阅读 [Espresso API 参考文档](https://developer.android.com/reference/androidx/test/package-summary)。

### 测多个应用界面

通过涉及多个应用中的用户交互的界面测试，您可以验证当用户流跨入其他应用或系统界面时，您的应用是否能够正常运行。短信应用就是此类用户流的一个例子，该应用先让用户输入短信，再启动 Android 联系人选择器，以便用户可以选择短信的收件人，然后将控制权返还给原来的应用，以便用户提交短信。

本课介绍如何使用 [AndroidX Test](https://developer.android.com/training/testing) 提供的 UI Automator 测试框架来编写此类界面测试。通过 UI Automator API，您可以与设备上的可见元素进行交互，而不管焦点在哪个 `Activity` 上。您的测试可以使用方便的描述符（如显示在相应组件中的文本或其内容描述）来查找界面组件。UI Automator 测试可以在搭载 Android 4.3（API 级别 18）或更高版本的设备上运行。

UI Automator 测试框架是基于插桩的 API，可与 [`AndroidJUnitRunner`](https://developer.android.com/reference/androidx/test/runner/AndroidJUnitRunner) 测试运行程序一起使用。

您还应阅读 [UI Automator API 参考文档](https://developer.android.com/reference/androidx/test/uiautomator/package-summary)，并尝试 [UI Automator 代码示例](https://github.com/android/testing-samples)。

**设置 UI Automator**

在使用 UI Automator 构建界面测试之前，请务必配置测试源代码位置和项目依赖项，如[针对 AndroidX Test 设置项目](https://developer.android.com/training/testing/set-up-project)中所述。

在 Android 应用模块的 `build.gradle` 文件中，您必须设置对 UI Automator 库的依赖项引用：

```groovy
    dependencies {
        ...
        androidTestImplementation 'androidx.test.uiautomator:uiautomator:2.2.0'
    }
    
```

要优化 UI Automator 测试，您应先检查目标应用的界面组件并确保它们可访问。这些优化提示将在接下来的两部分中进行介绍。

**检查设备上的界面**

在设计测试之前，请先检查设备上可见的界面组件。要确保 UI Automator 测试可以访问这些组件，请检查这些组件是否具有可见文本标签和/或 [`android:contentDescription`](https://developer.android.com/reference/android/view/View.html#attr_android:contentDescription) 值。

`uiautomatorviewer` 工具提供了一个方便的可视界面，用于检查布局层次结构以及查看在设备前台显示的界面组件的属性。利用此信息，您可以使用 UI Automator 创建更精细的测试。例如，您可以创建与特定可见属性匹配的界面选择器。

要启动 `uiautomatorviewer` 工具，请执行以下操作：

1. 在实体设备上启动目标应用。

2. 将设备连接到开发机器。

3. 打开终端窗口并导航至 `<android-sdk>/tools/` 目录。

4. 使用以下命令运行该工具：

   ```
   $ uiautomatorviewer
   ```

如需查看应用的界面属性，请执行以下操作：

1. 在 `uiautomatorviewer` 界面中，点击 **Device Screenshot** 按钮。
2. 将鼠标悬停在左侧面板中的快照上，以查看 `uiautomatorviewer` 工具识别的界面组件。右下方的面板中列出了属性，右上方的面板中列出了布局层次结构。
3. （可选）点击 **Toggle NAF Nodes** 按钮，以查看 UI Automator 无法访问的界面组件。对于这些组件，系统显示的相关信息可能很有限。

如需了解 Android 提供的常见类型的界面组件，请参阅[界面](https://developer.android.com/guide/topics/ui)。

**确保 Activity 可访问**

UI Automator 测试框架在已实现 Android 无障碍功能的应用上效果更好。当您使用类型为 `View` 或 SDK 中的 `View` 的子类的界面元素时，无需实现无障碍功能支持，因为这些类已经为您实现了这项支持。

不过，某些应用会使用自定义界面元素来提供更丰富的用户体验。此类元素不会提供自动无障碍功能支持。如果您的应用包含不是 SDK 中的 `View` 的子类的实例，请务必向这些元素添加无障碍功能，具体操作步骤如下：

1. 创建一个扩展 `ExploreByTouchHelper` 的具体类。
2. 通过调用 `setAccessibilityDelegate()`，将新类的实例与特定自定义界面元素相关联。

如需获得有关向自定义视图元素添加无障碍功能的其他指导，请参阅[构建无障碍自定义视图](https://developer.android.com/guide/topics/ui/accessibility/custom-views)。如需详细了解 Android 平台上无障碍功能的常规最佳做法，请参阅[改进应用的无障碍功能](https://developer.android.com/guide/topics/ui/accessibility/apps)。

**创建 UI Automator 测试类**

UI Automator 测试类的编写方式应与 JUnit 4 测试类相同。如需详细了解如何创建 JUnit 4 测试类以及如何使用 JUnit 4 断言和注释，请参阅[创建插桩单元测试类](https://developer.android.com/training/testing/unit-testing/instrumented-unit-tests#build)。

在测试类定义的开头添加 `@RunWith(AndroidJUnit4.class)` 注释。您还需要将 AndroidX Test 中提供的 [`AndroidJUnitRunner`](https://developer.android.com/reference/androidx/test/runner/AndroidJUnitRunner) 类指定为默认测试运行程序。[在设备或模拟器上运行 UI Automator 测试](https://developer.android.com/training/testing/ui-testing/uiautomator-testing#run)部分对此步骤进行了更详细的说明。

在 UI Automator 测试类中实现以下编程模型：

1. 通过调用 [`getInstance()`](https://developer.android.com/reference/androidx/test/uiautomator/UiDevice#getInstance(android.app.Instrumentation)) 方法并将 `Instrumentation` 对象作为参数传递给该方法，获取 [`UiDevice`](https://developer.android.com/reference/androidx/test/uiautomator/UiDevice) 对象以访问要测试的设备。
2. 通过调用 [`findObject()`](https://developer.android.com/reference/androidx/test/uiautomator/UiDevice#findObject(androidx.test.uiautomator.UiSelector)) 方法，获取 [`UiObject`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject) 对象以访问设备上显示的界面组件（例如，前台的当前视图）。
3. 通过调用 [`UiObject`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject) 方法，模拟需要在该界面组件上执行的特定用户交互；例如，调用 [`performMultiPointerGesture()`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject#performMultiPointerGesture(android.view.MotionEvent.PointerCoords[]...)) 以模拟多点触控手势，以及调用 [`setText()`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject#setText(java.lang.String)) 以修改文本字段。您可以根据需要反复调用第 2 步和第 3 步中的 API，以测试涉及多个界面组件或用户操作序列的更复杂的用户交互。
4. 执行这些用户交互后，检查界面是否反映了预期的状态或行为。

下面几部分更详细地介绍了这些步骤。

**访问界面组件**

[`UiDevice`](https://developer.android.com/reference/androidx/test/uiautomator/UiDevice) 对象是您访问和操纵设备状态的主要方式。在测试中，您可以调用 [`UiDevice`](https://developer.android.com/reference/androidx/test/uiautomator/UiDevice) 方法检查各种属性的状态，如当前屏幕方向或显示屏尺寸。您的测试可以使用 [`UiDevice`](https://developer.android.com/reference/androidx/test/uiautomator/UiDevice) 对象执行设备级操作，如强制设备进行特定旋转、按方向键硬件按钮，以及按主屏幕和菜单按钮。

最好从设备的主屏幕开始测试。在主屏幕（或您在设备中选择的其他某个起始位置）上，您可以调用 UI Automator API 提供的方法，以选择特定的界面元素并与之交互。

以下代码段展示了您的测试如何获取 [`UiDevice`](https://developer.android.com/reference/androidx/test/uiautomator/UiDevice) 实例并模拟按主屏幕按钮的操作：

```java
    import org.junit.Before;
    import androidx.test.runner.AndroidJUnit4;
    import androidx.test.uiautomator.UiDevice;
    import androidx.test.uiautomator.By;
    import androidx.test.uiautomator.Until;
    ...

    @RunWith(AndroidJUnit4.class)
    @SdkSuppress(minSdkVersion = 18)
    public class ChangeTextBehaviorTest {

        private static final String BASIC_SAMPLE_PACKAGE
                = "com.example.android.testing.uiautomator.BasicSample";
        private static final int LAUNCH_TIMEOUT = 5000;
        private static final String STRING_TO_BE_TYPED = "UiAutomator";
        private UiDevice device;

        @Before
        public void startMainActivityFromHomeScreen() {
            // Initialize UiDevice instance
            device = UiDevice.getInstance(InstrumentationRegistry.getInstrumentation());

            // Start from the home screen
            device.pressHome();

            // Wait for launcher
            final String launcherPackage = device.getLauncherPackageName();
            assertThat(launcherPackage, notNullValue());
            device.wait(Until.hasObject(By.pkg(launcherPackage).depth(0)),
                    LAUNCH_TIMEOUT);

            // Launch the app
            Context context = ApplicationProvider.getApplicationContext();
            final Intent intent = context.getPackageManager()
                    .getLaunchIntentForPackage(BASIC_SAMPLE_PACKAGE);
            // Clear out any previous instances
            intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
            context.startActivity(intent);

            // Wait for the app to appear
            device.wait(Until.hasObject(By.pkg(BASIC_SAMPLE_PACKAGE).depth(0)),
                    LAUNCH_TIMEOUT);
        }
    }
    
```

在本例中，`@SdkSuppress(minSdkVersion = 18)` 语句有助于确保测试只能在搭载 Android 4.3（API 级别 18）或更高版本的设备上运行（根据 Android Automator 框架的要求）。

使用 [`findObject()`](https://developer.android.com/reference/androidx/test/uiautomator/UiDevice#findObject(androidx.test.uiautomator.UiSelector)) 方法检索 [`UiObject`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject)，它表示符合给定选择器条件的视图。您可以根据需要重复使用已在应用测试的其他部分中创建的 [`UiObject`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject) 实例。请注意，每当您的测试使用 [`UiObject`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject) 实例以点击界面元素或查询属性时，UI Automator 测试框架都会在当前显示内容中搜索匹配项。

以下代码段展示了您的测试如何构建表示应用中的“取消”按钮和“确定”按钮的 [`UiObject`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject) 实例。

```java
    UiObject cancelButton = device.findObject(new UiSelector()
            .text("Cancel")
            .className("android.widget.Button"));
    UiObject okButton = device.findObject(new UiSelector()
            .text("OK")
            .className("android.widget.Button"));

    // Simulate a user-click on the OK button, if found.
    if(okButton.exists() && okButton.isEnabled()) {
        okButton.click();
    }
    
```

**指定选择器**

如果您需要访问应用中的特定界面组件，请使用 [`UiSelector`](https://developer.android.com/reference/androidx/test/uiautomator/UiSelector) 类。此类表示对当前显示的界面中特定元素的查询。

如果找到了多个匹配元素，系统会将布局层次结构中的第一个匹配元素作为目标 [`UiObject`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject) 返回。构建 [`UiSelector`](https://developer.android.com/reference/androidx/test/uiautomator/UiSelector) 时，您可以将多个属性链接在一起以优化搜索。如果未找到匹配的界面元素，系统会抛出 [`UiAutomatorObjectNotFoundException`](https://developer.android.com/reference/androidx/test/uiautomator/UiObjectNotFoundException)。

您可以使用 [`childSelector()`](https://developer.android.com/reference/androidx/test/uiautomator/UiSelector#childSelector(androidx.test.uiautomator.UiSelector)) 方法来嵌套多个 [`UiSelector`](https://developer.android.com/reference/androidx/test/uiautomator/UiSelector) 个实例。例如，以下代码示例展示了您的测试如何指定搜索，以在当前显示的界面中查找第一个 `ListView`，然后在该 `ListView` 中搜索，以查找具有文本属性“Apps”的界面元素。

```java
    UiObject appItem = device.findObject(new UiSelector()
            .className("android.widget.ListView")
            .instance(0)
            .childSelector(new UiSelector()
            .text("Apps")));
    
```

最佳做法是，在指定选择器时，应使用资源 ID（如果已将其分配给界面元素），而不是文本元素或内容描述符。并非所有元素都有文本元素（例如，工具栏中的图标）。文本选择器很脆弱，如果界面发生细微更改，可能会导致测试失败。此外，文本选择器也可能无法在不同语言之间扩展，它们可能与翻译的字符串不匹配。

在选择器条件中指定对象状态可能很有用。例如，如果要选择所有已选中元素的列表以便取消选中这些元素，请调用 [`checked()`](https://developer.android.com/reference/androidx/test/uiautomator/By#checked(boolean)) 方法并将参数设置为 `true`。

**执行操作** 

您的测试获取 [`UiObject`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject) 对象后，您可以调用 [`UiObject`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject) 类中的方法，在由该对象表示的界面组件上执行用户交互。您可以指定如下操作：

- [`click()`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject#click())：点击界面元素的可见边界的中心。
- [`dragTo()`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject#dragTo(int, int, int))：将此对象拖动到任意坐标。
- [`setText()`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject#setText(java.lang.String))：清除可修改字段的内容后，设置该字段中的文本。相反，[`clearTextField()`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject#clearTextField()) 方法用于清除可修改字段中的现有文本。
- [`swipeUp()`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject#swipeUp(int))：对 [`UiObject`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject) 执行向上滑动操作。同样，[`swipeDown()`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject#swipeDown(int))、[`swipeLeft()`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject#swipeLeft(int)) 和 [`swipeRight()`](https://developer.android.com/reference/androidx/test/uiautomator/UiObject#swipeRight(int)) 方法用于执行相应的操作。

通过 UI Automator 测试框架，您可以发送 `Intent` 或启动 `Activity`，无需使用 shell 命令，只需通过 `getContext()` 获取 `Context` 对象即可。

以下代码段展示了您的测试如何使用 `Intent` 启动被测应用。当您只想测试计算器应用而不关心启动器时，此方法很有用。

```java
    public void setUp() {
        ...

        // Launch a simple calculator app
        Context context = getInstrumentation().getContext();
        Intent intent = context.getPackageManager()
                .getLaunchIntentForPackage(CALC_PACKAGE);
        intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);

        // Clear out any previous instances
        context.startActivity(intent);
        device.wait(Until.hasObject(By.pkg(CALC_PACKAGE).depth(0)), TIMEOUT);
    }
    
```

**对集合执行操作**

如果需要模拟内容集合（例如，音乐专辑中的歌曲或收件箱中的电子邮件列表）上的用户交互，请使用 [`UiCollection`](https://developer.android.com/reference/androidx/test/uiautomator/UiCollection) 类。要创建 [`UiCollection`](https://developer.android.com/reference/androidx/test/uiautomator/UiCollection) 对象，请指定 [`UiSelector`](https://developer.android.com/reference/androidx/test/uiautomator/UiSelector)，用于搜索其他子界面元素的界面容器或封装容器，如包含子界面元素的布局视图。

以下代码段展示了您的测试如何构建 [`UiCollection`](https://developer.android.com/reference/androidx/test/uiautomator/UiCollection) 以表示 `FrameLayout` 中显示的视频专辑：

```java
    UiCollection videos = new UiCollection(new UiSelector()
            .className("android.widget.FrameLayout"));

    // Retrieve the number of videos in this collection:
    int count = videos.getChildCount(new UiSelector()
            .className("android.widget.LinearLayout"));

    // Find a specific video and simulate a user-click on it
    UiObject video = videos.getChildByText(new UiSelector()
            .className("android.widget.LinearLayout"), "Cute Baby Laughing");
    video.click();

    // Simulate selecting a checkbox that is associated with the video
    UiObject checkBox = video.getChild(new UiSelector()
            .className("android.widget.Checkbox"));
    if(!checkBox.isSelected()) checkbox.click();
    
```

**对可滚动视图执行操作**

使用 [`UiScrollable`](https://developer.android.com/reference/androidx/test/uiautomator/UiScrollable) 类模拟显示屏上的垂直或水平滚动。当界面元素位于屏幕外而您需要滚动屏幕以使其进入视野时，此方法很有用。

以下代码段展示了如何模拟向下滚动“设置”菜单并点击“关于平板电脑”选项的操作：

```java
    UiScrollable settingsItem = new UiScrollable(new UiSelector()
            .className("android.widget.ListView"));
    UiObject about = settingsItem.getChildByText(new UiSelector()
            .className("android.widget.LinearLayout"), "About tablet");
    about.click();
    
```

**验证结果**

`InstrumentationTestCase` 扩展了 `TestCase`，因此您可以使用标准的 JUnit [`Assert`](http://junit.org/javadoc/latest/org/junit/Assert.html) 方法测试应用中的界面组件是否会返回预期结果。

以下代码段展示了您的测试如何找到计算器应用中的几个按钮，按顺序点击它们，然后验证是否显示了正确的结果。

```java
    private static final String CALC_PACKAGE = "com.myexample.calc";

    public void testTwoPlusThreeEqualsFive() {
        // Enter an equation: 2 + 3 = ?
        device.findObject(new UiSelector()
                .packageName(CALC_PACKAGE).resourceId("two")).click();
        device.findObject(new UiSelector()
                .packageName(CALC_PACKAGE).resourceId("plus")).click();
        device.findObject(new UiSelector()
                .packageName(CALC_PACKAGE).resourceId("three")).click();
        device.findObject(new UiSelector()
                .packageName(CALC_PACKAGE).resourceId("equals")).click();

        // Verify the result = 5
        UiObject result = device.findObject(By.res(CALC_PACKAGE, "result"));
        assertEquals("5", result.getText());
    }
    
```

### API

```Java
import android.support.test.uiautomator.UiDevice;
```

作用：设备封装类，测试过程中获取设备信息和设备交互。

```Java
import android.support.test.uiautomator.UiObject;
```

作用：所有控件抽象，用于表示一个Android控件。

```Java
import android.support.test.uiautomator.UiObjectNotFoundException;
```

作用：异常处理机制，在预期控件不存在时抛出。

```Java
import android.support.test.uiautomator.UiSelector;
```

作用：控制选择器，利用控制属性描述目标控件，用于控件匹配使用。

```Java
import android.support.test.uiautomator.Configurator;
```

所用：配置基类，用以控制测试过程的事件等超时、控件可见超时等。

```Java
import android.support.test.uiautomator.UiCollection;
```

作用：控件集合，用于控件遍历。

```Java
import android.support.test.uiautomator.UiScrollable;
```

作用：滚动控件，当目标控件存在于屏幕之外时使用。

```Java
import android.support.test.uiautomator.UiWatcher;
```

作用：界面观察者，用于处理弹窗中断逻辑。

**定位控件** 

`Java import android.support.test.uiautomator.By;` 

作用：可以更简洁的方式使用ByScelector 选择器。 用法： `findObject(By.text(“text”))`

`import android.support.test.uiautomator.BySelector;`
作用： BySelector 调用findObject 时匹配UI 元素
用法：
`findObject(new BySelector().text(“text”))`

```
<br>
### By类常用定位方法

​```Java
/**
 * 通过文本 text 定位控件
 * 例如： text = "hello world"
 */
device.findObject(By.text("text"));
device.findObject(By.textContains("llo wor"));
device.findObject(By.textStartsWith("hello"));
device.findObject(By.textEndsWith("world"));

/**
 * 通过内容描述 content-dec 定位控件
 * 例如： desc = "content-dec"
 */
device.findObject(By.desc("content-dec"));
device.findObject(By.descContains("tent"));
device.findObject(By.descStartsWith("content"));
device.findObject(By.descEndsWith("-dec"));

/**
 *  通过包名package定位控件
 *  例如： package = "com.android.calculator2"
 */
device.findObject(By.pkg("com.android.calculator2"));

/**
 *  通过资源名 resource 定位控件
 *  例如： resource = "com.android.calculator2:id/digit_0"
 */
device.findObject(By.res("com.android.calculator2:id/digit_0"));

/**
 *  通过类名 class定位控件
 *  例如： class = "android.widget.Button"
 */
device.findObject(By.clazz("android.widget.Button"));
```


**UiDevice类常用方法**

```Java
// home键
device.pressHome();

// back 键
device.pressBack();

// 显示最近打开并置于后台的App
device.pressRecentApps();

// 快速设置键
device.openQuickSettings();

// 打开通知
device.openNotification();

// 虚拟键盘，参考appium
device.pressKeyCode(17);

// 获得屏幕高度和宽度
int x = device.getDisplayWidth();
int y = device.getDisplayHeight();
String xs =String.valueOf(x);
String ys =String.valueOf(y);
Log.e("xxxxxxxxxx", xs);
Log.e("yyyyyyyyyy", ys);

// 向下滑显示通知栏
device.swipe(200,0,200, 300,180);
// 向左滑显示右一屏
device.swipe(20,400,460, 400,180);

// 获取当前应用活动名 和 包名
String activity = device.getCurrentActivityName();
String packagea = device.getCurrentPackageName();
Log.e("activity", activity);
Log.e("packagea", packagea);

// 休眠屏幕
device.sleep();
// 如果屏幕熄灭，点亮
device.wakeUp();
```


**UiObject2类常用方法**

```Java
UiObject2 element = device.findObject(By.text("text"));

//清除元素，针对输入框
element.clear();

// 点击
element.click();

// 长按
element.longClick();

// 获取元素文本
element.getText();

//设置元素文本,相当于输入
element.setText("new text");

//获取元素scrollable属性，判断是否可滚动
element.isScrollable();

// 判断两个对象是否一致
UiObject2 element2 = device.findObject(By.text("text"));
element.equals(element2);

//获取元素content_desc属性
element.getContentDescription();

// 获取包名 package 属性
element.getApplicationPackage();

// 获取元素的子元素集合
element.getChildren();

// 获取元素的子元素的个数
element.getChildCount();

// 获取元素的class属性
element.getClassName();

// 获取元素的 resource-id 属性
element.getResourceName();

// 将元素拖动到指定位置
Point desPoint = new Point();
desPoint.x = 200;
desPoint.y = 20;
element.drag(desPoint, 2000);

// 点击并等待新窗口
element.clickAndWait(Until.newWindow(), 2000);
```


**Configurator类**

```Java
Configurator configurator = Configurator.getInstance();

//动作，设置延时， 默认3s
configurator.setActionAcknowledgmentTimeout(1000);

//键盘输入，设置延时,默认0s
configurator.setKeyInjectionDelay(1500);

// 滚动，设置延时， 默认200ms
configurator.setScrollAcknowledgmentTimeout(2000);

// 空闲，设置延时，默认10s
configurator.setWaitForIdleTimeout(2500);

// 组件查找, 设置延时, 默认10s
configurator.setWaitForSelectorTimeout(3000);
```


**UiWatcher 类用法**

```Java
final UiObject2 ui = mDevice.findObject(By.text("Messenger"));
//注册监听器
mDevice.registerWatcher("testWatcher", new UiWatcher() {
    @Override
    public boolean checkForCondition() {
        if(mDevice.hasObject(By.text("Contact"))){
            ui.click();
            Log.i("testWatcher", "监听器被触发了");
            return true;
        }
        Log.i("testWatcher", "监听器未被触发");
        return false;
    }
});

//重置监听器
mDevice.resetWatcherTriggers();

//移除监听器
mDevice.removeWatcher("testWatcher");

//运行所有的监听器
mDevice.runWatchers();
```


**UiScrollable 类的常用方法**

```Java
UiScrollable scroll  = new UiScrollable( new UiSelector()
        .scrollable(true));

// 以步长为5快速向后滑动
scroll.flingBackward();

//以步长为5快速向前滑动
scroll.flingForward();

// 是否允许滚动获取具备UiSelector条件元素集合后, 再以text属性的查找对象
scroll.getChildByText(
        new  UiSelector().resourceId("android:id/title"), "About emulated device", true);
// 是否允许滚动获取具备UiSelector条件元素集合后, 再以content-desc属性搜索子元素
scroll.getChildByDescription(new UiSelector().text("text"), "content-desc", true);

//通过实例查找子元素,资源id为"android:id/title"下的第1个实例
scroll.getChildByInstance(new UiSelector().resourceId("android:id/title"),0);

// 获取执行搜索滑动过程中的最大滑动次数，默认常量为30
scroll.getMaxSearchSwipes();

//设置最大可扫动次数
scroll.setMaxSearchSwipes(50);

/**
 * 设置listview校准常量为0.15，即距离listview顶部15%和底部15%的区域不可滑动，只有控件中部70%的区域可滑动。
 * 当校准常量设置为0.5时，控件的可滑动区域为0，滑动的动作效果将和单击的效果一样。
 */
scroll.setSwipeDeadZonePercentage(0.15);
// 获得校准常量，校准常量默认值为0.1(10%)
scroll.getSwipeDeadZonePercentage();

//设置滚动方向设置为水平滚动
scroll.setAsHorizontalList();
// 设置滚动方向设置为纵向滚动
scroll.setAsVerticalList();

// 滚动到某个元素上
scroll.scrollIntoView(new UiSelector().text("abc"));
//滚动到文本对象所在位置
scroll.scrollTextIntoView("abc");

//自定义最大滚动次数，滚动到开始/结束位置
scroll.scrollToBeginning(30);
scroll.scrollToEnd(30);
//以步长（速率）5滚动到列表底部，最多滚动10次。
scroll.scrollToEnd(10, 5);
```

**UiCollection 类的常用方法**

```Java
UiCollection  coll = new UiScrollable(new UiSelector()
.resourceId("android:id/title"));
// 查找元素下面子元素的数量
coll.getChildCount();
// 获取元素集合，再以text属性的查找对象
coll.getChildByText(new UiSelector().text("Display"), "Display");
// 获取元素集合，再查找其下面第1个元素
coll.getChildByInstance(new UiSelector().text("Display"), 0);
// 获取元素集合，再以content_desc属性的查找对象
coll.getChildByDescription(new UiSelector().text("Display"), "content_desc");
// 获得指定的子元素
coll.getChild(new UiSelector().text("aaa"));
```
