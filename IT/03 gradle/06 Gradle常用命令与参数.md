# 3、编译命令
```
3.1、检查依赖并编译打包
./gradlew build

3.2、编译并打出Debug包
./gradlew assembleDebug

3.3、编译打出Debug包并安装
./gradlew installDebug

3.4、编译并打出Release包
./gradlew assembleRelease

3.5、编译打出Release包并安装
./gradlew installRelease

3.6、Debug/Release编译并打印日志
./gradlew assembleDebug --info
./gradlew assembleRelease --info
```

# 4、清除命令
```
清除构建目录下的产物。
./gradlew clean     #等同于Build->Clean Project。
```

# 5、卸载命令
## 5.1、卸载Debug/Release安装包
./gradlew uninstallDebug  
./gradlew uninstallRelease  

## 5.2、adb卸载
在Android Studio中执行是直接卸载的当前项目安装包，如果是adb执行则需要指定包名。  
adb uninstall com.yechaoa.gradlex  

# 6、调试命令
```
调试命令在定位编译问题的时候非常有用。
当我们遇到编译错误的时候，经常会看到这个提示：
* Try:
> Run gradle tasks to get a list of available tasks.
> Run with --stacktrace option to get the stack trace.          # 编译并打印堆栈日志
> Run with --info or --debug option to get more log output.     # 日志级别
> Run with --scan to get full insights.
```

## 6.1、编译并打印堆栈日志
```
./gradlew assembleDebug --stacktrace
./gradlew assembleDebug -s

详细版：
./gradlew assembleDebug --full-stacktrace
./gradlew assembleDebug -S
```

## 6.2、日志级别
```
有时候构建日志会有很多，看到的可能不全，甚至不是真正的编译问题，而构建日志又不能像logcat那样可以可视化的筛选，这个时候就需要用日志级别来筛选一下。
-q，--quiet 仅记录错误。
-w，--warn 将日志级别设置为警告。
-i，--info 将日志级别设置为信息。
-d，--debug 调试模式（包括正常的stacktrace）。

示例：
./gradlew assembleDebug -w
```

# 7、任务相关
## 7.1、查看主要Task
./gradlew tasks

## 7.2、查看所有Task
./gradlew tasks --all

## 7.3、执行Task
./gradlew taskName  
./gradlew :moduleName:taskName  
同时，可在AS右侧工具类Gradle中查看项目及module的Task，并可以点击执行对应Task。  

# 8、查看依赖
编译有很多问题都是依赖导致的错误，查看依赖能帮我们快速定位问题所在。

## 8.1、查看项目根目录下的依赖
./gradlew dependencies

## 8.2、查看app模块下的依赖
./gradlew app:dependencies
./gradlew app:dependencies > dependencies.txt

# 9、性能相关
```
9.1-9.4的配置也都可以在gradle.properties中配置。
示例：
#并行编译
org.gradle.parallel=true

#构建缓存
org.gradle.caching=true
```

## 9.1、离线编译
./gradlew assembleDebug --offline

## 9.2、构建缓存
./gradlew assembleDebug --build-cache // 开启  
./gradlew assembleDebug --no-build-cache // 不开启  

## 9.3、配置缓存
./gradlew assembleDebug --configuration-cache // 开启  
./gradlew assembleDebug --no-configuration-cache // 不开启

## 9.4、并行构建
./gradlew assembleDebug --parallel // 开启  
./gradlew assembleDebug --no-parallel // 不开启

## 9.5、编译并输出性能报告
./gradlew assembleDebug --profile  
  
性能报告位于构建项目的GradleX/build/reports/profile/路径下  
See the profiling report at: file:///Users/yechao/AndroidStudioProjects/GradleX/build/reports/profile/profile-2022-11-29-23-13-29.html  
  
## 9.6、编译并输出更详细性能报告
./gradlew assembleDebug --scan  

# 10、动态传参
一个比较常用的传参属性，--project-prop，我们一般常用-P表示，用来**设置根项目的项目属性**。  

## 10.1、获取参数
```
示例：
 ./gradlew assembleDebug -PisTest=true  // 用-P传了一个isTest字段，并赋值为true。

那在代码里如何获取这个参数呢？然后在build.gradle中编写如下代码：
if (hasProperty("isTest")){
    println("---hasProperty isTest yes")
}else {
    println("---hasProperty isTest no")
}

我们可以用hasProperty来获取命令行(CLI)的参数，module或者插件也可以这么获取：
project.property('isTest')
```

## 10.2、获取参数值
```
我们可以用 getProperty()来获取：
if (hasProperty("isTest")) {
    println("---hasProperty isTest yes")
    if (Boolean.valueOf(getProperty('isTest'))) {
        println("---isTest true")
    } else {
        println("---isTest false")
    }
} else {
    println("---hasProperty isTest no")
}

注意，getProperty('isTest')这里要用单引号，另外命令行里面的参数值都是对象，还需要基本数据类型转换一下，即Boolean.valueOf(getProperty('isTest'))。
```

## 10.3、自定义操作
```
ok，现在我们就可以针对获取的参数去做一些自定义的操作了，比如修改我们的依赖。
app>build.gradle：
dependencies {
    implementation 'androidx.core:core-ktx:1.7.0'

    if (project.hasProperty("isTest")) {
        println("---hasProperty isTest yes")
        if (Boolean.valueOf(getProperty('isTest'))) {
            println("---isTest true")

            implementation 'com.yechaoa.gradlex.devtools:devtools:1.1.1'
        } else {
            println("---isTest false")

            implementation 'com.yechaoa.gradlex.devtools:devtools:2.2.2'
        }
    } else {
        println("---hasProperty isTest no")
    }

    testImplementation 'junit:junit:4.13.2'
}

这里举例，在isTest=true的时候依赖了devtools 1.1.1版本，isTest=false时依赖了devtools 2.2.2版本。
除了dependencies里面的依赖之外，Plugin、Task之类的也可以通过动态传参的方式去做自定义操作。
```