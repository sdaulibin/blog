---
title: "Claude Code 源码曝光，盘一下他们到底藏了什么"
source: "https://x.com/sitinme/status/2039967153072251088"
author:
  - "[[@sitinme]]"
published: 2026-04-03
created: 2026-04-03
description: "前几天发生了一件让AI圈内沸腾的事情。Anthropic 的 Claude Code 源码泄露了，51万行 TypeScript 代码直接摆在网上。更讽刺的是，泄露出来的代码里有一个功能叫 Undercover Mode，中文直译\"卧底模式\"，专门用来自动隐藏 Anthropic..."
tags:
  - "clippings"
---
![图像](https://pbs.twimg.com/media/HE9qVlHbIAApWR_?format=png&name=large)

前几天发生了一件让AI圈内沸腾的事情。

Anthropic 的 Claude Code 源码泄露了，51万行 TypeScript 代码直接摆在网上。

更讽刺的是，泄露出来的代码里有一个功能叫 Undercover Mode，中文直译"卧底模式"，专门用来自动隐藏 Anthropic 内部代码痕迹的。结果呢，就是因为构建配置一时疏忽，这套专门搞隐藏的代码自己先曝光了。

![图像](https://pbs.twimg.com/media/HE9qaXjbwAAv_uP?format=jpg&name=large)

这事发生的经过不复杂。npm 包 [@anthropic](https://x.com/@anthropic)\-ai/claude-code v2.1.88 上线时，有人忘了在构建配置里排除 .map 文件。结果一个 59.8MB 的 Source Map 文件夹就这么带着完整的 TypeScript 源码一起上传了。

给大家盘一下完整的时间线。

## 完整时间线

**3月31日 凌晨** — Anthropic 的 CI/CD 流水线把 Claude Code v2.1.88 推送到 npm（[@anthropic](https://x.com/@anthropic)\-ai/claude-code）。

这个版本里包含了一个 59.8MB 的 JavaScript Source Map 文件（[cli.js.map](https://cli.js.map/)），里面的 sourcesContent 字段存了完整的原始源码映射。任何人执行 npm install 就可以完整还原所有 TypeScript 源码。

根本原因很简单：构建配置里没在 .npmignore 排除 \*.map 文件，或者没在 Bun 的生产构建里关掉 source map 输出（Bun 默认是开着的）。

**3月31日 凌晨 4:23 AM ET** — 安全研究员 Chaofan Shou（[@Fried\_rice](https://x.com/@Fried_rice)，Solayer Labs 的实习生）在 X 上率先发出来，展示了怎么从 npm 包里提取完整源码。这条帖子最终超过 3400 万次浏览。

![图像](https://pbs.twimg.com/media/HE9qgXubgAAPBpG?format=jpg&name=large)

**3月31日 上午** — 开发者社区几乎秒级响应。大量镜像仓库涌上 GitHub，主要仓库几小时内涨到 84K Stars、82K Forks，成为 GitHub 历史上增速最快的仓库之一。知乎、V2EX、掘金同步爆发讨论。

**3月31日 日间** — Anthropic 发出官方声明：

> “Earlier today, a Claude Code release included some internal source code. No sensitive customer data or credentials were involved or exposed. This was a release packaging issue caused by human error, not a security breach.”

**3月31日 24小时内** — Bloomberg、CNBC、Axios、VentureBeat、TechCrunch、36氪、IT之家、爱范儿等几乎所有主流科技媒体全部跟进报道。

**4月1日** — Anthropic 法务发出 DMCA 下架申请。操作失误，约 8100 个 GitHub 仓库被误下架，其中大量是 Anthropic 自己公开项目的合法 fork。社区炸了，媒体称之为"DMCA Blunder"。

**4月1日 晚** — Claude Code 负责人 Boris Cherny 公开承认出错，撤回大部分 DMCA 通知，最终只保留对 1 个仓库及其 96 个 fork 的下架请求。

![图像](https://pbs.twimg.com/media/HE9qux7bkAAF4rc?format=jpg&name=large)

**4月1日之后** — 安全研究人员从泄露代码中发现关键安全漏洞，SecurityWeek 详细报道。多个基于泄露架构的开源替代品涌现，有人用 Rust 重写了一个 Claude Code（claurst 项目）。

## 泄了多少东西

用数字说比较直观：51.2 万行 TypeScript、1906 个文件、40 多个工具模块、144 个 UI 组件、44 个 Feature Flag。

没泄露的部分：模型权重、用户数据、API 密钥、服务端基础设施。Claude 本身没事，就是工程代码出去了。

## 那些还没发布的功能

![图像](https://pbs.twimg.com/media/HE9qzMpaoAAzYD9?format=jpg&name=large)

这块才是我真正感兴趣的地方。44 个 Feature Flag 里藏着一堆还没对外发布的功能，有些看了挺意外的。

**KAIROS** 在源码里出现了 150 多次。它是一个始终在线的后台守护 Agent，也就是说 Claude Code 将来可能一直在后台跑，主动帮你盯着代码库。现在的用法是你问它答，KAIROS 要做的是它自己主动干，不用你每次手动启动。

**ULTRAPLAN** 是云端深度规划模式，支持最长 30 分钟的远程思考。复杂任务可以直接丢给它，让它慢慢想。

**BUDDY** 在4月1号已经上线了，电子宠物伴侣系统，18 个物种、稀有度分级、还有闪光变种。说白了就是在 IDE 里嵌了个养宠物小游戏agent 走上游戏化了。

我给自己搞了个闪光变种的宠物。

[Claude Code 藏了个宠物彩蛋，分分钟拥有稀有度1%金卡！](https://mp.weixin.qq.com/s?__biz=Mzg3NzU2NjY3OQ==&mid=2247491260&idx=1&sn=7c70a70affa733a3381b64573641e6e6&scene=21#wechat_redirect)

模型代号这次也曝出来了几个：**Capybara** 是 Claude 某个 4.x 变体的内部代号，**Fennec** 是 Opus 4.x 的代号，**Numbat** 是一个还在测试、没正式发布的模型。

还有 **26 个隐藏命令**，没有写进任何文档，只存在于源码里。

## 架构层面值得学的几件事

对 Agent 感兴趣的朋友，这套代码是最佳的公开学习资料。

**权限验证**是六层的。每次工具调用要依次过：Config 白名单、Auto-mode 分类器、Coordinator 门控、Swarm Worker 门控、Bash 分类器、用户确认。

专门针对 Shell 命令还有 23 条安全分析规则。权限这块在很多自制 Agent 项目里基本是摆设，这套设计可以直接拿来参考。

**记忆系统**分三层：即时记忆（对话上下文）、会话记忆（MEMORY.md 指针索引）、长期记忆（主题文件目录）。

还有一个叫 autoDream 的机制，在 Agent 空闲时自动整理压缩记忆，有点像人睡觉时大脑在处理白天的东西，官方称之为做梦。

**上下文压缩**不是等满了再处理。它会主动在逼近上限前把对话历史重写折叠成摘要，有 5 种不同的压缩策略，还有 14 个缓存失效向量。这个主动管理的思路值得借鉴。

**多 Agent 编排**有三种子 Agent 执行模型：Fork、Teammate、Worktree，外加 Coordinator 协调模式和 Swarm 集群模式。

在这次泄露之前，几乎没有任何产品级的公开参考可以看，现在有了。

最后一个是代码量的问题。核心推理引擎 QueryEngine.ts 一个文件就有 46000 行。这个数字放在这里，大概就不用再解释"做一个真正可用的 Agent 有多复杂"了。

![图像](https://pbs.twimg.com/media/HE9q4Dob0AABN9O?format=png&name=large)

## 不止一次的错误

顺带说一句，这是第二次了。2025 年 2 月 Claude Code 首次发布时，同样因为 Source Map 没排除，泄露过一次。根本原因完全一样。同一个坑踩了两次，加上这次 DMCA 误操作，三件事撞在一起，对他们计划中的 IPO 多少有点影响。

## 变体已经出来了

源码公开之后，各种基于它的项目迅速涌现。

有人用 Rust 重写了一个 Claude Code，叫 claurst，跑得更快。有人用 Python 做了一个净室实现（clean-room implementation），叫 claw-code，绕开版权问题。有人直接让 Claude 自己花 10 分钟写了一份《从零构建 Code Agent》教程，素材就来自泄露的源码。

不得不说，大家的速度真的太快了。

说实话，对做 AI 的人来讲，这次泄露是一件难得的好事。

在这次之前，产品级的 AI Agent 架构从来没有完整的公开参考。现在 Anthropic 把全套答案摆出来了，不管他们愿不愿意。

对创业团队和独立开发者来说，这份材料的价值很难用钱衡量。你可以直接看顶尖团队是怎么解这些问题的。

这会动摇 Claude Code 的地位吗？

我觉得不会。

有人认为，源码公开之后，竞争对手可以迅速追上，Claude Code 的护城河消失了。这个逻辑听起来有道理，但忽略了一件事：代码只是工程的快照，真正的竞争力在模型本身。

Claude Code 之所以好用，核心是背后的 Claude 模型够强。权重没有泄露，推理基础设施没有泄露，Anthropic 持续迭代的能力也没有受影响。

跟上现在这个版本，Anthropic 早就在做下一个版本了。