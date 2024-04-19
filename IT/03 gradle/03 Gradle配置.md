# Gradle配置
- **管理Gradle的运行环境**，比如: `org.gradle.jvmargs=-Xmx2048m`
- **构建工具**，主要是编译打包apk

# 配置的优先级（**由高到低**）
- **Command-line flags：命令行标志**，如--stacktrace，这些优先于属性和环境变量；
- **System properties：系统属性**，如systemProp.http.proxyHost=somehost.org存储在gradle.properties文件中；
- **Gradle properties：Gradle属性**，如org.gradle.caching=true，通常存储在项目根目录或GRADLE_USER_HOME环境变量中的gradle.properties文件中；
- **Environment variables：环境变量**，如GRADLE_OPTS，由执行Gradle的环境源；

# Gradle官方的版本
- https://services.gradle.org/distributions/
- 这里有几种类型，分别是all、bin、doc：
    - doc：顾名思义，用户文档；
    - bin：即binary，可运行并不包含多余的东西；（**一般选择这个就够用了**）
    - all：包含所有，除了bin之外还有用户文档、sample等；

# Gradle、Android Gradle Plugin、Android Studio三者的版本映射关系
- AGP < Gradle

| Plugin version |	Minimum required Gradle version|
|--|--|
| 8.3	| 8.4 |
| 8.2	| 8.2 |

| Android Studio version |	Required AGP version |
|--|--|
| Jellyfish - 2023.3.1	| 3.2-8.4  |
| Iguana - 2023.2.1	    | 3.2-8.3  |


# Gradle配置
- gradle wrapper
- build.gradle（project）
- build.gradle（module）
- settings.gradle
- gradle.properties
- local.properties

# Gradle配置，执行流程
- https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8b7ad38070084d96b7237bd21a247f0a~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp
- 通过依赖'com.android.application'插件，有了android{}这个配置DSL；
- 一个项目对应一个Project对象，Project对象里面包含dependencies函数；
- android{}这个配置DSL点进去就是Project对象里面dependencies这个函数，二者都接收一个闭包；
- 然后通过DependencyHandler这个类，执行android(Closure configuration)这个闭包并委托给dependencies(Closure configureClosure)，也就是Project对象；
- 最后Gradle在执行阶段去解析这个Project对象并拿到android{}里面的配置参数，后面就是执行Task等等；

# gradle wrapper
- wrapper是**对Gradle的一层封装**
- **封装了gradle的运行的版本**

```
.
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
└── gradlew.bat
```

这几个文件的作用
- gradle-wrapper.jar：主要是**Gradle的运行逻辑**，包含下载Gradle；
- gradle-wrapper.properties：gradle-wrapper的配置文件，核心是**定义了Gradle版本**；
- gradlew：gradle wrapper的简称，linux下的执行脚本
- gradlew.bat：windows下的执行脚本

## gradle-wrapper.properties
```
# Gradle版本的下载地址
distributionUrl=https://services.gradle.org/distributions/gradle-7.4-bin.zip

# Gradle下载文件的主目录（zip包 和 解压后的包）
zipStoreBase=GRADLE_USER_HOME
distributionBase=GRADLE_USER_HOME

# Gradle下载文件的相对目录--相对于主目录（zip包 和 解压后的包）
zipStorePath=wrapper/dists
distributionPath=wrapper/dists
```

## build.gradle（Project）
- 位于项目的**根目录**下，用于**定义适用于项目中所有模块的依赖项**。
- **Gradle7.0**之后，project下的build.gradle**文件变动很大**
    - 默认只有plugin的引用了
    - 其他原有的配置挪到settings.gradle文件中了

### 7.0之后
```
plugins {
    // id «plugin id» version «plugin version» [apply «false»]   
    // apply false, 只想在某个子项目依赖该plugin
    id 'com.android.application' version '7.3.0' apply false
    id "yechaoa" version "1.0.0" apply false
}

subprojects { subproject ->
    if (subproject.name == "subProject") {
        apply plugin: 'yechaoa'
    }
}
```

### 7.0之前
```
// 引入插件
// 引入二进制插件，
apply plugin: 'kotlin-android'
// 引入应用脚本插件，可以是本地的，也可以是网络的，如果是网络的话要使用HTTP URL
apply from: 'gradle/nexus.gradle'

// 声明gradle脚本自身需要使用的资源
// 包括依赖项、第三方插件、maven仓库地址
buildscript {
// ext，项目全局属性，多用于自定义
    ext.kotlin_version = "1.5.0"

    repositories {
        google()
        mavenCentral()
    }

// Gradle需要的插件
    dependencies {
        classpath "com.android.tools.build:gradle:4.2.1"
    }
}

// 为所有项目提供共同所需依赖包
allprojects {
    repositories {
        google()
        mavenCentral()
        jcenter() 
    }
}

// 运行gradle clean时，执行此处定义的task
// 该任务继承自Delete，相当于执行Delete.delete(rootProject.buildDir)
task clean(type: Delete) {
    delete rootProject.buildDir
}
```

## build.gradle（Module）
- 位于每个 project/module/ 目录下，用于为其所在的特定模块配置 build 设置

### 7.0之后
- 其实跟7.0之前差别也不是太大

```
// plugins == apply plugin
plugins {
// android项目专属
// 使用 android{} 这个配置DSL
    id 'com.android.application'
}

// Gradle是一个通用的自动化构建工具，通用也就是说，不仅可以构建Android项目，还可以构建java、kotlin、swift等项目。
// 再结合android{}里面的配置属性，我们可以确定，那这就是Android项目专属的配置DSL了
// 可以让我们自定义Gradle Android工程，是Gradle Android工程配置的唯一入口
// android{}是通过插件的方式引入的
android {
// 应用程序的命名空间。主要用于访问应用程序资源
    namespace 'com.yechaoa.gradlex'

// Android SDK 版本，即API Level    
// android版本 和 Android sdk 版本对应关系。https://developer.android.google.cn/guide/topics/manifest/uses-sdk-element?hl=en
    compileSdk 32   //compileSdkVersion 32
    
// buildToolsVersion 是构建该Android工程所用构建工具的版本
// 7.0之后，被废弃了，因为AGP会应用一个对应的默认的版本
    buildToolsVersion '30.0.3'

// 默认配置，它是一个ProductFlavor。
// ProductFlavor允许我们根据不同的情况同时生成多个不同的apk包
    defaultConfig {
        applicationId "com.yechaoa.gradlex"
        minSdk 23

// 标示app基于哪个Android版本开发的
        targetSdk 32

// 版本号，一般用于版本控制        
        versionCode 1
// 版本名称，公开信息，默认1.0，7.0之前默认是1.0.0，行业规范3位数，但并没有强制        
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

// 源代码集合，是Java插件用来描述和管理源代码及资源的一个抽象概念，是一个Java源代码文件和资源文件的集合
// 我们可以通过sourceSets更改源集的Java目录或者资源目录等。
    sourceSets {}


// buildTypes 构建类型
// name：build type的名字
// applicationIdSuffix：应用id后缀
// versionNameSuffix：版本名称后缀
// debuggable：是否生成一个debug的apk
// minifyEnabled：是否混淆
// proguardFiles：混淆文件
// signingConfig：签名配置
// manifestPlaceholders：清单占位符
// shrinkResources：是否去除未利用的资源，默认false，表示不去除。
// zipAlignEnable：是否使用zipalign工具压缩。
// multiDexEnabled：是否拆成多个Dex。一般用程序中代码太多，超过了65535个方法的时候。
// multiDexKeepFile：指定文本文件编译进主Dex文件中
// multiDexKeepProguard：指定混淆文件编译进主Dex文件中
// buildConfigField：他是BuildConfig文件的一个函数，而BuildConfig这个类是Android Gradle构建脚本在编译后生成的。比如版本号、一些标识位什么的
    buildTypes {
        debug {
            buildConfigField("String", "AUTHOR", ""yechaoa"")
            minifyEnabled false
        }
        release {
            buildConfigField("String", "AUTHOR", ""yechaoa"")
            signingConfig signingConfigs.release
            shrinkResources true
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

//signingConfigs 签名配置，一个app只有在签名之后才能被发布、安装、使用，签名是保护app的方式，标记该app的唯一性。
    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile file(keystoreProperties['storeFile'])
            storePassword keystoreProperties['storePassword']
            v1SigningEnabled true
            v2SigningEnabled true
        }
    }
    
// 多渠道打包配置，可以实现定制化版本的需求    
    productFlavors {
    }   

// 开启或关闭构建功能，常见的有viewBinding、dataBinding、compose
    buildFeatures {
        viewBinding = true
        // dataBinding = true
    }     


// Android中的Java/kotlin源代码被编译成class字节码后，在打包成apk的时候被dx命令优化成Android虚拟机可执行的dex文件
// Android Gradle插件会调用SDK中的dx命令进行处理。
// 可以设置编译时所占用的内存（javaMaxHeapSize），从而提升编译速度。
// 7.0之后已经废弃。
    dexOptions{}

// Java编译选项，指定java环境版本。
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }

// Kotlin编译选项，通常指定jvm环境。    
    kotlinOptions {
        jvmTarget = '1.8'
    }

// Compose功能的可选设置。比如指定Kotlin Compiler版本，一般用默认。
    composeOptions{}

// 用于配置lint选项
    lintOptions{}
}

// dependencies
    // compile > implementation/api
    // androidTestCompile > androidTestImplementation
    // testCompile > testImplementation
    // instrumentTest > androidTest
// implementation和api的区别
    // implementation指令依赖是不会传递的，也就是说当前引用的第三方库仅限于本module内使用，其他module需要重新添加依赖才能用.
    // 使用api指令的话可以传递。
dependencies {
    implementation 'androidx.core:core-ktx:1.7.0'
    testImplementation 'junit:junit:4.13.2'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
}
```

### 7.0之前
```
apply plugin: 'com.android.application'
android {
    compileSdkVersion 30

    defaultConfig {
        applicationId "com.yechaoa.app"
        minSdkVersion 19
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner 'androidx.test.runner.AndroidJUnitRunner'
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }

    buildFeatures {
        viewBinding = true
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'androidx.constraintlayout:constraintlayout:2.0.4'
    testImplementation 'junit:junit:4.13.2'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.3.0'
    implementation project(':yutilskt')
}
```

## settings.gradle
- 位于项目的根目录下，用于定义项目级代码库设置
```
// pluginManagement 
// 插件管理，指定插件下载的仓库，及版本
pluginManagement {
    repositories {
        gradlePluginPortal()
        google()
        mavenCentral()
    }
    plugins {
// yechaoaPluginVersion，引用自 gradle.properties
        id 'com.yechaoa.gradlex' version "${yechaoaPluginVersion}"
    }
}

// dependencyResolutionManagement
// 依赖管理，指定依赖库的仓库地址，及版本。即7.0之前的allprojects。
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
    }
}

rootProject.name = "GradleX"
include ':app'
```

## gradle.properties
- 位于项目的根目录下，用于指定 Gradle 构建工具包本身的设置，也可用于项目版本管理。
- 比如Gradle 守护程序的最大堆大小、编译缓存、并行编译、是否使用Androidx等等
```
org.gradle.jvmargs=-Xmx2048m -XX:MaxPermSize=512m
android.useAndroidX=true
android.enableJetifier=true
kotlin.code.style=official

#并行编译
org.gradle.parallel=true

#构建缓存
org.gradle.caching=true

# 版本相关
# 本质上是key-value形式的参数
# 不仅可以作用于依赖库，也可作用于依赖插件。
yechaoaPluginVersion="1.0.0"
```

## local.properties
- 位于项目的根目录下，用于指定 Gradle 构建配置本地环境属性，也可用于项目环境管理。
```
// SDK 的路径
sdk.dir=/Users/yechao/Library/Android/sdk
// NDK 的路径，已废弃，用SDK目录下的NDK目录。
ndk.dir=/Users/yechao/Library/Android/ndk

// 可以用作项目本地调试的一些开关
isRelease=true
#isDebug=false
#isH5Debug=false
```
