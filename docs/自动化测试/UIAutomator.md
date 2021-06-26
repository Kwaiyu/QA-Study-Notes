1. IDEA创建android project

2. setting：

   gradle > gradle-7.0 > gradle jvm java 14.0.1 > build/RUN (IDEA)

   android project structure > plugin : 3.6.0-rc01 / gradle : 7.0 

3. build.gradle(project)

   ```java
   // Top-level build file where you can add configuration options common to all sub-projects/modules.
   
   buildscript {
       
       repositories {
           google()
           jcenter()
           
       }
       dependencies {
   //        classpath 'com.android.tools.build:gradle:4.2.0-alpha02'
           classpath 'com.android.tools.build:gradle:3.6.0-rc01'
   
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
   apply plugin: 'com.android.application'
   
   android {
       compileSdkVersion 30
       buildToolsVersion "30.0.3"
   
       defaultConfig {
           applicationId "com.example.uiautomator"
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
   
   }
   
   dependencies {
       implementation fileTree(dir: 'libs', include: ['*.jar'])
       implementation 'androidx.appcompat:appcompat:1.1.0'
       androidTestImplementation 'junit:junit:4.13.2'
       testImplementation 'junit:junit:4.13.2'
       implementation 'junit:junit:4.13.2'
       androidTestImplementation 'com.android.support:support-annotations:28.0.0'
       implementation 'com.android.support.test:runner:1.0.2'
       implementation group: 'com.android.support.test.espresso', name: 'espresso-core', version: '3.0.2'
       implementation group: 'com.android.support.test.uiautomator', name: 'uiautomator-v18', version: '2.1.3'
   }
   
   ```

   