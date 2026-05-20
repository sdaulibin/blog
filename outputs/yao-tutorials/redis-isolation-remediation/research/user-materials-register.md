# User Materials Register

| id | type | title_or_label | user_priority | use_for | key_takeaway | limits_or_cautions |
| --- | --- | --- | --- | --- | --- | --- |
| U1 | problem statement | 多业务共用Redis实例的风险描述 | must_use | 角度、结构、案例 | 五大业务系统（公募基金投顾、贵金属、理财代销、财富信托、商业养老金）共用一个Redis哨兵实例，通过逻辑库隔离，存在资源竞争、配置冲突、故障传导、监控缺失、安全风险 | 用户为架构师视角，需补充运维和开发视角 |
| U2 | risk item | 共享CPU/内存/网络/进程资源 | must_use | 风险分析、架构设计 | 大Key慢查询或大量写入会拖累所有应用 | 需要具体技术原理解释 |
| U3 | risk item | 共享全局配置 | must_use | 风险分析、整改方案 | 内存上限、淘汰策略、持久化、连接数为全局配置，单一业务占用内存满导致其他业务无法写入 | 需要配置层面的解决方案 |
| U4 | risk item | 实例宕机影响范围 | must_use | 风险分析、高可用设计 | 主从切换导致所有关联业务同时受影响 | 需要拆分后的高可用方案 |
| U5 | production incident | 大Key写入导致应用异常 | must_use | 案例分析、治理方案 | 生产环境多次出现大Key问题 | 需要大Key治理的具体方案 |
| U6 | production incident | 淘汰策略设置不合理导致内存告警 | must_use | 案例分析、配置优化 | 淘汰策略不当引发内存告警 | 需要各业务独立淘汰策略设计 |
| U7 | risk item | 监控粒度不足 | must_use | 监控体系建设 | 监控到实例级别，无法按逻辑库/业务维度细化，难以快速定位归属业务系统 | 需要按业务维度的监控方案 |
| U8 | risk item | 安全隔离缺失 | must_use | 安全加固 | 连接Redis后可访问所有库数据，Redis官方不推荐多库方案 | 需要ACL和网络隔离方案 |
