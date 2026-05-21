# User Materials Register

| id | type | title_or_label | user_priority | use_for | key_takeaway | limits_or_cautions |
| --- | --- | --- | --- | --- | --- | --- |
| U1 | source code | app.py — Streamlit 主入口 | must_use | 路由结构、页面组织、导航设计 | 7 大模块 + 20 个子页面，两级侧边栏导航 | — |
| U2 | source code | src/db/operations.py — 数据库操作层 | must_use | 数据架构、28 张表设计、CRUD 操作 | SQLite 单文件数据库，60s 超时防锁，幂等 DDL | — |
| U3 | source code | src/llm/biz_analyzer.py — 主营业务分析引擎 | must_use | LLM 调用链、多模型并行、证据溯源 | ThreadPoolExecutor(max=8)，两种上下文模式 | — |
| U4 | source code | src/ontology/owl_store.py — 本体存储层 | must_use | Owlready2 使用方式、OWL CRUD | 每个 .sqlite 文件一个本体，World 管理 | — |
| U5 | source code | src/ontology/swrl_builder.py — SWRL 规则编译器 | must_use | 规则形式化、DNF 分解 | AND/OR/NOT → 标准 SWRL | — |
| U6 | source code | src/parser/ — 文档解析器 | must_use | docx/pdf 解析、结构化提取 | 12 章标准骨架 + 大纲 + 自由文本三种模式 | — |
| U7 | source code | src/fin/calculator.py — 财务指标计算引擎 | must_use | AST 安全公式求值、拓扑排序 | 234 项指标，4 种计算模式 | — |
| U8 | source code | config.yaml — 配置文件 | must_use | LLM 模型配置、提示词模板 | 18 个模型，单步提示词含详细指令 | — |
| U9 | source code | src/llm/regulation_analyzer.py — 制度规则拆解引擎 | must_use | 制度文档解析、规则提取 | 三种粒度模式，基础对象管理 | — |
| U10 | source code | src/llm/industry_tagger.py — 智能打标引擎 | must_use | 多模型并行打标、置信度评分 | 三层标签体系，会话快照隔离 | — |
| U11 | source code | src/ontology/policy_refinement_apply.py — 制度驱动本体完善 | must_use | TBox 层本体自动扩展 | LLM 建议 → 校验 → 批量写入 | — |
| U12 | source code | src/ontology/policy_instance_store.py — 制度实例化 | must_use | ABox 层实例创建 | 政策/条款/规则实例 + 关系链接 | — |
