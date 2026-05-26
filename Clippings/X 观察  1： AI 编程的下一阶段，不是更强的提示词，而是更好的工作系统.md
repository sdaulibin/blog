---
title: "X 观察  #1： AI 编程的下一阶段，不是更强的提示词，而是更好的工作系统"
source: "https://x.com/blackanger/status/2059374176012845376"
author:
  - "[[@blackanger]]"
published: 2026-05-27
created: 2026-05-27
description: "「X 观察」是我对每天阅读的 AI 信息流的一次梳理。我认为单条碎片不构成判断，把同一天里彼此呼应、彼此矛盾、彼此补充的碎片放在一起，才看得出方向。理想节奏是每天一篇。如果这个系列对你有帮助，欢迎订阅支持。https://x.com/blackanger/creator-subs..."
tags:
  - "clippings"
---
![图像](https://pbs.twimg.com/media/HJRd1ZpakAAP3Gk?format=jpg&name=large)

> 「X 观察」是我对每天阅读的 AI 信息流的一次梳理。我认为单条碎片不构成判断，把同一天里彼此呼应、彼此矛盾、彼此补充的碎片放在一起，才看得出方向。理想节奏是每天一篇。

> 如果这个系列对你有帮助，欢迎订阅支持。[https://x.com/blackanger/creator-subscriptions/subscribe](https://x.com/blackanger/creator-subscriptions/subscribe)

如果只看 AI 编程工具的演示，很容易以为主线是模型竞赛：谁更会写代码，谁上下文更长，谁的 prompt 更神，谁能一把梭生成完整产品。

但把 2026 年 5 月这组链接（文后参考）和评论区放在一起看，我更倾向于另一个判断：

**AI 编程正在从“生成能力竞赛”，转向“工作系统竞赛”。**

接下来真正拉开差距的，不只是模型本身，而是模型周围的环境：上下文、工具、权限、记忆、脚本、测试、日志、回滚、成本预算、人工介入点，以及团队如何把这些东西变成可维护的工程资产。

换句话说，prompt 没有消失。prompt 正在被吸收到 AGENTS.MD、CLAUDE.md、skills、hooks、scripts、MCP server、测试套件和工作流里，变成代码库的一部分。

## 1\. 自动化不是无人化

Dan Shipper 在 Every 的文章 [After Automation](https://every.to/p/after-automation) 里给了一个很好的反直觉样本：Every 已经把 Codex、Claude Code 等工具用进编码、写作、设计、客服等工作流，但团队并没有因此变成“无人公司”。相反，Every 现在接近 30 人，客服 agent Fin 在 5 月某一周参与了 65% 的支持对话，AI 还处理了 Dan 近期 95% 的工作邮件，但他仍然强调人需要审核、维护和延展这些系统。

这也是他在 Lenny 的访谈里说 “automation is a lie” 的真正含义。不是说自动化不存在，而是每一个自动化背后都需要一个人负责：看它是否仍然有效，处理例外，更新规则，判断什么时候该停止自动执行。[Lenny 对这次访谈的整理](https://x.com/lennysan/status/2058914803360600238)把趋势说得很清楚：未来很多知识工作可能发生在 Codex、Claude Code 这类 agent 工作面里，SaaS 不是死亡，而是被装进 agent 的 in-app browser，成为人和 agent 共同调用的能力。

这点很关键。过去的 SaaS 主要为人设计按钮、页面和流程；下一阶段的 SaaS 还要为 agent 设计可调用、可审计、可组合的接口。过去产品经理设计的是用户路径，未来还要设计 agent 路径。过去工程师维护脚本，未来要维护一套 agent 可以持续工作的系统。

评论区的争论也集中在这里。支持者看到个人杠杆变大：一个人可以调度多个 agent，完成过去小团队才能完成的探索。反对者追问的是系统账：上下文漂移怎么办？CLI、IDE、浏览器内工作面到底哪个更稳定？权限、审批、日志、合规如何处理？token 成本失控算谁的？这些质疑不是保守，而是在提醒我们：AI 编程进入组织以后，问题从“能不能生成”，变成“生成完了之后还在不在”。

## 2\. Vibe coding 的天花板，是单会话

Vibe coding 最大的价值，是把原型速度拉到极高。你可以用自然语言描述一个想法，让模型生成代码，再凭感觉不断修正，直到东西能跑。这对探索阶段非常有效，也确实改变了开发体验。

但它有一个硬天花板：单会话。

真实工程不是一个聊天窗口。真实工程有需求来源、历史约束、架构边界、测试策略、代码规范、部署环境、回滚机制、权限边界、评审流程、业务语境和长期维护成本。如果这些东西只存在于人的脑子里，或者散落在临时聊天记录里，agent 很难稳定工作。

所以，真正成熟的 AI 编程流程不会停在“一个超长 prompt 解决一切”。它会走向更明确的中间产物：研究要留下事实，需求要变成 story，story 要变成 spec，spec 要约束实现，实现要接受测试，测试要形成反馈，反馈要进入下一轮修改。

这和 Martin Fowler 网站上 Kief Morris 的文章 [Humans and Agents in Software Engineering Loops](https://www.martinfowler.com/articles/exploring-gen-ai/humans-and-agents.html) 是同一条线。文章把人类位置分成三种：outside the loop、in the loop、on the loop。真正可扩展的不是人逐行审查 agent 的每一段输出，而是人维护 agent 所在的 loop：目标、规格、检查、反馈、改进机制。

换句话说，工程师不是被赶出循环，而是从最低层的“亲手修每个 artifact”，上移到“设计和维护产生 artifact 的系统”。

如果说 vibe coding 的天花板是单会话，那么突破天花板的方法不是更长的会话，而是把会话变成系统。

## 3\. Harness engineering：Prompt 变成基础设施

如果要给这一变化找一个工程词汇，最贴切的是 harness engineering。

Anthropic Labs 的 Prithvi Rajasekaran 在 [Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps) 里把问题讲得很实在：长任务里，模型会在上下文变长后失去连贯性，也会出现“context anxiety”，接近上下文限制时提前收工；agent 自评也经常过度乐观。他的解法不是再写一个更神的 prompt，而是搭 harness：planner、generator、evaluator，多阶段 handoff，Playwright MCP 验证，sprint contract，以及在模型能力变化后不断删减不再必要的脚手架。

其中一句话很重要：harness 里的每个组件，都编码了一个“模型自己做不到什么”的假设。模型进步后，这些假设需要重新压力测试；任务变复杂后，又会出现新的脚手架需求。

Addy Osmani 在 [Agent Harness Engineering](https://www.oreilly.com/radar/agent-harness-engineering/) 里进一步梳理了这个概念：Anthropic 的 Viv Trivedy 命名了 harness engineering，并把 agent 压缩成一个公式：Agent = Model + Harness。模型只是输入之一，真正让 agent 能完成工作的，是它周围的 prompts、tools、context policies、hooks、sandboxes、subagents、feedback loops 和 recovery paths。Simon Willison 对 agent 的简化定义也很有用：agent 是一个为了达成目标而循环运行工具的系统。这个 loop 和工具怎么设计，才是工程问题。

Claude Code 在 2026 年 3 月底的 source map 泄露，则让工程社区第一次大规模看到一个生产级 coding agent 的内部形态。多家媒体报道这次 npm 包发布事故暴露了约 1,900 个 TypeScript 文件和 50 万行以上代码；Anthropic 对 CNBC 的回应是没有客户数据或凭证泄露，这是一次发布打包错误，不是安全入侵。[TechRadar 对事件的整理](https://www.techradar.com/pro/security/anthropic-confirms-it-leaked-512-000-lines-of-claude-code-source-code-spilling-some-of-its-biggest-secrets)给出了关键事实。

这次泄露之后，Bilgin Ibryam 在 [12 Agentic Harness Patterns from Claude Code](https://generativeprogrammer.com/p/12-agentic-harness-patterns-from) 里把 12 个 pattern 分成四类：记忆与上下文、工作流与编排、工具与权限、自动化。代表性的 pattern 包括 persistent instruction file、scoped context assembly、tiered memory、progressive context compaction、multi-agent delegation、command risk classification 和 deterministic lifecycle hooks。它不是 harness engineering 的一手出处，但它很好地说明了这个领域正在从“经验技巧”进入“模式语言”。

Martin Fowler 站上 Birgitta Böckeler 的 [Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html) 则补上了用户侧视角：好的 harness 既要有 feedforward guides，在 agent 行动前约束它；也要有 feedback sensors，在 agent 行动后让它自我修正。传统一些的 tests、linters、type checkers 是便宜、确定的 computational sensors；AI review、语义判断、LLM-as-judge 则是更贵、更不确定的 inferential sensors。二者结合，才可能把人从逐行盯输出，移动到维护控制系统本身。

再看两个仓库，趋势会更清楚。

[steipete/agent-scripts](https://github.com/steipete/agent-scripts) 把 AGENTS.MD、skills/、scripts/、hooks/ 放在一个共享仓库里，作为 Peter Steinberger 多个本地工作区的 agent 规则层。README 里明确写着：skills 是主要 routing layer，description 要短、通用、为 routing 优化，而不是写成长文档；全局 ~/.codex/skills 和 ~/.claude/skills 都可以指向同一份 skills。这里的重点不是“写 prompt”，而是把 agent 行为变成可版本化、可复用、可验证的仓库资产。

[academic-research-skills-codex](https://github.com/Imbad0202/academic-research-skills-codex) 则提供另一个方向：把 Academic Research Skills 打包成 Codex-native skill，目录里有 SKILL.md、manifest.json、agents/openai.yaml，以及 deep-research、academic-paper、academic-pipeline、experiment-agent、commands、hooks、docs、tests 等子模块。它更像是“跨 agent 平台迁移工作流”的例子。一个用 symlink 让一份规则同时服务 Claude 和 Codex，一个用专门的 adapter 把同一套学术 workflow 重新打包给 Codex；前者押注 agent 趋同，后者押注 agent 异构。两种押注都假设同一件事：skill 已经不是 prompt，而是软件。

Anthropic 的 [Code execution with MCP](https://www.anthropic.com/engineering/code-execution-with-mcp) 还给了一个很硬的数字：把 MCP 工具暴露为代码 API，让 agent 按需读取工具定义，而不是把所有 tool schema 塞进上下文，在示例里把 token 使用从 150,000 降到 2,000，节省 98.7%。这就是“prompt 变基础设施”的最好例子：真正的优化不是让模型更会省字，而是改变工具暴露方式、执行位置和上下文流动方式。

## 4\. 新瓶颈是成本、权限和治理

当 AI 编程停留在个人 demo 阶段，成本可以被忽略；当它进入 5,000 人工程组织，token 就变成财务工程问题。

Microsoft 的案例很典型。根据 The Verge 报道并被 [Windows Central](https://www.windowscentral.com/microsoft/microsoft-cancels-claude-code-licenses-shifting-developers-to-github-copilot-cli-a-move-likely-driven-by-financial-motives)、[TechRadar](https://www.techradar.com/pro/microsoft-may-discontinue-claude-code-internally-as-it-looks-to-push-users-towards-github-copilot) 等媒体转述，Microsoft 的 Experiences + Devices 部门要求工程师在 2026 年 6 月 30 日前从 Claude Code 转向 GitHub Copilot CLI。这里不能简化成“AI 比人贵所以禁用”。更准确地说，这是一次工具收敛：官方口径强调 Copilot CLI 能与 GitHub 一起为 Microsoft 的 repo、workflow、安全需求和工程实践定制；媒体同时指出，财政年度节点和许可证成本也是现实因素。

Uber 则更直接。多家媒体援引 The Information 的报道，称 Uber 在 2025 年 12 月把 Claude Code 推给工程团队后，AI coding 工具采用速度远超预算模型，CTO Praveen Neppalli Naga 表示预算假设需要重做。Axios 同一轮报道还提到，ServiceNow 也出现了全年 token 预算被提前消耗完的情况。关于“全年预算四个月消耗完”“单工程师月成本 500 到 2,000 美元”等数字，不适合脱离原始报道写成铁板钉钉的结论，但它们已经足够说明一个方向：agentic coding 不是传统 $20/月订阅模型，而是按上下文、工具调用、重试、并行 agent 和长任务不断放大的 token 消耗模型。Tom's Hardware 转述 Business Insider / Rapid Response 播客时还提到，Uber COO Andrew Macdonald 的问题不是“能不能多用 AI”，而是 token 使用量和真正交付给用户的功能之间，还没有形成足够清楚的因果线。

平台侧也在改口径。Axios 4 月报道 Anthropic 阻止 Claude 订阅额度继续支撑 OpenClaw 等第三方 agent 工具，理由指向成本、容量和使用方式；5 月又报道 Anthropic 恢复付费 Claude 计划对外部 agent 工具的支持，但把这部分使用放进单独的 credit meter。同一篇报道里，OpenAI 的对位动作是给新的企业客户两个月免费 Codex 使用。这个变化本身就是行业压力测试的结果：人类聊天有天然上限，agent workload 可以连续运行、反复调用工具、递归触发模型请求，订阅制的“自助餐”很难无限承载，而平台竞争也会围绕这种新用量模型重新定价。

这不是 AI 失败，而是 AI 进入生产后的正常代价暴露。

治理也不只是钱。Pope Leo XIV 在 2026 年 5 月 25 日发布首份通谕 Magnifica humanitas，呼吁对 AI 做强监管并要求开发者服务公共利益；Anthropic 联合创始人 Christopher Olah 在梵蒂冈活动中回应说，AI 可能大规模替代劳动，AI 实验室需要不被商业激励折弯的外部道德声音。Anthropic 官方发布的 [Olah 完整发言](https://www.anthropic.com/news/chris-olah-pope-leo-encyclical)里，还有一段更能扣回工程主题：他领导的团队研究模型内部结构，却不断发现 “mysterious, even unsettling” 的东西，包括和人类神经科学相呼应的结构，以及类似内省和情绪状态的内部现象。他没有下结论，只说这需要持续辨析。

这正好解释了为什么 harness 重要：模型内部仍有不可知的一面，所以模型外部的约束、日志、权限、验证和人工判断必须尽可能可知。

把这句话放回工程现场，它不只是宏大伦理。agent 能读文件、改代码、跑命令、调用内部系统、访问生产数据时，安全边界就必须工程化：哪些命令自动允许，哪些必须确认，哪些永远禁止；哪些 MCP server 可信，哪些 skill 能执行任意代码；哪些日志必须保留，哪些数据不能进入模型上下文；哪些变更必须人工审批，哪些可以自动合并。

所以，下一阶段的 AI 编程能力，一半是生成，一半是治理。

## 5\. 代码库会变，但简单性不会过时

代码库的形态正在被重新讨论：如果 agent 未来参与的不只是代码文件，而是需求、任务、文档、运行数据、部署状态和业务流程，那么传统 repo 可能会被更宽的工作图谱包围。代码、ADR、issue、测试结果、运行日志、设计图、产品指标，都可能成为 agent 读写的对象。

但这不意味着工程基本功失效。恰恰相反，agent 越多，简单性越重要。

这里最稳的底座反而来自一篇 2016 年文章。Una Kravets 在 [Simplicity in Design: Insights from an Industrial Engineer](https://una.im/simplicity-in-eng) 里采访了她的父亲 Val Kravets。Val 是有 20 多年经验的工业工程师，做的是大型工厂控制系统。文章里两句核心引言都来自 Val，而不是 Una 本人：一是操作员和机器之间最重要的是简单、直觉、自导航；二是他写代码时总想着后面要接手的那个技术员。

这个归属很重要。它不是 Web 工程师在讲 Web 最佳实践，而是工业控制系统工程师在讲 20 年生命周期、陌生维护者、真实机器和真实操作员。Una 的总结也很关键：成熟工程师不会再追逐别人看不懂的新奇优雅写法，而会采用团队化的方式，让系统更容易被维护。

这句话放到 AI 编程时代更尖锐。因为下一个读你代码的人，可能是新人，也可能是另一个 agent 会话。一个边界清晰、命名稳定、测试可靠、文档贴近代码的系统，会让 agent 更容易做对事；一个隐性规则很多、历史包袱很重、测试缺失、接口随意、文档过期的系统，会让 agent 更快地产生“看起来合理但实际危险”的改动。

人类工程师面对复杂系统时，常常能凭经验察觉“不对劲”。agent 的这种直觉更脆弱。它更依赖可见上下文、可执行约束和快速反馈。因此，简单性不只是为了人，也是为了 agent。

工具会变，语言会变，代码仓库形态也可能变。但“让系统对下一位维护者足够简单”不会过时。只是现在，下一位维护者可能同时包括人类和 agent。

## 6\. 工程师的新价值：把混乱变成可执行结构

如果 AI 能写越来越多代码，工程师还剩什么价值？

答案不是“工程师变成 prompt 工程师”。这个说法太窄了。

更准确地说，工程师会越来越像系统设计者、流程设计者和验证设计者。写代码仍然重要，但更大的杠杆来自把模糊问题变成 agent 可执行、可检查、可恢复的结构。

一个强工程师会做几件事：

他会把业务目标拆成清楚的规格，而不是让 agent 猜；把项目约定写进 AGENTS.MD、skills、脚本和测试，而不是靠口头传承；把复杂改动拆成小步，让每一步都有可验证结果；设计 agent 的工作边界，让 agent 在低风险区域高效行动，在高风险区域主动停下；看懂 agent 的输出，知道哪些地方只是“看起来对”，哪些地方必须进一步验证；控制成本，不让无限上下文、无限重试和无限生成变成组织黑洞。

这不是降低工程师要求，而是提高要求。因为当实现成本下降，错误决策的传播速度也会变快。过去一个坏设计可能需要几周才扩散；现在 agent 可以在一天内把坏抽象复制到十几个模块里。过去一个模糊需求可能只影响一个人；现在它可能驱动多个 agent 同时朝错误方向努力。

AI 没有取消工程判断，它放大了工程判断。

## 7\. 一套更现实的 agent-native 工作流

如果把这些讨论落到实践，我会把 agent-native workflow 理解成五层。

这五层不是新概念，而是把已经存在的工程实践，按 agent 能读懂、能执行、能被审计的方式重新组织一遍。

第一层是上下文层：架构说明、模块边界、关键业务规则、运行方式、测试命令、部署约束、常见陷阱，要贴近代码、持续更新、agent 可读取。

第二层是任务层：不要把“做一个功能”直接丢给 agent。先让需求变成 story，再变成 spec，再变成可验收的小任务。任务越清楚，agent 越少猜。

第三层是执行层：研究、设计、实现、测试、文档、复核可以拆开。拆分不是制造仪式，而是减少上下文污染，让不同 agent 在明确边界内完成明确工作。

第四层是反馈层：测试、lint、类型检查、截图、日志、运行验证、人工 review 都是反馈。好的工程系统会把这些反馈做成 agent 可读的信号。

第五层是治理层：权限、审批、成本、日志、回滚、审计都在这里。没有治理的 agent workflow，只适合 demo 和个人项目，很难进入企业核心系统。

第一步可以非常朴素：写好 AGENTS.MD，整理常用脚本，把测试命令固定下来，把高风险区域标出来，把完成标准写清楚，把常见失败路径记录下来。然后再把研究、规格、实现、验证、复核这些环节拆开。

AI 编程不是从“更强模型”直接跳到“全自动公司”。中间真正漫长的一段，是把组织知识变成 agent 可执行、可检查、可维护的结构。

## 结论：下一个竞争点是维护 agent 工作系统的能力

这组材料放在一起，结论并不是“软件工程要结束了”。恰恰相反，软件工程正在变得更工程化。

当 AI 只能补全代码时，我们关心模型能力。当 AI 能参与任务流时，我们开始关心环境、上下文、规则和反馈。当 AI 进入组织生产系统时，我们必须关心成本、权限、审计、回滚和责任。当 AI 开始读写更宽的工作图谱时，我们还要重新思考代码库、文档、任务系统和协作平台的形态。

所以，AI 编程的下一阶段，不是更强的提示词，而是更好的工作系统。

个人层面，差距会来自谁更会把问题表达清楚、把上下文组织好、把验证做扎实。

团队层面，差距会来自谁更早把 prompt、skills、scripts、hooks、测试、日志和治理沉淀成共享资产。

组织层面，差距会来自谁能让 agent 在明确边界内高效行动，同时让人类保留关键判断权。

真正的 AI-native 工程能力，不是让 agent 替你写更多代码，而是让人和 agent 在同一个可维护系统里稳定协作。

## 参考资料

- [Dan Shipper: After Automation](https://every.to/p/after-automation)
- [Lenny Rachitsky: Dan Shipper 访谈整理](https://x.com/lennysan/status/2058914803360600238)
- [Kief Morris / Martin Fowler: Humans and Agents in Software Engineering Loops](https://www.martinfowler.com/articles/exploring-gen-ai/humans-and-agents.html)
- [Anthropic: Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- [Addy Osmani: Agent Harness Engineering](https://www.oreilly.com/radar/agent-harness-engineering/)
- [Birgitta Böckeler: Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html)
- [Anthropic: Code execution with MCP](https://www.anthropic.com/engineering/code-execution-with-mcp)
- [TechRadar: Claude Code source map leak](https://www.techradar.com/pro/security/anthropic-confirms-it-leaked-512-000-lines-of-claude-code-source-code-spilling-some-of-its-biggest-secrets)
- [Bilgin Ibryam: 12 Agentic Harness Patterns from Claude Code](https://generativeprogrammer.com/p/12-agentic-harness-patterns-from)
- [Peter Steinberger: agent-scripts](https://github.com/steipete/agent-scripts)
- [Academic Research Skills for Codex](https://github.com/Imbad0202/academic-research-skills-codex)
- [Windows Central: Microsoft shifts engineers from Claude Code to Copilot CLI](https://www.windowscentral.com/microsoft/microsoft-cancels-claude-code-licenses-shifting-developers-to-github-copilot-cli-a-move-likely-driven-by-financial-motives)
- [TechRadar: Microsoft may discontinue Claude Code internally](https://www.techradar.com/pro/microsoft-may-discontinue-claude-code-internally-as-it-looks-to-push-users-towards-github-copilot)
- [Stack Futures: Uber Claude Code budget report](https://stackfutures.com/blog/uber-claude-code-budget-exhausted-2026/)
- [Tom's Hardware: Uber warns tokenmaxxing has not yet shown a clear link to shipping successful products](https://www.tomshardware.com/tech-industry/artificial-intelligence/uber-chief-warns-no-link-yet-between-ai-tokenmaxxing-and-shipping-successful-products-company-pumps-the-brakes-on-all-out-ai-spending)
- [Axios: The AI agent buffet is closed](https://www.axios.com/2026/04/06/anthropic-openclaw-subscription-openai)
- [Axios: Anthropic tightens Claude limits as OpenAI courts agent users](https://www.axios.com/2026/05/14/anthropic-claude-price-openai-tokens)
- [Anthropic: Chris Olah's remarks on Pope Leo XIV's encyclical](https://www.anthropic.com/news/chris-olah-pope-leo-encyclical)
- [AP News: Pope Leo XIV urges AI regulation; Christopher Olah responds](https://apnews.com/article/pope-ai-tech-trump-vatican-anthropic-d92d0108730d146baa46da041b8523da)
- [Una Kravets: Simplicity in Design](https://una.im/simplicity-in-eng)