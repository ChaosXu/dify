# Dify 项目架构文档

## 1. 逻辑视图

### 1.1 组件结构

```mermaid
graph TD
    A[Dify 应用] --> B[API 层]
    A --> C[Web 层]
    
    B --> B1[Controllers]
    B --> B2[Core Services]
    B --> B3[Models]
    B --> B4[Extensions]
    
    B1 --> B1A[Console API]
    B1 --> B1B[Service API]
    B1 --> B1C[Web API]
    
    B2 --> B2A[Agent]
    B2 --> B2B[App]
    B2 --> B2C[RAG]
    B2 --> B2D[Workflow]
    B2 --> B2E[Model Runtime]
    
    C --> C1[Next.js 前端]
    C --> C2[React 组件]
    C --> C3[Hooks]
    C --> C4[Services]
    
    B <--> D[(数据库)]
    B <--> E[(Redis)]
    B <--> F[(向量存储)]
```

### 1.2 核心模块说明

- **Agent**: 实现智能体(Agent)功能，包括工具调用、决策制定等
- **App**: 应用管理模块，处理不同类型的应用(聊天应用、工作流应用等)
- **RAG**: 检索增强生成(Retrieval-Augmented Generation)相关功能
- **Workflow**: 工作流引擎，实现可视化编排和执行
- **Model Runtime**: 模型运行时，统一接口访问各种大语言模型

## 2. 开发视图

```mermaid
graph TD
    A[dify] --> B[api]
    A --> C[web]
    A --> D[docker]
    A --> E[sdks]
    
    B --> B1[controllers]
    B --> B2[core]
    B --> B3[models]
    B --> B4[services]
    B --> B5[extensions]
    B --> B6[tasks]
    
    B2 --> B2A[agent]
    B2 --> B2B[app]
    B2 --> B2C[rag]
    B2 --> B2D[workflow]
    B2 --> B2E[model_runtime]
    
    C --> C1[app]
    C --> C2[components]
    C --> C3[services]
    C --> C4[hooks]
    C --> C5[context]
    
    subgraph 分层结构
        B1A[表现层]
        B1B[业务逻辑层]
        B1C[数据访问层]
        B1D[公共组件层]
        
        B1A --- B1B
        B1B --- B1C
        B1C --- B1D
    end
```

## 3. 进程视图

```mermaid
graph TD
    A[Web 浏览器] <--> B[Web 服务器]
    A <--> C[API 服务器]
    
    B --> D[API 服务器]
    C <--> D
    
    D --> E[Celery Worker 1]
    D --> F[Celery Worker 2]
    D --> G[Celery Worker N]
    
    E <--> H[(数据库)]
    F <--> H
    G <--> H
    
    E <--> I[(Redis)]
    F <--> I
    G <--> I
    
    D <--> J[(向量数据库)]
    E <--> J
    F <--> J
    G <--> J
    
    subgraph 应用服务器
        D
    end
    
    subgraph 异步工作进程
        E
        F
        G
    end
    
    subgraph 数据存储
        H
        I
        J
    end
```

## 4. 物理视图

```mermaid
graph TD
    A[客户端<br/>浏览器] --> B[负载均衡器]
    
    B --> C1[Web 服务器 1]
    B --> C2[Web 服务器 2]
    B --> C3[Web 服务器 N]
    
    B --> D1[API 服务器 1]
    B --> D2[API 服务器 2]
    B --> D3[API 服务器 N]
    
    D1 --> E[Celery Workers]
    D2 --> E
    D3 --> E
    
    E --> F[(PostgreSQL)]
    E --> G[(Redis)]
    E --> H[(Weaviate/Qdrant)]
    
    subgraph 应用层
        C1
        C2
        C3
        D1
        D2
        D3
    end
    
    subgraph 服务层
        E
    end
    
    subgraph 存储层
        F
        G
        H
    end
```

## 5. 场景视图

### 5.1 用户创建聊天应用流程

```mermaid
sequenceDiagram
    participant U as 用户
    participant W as Web 前端
    participant API as API 服务器
    participant DB as 数据库
    participant CW as Celery Worker
    
    U->>W: 登录系统
    W->>API: 发起认证请求
    API->>DB: 验证用户凭据
    DB-->>API: 返回验证结果
    API-->>W: 返回认证令牌
    W-->>U: 显示主界面
    
    U->>W: 创建新应用
    W->>API: 发送创建应用请求
    API->>DB: 存储应用信息
    DB-->>API: 返回确认
    API-->>W: 返回应用详情
    W-->>U: 显示新应用
    
    U->>W: 配置提示词
    W->>API: 更新应用配置
    API->>DB: 更新应用配置
    DB-->>API: 返回确认
    API-->>W: 返回更新结果
    W-->>U: 显示成功消息
```

### 5.2 用户运行聊天应用流程

```mermaid
sequenceDiagram
    participant U as 用户
    participant W as Web 副本
    participant API as API 服务器
    participant CW as Celery Worker
    participant VS as 向量存储
    participant LM as 大语言模型
    
    U->>W: 输入问题
    W->>API: 发送聊天消息
    API->>CW: 异步处理任务
    CW->>VS: 检索相关知识
    VS-->>CW: 返回检索结果
    CW->>LM: 调用模型生成回复
    LM-->>CW: 返回生成内容
    CW->>API: 返回处理结果
    API-->>W: 推送响应
    W-->>U: 显示回复
```

## 总结

Dify 采用前后端分离的架构设计，后端基于 Flask 框架构建，前端使用 Next.js 框架。系统通过模块化设计实现了良好的可扩展性和可维护性，同时利用 Celery 实现异步任务处理，提高了系统的响应能力。整体架构支持水平扩展，能够满足不同规模的部署需求。
