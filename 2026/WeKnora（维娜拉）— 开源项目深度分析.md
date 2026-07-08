

> 本文档基于对 [Tencent/WeKnora](https://github.com/Tencent/WeKnora) 源码（v0.6.3）的逐层剖析，系统梳理其**项目定位、应用架构、技术架构与技术栈**，供技术评估、二次开发与选型参考。
>
> 官方入口文档请参见 [README.md](./README.md) / [README_CN.md](./README_CN.md)。

---

## 目录

- [一、项目定位](#一项目定位)
- [二、核心能力](#二核心能力)
- [三、技术栈总览](#三技术栈总览)
- [四、应用架构](#四应用架构)
- [五、技术架构](#五技术架构)
- [六、目录结构](#六目录结构)
- [七、部署形态](#七部署形态)
- [八、快速开始](#八快速开始)
- [九、关键设计亮点](#九关键设计亮点)

---

## 一、项目定位

**WeKnora（维娜拉）** 是腾讯开源的一款基于大语言模型（LLM）的企业级**知识管理框架**，面向文档理解、语义检索与智能推理场景。

它围绕三大核心能力构建：

| 能力 | 说明 |
|------|------|
| **RAG 快速问答** | 基于检索增强生成的知识库问答，适用于日常快速查询 |
| **ReAct 智能体（Agent）** | 自主编排知识检索、MCP 工具与网络搜索，处理复杂多步推理任务 |
| **Wiki 模式** | Agent 将原始文档蒸馏为自维护、相互链接的 Markdown 知识库，并生成可视化知识图谱 |

配合多源数据接入（飞书 / Notion / 语雀 / RSS）、20+ LLM 厂商集成、全链路 Langfuse 可观测、企业级多租户 RBAC，以及完全可自部署的模块化架构，WeKnora 将分散的文档转化为**可查询、可推理、可持续演进**的知识资产。

- **协议**：MIT License
- **当前版本**：v0.6.3
- **代码仓库**：https://github.com/Tencent/WeKnora

---

## 二、核心能力

### 智能对话
- **智能推理**：ReAct 渐进式多步推理，自主编排知识检索、MCP 工具与网络搜索
- **快速问答**：基于知识库的 RAG 问答
- **Wiki 模式**：Agent 驱动的结构化、互链 Markdown Wiki 自动生成
- **工具调用**：内置工具、MCP 工具（含 OAuth2 远程服务）、网络搜索
- **会话策略**：在线 Prompt 编辑、检索阈值调优、多轮上下文感知
- **引用与 RAG 进度**：内联引用气泡、共享 Markdown 渲染、分阶段 RAG 流水线进度

### 知识管理
- **知识库类型**：FAQ / 文档 / Wiki，支持文件夹导入、URL 导入、多标签管理、在线录入
- **按上传流程配置**：每批次可覆盖解析器、分块、多模态（VLM/ASR）、图谱抽取、问题生成等配置
- **数据源导入**：飞书 / Notion / 语雀 / RSS 自动同步（增量 + 全量）
- **文档格式**：PDF / Word / Txt / Markdown / HTML / EPUB / MHTML / 图片 / CSV / Excel / PPT / JSON
- **检索策略**：BM25 稀疏检索 / 稠密检索 / GraphRAG / 父子分块 / HNSW 加速 pgvector / 多维索引
- **端到端测评**：全链路可视化，召回命中率、BLEU/ROUGE 指标评估

### 集成与扩展
- **LLM**：OpenAI / Azure / Anthropic / DeepSeek / 通义千问 / 智谱 / 混元 / 豆包 / Gemini / MiniMax / NVIDIA / Novita / SiliconFlow / OpenRouter / Ollama 等 25+ 厂商
- **向量库**：PostgreSQL(pgvector) / Elasticsearch / OpenSearch / Milvus / Weaviate / Qdrant / Apache Doris / 腾讯云 VectorDB
- **对象存储**：本地 / MinIO / AWS S3 / 火山引擎 TOS / 阿里云 OSS / 金山云 KS3 / 华为云 OBS
- **IM 渠道**：企业微信 / 飞书 / Slack / Telegram / 钉钉 / Mattermost / 微信
- **网站嵌入**：Embed Widget，支持域名白名单、限流、安全模式令牌交换
- **网络搜索**：DuckDuckGo / Bing / Google / Tavily / 百度 / Ollama / SearXNG

### 平台能力
- **部署**：本地 / Docker / Kubernetes(Helm)，支持私有化与离线部署
- **UI 形态**：Web UI / RESTful API / CLI（`weknora`）/ Chrome 扩展 / 网站嵌入 / 微信小程序 / Wails 桌面端
- **访问控制**：多租户 RBAC，4 级角色矩阵（Owner / Admin / Contributor / Viewer）+ 每知识库资源归属 + 租户审计日志
- **安全**：API Key 与凭据 AES-256-GCM 落盘加密；应用与 docreader 间 gRPC TLS + Token；SSRF 防护；Agent Skills 沙箱隔离
- **可观测性**：集成 Langfuse（唯一追踪后端），覆盖 ReAct 循环、Token 统计、工具调用与流水线追踪

---

## 三、技术栈总览

### 后端核心（Go）

| 类别 | 技术选型 |
|------|----------|
| **语言** | Go 1.26.0 |
| **Web 框架** | Gin (`gin-gonic/gin`) + CORS / Swagger (`swaggo`) |
| **依赖注入** | uber/dig（单一组合根 `internal/container/container.go`） |
| **ORM / 数据库** | GORM + golang-migrate；主库 PostgreSQL，Lite 模式 SQLite |
| **缓存 / 队列** | Redis + **Asynq**（异步任务，Redis 后端）；Lite 模式下退化为进程内 `SyncTaskExecutor` |
| **向量检索** | pgvector / Elasticsearch(v7,v8) / OpenSearch / Milvus / Weaviate / Qdrant / Apache Doris / 腾讯 VectorDB / sqlite-vec |
| **分析型查询** | DuckDB（Agent 数据分析工具，加载 CSV/Excel 到内存表） |
| **图数据库** | Neo4j（GraphRAG / 知识图谱） |
| **gRPC** | gRPC + Protobuf（与 docreader 通信） |
| **认证** | JWT (`golang-jwt/jwt/v5`) + API Key + OIDC |
| **配置** | Viper + 环境变量 + YAML（`config/`） |
| **并发** | `panjf2000/ants` 协程池 |
| **WebSocket / SSE** | `gorilla/websocket`、`@microsoft/fetch-event-source`（流式输出） |
| **日志** | logrus + lumberjack（滚动） |
| **加密** | AES-256-GCM（凭据落盘） |
| **LLM SDK** | `sashabaranov/go-openai`、自研 Anthropic/Ollama 适配；Token 计数 `tiktoken-go` |
| **MCP SDK** | `mark3labs/mcp-go`（Go 端 MCP 客户端/服务端） |

### 文档解析服务（docreader，Python）

| 类别 | 技术选型 |
|------|----------|
| **语言/运行时** | Python ≥ 3.10.18，uv 包管理 |
| **通信** | gRPC（`grpcio` + `grpcio-tools` + health-check） |
| **PDF** | `pypdfium2`(pdfium 渲染/文本)、`pypdf`、可选 OpenDataLoader（版面分析，需 JVM） |
| **Word/Excel/PPT** | `python-docx`、Microsoft `markitdown`、`pandas` + `openpyxl`/`xlrd`、LibreOffice(`soffice`)、`antiword` |
| **网页/HTML** | Playwright(WebKit)、`trafilatura`、BeautifulSoup、`lxml` |
| **EPUB/MHTML** | `ebooklib`、Python `email` 模块 |
| **图像** | Pillow（图片返回原图字节，OCR/VLM 由 Go 侧完成） |

### 前端（Vue 3）

| 类别 | 技术选型 |
|------|----------|
| **框架** | Vue 3（Composition API）+ TypeScript |
| **构建** | Vite 7（主构建器）+ `@vitejs/plugin-vue-jsx` |
| **UI 库** | TDesign Vue Next |
| **状态管理** | Pinia |
| **路由** | Vue Router |
| **国际化** | vue-i18n（简中 / 英语 / 韩语 / 俄语，legacy: false） |
| **HTTP / 流式** | Axios + `@microsoft/fetch-event-source`（SSE 流式对话） |
| **富文本渲染** | marked + marked-katex-extension + highlight.js + Mermaid + DOMPurify |
| **办公文档预览** | `@vue-office/pptx`、`docx-preview`、`xlsx` |
| **静态搜索** | pagefind |

### 扩展端

| 端 | 技术栈 |
|----|--------|
| **CLI**（`weknora`） | Go + Cobra + Charm(huh/bubbletea) + 官方 MCP Go SDK；内置只读 10 工具 MCP 服务 |
| **MCP Server**（`mcp-server/`） | Python + 官方 `mcp` SDK；stdio / SSE / HTTP 三种传输，29 个工具 |
| **桌面端**（`cmd/desktop/`） | Wails v2，内嵌完整 Go 后端 + 反向代理前端 |
| **微信小程序**（`miniprogram/`） | 原生微信小程序 |
| **Chrome 扩展** | 浏览器扩展，一键采集网页内容入库 |

---

## 四、应用架构

### 4.1 整体应用拓扑

WeKnora 是一个**多语言、多端、多服务**的工程，整体由以下部分组成：

```
┌─────────────────────────────────────────────────────────────────────┐
│                         客户端 / 接入层                                │
│  Web UI │ CLI │ Chrome 扩展 │ 网站嵌入 │ 微信小程序 │ Wails 桌面端 │ IM 渠道 │
└──────────────┬───────────────────────────────────┬──────────────────┘
               │ HTTP / SSE / WebSocket            │
               ▼                                   ▼
┌──────────────────────────────┐   ┌──────────────────────────────────┐
│   WeKnora 主应用 (Go · Gin)   │   │   MCP Server / MCP 工具           │
│  RESTful API + RBAC + 多租户  │◄──┤  Python(stdio/SSE/HTTP)          │
│  Chat Pipeline · ReAct Agent  │   │  Go CLI(stdio, 只读)              │
└───────┬──────────┬────────────┘   └──────────────────────────────────┘
        │ gRPC     │ 依赖
        ▼          ▼
┌──────────────┐ ┌────────────────────────────────────────────────────┐
│  docreader   │ │           基础设施层 (可插拔)                          │
│  Python      │ │ PostgreSQL │ Redis+Asynq │ MinIO/S3/... │ Neo4j     │
│  文档解析     │ │ pgvector/Milvus/Qdrant/Weaviate/ES/OpenSearch/...   │
└──────────────┘ │ Langfuse(可观测) │ SearXNG │ Sandbox(Docker) │ ODL    │
                 └────────────────────────────────────────────────────┘
                                        ▼
                            外部 LLM / Embedding / Rerank / VLM / ASR
                  (OpenAI / DeepSeek / Qwen / 智谱 / 混元 / Gemini / Ollama / ...)
```

### 4.2 三大运行形态

WeKnora 支持三种部署形态，**同一套代码**通过环境变量切换：

| 形态 | 说明 | 适用场景 |
|------|------|----------|
| **完整模式（Full）** | 全部容器化服务，Redis + Asynq 异步队列、多向量引擎、可观测齐全 | 生产 / 团队 |
| **开发模式（Dev）** | `make dev-start` 仅启动基础设施，后端用 Air 热重载、前端 Vite 热更新 | 开发调试 |
| **Lite 模式** | 单一二进制，SQLite + 进程内队列，无需 Redis | 个人 / 单机 / 桌面端 |

> 关键：当 `REDIS_ADDR` 未设置时，DI 容器自动用 `SyncTaskExecutor` + `NoopTaskInspector` 替换 Asynq，实现"零外部依赖"的单机运行。

### 4.3 模块划分（仓库视角）

| 目录 | 角色 | 语言 |
|------|------|------|
| `cmd/server/` | 服务端入口（main + bootstrap） | Go |
| `internal/` | 后端业务核心（分层架构） | Go |
| `frontend/` | Web 前端 SPA | Vue 3 / TS |
| `docreader/` | 文档解析微服务 | Python |
| `mcp-server/` | MCP Server（工具桥接） | Python |
| `cli/` | 命令行工具 `weknora` | Go |
| `client/` | Go SDK（供 CLI 使用） | Go |
| `cmd/desktop/` | Wails 桌面端外壳 | Go |
| `miniprogram/` | 微信小程序 | 原生 |
| `skills/preloaded/` | 内置 Agent Skills | Markdown/Python |
| `helm/` | Kubernetes Helm Chart | YAML |
| `docker/` | Dockerfile 与部署配置 | Dockerfile |
| `config/` | 应用配置 + Prompt 模板 | YAML |

---

## 五、技术架构

### 5.1 后端分层架构

后端采用**清晰分层 + 依赖注入**设计，自底向上：

```
┌─────────────────────────────────────────────────────┐
│  Router (Gin)  ← 中间件链: CORS/Auth/RBAC/审计/限流    │  internal/router/
├─────────────────────────────────────────────────────┤
│  Handler (HTTP 控制器, ~75 个)                        │  internal/handler/
├─────────────────────────────────────────────────────┤
│  Application Service (领域服务, ~50 个)                │  internal/application/service/
│  ├─ chat_pipeline (RAG 插件链)                        │
│  └─ retriever (检索引擎注册表)                        │
├─────────────────────────────────────────────────────┤
│  Repository (GORM 数据访问, ~40 个)                   │  internal/application/repository/
├─────────────────────────────────────────────────────┤
│  Types (领域实体 + 接口契约, ~90 个)                   │  internal/types/
├─────────────────────────────────────────────────────┤
│  Infra: DocParser / Chunker / WebSearch / Sandbox     │  internal/infrastructure/
├─────────────────────────────────────────────────────┤
│  Models: Chat / Embedding / Rerank / VLM / ASR / Provider │  internal/models/
└─────────────────────────────────────────────────────┘
        ▲
        │ 全部由 container (uber/dig) 装配注入
```

**关键点：**
- **依赖注入**：`internal/container/container.go` 是唯一的组合根，通过 `dig.Container` 注册全部 Provider，按环境变量动态选择具体基础设施（DB 驱动、存储类型、检索引擎列表）。
- **接口解耦**：`types/interfaces/` 定义全部服务/仓储接口，实现可替换。
- **启动容错**：`bootstrap` 为"尽力而为"，绝不阻断启动（如：仅在无系统管理员时才提升 bootstrap 用户，幂等）。

### 5.2 两条 AI 执行路径（核心架构特色）

WeKnora 区分**确定性 RAG** 与**自主 Agent** 两条路径，共享同一套检索引擎与 LLM 抽象：

#### 路径 A：Chat Pipeline（RAG 插件链）
入口 `/knowledge-chat/:session_id`。`internal/application/service/chat_pipeline/` 实现了一个**可反转的插件链**，每个插件调用 `next()`：

```
query.received → query.rewrite → retrieval.start
  → retrieval.vector / retrieval.keyword / retrieval.entity
  → rerank.start → merge.start → chat.start → chat.stream → chat.complete
```

插件清单：`query_understand`（查询理解）、`query_expansion`（查询扩展）、`load_history`、`extract_entity`、`search_entity`、`search`（并发知识库 + 网络搜索）、`rerank`、`web_fetch`、`filter_top_k`、`merge`（含 `merge_expand`/`merge_faq`/`merge_history`/`merge_overlap` 父子/重叠块扩展）、`wiki_boost`、`memory`、`data_analysis`、`into_chat_message`、`chat_completion[_stream]`。

#### 路径 B：ReAct Agent（自主推理）
入口 `/agent-chat/:session_id`。`internal/agent/engine.go` 的 `AgentEngine` 实现 Think → Analyze → Act → Observe 循环：

- **无状态**：每轮从 DB 重建对话历史，引擎内不缓存。
- **流式思考**：分离 `<think>`/`reasoning_content` 与正文，分别推送"思考"与"答案"通道。
- **并行工具**：`ParallelToolCalls` 且工具数 ≥ 2 时用 errgroup 并发执行。
- **优雅降级**：LLM 硬失败但有工具结果时，基于已有结果合成最终答案；上下文取消时抢救现有结果。
- **卡死检测**：相同内容重复 N 轮自动跳出。
- **Human-in-the-loop**：MCP 工具调用可配置审批门（`approval/gate.go`），Agent 暂停等待人工放行。

**Agent 内置工具矩阵：**

| 类别 | 工具 |
|------|------|
| 知识检索 | `knowledge_search`（语义+可选 rerank+MMR 去重）、`grep_chunks`（关键词/正则）、`list_knowledge_chunks`、`get_document_info` |
| 结构化数据 | `database_query`（租户级 SQL，强制安全校验）、`data_analysis`（DuckDB）、`data_schema` |
| 知识图谱 | `query_knowledge_graph`（Neo4j） |
| 网络 | `web_search`、`web_fetch` |
| Wiki | `wiki_read/write/replace_text/rename/delete_page`、`wiki_search`、`wiki_read_source_doc`、`wiki_flag/read/update_issue` |
| 推理辅助 | `thinking`、`todo_write` |
| Skills | `read_skill`、`execute_skill_script`（沙箱执行） |
| MCP | 动态注入已注册 MCP 服务工具，支持每用户 OAuth |

> 安全：`ToolRegistry.RegisterTool` 采用**先到先得**策略，防止工具名劫持（对应安全公告 GHSA-67q9-58vj-32qx）。

### 5.3 检索引擎抽象（RAG）

`internal/application/service/retriever/` 定义了**多引擎、多类型**的统一检索层：

- **11 种引擎类型**：postgres、sqlite、elasticsearch、opensearch、qdrant、milvus、weaviate、doris、tencent_vectordb、infinity、elasticfaiss。
- **3 种检索器类型**：`keywords`(BM25)、`vector`(语义)、`websearch`。
- **双注册表**：`byEngineType`（环境变量配置的全局引擎）+ `byStoreID`（DB 中 `VectorStore` 记录的按需引擎）。
- **CompositeRetrieveEngine**：包装多个引擎，对每个 `RetrieveParams` 分派给支持它的引擎，并发检索。
- **混合索引**：`keywords_vector_hybrid_indexer.go` 同时写入关键词与向量索引。
- **跨引擎归一化**：`EngineAwareNormalizer` 对异构引擎打分归一化。
- **租户隔离**：`CreateRetrieveEngineForKB` 校验租户对 VectorStore 的归属，违例返回 `ErrVectorStoreForbidden`。

**检索后处理**（`internal/searchutil/`）：`chunkmerge.go` 的 `AppendWithOverlap` 基于**文本**（而非位置）检测重叠以重建父文档，对表格头重插与 HTML 实体鲁棒。

### 5.4 文档解析服务 docreader

docreader 是**独立 Python gRPC 微服务**，与 Go 主应用解耦：

- **契约**：gRPC `DocReader` 服务，三个 RPC：`Read`（一元）、`ReadStream`（流式，优先，规避大消息上限）、`ListEngines`。
- **职责边界**：docreader **只负责解析为 Markdown + 返回原始图片字节**，**不做** OCR/VLM/分块/图片持久化——这些由 Go 侧完成（架构上明确解耦）。
- **三种解析引擎**：
  - `builtin`：pdfium(PDF) / python-docx(Word) / pandas(Excel) / Playwright+trafilatura(网页) / ebooklib(EPUB) 等
  - `markitdown`：微软 markitdown 库
  - `opendataloader`：版面分析（需 JVM，可选，支持 hybrid 模式）
- **组合模式**：`FirstParser`（责任链，取首个有效结果）+ `PipelineParser`（流水线，逐级转换）。
- **PDF 智能路由**：按图片面积占比判定"扫描页"，扫描页渲染为 JPEG 交由 Go 侧 OCR/VLM；文本页做版面感知抽取（XY-cut 多栏、字号标题提升）。
- **安全**：`SandboxExecutor` 阻断 LibreOffice/antiword 网络外发（默认黑洞代理）；SSRF URL 守卫与 Go 侧镜像。

### 5.5 MCP 集成（Model Context Protocol）

WeKnora 既是 MCP **客户端**也是 **服务端**：

| 角色 | 实现 | 工具数 | 传输 |
|------|------|--------|------|
| **客户端**（`internal/mcp/`） | `mark3labs/mcp-go`，Agent 动态加载已注册 MCP 服务工具 | 动态 | stdio/SSE/HTTP，支持 OAuth2、每用户凭证、Human-in-the-loop 审批 |
| **服务端 · Python**（`mcp-server/`） | 官方 `mcp` SDK，桥接 WeKnora REST API | 29 | stdio / SSE / HTTP（Bearer 认证） |
| **服务端 · Go CLI**（`cli/cmd/mcp/`） | 官方 MCP Go SDK，只读 | 10 | stdio，安全默认（破坏性操作走"询问用户"协议） |

### 5.6 多租户与安全

- **RBAC 4 级矩阵**：Owner / Admin / Contributor / Viewer，叠加每知识库归属、每 Agent 归属。
- **作用域 API Key**：带能力标签（`apiKeyRetrieve`/`apiKeyIngest`/`apiKeyChat`/`apiKeyManageModels` 等），未声明路由对 API Key **默认拒绝**。
- **启动校验**：`assertAPIKeyPoliciesMatchRoutes` 在启动时断言声明的策略都指向真实路由，防止 403 静默漂移。
- **加密**：API Key、MCP/数据源凭据 AES-256-GCM 落盘加密，支持平滑密钥轮换。
- **传输**：应用与 docreader 间 gRPC TLS + Bearer Token。
- **SSRF**：内置 SSRF 安全 HTTP 客户端。
- **沙箱**：Agent Skills 脚本在 Docker/本地沙箱执行（`internal/sandbox/`，支持超时）。

### 5.7 可观测性

- **Langfuse**（唯一追踪后端）：覆盖 ReAct 循环（`agent.round.N` span）、LLM 调用、Token 统计、工具调用、RAG 流水线。
- **文档解析追踪**：内置 Langfuse 风格的解析 trace 时间线，分阶段进度展示。
- **事件总线**：`internal/event/` 进程内 pub/sub，~30 种事件类型，驱动实时 SSE 流推送到前端。
- **结构化流水线日志**：`common/tools.go` 的 `PipelineInfo`/`PipelineError` 贯穿 Agent 全链路。

### 5.8 异步任务

- **完整模式**：Asynq（Redis 后端），`router/task.go` 入队 / `task_inspector.go` 检查与按 knowledge-id 取消。
- **Lite 模式**：`SyncTaskExecutor`（进程内 goroutine）+ `NoopTaskInspector`。
- **数据库迁移**：golang-migrate，版本升级时自动执行（`internal/database/migration.go`）。

---

## 六、目录结构

```
WeKnora/
├── cmd/
│   ├── server/          # 服务端入口（main + bootstrap）
│   ├── desktop/         # Wails 桌面端外壳
│   └── download/        # DuckDB 扩展下载工具
├── internal/            # ★ 后端业务核心（分层架构）
│   ├── agent/           #   ReAct 引擎、工具注册表、Skills、审批门
│   ├── application/     #   领域服务 + 仓储（repository / service）
│   │   ├── service/chat_pipeline/   # RAG 插件链
│   │   └── service/retriever/       # 检索引擎注册表
│   ├── container/       #   uber/dig 依赖注入组合根
│   ├── router/          #   Gin 路由 + RBAC 守卫 + Asynq 任务
│   ├── handler/         #   HTTP 控制器（~75 个）
│   ├── middleware/      #   Auth / RBAC / 限流 / 审计
│   ├── models/          #   LLM 抽象（chat/embedding/rerank/vlm/asr/provider）
│   ├── infrastructure/  #   DocParser / Chunker / WebSearch / WebFetch
│   ├── mcp/             #   MCP 客户端 + OAuth
│   ├── im/              #   8 个 IM 平台适配器
│   ├── datasource/      #   飞书/Notion/语雀/RSS 连接器 + 调度器
│   ├── sandbox/         #   Agent Skills 执行沙箱
│   ├── tracing/         #   Langfuse 封装
│   ├── event/           #   进程内事件总线
│   ├── stream/          #   SSE 流会话管理（Redis/内存）
│   ├── types/           #   领域实体 + 接口契约（~90 个）
│   └── ...
├── frontend/            # Vue 3 Web 前端
├── docreader/           # ★ Python 文档解析 gRPC 微服务
├── mcp-server/          # Python MCP Server（29 工具）
├── cli/                 # Go CLI 工具 weknora
├── client/              # Go SDK
├── miniprogram/         # 微信小程序
├── skills/preloaded/    # 内置 Agent Skills
├── config/              # 应用配置 + Prompt 模板
│   └── prompt_templates/#   system/rewrite/rerank/... YAML
├── migrations/          # 数据库迁移 SQL
├── docker/              # Dockerfile（app/docreader/sandbox/odl-hybrid）
├── helm/                # Kubernetes Helm Chart
├── scripts/             # 构建/部署/开发脚本
├── docs/                # 文档与图片
├── docker-compose.yml   # 完整编排
├── Makefile             # 构建/开发/迁移任务
└── .env.example         # 环境变量模板
```

---

## 七、部署形态

### 7.1 编排服务全景（docker-compose）

通过 `--profile` 组合启用，默认仅启动核心：

| 服务 | 用途 |
|------|------|
| `app` + `frontend` | 主应用 + Web UI |
| `docreader` | 文档解析（gRPC:50051） |
| `postgres` | 主数据库 |
| `redis` | 缓存 + Asynq 队列 |
| `sandbox` | Agent Skills 执行沙箱 |
| `dex` | OIDC 身份提供商 |
| `searxng` | 元搜索引擎 |
| `odl-hybrid` | OpenDataLoader 版面分析（可选） |
| Profile `neo4j` | 知识图谱 |
| Profile `minio` | 对象存储 |
| Profile `langfuse*` | 可观测（Web/Worker/ClickHouse/MinIO） |
| Profile `full` | 全量向量库等（milvus/qdrant/weaviate/doris） |

### 7.2 三套部署方式

| 方式 | 命令 | 适用 |
|------|------|------|
| **Docker Compose** | `docker compose up -d` | 推荐，一键拉起 |
| **Kubernetes** | `helm install weknora ./helm` | 生产集群 |
| **Lite 单机** | `make run-lite`（单二进制 + SQLite） | 个人/桌面 |

---

## 八、快速开始

### 前置条件
- Docker + Docker Compose
- Git

### 安装与启动

```bash
git clone https://github.com/Tencent/WeKnora.git
cd WeKnora
cp .env.example .env   # 按需编辑，文件内有详细注释
docker compose up -d   # 启动核心服务
```

启动后访问 **http://localhost**。

> 使用本地 Ollama 模型时，先 `ollama serve > /dev/null 2>&1 &`。

### 可选服务（Docker Compose Profiles）

| Profile | 说明 | 命令 |
|---------|------|------|
| _(默认)_ | 核心服务 | `docker compose up -d` |
| `full` | 全部功能 | `docker compose --profile full up -d` |
| `neo4j` | 知识图谱 | `docker compose --profile neo4j up -d` |
| `minio` | 对象存储 | `docker compose --profile minio up -d` |
| `langfuse` | 追踪 | `docker compose --profile langfuse up -d` |

可组合：`docker compose --profile neo4j --profile minio up -d`

### 服务地址

| 服务 | 地址 |
|------|------|
| Web UI | `http://localhost` |
| 后端 API | `http://localhost:8080` |
| Langfuse 追踪 | `http://localhost:3000` |

### 开发模式（推荐频繁改动时）

```bash
make dev-start       # 启动基础设施
make dev-app         # 新终端：后端（Air 热重载，5-10s 重启）
make dev-frontend    # 新终端：前端（Vite 热更新）
```

优势：前后端改动自动重载、无需重建镜像、支持 IDE 断点调试。

---

## 九、关键设计亮点

1. **双 AI 路径共享底座**：确定性 RAG 插件链与自主 ReAct Agent 共用同一套检索引擎与 LLM 抽象，既保证快速问答的稳定性，又赋予复杂任务的自主推理能力。

2. **全链路可插拔**：11 种向量引擎、9 种对象存储、25+ LLM 厂商、8 个 IM 平台、4 个数据源连接器——全部通过环境变量在启动时注入注册表，**无一处硬编码绑定**。

3. **优雅降级哲学**：bootstrap 尽力而为不阻断启动；LLM 失败回退合成答案；无 Redis 自动退化为进程内队列；误配置永不"砖化"。

4. **安全默认**：作用域 API Key 默认拒绝、工具名先到先得防劫持、AES-256-GCM 凭据加密、SSRF 守卫、Skills 沙箱隔离、启动时策略-路由一致性断言。

5. **多租户一等公民**：数据按租户隔离、每知识库归属校验延伸到检索工厂层、跨租户超级用户、租户级审计日志。

6. **同一套代码三形态**：Full / Dev / Lite 通过环境变量切换，从生产集群到单机桌面共享同一二进制。

7. **可观测内建**：Langfuse 贯穿 Agent 轮次与 LLM 调用，事件总线驱动实时 SSE，文档解析亦有分阶段 trace 时间线。

---

## 许可证

[MIT License](./LICENSE)

## 相关链接

- **官方仓库**：https://github.com/Tencent/WeKnora
- **官网**：https://weknora.weixin.qq.com
- **微信对话开放平台**：https://chatbot.weixin.qq.com
- **变更日志**：[CHANGELOG.md](./CHANGELOG.md)
- **API 文档**：[docs/api/](./docs/api/README.md)
- **常见问题**：[docs/QA.md](./docs/QA.md)
- **路线图**：[docs/ROADMAP.md](./docs/ROADMAP.md)

---

> 📝 本分析文档基于 v0.6.3 源码静态分析生成，如与最新代码有出入，以仓库实际代码为准。
