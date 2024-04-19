当版本冲突时，就要用到版本决议
- **用最新版本**
- 只有**主模块才能通过strictly使用低版本**
- force优先级高于strictly，如果二者同时显式声明，则会报错，推荐使用strictly

# 声明依赖项
- maven同样遵循着这样一个协议来保证唯一性，也就是**GAV（坐标）： groupId + artifactId + version**
```
    implementation 'com.google.android.material:material:1.8.0'  
    implementation group: 'com.google.android.material', name: 'material', version: '1.8.0'  // 等价于上一行  
```

# 依赖传递
- 对应着maven里面的Scope，比如我们常用的implementation、api
- 比如A依赖B，B依赖C，当用implementation声明依赖，在A不依赖C

# 版本决议
查看依赖关系
```
./gradlew app:dependencies
// 还可以使用build --scan，或者AS右上角的Gradle>app>help>dependencies，点击执行也可
```

## 依赖信息
```
+--- androidx.core:core-ktx:1.7.0
|    +--- org.jetbrains.kotlin:kotlin-stdlib:1.5.31 -> 1.7.10 (*)
它表示core-ktx中依赖的kt标准库由1.5.31被拉高到1.7.10了。

androidx.annotation:annotation:1.1.0 -> 1.3.0
org.jetbrains.kotlin:kotlin-stdlib:1.5.31 -> 1.7.10 (*)
org.jetbrains.kotlin:kotlin-stdlib:1.7.10 (*)
androidx.test:core:{strictly 1.4.0} -> 1.4.0 (c)

-> 有冲突，gradle使用 -> 后面的版本
*：*其实是省略的意思，层级太深，Gradle就省略了一部分，而越深的信息也不太重要
c：c是constraints的简称，主要是用来保证当前依赖项所需要的依赖的版本的一致性，白话讲就是为了防止其他依赖项把我需要的依赖给拉高而导致我自己不可用的情况。
strictly：strictly跟force一样表示强制使用该版本，区别在于strictly可以在依赖树里标示出来，而force则没有任何标示，所以force在高版本里也被废弃了。
```

## 决议规则
版本决议是指在某个依赖出现多个版本的时候（版本冲突），Gradle如何选择最终的版本来参与编译。  

结论
- 同一个模块的多个相同依赖，优先选择最高版本。
- 多个模块的多个相同依赖，在没有约束条件的情况下，Gradle会选择「最新策略」的决议方式，**即选择最高版本**；
- 有strictly关键字的约束时，在主模块(app)声明时，才能使用低版本

```
 implementation('com.squareup.okhttp3:okhttp') {
        version{
            strictly("4.9.3")
        }
    }
```

## 解决冲突
- 强制版本，force，strictly
- 排除，exclude
- 排除当前依赖里包含的可传递依赖项，transitive
- configurations，基于Gradle生命周期hook的后置操作

