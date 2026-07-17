---
title: "智能体软件工程 #4 ｜ 当 Agent 写完代码，谁来说「可以合并」？"
source: "https://x.com/blackanger/article/2030396470554395015"
author:
  - "[[AlexZ 🦀 (@blackanger)]]"
published: 2026-03-08
created: 2026-07-09
description: "系列文章：AlexZ @blackanger·3月5日 文章智能体软件工程 #2 ｜重新思考 Code Review本文是\"智能体软件工程\"系列的一部分。上一篇：《AI Native 任务估算：轮次、波次与人工时间锚定偏差》。 人工古法编程死于2025年。人工肉眼代码审查将死..."
tags:
  - "clippings"
---
![图像](https://pbs.twimg.com/media/HC1qzubagAAMgLV?format=jpg&name=large)

系列文章：

> 3月5日

> 3月6日

在这个系列的前两篇中，我们分别拆解了两个问题：当 Agent 每天可以产出几十个 PR 时，传统的 Code Review 流程会崩溃（#2）；当多个 Agent 并发工作时，Git 的心智模型会成为瓶颈（#3）。

这两个问题指向同一个更根本的空白：**在 Agent 完成编码之后、代码合并到主分支之前，缺少一个确定性的、机器可执行的质量门禁。**

传统流程中，这个门禁是人类 reviewer。他打开 PR，读几百行 diff，在脑子里模拟代码行为，判断"这段代码对不对"，然后点击 Approve。这个过程依赖人的经验、注意力和时间，这三种在 Agent 时代都是不可扩展的资源。

CI 测试是机器门禁，但它只检查"已有的测试是否通过"，不检查"Agent 的实现是否满足了这次任务的意图"。一个 Agent 可以让所有已有测试通过，但实现了一个完全错误的功能。因为没有人告诉 CI "这次任务的意图是什么"。

**这就是 agent-spec 要解决的问题**。

![图像](https://pbs.twimg.com/media/HC1qVCsaYAAdfKe?format=jpg&name=large)

> agent-spec 不是一个"更好的 Code Review 工具"，它是一个**不同的范式**。它把审查的对象从代码变成了合约，把审查的时间点从编码之后移到了编码之前，把验证的执行者从人变成了机器。

![图像](https://pbs.twimg.com/media/HC1qL_oaMAAH8zt?format=jpg&name=large)

## 一、审查点的位移

[agent-spec](https://github.com/ZhangHanDong/agent-spec) 的核心理念可以用一句话概括：**把人类的审查点从"代码写完之后"移到"代码写之前"。**

传统流程中，人类注意力的分配大致是这样的：

写 Issue (10%) → Agent 编码 (0%) → 读代码 diff (80%) → 点 Approve (10%)

80% 的人类时间花在"读代码 diff"上，这是一个低效的活动。Reviewer 在 diff 中寻找的东西本质上是两类：**"Agent 有没有做对"**（功能正确性）和 **"Agent 有没有做了不该做的事"** （越权行为）。前者需要理解业务意图，后者需要理解代码边界。两者都隐含在 Issue 的自然语言描述中，没有被形式化。

agent-spec 把这个分配变成：

写 Contract (60%) → Agent 编码 (0%) → 看 explain 摘要 (30%) → 点 Approve (10%)

![图像](https://pbs.twimg.com/media/HC1qPMRaYAAfzxj?format=jpg&name=large)

人类的主要工作从"读代码"变成了"写 Contract"。用结构化的方式定义任务的意图、技术决策、文件边界和验收标准。这是一个更高价值的活动：**你在定义"什么是对的"，而不是在代码 diff 中猜测"这是不是对的"。**

代码写完之后，人类看到的不再是几百行 diff，而是一个 Contract 级别的执行摘要：Contract 定义了什么意图？Agent 修改了哪些文件？是否在允许范围内？绑定的测试是否全部通过？如果这些都是"是"，approve 就是一个 5 秒钟的操作。

## 二、Task Contract：控制面的四要素

agent-spec 的核心抽象是 **Task Contract** 。一个结构化的任务规格，包含四个要素。

**第一要素：Intent（意图）**

不是一个模糊的 Issue 标题，而是一段聚焦的说明。这个任务要做什么、为什么做、在什么上下文中做。意图给 Agent 提供方向感，也让 reviewer 在最后判断"Contract 定义是否正确"时有据可依。

\## Intent Add a user registration endpoint to the existing auth module. New users register with email + password; a verification email is sent on success. This is the first step of the user system.

**第二要素：Decisions（已定决策）**

已经确定的技术选择。不是"建议"或"考虑"，而是**不可商量的决定**。Agent 照着做，不需要自己选技术方案。这消除了 Agent 最容易产生偏差的环节：技术选型。

\## Decisions - Route: POST /api/v1/auth/register - Password hash: bcrypt, cost factor = 12 - Verification token: crypto.randomUUID(), stored in DB, 24h expiry - Email: use existing EmailService, do not create a new one

**第三要素：Boundaries（边界）**

Agent 可以修改什么、不可以修改什么。路径级的边界由 agent-spec 的 BoundariesVerifier **机械执行**。如果 Agent 修改了不在 Allowed Changes 列表中的文件，验证直接失败。

\## Boundaries ### Allowed Changes - crates/api/src/auth/\*\* - crates/api/tests/auth/\*\* - migrations/ ### Forbidden - Do not add new dependencies - Do not modify the existing login endpoint

这个设计的意义在于： **Agent 的行为边界不再靠 prompt 里的一句"请不要修改其他文件"来约束，那是概率性的约束，Agent 可能遵守也可能不遵守。边界验证是确定性的，修改了不该改的文件就是 fail，没有灰色地带。**

**第四要素：Completion Criteria（完成条件）**

BDD 风格的验收场景，每个场景显式绑定一个测试函数。**这不是普通的 BDD**，普通 BDD 需要一个 step definition 层把自然语言映射到代码。agent-spec 跳过了这个中间层，直接绑定到已有的测试名：

\## Completion Criteria Scenario: Successful registration Test: test\_register\_returns\_201\_for\_new\_user Given no user with email "alice@example.com" exists When client submits the registration request Then response status should be 201 Scenario: Duplicate email rejected Test: test\_register\_rejects\_duplicate\_email Given a user with email "alice@example.com" already exists When client submits the same email for registration Then response status should be 409

![图像](https://pbs.twimg.com/media/HC1qbnObQAAmGJN?format=jpg&name=large)

这里有一个关键的写作原则：**异常路径的场景数量应该大于等于正常路径。** 上面的例子中，如果只写"注册成功返回 201"这一个场景，Agent 可以用任何方式实现，包括完全不做输入校验、不处理重复邮箱、不检查密码强度。因为这些边界情况在 Contract 中没有被定义为验收标准。

每多写一个异常场景，就多覆盖一个 Agent 可能忽略的边界条件。写 Contract 的过程迫使你在 Agent 动手之前就想清楚所有需要处理的情况。

agent-spec 的 DSL 同时支持中文和英文的关键词：

场景: 重复邮箱被拒绝 测试: test\_register\_rejects\_duplicate\_email 假设 已存在邮箱为 "alice@example.com" 的用户 当 客户端提交相同邮箱的注册请求 那么 响应状态码为 409

这不是装饰性的双语，它让中文团队可以用母语编写和阅读 Contract，降低 Spec 写作的认知门槛。

## 三、七步工作流

Contract 写好之后，整个流程是一条清晰的流水线，三个角色各司其职：

**Step 1：人类编写 Contract**

agent-spec init --level task --lang en --name "User Registration"

生成模板，人类填写四要素。这是整个流程中人类注意力最集中的环节。60% 的人类时间花在这里。

当然，这里也可以交给 AI 辅助编写 Contract ，但 Contract 的 DSL 对人类开发者来说是非常方便审阅和修改的。

**Step 2：Contract 质量门禁**

agent-spec lint specs/user-registration.spec --min-score 0.7

在把 Contract 交给 Agent 之前，先检查 Contract 本身的质量。agent-spec 内置了 8 个 linter，检测模糊动词（"处理"、"manage"）、缺少量化的约束（"应该快速"而不是"< 200ms"）、不可机械验证的断言（"界面应该美观"）、非确定性措辞（"大约"、"approximately"）、缺少测试绑定的场景、以及可能诱发 Agent Sycophancy 的语言（"find all bugs"这样的措辞会让 Agent 编造问题来讨好你）。

如果质量分低于阈值，先修 Contract 再继续。这是"审查点上移"的第一步。**在 Agent 动手之前，确保 Contract 本身是高质量的。**

**Step 3：Agent 读取 Contract 并编码**

agent-spec contract specs/user-registration.spec

输出结构化的 prompt fragment。Agent 受到 Contract 的三重约束：Decisions 告诉它"怎么做"，Boundaries 告诉它"能碰什么"，Completion Criteria 告诉它"做到什么程度算完"。

**Step 4：lifecycle 验证（自动 retry 循环）**

agent-spec lifecycle specs/user-registration.spec \\ --code . --change-scope worktree --format json

这是整个流程的核心。lifecycle 运行四层验证：

**L1 Structural Verifier**：对 Contract 的 Must Not 约束做代码模式匹配。如果 Contract 说"禁止使用 .unwrap()"，verifier 扫描所有源文件中的 .unwrap() 调用。零 token 成本，确定性结果。

**L2 Boundaries Verifier**：检查 Agent 实际修改的文件是否在 Contract 的 Allowed Changes 范围内。路径 glob 匹配，零 token 成本，确定性结果。

**L3 Test Verifier**：运行 Completion Criteria 中绑定的测试。每个 Test: test\_register\_returns\_201 会被实际执行。这是最重要的一层。它直接回答"Agent 的代码是否满足了 Contract 定义的验收标准"。

**L4 AI Verifier**：对前三层未覆盖的场景，提供 AI 分析（当前支持 stub 模式和 caller 模式）。AI 的判断不是 pass 或 fail，而是 uncertain，它不声称确定性，需要人类最终判断。

如果 lifecycle 失败，Agent 收到结构化的 failure\_summary，精确到每个失败场景的原因和证据。Agent 据此修复代码并重新运行 lifecycle。这个 retry 循环可以自动进行，不需要人类介入：

Code → lifecycle → FAIL (2/5) → failure\_summary → fix → lifecycle → PASS (5/5) ✓

agent-spec 的 run log 会记录这个过程，"这个 Contract 跑了 3 次才通过"。

**Step 5：guard 门禁**

agent-spec guard --spec-dir specs --code . --change-scope staged

pre-commit hook 或 CI 检查。对 specs/ 下所有 spec 文件运行 lint + verify。任何 spec 不通过，commit 被阻止或 PR 失败。

![图像](https://pbs.twimg.com/media/HC1qmuCaYAA_JLV?format=jpg&name=large)

(本项目也是用 agent-spec 自举，图为 CI 失败拦截)

Step 6：Contract Acceptance

agent-spec explain specs/user-registration.spec --format markdown

这是人类的最后一个审查点。Reviewer 看到的不是代码 diff，而是 Contract 级别的执行摘要：意图是什么、做了哪些决策、修改了哪些文件（是否在允许范围内）、所有验收标准的通过/失败状态。

Reviewer 需要回答两个问题：Contract 定义对吗？验证全部通过了吗？如果两个都是"是"，approve。

如果需要更多上下文，可以查看执行历史：

agent-spec explain specs/user-registration.spec --history

"这个任务 Agent 试了 3 次才通过"，说明 Contract 的验收标准确实在起作用。

Step 7：stamp 归档

agent-spec stamp specs/user-registration.spec --dry-run

通过 Git commit trailer 建立 Contract → Commit 的追溯链。以后有人问"这段代码为什么这样写"，可以从 trailer 追溯到 Contract，从 Contract 中看到完整的意图和决策。

## 四、四种 verdict，零歧义

agent-spec 的验证结果有且仅有四种语义，不多不少：

**pass**，场景被确定性验证器或 AI 验证器确认通过。

**fail**，场景被验证器发现具体违规，附带证据（代码片段、测试输出、模式匹配结果）。

**skip**，没有任何验证器覆盖这个场景。这通常意味着绑定的测试不存在或选择器配置错误。

**uncertain**，AI 验证器分析了代码但无法做出确定性判断，附带结构化的分析推理和置信度。

最重要的规则是：**skip ≠ pass**。一个没有被验证的场景不等于一个通过了验证的场景。is\_passing 要求 failed == 0 且 skipped == 0 且 uncertain == 0，只有所有场景都被确定性地验证为 pass，整个 Contract 才算通过。

这种四值语义比大多数工具的 pass/fail 二值系统更诚实。它承认了一个现实：**有些东西我们能确定性地验证，有些东西我们只能概率性地判断，有些东西我们完全没有覆盖。** 把这三种状态区分开来，而不是都塞进 "pass" 里，是建立信任的基础。

## 五、AI Verifier：两种模式，一个协议

四层验证金字塔中，前三层（Structural、Boundaries、Test）是确定性的——零 token 成本，零假阴性。但很多质量维度（竞态条件、资源泄漏、隐含的安全漏洞）不在这三层的覆盖范围内。

AI Verifier 填补这个空白，但它的设计哲学和传统的 AI Code Review 工具不同：**它不假装能做确定性判断。** 它的输出永远是 uncertain，附带结构化的分析和置信度。人类（或更高层级的验证系统）做最终裁决。

agent-spec 支持两种 AI 验证模式：

**Caller 模式**：调用方 Agent （比如 claude code）自己做 AI 验证。这是 tool-first 场景下的默认路径。lifecycle 运行机械验证后，把未覆盖的场景生成为结构化的 AiRequest，写入 .agent-spec/pending-ai-requests.json。调用方 Agent（Claude Code、Codex 等）读取请求，用自己的推理能力分析代码，生成 AiDecision，通过 resolve-ai 命令合并回最终报告。

\# Step 1: lifecycle 发现 skip 的场景，生成 AI 请求 agent-spec lifecycle specs/task.spec --code . --ai-mode caller # Step 2: Agent 读取请求，分析代码，生成决策文件 # Step 3: 合并 AI 决策到最终报告 agent-spec resolve-ai specs/task.spec --decisions decisions.json

这种模式下，AI 验证的输出流向人类而非流向自动化流程。Agent 的 retry 循环基于确定性信号（测试通过/失败），AI 的分析留给人类在 Contract Acceptance 阶段做判断。

**Backend 模式**：通过 Rust API 注入独立的 AI 后端。适合编排系统（类 Symphony 的 orchestrator）使用不同模型做独立验证。宿主程序实现 AiBackend trait，注入 spec-gateway，AI 验证在 agent-spec 内部完成。

let report = gw.verify\_with\_ai\_backend(code, Arc::new(my\_backend)).unwrap();

两种模式共享同一套数据结构（AiRequest / AiDecision），agent-spec 保持 provider-agnostic。

## 六、与 jj 的结合：可选加速而非必要依赖

在智能体软件工程第三篇文章中我们详细分析了 jj（Jujutsu）的 Agent-friendly 特性：一切皆 commit（无 staging area 仪式）、stable change ID（跨 amend 不变）、conflicts as data（不阻塞 rebase）、operation log（原子级回退）。

agent-spec 和 jj 的结合点是自然的，但设计原则是 **jj 作为可选加速器，不是必要依赖**。所有核心功能在纯 Git 环境下完全工作。

Change Scope：统一的变更发现

\# Git 环境 agent-spec lifecycle specs/task.spec --code . --change-scope staged agent-spec lifecycle specs/task.spec --code . --change-scope worktree # jj 环境 agent-spec lifecycle specs/task.spec --code . --change-scope jj

在 Git 中，agent-spec 需要三个命令（staged + unstaged + untracked）才能获取完整的变更文件列表。在 jj 中，只需要 jj diff --name-only 一个命令——因为 jj 没有 staging area 的区分。

BoundariesVerifier 不关心文件列表来自哪里——它只检查"这些修改的文件是否都在 Contract 的 Allowed Changes 范围内"。

Stamp：stable change ID 追踪

在 jj 环境下，stamp 命令额外输出 Spec-Change trailer：

Spec-Name: 用户注册API Spec-Passing: true Spec-Summary: 4/4 passed Spec-Change: kkmpptqz

kkmpptqz 是 jj 的 change ID，它跨 amend 稳定。即使 Agent 后续修改了这个 commit（格式化代码、修复 typo），change ID 不变。Contract → Change 的追溯链不会断裂。Git 的 commit hash 在 amend 后就变了，这种追溯链在 amend 后会失效。

Run History：跨运行 diff

agent-spec explain specs/task.spec --history

如果两次 lifecycle 运行之间的 run log 都包含 jj operation ID，agent-spec 会调用 jj op diff 展示两次运行之间 Agent 修改了哪些文件。这回答了一个 debug 时很有价值的问题："上次 fail、这次 pass，中间 Agent 到底改了什么？"

检测而非配置

agent-spec 自动检测 .jj/ 目录的存在。有就利用 jj 的能力，没有就退回 Git。用户不需要在任何配置文件中声明"我用的是 jj"。在 colocated 仓库中（.git/ 和 .jj/ 同时存在），agent-spec 优先使用 jj。

所有 jj 交互通过 std::process::Command 调用 jj CLI，不链接 jj-lib。这和 agent-spec 对 Git 的集成方式完全一致，调用命令而不是链接库。如果将来出现了其他 Agent-Native VCS，只需要增加一个新的检测分支。

## 七、组合性：agent-spec 不解决所有问题

这是一个需要诚实讨论的边界。

agent-spec 保证的是**合约符合性**。代码是否满足 Contract 中定义的验收标准。但"代码质量"远比合约符合性宽泛。一段代码可以让所有验收标准通过，但仍然有竞态条件、资源泄漏、不合理的抽象层次、或者不符合团队的编码风格。

这不是 agent-spec 的设计失误，它是一个有意识的边界选择。如果 agent-spec 试图同时解决合约验证和代码质量的所有维度，它会变成一个大而全的工具，每个维度都做得不够深。

agent-spec 的定位是**编排者**，它提供一个框架，让其他工具的输出变成 Contract 的验收标准。代码质量的提升通过组合实现：

把质量规则编码到 Spec 层级

很多代码质量规则是可以形式化的：

spec: project name: "项目规则" --- ## Constraints ### Must NOT - 禁止使用 \`.unwrap()\` 和 \`.expect()\` - 禁止使用 \`panic!\` 和 \`todo!\` - 禁止使用 \`f32\` 或 \`f64\` 处理金额

project.spec 的约束会被所有 task spec 继承。agent-spec 的 StructuralVerifier 对其中的代码模式做机械检测。这不需要额外的 lint 工具，agent-spec 自己就能检查 ".unwrap() 是否出现在源码中"。

把外部工具集成到 Completion Criteria

更强大的组合方式是把 clippy、覆盖率工具、安全扫描器等现有工具的输出变成验收标准：

Scenario: Code passes clippy strict check Test: test\_clippy\_passes\_with\_deny\_warnings Given the implementation is complete When running \`cargo clippy -- -D warnings\` Then exit code should be 0 Scenario: Test coverage above threshold Test: test\_coverage\_above\_80\_percent Given the implementation is complete When running coverage tool on new code Then line coverage should be >= 80%

这样 Agent 不只需要让功能测试通过，还需要让 clippy 通过、覆盖率达标。agent-spec 不替代这些工具。它给它们一个统一的"是否达标"判断框架。

AI Verifier 覆盖残余维度

机械工具无法检测的维度（竞态条件、设计合理性、安全漏洞推理），由 AI Verifier 做启发式检查。它的输出是 uncertain，不是判决，而是给人类的补充信息。

**组合起来**：project.spec 编码通用规则（L0/L1 约束），task spec 定义任务特定的验收标准（L2 criteria），Completion Criteria 集成外部工具的输出，AI Verifier 覆盖残余的概率性维度。每个环节用最擅长的工具，agent-spec 提供组合框架。

## 八、三层 Spec 继承

agent-spec 支持三层 Spec 继承：

org.spec → project.spec → task.spec

**org.spec（组织级）** 定义跨项目的安全策略和编码标准。比如"禁止硬编码凭证"、"所有用户输入必须校验"、"认证相关代码必须有安全测试"。这些规则对组织内所有项目生效。

**project.spec（项目级）** 定义项目的技术栈决策和约定。比如"使用 PostgreSQL"、"所有 API 返回结构化错误"、"使用 thiserror 统一错误类型"。这些规则对项目内所有 task 生效。

**task.spec（任务级）** 定义单个任务的意图、边界和验收标准。这是 Agent 直接消费的 Contract。

约束和决策自动向下继承。task spec 中的 Agent 不需要知道 org.spec 和 project.spec 的存在。它看到的 Contract 已经合并了所有祖先层级的约束。但人类可以分层管理：安全团队维护 org.spec，项目 lead 维护 project.spec，开发者写 task spec。各司其职，机械合并。

## 九、Skills：让 Agent 学会工作流

agent-spec 不只是一个 CLI 工具。它还需要教 Agent 怎么使用自己。这是 Skills 的作用。

安装

npx skills add ZhangHanDong/agent-spec

一行命令把 agent-spec 的 Skills 安装到项目中。

两个 Skill 的分工

**agent-spec-tool-first** 是默认的工作流 Skill。它教 Agent 七步工作流的完整流程：什么时候读 Contract、什么时候跑 lifecycle、失败后怎么读 failure\_summary 并修复、什么时候生成 explain 给人类、什么时候跑 stamp。它还包含一条关键指引：**lifecycle 失败后修复代码，不要修改 spec 文件**，防止 Agent "聪明地"修改验收标准来让验证通过。

**agent-spec-authoring** 是 Spec 写作 Skill。它教 Agent 怎么编写高质量的 Contract：四要素的结构、双语关键词、测试绑定的格式、step table 的语法、"异常路径 ≥ 正常路径"的原则。当人类让 Agent 帮忙写 Contract 时，这个 Skill 确保 Agent 产出的 spec 符合最佳实践。

多 Agent 支持

Skills 不只针对 Claude Code。agent-spec 项目中还包含 Codex 的 AGENTS.md、Cursor 的 .cursorrules、Aider 的 .aider.conf.yml。虽然这些文件比 Claude Code Skill 简略，但它们包含了核心的命令参考和工作流步骤，让不同的 Agent 工具都能和 agent-spec 配合工作。

agent-spec 的设计原则是 **CLI-first, Agent-agnostic**，核心功能通过 CLI 命令暴露，任何能调用 shell 命令的 Agent 都能使用。Skill 文件是适配层，CLI 是通用层。

## 十、一个真实的 self-hosting 循环

agent-spec 用自己来验证自己。项目的 specs/ 目录下有一个 project.spec，定义了 agent-spec 自身的开发约束：

spec: project name: "agent-spec 项目规则" --- ## Constraints ### Must - 公开 CLI 与 gateway 行为必须有回归测试 - DSL 语法变更必须同时更新 AST、解析输出和回归测试 - 验证结果必须区分 pass、fail、skip、uncertain ...

每次提交都通过 agent-spec guard 检查。每个新功能的开发都有对应的 task spec。specs/roadmap/ 下有 Phase 0 到 Phase 6 的完整路线图 spec，它们本身就是 agent-spec 格式的 Task Contract。

这不是作秀——它是最好的测试。如果 agent-spec 不能用自己来管理自己的开发，它就没有资格管理其他项目。

## 十一、当前状态和诚实的边界

agent-spec 当前最强的场景是 Contract 可以被以下方式检查的情况：

从 Completion Criteria 中选取的显式测试、StructuralVerifier 的代码模式匹配、BoundariesVerifier 的路径 glob 检查、以及针对显式或 staged 变更集的 boundary 验证。

它目前的限制也很清楚：

TestVerifier 是 Rust/Cargo 专属的（通过 cargo test 执行）。非 Rust 项目可以使用 agent-spec 的 Contract 和 Boundaries 能力，但测试执行需要自定义扩展。

AI Verifier 的真实后端还没有接入（stub 和 caller 模式可用）。对抗性多 Agent 验证（Bug Finder / Skeptic / Referee 三方博弈）是 --adversarial 标志的预留能力，实现留待真实 AI 后端接入后。

Resolver 当前只继承 Constraints 和 Decisions，不继承 Boundaries。如果 project.spec 定义了 Forbidden: tests/golden/\*\*，task spec 不会自动继承这个 boundary。

Quality score 只衡量三个维度（determinism、testability、coverage），不反映 vague-verb、unquantified、sycophancy 等 lint warning。这些 warning 出现在诊断列表中但不影响数值分数。

这些限制是已知的、已记录的、有明确的改进路径的。它们不妨碍 agent-spec 在当前状态下为 Rust 项目提供真实价值。

## 十二、AI Coding 团队的协作模式

当一个团队中有多个开发者、多个 AI Agent 同时工作时，协作的核心挑战不再是"代码冲突"，jj 和 Git 都能处理文件级的合并。真正的挑战是**意图冲突**：两个 Agent 各自完成了自己的任务，代码都能编译、测试都能通过，但合在一起时行为不一致，因为它们对系统的理解不在同一个频道上。

传统团队靠 Code Review 来发现这种问题。一个有经验的 reviewer 看到两个 PR 的 diff，脑子里模拟合并后的行为，发现矛盾。但当 PR 数量从每天 5 个变成每天 50 个，这种靠人脑交叉验证的方式就不可行了。

agent-spec 的三层 Spec 继承在这里提供了一个结构化的协调机制。

org.spec 和 project.spec 是团队的共识锚点

在一个 AI Coding 团队中，人类的首要工作不是写代码，也不是一个一个地审查 PR，而是**维护 Spec 层级**。Tech Lead 维护 project.spec，定义项目的技术栈决策、API 约定、错误处理规范。安全团队维护 org.spec，定义不可违反的安全底线。这些文件是团队所有 Agent 的共同约束。不管哪个 Agent 在执行哪个任务，它继承的 project.spec 约束是一样的。

这意味着当两个 Agent 并行开发时，它们不会在技术选型上产生分歧（因为 project.spec 已经决定了用 bcrypt 还是 argon2），不会在 API 风格上产生不一致（因为 project.spec 已经定义了错误码格式），不会违反安全规则（因为 org.spec 的约束被机械执行）。**Spec 层级把"团队共识"从人脑中的隐性知识变成了机器可执行的显性约束。**

Task Contract 是任务的隔离边界

每个 task spec 的 Boundaries 段落定义了 Agent 可以修改的文件范围。当团队用 agent-spec 分配任务时，一个自然的实践是确保不同任务的 Allowed Changes 不重叠。或者如果必须重叠，在 Contract 中显式声明共享区域的协调策略。

agent-spec 的 lint --cross-check 可以检测这种冲突：如果 task-A 的 Allowed Changes 和 task-B 的 Allowed Changes 有重叠路径，cross-check 会报 warning。这不会阻塞开发，有时候两个任务确实需要修改同一个文件——但它让团队在分配任务时就意识到潜在的并发冲突，而不是等到两个 PR 合并时才发现。

**一个典型的团队日常**

想象一个四人 AI Coding 团队的日常工作流。

上午，Tech Lead 花 30 分钟审查和更新 project.spec。上周团队决定统一使用结构化日志，这个决策需要写进 project.spec 的 Decisions 段落，这样今后所有 Agent 都会遵循这个约定。

然后 Tech Lead 创建三个 task spec，分别分配给三个开发者。每个 task spec 定义了清晰的意图、技术决策（继承自 project.spec 加上任务特有的）、文件边界（不重叠或有意识地重叠）、和 4-6 个验收场景（含异常路径）。

三个开发者各自启动自己的 AI Agent（可能是 Claude Code、可能是 Codex、可能是 Cursor，agent-spec 不关心）。Agent 读取 Contract，在 Boundaries 内编码，跑 lifecycle 验证，失败就自动重试。开发者在 Agent 工作期间可以做其他事。写下一个任务的 Contract、审查上一个任务的 explain 输出、或者更新 project.spec。

Agent 完成后创建 PR。CI 中的 guard 自动跑机械验证。PR 评论中自动贴上 explain 的 Contract 摘要。Tech Lead 做 Contract Acceptance，不需要读三个 PR 的代码 diff（可能加起来有 1500 行），只需要看三个 explain 摘要（每个大约 30 行），判断 Contract 定义是否正确、验证是否全部通过。

如果三个 PR 都通过了 Contract Acceptance，guard 的 cross-check 没有报 boundary 冲突，它们可以被合并。整个过程中，Tech Lead 的审查时间从"读 1500 行 diff"变成了"看 90 行 Contract 摘要"。大约节省了 80% 的 review 时间，同时因为每个 PR 都经过了确定性的四层验证，实际的质量保证反而更强。

当 Agent 也是贡献者

在开源场景中，情况更有趣。外部贡献者可能直接用 AI Agent 来贡献代码。这时候维护者面临一个信任问题：你不认识这个贡献者，你也不知道他的 Agent 用了什么 prompt、做了多少轮修改、中间过程是什么样的。

agent-spec 在这里提供的不是"信任"而是"可验证性"。维护者不需要信任贡献者或他的 Agent，他只需要检查两件事：Contract 定义的意图和边界是否合理（这是人类能快速判断的），以及 lifecycle 的所有确定性检查是否通过（这是机器已经验证的）。**信任的对象从"人"转向了"过程"。** 不管代码是人写的还是 AI 写的，它通过了同一套验证管道，这就是可合并的充分条件。

这就是为什么 agent-spec 在贡献指南中强调"Contract 质量和代码质量同等重要"。一个外部贡献者如果提交了一个写得很好的 Contract——意图清晰、决策明确、异常路径充分、所有场景都有测试绑定。即使他的代码风格和团队不一致，这个贡献也是可以被安全接受的，因为正确性是被 Contract 和验证管道保证的，而不是被 reviewer 的主观判断保证的。

## 结语：不是更好的 Code Review，是不同的 Code Review

回到这个系列的核心论点。

本系列文章第二篇中我们说：Agent 时代的 Code Review 不应该是"人读更多 diff"，它应该变成"人定义意图，机器验证符合性"。

第三篇中我们说：Agent 时代的版本控制不应该是"Agent 学会 git add / git commit"。它应该是"VCS 自动捕获 Agent 的工作过程"。

本文给出了一个具体的实现。agent-spec 不是一个"更好的 Code Review 工具"，它是一个**不同的范式**。它把审查的对象从代码变成了合约，把审查的时间点从编码之后移到了编码之前，把验证的执行者从人变成了机器。

人类的角色不是消失了，而是升级了。从"读代码找 bug"变成了"定义什么是对的"。这是一个更高价值的活动，也是一个更可扩展的活动：一个好的 Contract 可以被无限多个 Agent 执行和验证，而一个好的 reviewer 每天只能读有限数量的 diff。

\# 开始你的第一个 Contract cargo install agent-spec agent-spec init --level task --name "my-first-task"

项目地址：[github.com/ZhangHanDong/agent-spec](https://github.com/ZhangHanDong/agent-spec)