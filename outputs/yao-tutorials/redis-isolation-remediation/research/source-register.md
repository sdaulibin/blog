# Source Register

| id | title | url | source_type | date_accessed | published_or_updated | authority_reason | use_for | key_takeaway | limits_or_cautions |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A1 | Redis Documentation - Keyspace | https://redis.io/docs/latest/commands/select/ | official | 2026-05-20 | 2026 | Redis官方维护文档 | 理论支撑 | Redis官方文档说明逻辑数据库不是为不同应用设计的隔离方案，建议使用多个Redis实例 | 仅覆盖官方立场，不涉及具体实施 |
| A2 | Redis Documentation - Memory Management | https://redis.io/docs/latest/management/optimization/memory-management/ | official | 2026-05-20 | 2026 | Redis官方维护文档 | 配置优化、淘汰策略 | maxmemory和淘汰策略为实例级全局配置，无法按逻辑库独立设置 | 不涉及银行场景 |
| A3 | Redis Documentation - Sentinel | https://redis.io/docs/latest/management/sentinel/ | official | 2026-05-20 | 2026 | Redis官方维护文档 | 高可用架构设计 | Sentinel监控粒度为实例级别，主从切换影响整个实例 | 不涉及多实例管理 |
| A4 | Redis Documentation - ACL | https://redis.io/docs/latest/management/security/acl/ | official | 2026-05-20 | 2026 | Redis官方维护文档 | 安全加固 | Redis 6.0+支持ACL，可按用户控制命令和Key访问权限 | 需要Redis 6.0+版本 |
| A5 | Redis Documentation - Latency | https://redis.io/docs/latest/management/optimization/latency/ | official | 2026-05-20 | 2026 | Redis官方维护文档 | 性能分析 | Redis单线程模型下，慢命令阻塞所有客户端，包括不同逻辑库的客户端 | 不涉及多实例对比 |
| G1 | redis-shake Migration Tool | https://github.com/tair-opensource/RedisShake | GitHub | 2026-05-20 | 2026-04 | 阿里云Tair团队维护的Redis迁移工具 | 数据迁移方案 | 支持在线迁移、断点续传、多线程同步，适合Redis实例拆分场景 | 需要评估版本兼容性 |
