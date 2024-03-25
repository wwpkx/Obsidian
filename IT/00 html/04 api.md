# 什么是 API？
- API 是允许两个软件组件使用一组定义和协议相互通信的机制
- 比如，手机上的**天气应用程序**通过 API 与该系统“对话”
- API 代表应用程序编程接口
- API 架构通常**从客户端和服务器的角度来解释**
- API 不同的工作方式，具体取决于其创建时间和创建原因。

# API 的工作方式
- SOAP API，**这是一个不太灵活的 API，它在过去比较流行**
    - 使用简单对象访问协议
    - 客户端和服务器使用 XML 交换消息

- REST API，**最流行、最灵活的 Web API**
    - Representational State Transfer，表述性状态传递
    - **以URI指向资源，统一接口使用标准的HTTP方法（get/查询，post/新建， delete/删除）**
    - **GraphQL服务将性能作为首要任务，而RESTful服务则保持可靠性。**
    - **RPC 更侧重于动作。Restful 的主体是资源**
    
- RPC API，**远程过程调用**，（Remote Procedure Call）
    - **总结：RPC 主要用于公司内部的服务调用，性能消耗低，传输效率高，实现复杂**
    - RPC API **调用服务器中的函数，就像在本地调用一样**
        - **屏蔽远程调用跟本地调用的区别**，让我们感觉就是调用项目内的方法；
        - **隐藏底层网络通信的复杂性**，让我们更专注于业务逻辑。

- Websocket API
    - 使用 **JSON对象传递数据** 的 **现代 Web API 开发方式**
    - **服务器可以向连接的客户端发送回调消息**

- GraphQL，**REST 的替代方案**
    - 由Facebook于2012年创建，并于2015年开源
    - **GraphQL服务将性能作为首要任务，而RESTful服务则保持可靠性。**
    - 是一种专门为 API 开发的查询语言

# 在哪里可以找到新的 API？
- 可以在 API 市场和 API 目录中找到新的 Web API
- API 市场是一个开放的平台，任何人都可以在这里上架 API 来出售
- API 目录是受目录拥有者监管的受控存储库。
- 部分热门 API 网站
    - Rapid API 
        – 全球最大的 API 市场，拥有 10000 多个公有 API 和 100 万名在线活跃的开发人员。
        - RapidAPI 允许用户在购买之前**直接在平台上测试 API**。
    - Public APIs 
        – 该平台将远程 API 分为 40 个细分类别
    - APIForThat 和 APIList 
        – 这两个网站提供包含 500 多个 Web API 的列表

# API 端点（API endpoint）
- 说白了，就是服务器
- API 端点是 API 通信系统中的最终接触点

