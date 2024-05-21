# 总结SmartChatContent
回顾完成SmartChatContent功能模块开发与测试的关键环节

## 1. 数据库表设计 smart_chat_content_table，包括必要字段
Libraries -> Smarties.Core -> DbUpFile -> Scripts_2024
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/b90bbec5-068d-4eaa-91f0-e6738d20f864)

Libraries-> Smarties.Core  -> Domain(填入必要字段，注意需要加上 created_date \ created_by \ last_modified_by \ last_modified_date )
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/bae3954a-40b1-427b-a272-6f3545253fa5)


## 2. 接口与逻辑的实现
开发流程图：   
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/e9fee59c-40ee-4f0e-ad39-a8d5859dc548)

### （1）DTO 
DTO 不一定需要，要看实际情况。用在系统的不同层次之间传递数据，简单封装数据

Message -> DTO
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/8f49c8e1-db00-49d6-8774-c9fd3b264adf)

### （2）Request & Command & Response 
- Command: 用于封装客户端向服务端发送的请求数据，如 AddSmartChatContentCommand。
- Request: 如果需要用 mediator 执行 Get 方法，则需要将对应的 Command 改为对应的 Request
- Response: 定义服务端返回给客户端的数据结构，如 GetSmartChatContentResponse。
  
Message -> Commands

表明 AddSmartChatContentResponse 类是从另一个名为 SmartiesResponse 的类泛型版本继承而来。
这里的泛型参数 <SmartChatContentDto> 指定了 SmartiesResponse 类内部处理的数据类型。这意味着 AddSmartChatContentResponse 类将专门用于处理 SmartChatContentDto 类型的数据。  

添加那个接口需要用到的字段在里面，如 Add接口 需要用到下面这四个 
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/8ff09eb2-807e-4fd0-b184-663a58ae1576)

Message -> Requests

用于封装从智能聊天内容查询操作返回的数据，Result 和 Count
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/05ae8476-93b7-4b76-8ba8-be98c6f63e33)

### （3）Controller
负责接收HTTP请求，调用Mediator分发请求至相应的Handler处理。

Facade -> Smarties.Api -> Controllers
除了 Get 接口外，路由处 都是 HttpPost，Get 接口是 HttpGet  
####  ▶️ [FromQuery]、[FromBody] & [FromRoute]

- [FromQuery]
  - 何时使用：当你需要从URL的查询字符串中获取参数值时使用。这适用于GET请求，因为GET请求的数据通常放在URL中。
  - 适用场景：适合获取过滤、排序或分页等查询条件，如API中常见的search, page, size等参数。

- [FromBody]
  - 何时使用：当参数值应该从请求体（Request Body）中获取时使用。这对于 POST、PUT、PATCH 等请求类型很常见，因为这些请求通常携带更多的数据，且数据格式多样（如JSON、XML）。
  - 适用场景：适用于提交表单数据、JSON对象或任何非简单类型的数据，特别是当数据量大或结构复杂时。

- [FromRoute]
  - 何时使用：当参数值应从URL路径段中获取时使用。例如，在 RESTful 风格的API中，资源ID通常作为路径的一部分传递，这时就可以使用 [FromRoute]。
  - 适用场景：用于标识资源的唯一标识符，如 /users/{userId} 中的 userId 

![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/e8fcb3b5-a65c-4f9d-bc40-864dae72fe6c)

### （4）Handler
- 调用Service层执行业务逻辑。
- 根据Request构建并执行Command或直接调用Service方法。
根据类型选择 Handlers 文件夹 
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/ae77d998-f6d3-47a3-96c6-353beda305ef)

Libraries -> Smarties.Core -> Handlers

AddSmartChatContentCommandHandler 类负责处理添加智能聊天内容的命令。它通过调用智能聊天系统服务的异步方法来添加内容，并在处理完成后发布一个事件，最后返回一个响应对象，其中包含添加的内容数据。
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/ddfc0af3-78b0-45ae-b3e4-29bd95a16f0f)

### （5）Service
定义业务逻辑，如查询、创建、更新或删除SmartChatContent。 

Libraries -> Smarties.Core -> Services -> 20SystemData
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/816483cf-6c31-4982-82be-62dca53b6628)

写逻辑
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/abe965b5-88d9-4418-a816-76cda3e86e41)

### （6）DataProvider
- 数据访问层，实现与数据库的交互
- 定义了智能聊天系统数据 提供器接口ISmartChatSystemDataProvider 和 它的实现类 SmartChatSystemDataProvider
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/adef86d5-3256-4166-b7f9-e10d8eddd847)

接口 ISmartChatSystemDataProvider

删除内容：DeleteSmartChatContentAsync方法用于根据contentId和collectionId删除智能聊天内容，可选参数forceSave决定是否强制保存更改，cancellationToken用于取消操作。

添加内容：AddSmartChatContentAsync方法用于添加新的智能聊天内容到数据库。

添加集合内容：AddSmartChatCollectionContentAsync方法用于添加智能聊天内容到特定的集合中。

获取内容：GetSmartChatContentAsync方法用于根据条件异步获取智能聊天内容列表，支持分页查询，返回内容列表和总数量。

![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/eb79f3d0-cd69-459f-a183-50535b7148bd)

实现类 SmartChatSystemDataProvider

删除内容实现：在DeleteSmartChatContentAsync中，首先通过连表查 SmartChatCollectionContent 找到与给定 contentId 的记录，然后根据 collectionId 和 forceSave 参数决定删除哪些记录，并最终保存更改到数据库。

添加内容实现：AddSmartChatContentAsync 和 AddSmartChatCollectionContentAsync 方法通过插入新记录到数据库并保存更改，实现了内容的添加功能。

获取内容实现：在 GetSmartChatContentAsync 方法中，首先构建了一个查询来关联 SmartChatCollectionContent 和 SmartChatContent 表，根据 collectionId 进行筛选，并按创建日期降序排序。接着计算总数Count，并根据分页参数进行跳过和取数据操作，最后使用AutoMapper将查询结果投影到DTO(SmartChatContentDto)并返回。

## 3. 编写测试用例
Test -> Smarties.IntegrationTests -> Services

编写测试的用例，需要建立一些 unhappy case 进行测试

测试的目的是验证智能聊天系统的某些功能，包括删除、获取和添加智能聊天内容
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/0f38a6b0-3031-409e-ab16-a2b8cdb4c2c5)


## 4. 实施接口测试
运行测试，看看能不能测试成功。如果有错误需要 debug 断点后找出问题
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/888a46f9-10b8-4f65-a780-8c5d9901544e)

