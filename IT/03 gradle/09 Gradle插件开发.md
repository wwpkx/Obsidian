# Gradle插件 用途
- 特别是在性能优化领域
- 跟我们日常的编译打包也息息相关

# Gradle插件是什么
- 用于扩展和定制 Gradle构建系统功能的机制
- **Gradle插件可以执行各种任务**，包括编译代码、执行测试、打包文件、生成文档等等
- 插件可以访问和操作 Gradle的构建模型，如项目、任务、依赖关系等，从而实现对构建过程的控制和定制

# 插件写在哪
- 跟Task一样，写在build.gradle文件中，作用域当前Project；
- 写在buildSrc里，作用域当前项目所有Project；
- 写在单独项目里，发布后可提供给所有项目所有Project；（等同于buildSrc）

# 直接写在build.gradle文件中
```
// 插件扩展
class YechaoaPluginExtension {
    String title
    int chapter
    SubExtension subExtension

    YechaoaPluginExtension(Project project) {
        subExtension = project.extensions.create('sub', SubExtension.class)
    }
}
class SubExtension {
    String author
}

class YechaoaPlugin implements Plugin<Project> {
    @Override
    void apply(Project project) {
        println("这是插件：${this.class.name}")
        def extension = project.extensions.create("yechaoa", YechaoaPluginExtension)
        // 设置默认值 可以定义set()方法 然后在这里set
        project.task("YechaoaPluginTask") { task ->
            task.doLast {
                println("这是插件${this.class.name}，它创建了一个Task：${task.name}")
                println("title = ${extension.title}")
                println("chapter = ${extension.chapter}")
                println("author = ${extension.subExtension.author}")
            }
        }
    }
}

apply plugin: YechaoaPlugin

// 是不是跟android{ }一毛一样
yechaoa {
    title = "【Gradle-8】Gradle插件开发指南"
    chapter = 8
    sub {
        author = "yechaoa"
    }
}

apply plugin: YechaoaPlugin
//apply(plugin: YechaoaPlugin)

执行：
./gradlew YechaoaPluginTask

输出：
> Task :app:YechaoaPluginTask
title = 【Gradle-8】Gradle插件开发指南
chapter = 8
author = yechaoa
```

# 编写在单独项目中
参考: https://github.com/yechaoa/GradleX/tree/master

步骤
1. 创建lib，就像创建java lib，Android lib一样
2. 创建 YechaoaPlugin，如上一节
3. 添加依赖
```
plugins {
    id 'java-gradle-plugin'
}
gradlePlugin{
    plugins{
        DependenciesPlugin{
            id = 'com.yechaoa.plugin.dependencies'
            implementationClass = 'com.yechaoa.plugin.DependenciesPlugin'
        }
    }
}
```

# 本地发布
## 发布需要的依赖的插件
```
在plugin>build.gradle文件中先依赖一个maven发布的插件'maven-publish'

plugins {
    id 'maven-publish'
}

dependencies {
    implementation 'com.android.tools.build:gradle:7.3.0'
}
```

## 发布配置
```
group 'com.yechaoa.plugin'
version '1.0.0'

publishing {
    // 配置Plugin GAV
    publications {
        maven(MavenPublication) {
            groupId = group
            artifactId = 'dependencies'
            version = version

            from components.java
        }
    }
    // 配置仓库地址
    repositories {
        maven {
            url layout.buildDirectory.dir("maven-repo")
        }
    }
}
```

## 执行发布操作
./gradlew publish

## 发布产物
plugin > build > maven-repo

## 使用
```
// 在settings.gradle文件中配置插件仓库地址
pluginManagement {
    repositories {
        // ...
        maven {
            url './maven-repo'
        }
    }
}

// 在project>build.gradle文件中添加插件依赖
buildscript {
    dependencies {
        classpath('com.yechaoa.plugin:dependencies:1.0.0')
    }
}

// 在app:build.gradle文件中依赖我们的plugin
plugins {
    id 'com.yechaoa.plugin.dependencies'
}
```