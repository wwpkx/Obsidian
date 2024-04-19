# Gradle生命周期
- Initialization (初始化)
    - 会决定构建中包含哪些项目，并会**为每个项目创建Project实例**
    - 首先会寻找settings.gradle来决定此次为单项目构建还是多项目构建
    - 单项目就是module，多项目即project+app+module(1+n)
- Configuration (配置) 
    - 会评估构建项目中包含的所有构建脚本，随后应用插件、使用DSL配置构建，并在最后注册Task
- Execution (执行)
    - 会执行构建所需的Task集合

# Hook生命周期
- 整个生命周期都可以hook