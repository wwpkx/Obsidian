# Task是什么
- Task是一个任务，是Gradle中最小的构建单元
- 构建的核心就是由Task组成的有向无环图
- Task管理了一个Action的List，你可以在List前面插入一个Action（doFirst），也可以从list后面插入（doLast）

# Task增量构建
- 任何构建工具的一个重要部分是能够避免做已经完成的工作
- 比如task，从A文件夹拷贝到B文件夹
    - 全量构建，不管什么情况，执行task 从A文件夹拷贝到B文件夹
    - 增量构建，拷贝时，只拷贝修改的部分
        - ADDED
        - MODIFIED
        - REMOVED

```
class CopyTask extends DefaultTask {

    // 指定增量输入属性
    @Incremental
    // 指定输入
    @InputFiles
    FileCollection from

    // 指定输出
    @OutputDirectory
    Directory to

    // task action 执行
    @TaskAction
    void execute(InputChanges inputChanges) {

        boolean incremental = inputChanges.incremental
        println("isIncremental = $incremental")

        inputChanges.getFileChanges(from).each {
            if (it.fileType != FileType.DIRECTORY) {
                ChangeType changeType = it.changeType
                String fileName = it.file.name
                println("ChangeType = $changeType , ChangeFile = $fileName")

                if (changeType != ChangeType.REMOVED) {
                    copyFileToDir(it.file, to)
                }
            }
        }
    }

    /**
     * 复制文件到文件夹
     * @param src 要复制的文件
     * @param dir 接收的文件夹
     * @return
     */
    static def copyFileToDir(File file, Directory dir) {
        File dest = new File("${dir.getAsFile().path}/${file.name}")

        if (!dest.exists()) {
            dest.createNewFile()
        }

        dest.withOutputStream {
            it.write(new FileInputStream(file).getBytes())
        }
    }
}
```

# 创建task
```
class YechaoaTask extends DefaultTask {

// 对外暴露的参数需要使用@Internal注解
    @Internal
    def taskName = "default"

    @TaskAction
    def MyAction1() {
        println("$taskName -- MyAction1")
    }

    @TaskAction
    def MyAction2() {
        println("$taskName -- MyAction2")
    }
}

// tasks.register('yechaoa', YechaoaTask, 'xxx') //如果是Action方法的构造函数传参，参数写在type类型后面即可
tasks.register("yechaoa", YechaoaTask) {
    taskName = "我是传入的Task Name "
}

task("yechaoa") {
    println "aaa"
}
```

## Task的Action
- doFirst：倒序
- Action：倒序 (应该是，看评论)
- doLast：正序

```
tasks.register("yechaoa") {
    it.doFirst {
        println("${it.name} = doFirst 111")
    }
    it.doFirst {
        println("${it.name} = doFirst 222")
    }

    println("Task Name = " + it.name)

    it.doLast {
        println("${it.name} = doLast 111")
    }
    it.doLast {
        println("${it.name} = doLast 222")
    }
}
```

# Task依赖
- dependsOn，指定上一个执行的Task
- finalizedBy，指定下一个执行的Task
- mustRunAfter，多个task一起执行才能看出先后顺序
- shouldRunAfter，

```
def yechaoa111 = tasks.register("yechaoa111") {
    it.doLast {
        println("${it.name}")
    }
}

def yechaoa222 = tasks.register("yechaoa222") {
    it.doLast {
        println("${it.name}")
    }
}

yechaoa111.configure {
    dependsOn yechaoa222
}
```

# 跳过Task
- 条件跳过
- 异常跳过
- 禁用跳过
- 超时跳过


# 执行Task
```
./gradlew taskname
./gradlew taskname taskname taskname
```

## Task执行结果
- EXCUTED，执行
- UP-TO-DATE，它表示Task的输出没有改变
- FOME-CACHE，字面意思，表示可以从缓存中复用上一次的执行结果
- SKIPPED，字面意思，表示跳过
- NO-SOURCE，Task不需要执行。有输入和输出，但没有来源。