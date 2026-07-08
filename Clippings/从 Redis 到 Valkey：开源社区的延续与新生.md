---
title: "从 Redis 到 Valkey：开源社区的延续与新生"
source: "https://mp.weixin.qq.com/s/X8FzvB2EkCANXmC4D5RF2Q"
author:
  - "[[赵钊 soloestoy]]"
published:
created: 2026-07-01
description: "从 Redis 到 Valkey：开源社区的延续与新生"
tags:
  - "clippings"
---
赵钊 soloestoy Valkey China *2026年6月23日 11:58*

## 写在前面

我是赵钊（GitHub: soloestoy），来自阿里云 Tair 团队。 **从最初开始参与 Redis 开源社区建设，后来成为 Redis core-team 成员，再到 Redis 闭源后与多位核心开发者一起创建 Valkey 社区，如今担任 Valkey TSC（Technical Steering Committee）member。**

回顾这段经历，有太多感触。 **十年来，从刚开始接触开源的新人，到站在开源社区最核心的位置，再到亲历一场堪称开源历史上最大规模的 fork 行动** ——我很幸运身处其中，也想把这一路的故事分享给大家。

## 结缘 Redis：从一个 Bug 开始

2017 年，Redis 4.0 发布，带来了 PSYNC2、lazyfree、LFU、Modules 等一系列重磅功能。当时我们的云服务准备升级到 4.0，作为云产品，服务的是用户的线上生产环境，对稳定性的要求极高。在调研过程中，我很快发现了几个高风险问题—— **PSYNC2 在特定场景下会导致主备数据不一致、lazyfree 在 FLUSHALL 时存在内存泄漏、Module 的安装卸载有 crash 风险** 等等。

我把这些问题提交给了社区，和 antirez 进行了深入讨论。 **就这样，一个 Bug 成了我踏入 Redis 开源社区的起点。**

说实话，第一次收到 antirez 回复时的心情，现在想起来还是很激动。那时候 Redis 在我心里就像一座精巧的建筑，而 antirez 就是那个建筑师。 **能直接和他对话、讨论技术细节，对一个年轻的数据库开发者来说，简直像做梦一样。**

此后的几年里，我陆续参与了 Streams 数据结构的完善、 **Pipeline 协议解析优化（这个优化让 Redis 单线程可以跑到百万 QPS，也为后来 6.0 多 IO 线程的开发奠定了基础）** 、以及 Hotkey 扫描等可观测性提升的工作。 **每一次 commit 被 merge，每一次在 Release Notes 里看到自己的名字，都是一种无法替代的满足感——你写的代码，正在全世界数以百万计的服务器上运行。**

## 关于 antirez

提到 Redis，就不得不提它的创造者 antirez。

antirez 生活在意大利西西里岛，风景很美，有一个可爱的女儿，还教她写代码。他的工作节奏大概是这样的：早上起床吃个早饭，去健身房锻炼加游泳，午饭后才正式开始工作——查看邮件、写代码、处理社区问题，晚上则陪伴家人。因为时差的关系，意大利下午恰好是中国的深夜， **我经常半夜和他讨论问题。** 而且到了夏天的 Summer Vacation，差不多有两个月找不到人，哈哈。

他本人很随性，脑洞也很大。比如 **Streams 数据结构就是突发奇想、毫无预兆地实现了出来** ；还有像 LOLWUT 这种 just for fun 的娱乐命令，每个版本画不同的 ASCII 艺术画。 **这种浪漫主义和工程能力的结合，在技术圈是非常罕见的。** 2017 年他来杭州参加云栖大会，我们在线下一起讨论 Redis 的未来，一家人也在杭州深度游了一圈，后来和我说灵隐寺的素食太好吃了，能把豆腐做出肉的味道，回意大利之后再没吃到过那么好的豆腐。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/1who6iblk2qpZ49Z7OGj8PVtEwC8dLdbMZlcSIhl0mQzvMRHialiaCzeF52ibezDCXrkdNeT6nRanzAdbrmGpWZicBs0tamJWpOdgPljs2SjVyl8/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0)

2019 年，我参加了 RedisConf，和 antirez 再次相聚。在大会现场，我们一起调试多 IO 线程的实现—— **两个人挤在一起盯着终端，一行一行地看代码逻辑，非常 geek。** 那是一段让人怀念的时光。 **现在想起来，那可能是 Redis 黄金时代的最后一个夏天。**

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1who6iblk2qoptluHQzqOHMd4zfG4X0TkP9dtakn4LlLfM6OxageF5SWXyssOl6N725gS9U7eMQQ1VKoVfzYaSEojnuQeWp2xAnQ7FXRibZ2c/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=1)

Redis 能够如此流行和成功，离不开 antirez 十多年如一日的付出。维护一个开源项目，尤其是大量应用在生产环境中的项目，其实是非常累人的——修复 Bug、开发新 Feature、听取用户意见、权衡各种取舍，都会消耗巨大的精力。 **长期的高压让他最终选择了离开。** 2020 年 6 月，antirez 宣布不再担任 Redis 的 maintainer，他在博客中写道：

"I write code in order to express myself."

—— antirez，2020.06

**对他来说，当维护开源项目变成了一种义务而非创造，是时候放手了。**

对此我深表理解和敬意。 **antirez 给这个世界留下了一个伟大的作品，这就足够了。**

## 从 Core-team 到闭源

antirez 离开后，社区成立了 core-team 来负责后续运营， **成员包括 Redis 公司的三位工程师、AWS 的 Madelyn 以及我，一共五人。** 能够加入 Redis core-team 我感到非常荣幸，这既是阿里云长期在 Redis 生态上深耕的成果，也要感谢社区和 antirez 一直以来的信任。

成为 core-team 成员后我也深有体会—— **不仅要负责 Roadmap 的制定，还有大量的 Issue 和 PR 需要处理，每周还要半夜跨时区开会。** 说实话，那几年过得并不轻松。一方面要保证 Redis 的技术质量，另一方面各方对项目方向的看法也不尽相同。但无论如何，我们几个人都在尽力让 Redis 变得更好。

然而， **2024 年 3 月，Redis 公司宣布将许可证从 BSD 变更为 SSPLv1 + RSALv2 双许可证。通俗地说——闭源了。**

这个消息来得并不意外，从 antirez 离开社区后大家都或多或少的觉得这一天注定要到来，但真正发生的时候，心里还是一沉。 **不是愤怒，更多的是惋惜。** 一个伴随了我七年的社区，一行一行写进去的代码，无数个深夜的讨论和争辩—— **这些记忆不会因为一纸许可证的变更而消失，但社区作为一个共同体，确实在那一刻被撕裂了。**

## 为什么说这是一个遗憾

我对 Redis 公司的商业决策表示理解。开源项目的可持续性一直是一个未解的难题，很多公司在"如何靠开源赚钱"这件事上挣扎多年。 **但从技术生态的角度，这个决定的时机实在让人扼腕——因为它恰好发生在 AI 时代全面爆发的前夜。**

看看隔壁 PostgreSQL 的生态：pgvector 让 PG 成为向量数据库的热门选择，mem0 等 AI 记忆服务将 PG 作为首选后端，Supabase 将 PG 打造成了 Backend-as-a-Service 的标杆…… **开源社区在 AI 浪潮中获得了前所未有的发展机遇。**

Redis 作为 KV 数据库的代表，凭借其亚毫秒级延迟的天然优势，在 AI 应用中本可以有更广阔的作为—— **语义缓存、向量检索、Agent 记忆存储、实时特征服务……这些场景对 Redis 来说简直是天作之合。** 但闭源的决定切断了社区贡献的纽带，而 Valkey 在 Search、Vector 这些方向上又需要付出额外的重建成本。 **两个社区未能形成合力，对整个生态来说是一种损失。**

## Valkey 的诞生

消息公布后的那几天，我的各种通讯工具几乎没停过。 **来自 AWS、Google、Ericsson、腾讯等的核心开发者们迅速聚在了一起** ——大家都有一个共同的想法： **BSD 许可证下最后一个版本的 Redis 不能就这么消亡，它属于整个社区。**

FROM ANNOUNCEMENT TO RELEASE

8 DAYS

从许可证变更公告到 Valkey 的第一次发布

**8 天。回过头来看这个数字，依然觉得不可思议。** 这背后是无数次跨时区的电话会议、深夜的 Slack 消息、以及每个人对开源的坚定信念。我们需要在最短时间内完成基础设施搭建、CI/CD 迁移、社区治理框架确立——同时还要起一个名字。

起名的过程很有趣，也很纠结。备选方案很多——kvdb、kiwidb、odis、redict……大家在 Slack 群里投票、争论、开玩笑。 **最终我们选择了 "Valkey"——取自 val(ue)-key 的组合，简洁有力，也寓意着对 KV 数据库精神的传承。** 说实话刚开始我还觉得这名字有点怪，但叫着叫着就顺嘴了，现在反而觉得它有一种独特的辨识度。

**Valkey 项目落地在 Linux Foundation 旗下，由阿里云、AWS、Google、腾讯、火山引擎、Ericsson 等多家公司联合推动。** 这意味着它不依赖任何单一公司的商业决策， **会作为一个真正的、中立的社区项目长期维护下去。**

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1who6iblk2qrk4pdVOiawQvOakOw5nJhRkpVz2HzG7KEFpAIm5wEcyeTaFbwQW0PwytYziaicjH3n2692GIAZic8HxWwKxF7ycPl1vfu9of0hA0U/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=3)

还记得 TSC 成员们第一次在西雅图聚餐的场景——大家终于从屏幕背后走到了现实中。那天晚上聊了很多，关于技术方向，关于社区治理，也关于各自这些年在 Redis 生态中的故事。 **那种"我们终于可以按自己的意愿来做事了"的兴奋感，弥漫在每个人的眼睛里。**

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/1who6iblk2qood4Y24rcKlH2enE9biaBgJlxZVkerrCNcwlK7TH7wSVMu7p4Lic1oMPqHd1YH5ZAJRpnKnWWvK5NeruTAl8ygSj7HrTIzEchxQ/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=4)

## 高速成长：两年四个大版本

**Valkey 社区成立两年多来的发展速度，坦率地说，超出了我最乐观的预期。**

先说几个数字：截至 2025 年底，

| 346  活跃贡献者 | ~50  参与贡献公司 | 20K+  GitHub Stars |
| --- | --- | --- |

Corey Quinn 曾这样评价：

"Valkey got roughly **10 times as many contributors** and is hundreds of commits ahead of Redis."

—— Corey Quinn

**这不是营销口号，这是社区力量的真实写照。**

更重要的是版本迭代的节奏—— **两年时间，我们发布了 8.0、8.1、9.0、9.1 四个大版本，每一个都有实质性的技术跃进。这种速度在基础设施类开源项目中是极为罕见的。**

回顾这两年的版本历程，我最大的感受是： **社区驱动的力量远超想象。** 没有单一公司的 Roadmap 约束，每个特性的优先级都来自真实用户的投票和讨论。大家争论得很激烈，但最终总能达成共识， **因为目标是一致的——做最好的开源 KV 数据库。**

## 拥抱 AI：valkey-search 与向量检索

如果说性能和稳定性是 Valkey 的"内功"，那么 **valkey-search 模块则是面向 AI 时代的"亮剑"。**

valkey-search 为 Valkey 提供了原生的向量相似性搜索能力—— **支持 L2、内积、余弦距离三种度量方式，提供精确搜索和基于 HNSW 的近似最近邻算法，可以在数十亿向量中实现毫秒级检索。** 更实用的是它的混合查询能力：向量搜索可以结合数值范围过滤和标签精确匹配，这在真实业务场景中几乎是必需的——你不只是要"找最相似的"，还要"在某个时间范围内、属于某个类目的最相似的"。

更重要的是， **valkey-search 兼容 RediSearch 的 API 子集，现有的 RediSearch 客户端库大多可以直接对接——这大大降低了迁移成本。**

围绕 valkey-search，社区还构建了 **Valkey Bundle** ——整合了 valkey-json（原生 JSON 支持）、valkey-bloom（布隆过滤器）等模块，并通过 valkeymodule-rs SDK 支持用 Rust 开发高性能模块。 **一个完整的 AI 数据层正在成型。**

在实际应用中，Valkey 正在越来越多的 AI 场景中扮演关键角色：为 RAG 应用提供毫秒级向量检索，为 LLM 提供语义缓存以大幅降低推理成本，为 Agent 提供实时会话存储和记忆管理，为推荐系统提供实时特征服务。

**Valkey 的定位很清晰：不做大而全的万能数据库，而是专注于做 AI 基础设施中那个速度最快、离应用最近的数据层。**

## 开源是一种信仰

两年来，Valkey 社区在全球举办了多场线下活动——Contributor Summit、Keyspace 大会等等…… **从一个应急 fork，到拥有自己的技术大会和独立品牌影响力，这个速度让我自己都觉得恍惚。**

但 Valkey 又不仅仅是"再造一个 Redis"。 **它代表的是一种新的开源协作模式——多家云厂商在竞争关系中寻找合作的最大公约数，通过中立的基金会治理来确保项目的长期可持续性。** 没有任何一家公司可以单方面改变 Valkey 的许可证或方向， **这是从 Redis 的教训中汲取的最重要的制度设计。**

**Fedora 41 已经直接用 Valkey 替代了 Redis** （packages marked as "obsoletes Redis"）。很多云厂商也提供了 Valkey 服务， **阿里云的 Valkey 服务也即将推出。** 对于大部分用户来说， **从 Redis 迁移到 Valkey 甚至不需要修改一行代码——只需要更新基础设施配置即可。**

— 这一切发生在仅仅两年之内 —

## 写在最后

从 2017 年那个提 Bug 的深夜，到 2024 年 Valkey 诞生的那个凌晨，再到 2026 年 9.1 发布—— **九年时间，我见证了一个开源社区从繁荣到分裂，又从废墟中重建的全过程。**

有时候我会想，如果没有那次闭源，大家可能还是会在 Redis 社区里继续合作，一切平静如常。但也许正是这次变故，让我们更加深刻地理解了开源治理的重要性——

代码可以 fork，但信任不能、社区不能、共识不能。

**Valkey 的存在，本身就是对"开源应该如何运作"这个问题的一次回答。**

从 Redis 到 Valkey， **变的是名字，不变的是我们对这个社区的热爱和承诺** ——为用户提供最好的开源 KV 数据库，长期、开放、共建。

而对我个人来说， **能和这些全球顶尖的工程师们一起，在 AI 时代为基础设施领域做出贡献，是一件无比幸运的事。**

**这段旅程还在继续，最精彩的章节，也许还在前面。**

$ join the community

如果你也在使用 Redis/Valkey，或者对 KV 数据库在 AI 场景下的应用感兴趣，欢迎通过链接或点击原文加入 Valkey 社区一起交流：

https://github.com/valkey-io/valkey

---

*首发于 Valkey China 微信公众号，欢迎转载，转载请保留作者署名与出处。*

阅读原文