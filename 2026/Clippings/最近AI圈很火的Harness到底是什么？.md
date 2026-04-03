---
title: "最近AI圈很火的Harness到底是什么？"
source: "https://x.com/zstmfhy/article/2039991454966354070"
author:
  - "[[AI奶爸 (@zstmfhy)]]"
published: 2026-04-03
created: 2026-04-03
description: "最近刷AI圈，发现Harness这个词突然就火起来了——Anthropic、OpenAI、LangChain、Martin Fowler都在聊，harness engineering、agent harness各种说法都有。我自己研究了一圈，今天用大白话跟大家聊聊，这玩意儿到底是..."
tags:
  - "clippings"
---
最近刷AI圈，发现**Harness**这个词突然就火起来了——Anthropic、OpenAI、LangChain、Martin Fowler都在聊，harness engineering、agent harness各种说法都有。

我自己研究了一圈，今天用大白话跟大家聊聊，这玩意儿到底是什么，为什么现在突然火了，以及我们普通人需要知道些什么。

## 先给结论：Harness就是AI Agent的"脚手架"

你可以这么理解：

- **AI Agent** = 那个帮你干活的AI智能体，相当于"工人"
- **Harness** = 支撑这个Agent跑起来、稳定工作的**全套基础设施**，相当于"脚手架+安全绳+工具包+仪表盘"

没有Harness，Agent就是裸奔——跑一次可能没问题，跑多了就容易出问题，出了问题你也不知道，更没法修复。

> 💡 一句话说清楚：**Harness = 让AI Agent能长期、稳定、可靠跑起来的所有工程化配套**。

## 为什么最近突然火起来了？

因为AI Agent的发展阶段变了：

阶段 核心问题 需要什么 早期 能不能让Agent跑起来？做出来一个demo看看 核心算法、prompt工程、基础能力 现在 能不能让Agent**长期稳定**跑起来？在生产环境用 工程化、可靠性、可观测、可调试

简单说：**过去我们在问"AI Agent能不能干活"，现在我们在问"AI Agent能不能天天稳定干活"**。Harness就是解决第二个问题的。

这就像盖房子：

- 早期你只要把房子搭起来，能住人就行
- 现在你要入住了，需要水电、消防、电梯、监控、保安——这些配套就是Harness

## 一个完整的AI Harness，应该包含哪些东西？

结合几家大厂大佬文章里的说法，我整理一下，一个生产级Harness，至少需要这几个部分：

## 1\. 沙箱/隔离环境 📦

Agent要执行代码、调用工具，你不能让它直接在你服务器上瞎跑吧？万一删库了怎么办？

所以需要一个安全的沙箱环境，让Agent在里面折腾，出事了也不会影响外面。

**作用**：安全隔离，防止恶意操作，用完就能销毁。

## 2\. 预算控制与配额管理 💰

Agent跑起来是要花钱的——调用LLM API要钱，跑计算要钱，存数据要钱。没有预算控制，一不小心你账单就炸了。

Harness需要帮你管：

- 每个任务最多花多少钱
- 每天总预算上限
- 超时自动停止
- 费用监控告警

## 3\. 人类介入通道 🤝

Agent不是什么时候都能搞定一切，遇到拿不准的，它得能喊人。

Harness需要支持：

- Agent遇到不确定的问题，自动暂停，申请人类确认
- 人类可以随时介入，给Agent指导
- 人类可以审批关键操作（比如打钱、删数据）

## 4\. 完整的可观测性 👀

Agent跑了一圈，它到底干了啥？对不对？出了什么问题？

Harness需要记录：

- 每一步思考轨迹
- 每一次工具调用
- 每一块 Token 使用
- 花费了多少钱
- 最终结果是什么

出了问题，你能回溯，能调试，能知道哪里错了。

## 5\. 持久化与断点续跑 💾

长任务不可能一次跑完，Agent需要能保存状态，下次接着跑。

Harness帮你：

- 自动保存Agent当前状态
- 崩溃了能自动恢复，断点续跑
- 历史任务能存档，随时可以查看

## 6\. 错误处理与自动重试 ♻️

LLM调用会失败，网络会断，工具会报错。Harness需要帮你自动处理这些异常：

- 自动识别错误类型
- 合适的错误自动重试
- 重试几次还不行，就喊人或者终止
- 不会一出错整个任务就挂了

## 7\. 工具调用框架 🔧

Agent需要调用各种外部工具——API、数据库、代码解释器、浏览器等等。Harness提供统一的框架：

- 标准化的工具接入方式
- 参数校验
- 结果格式化
- 超时控制

## 各家大佬是如何看待

## LangChain：《Agent Harness的解剖》

LangChain说，Agent Harness不是"另一个Agent框架"，它是**在Agent之外，让Agent能在生产环境运行的一层**。

核心观点：**真正的生产级Agent，大部分代码都不是Agent本身，而是Harness**。

## Martin Fowler：《Harness Engineering》

Martin Fowler说，Harness Engineering是一个独立的软件工程领域，它关注的是**如何给AI Agent提供一个安全、可控、可观测的运行环境**。

核心观点：**AI Agent应用开发，本质上就是Harness Engineering**。因为Agent的智力部分越来越多是LLM提供的，你要做的主要工作就是搭好Harness。

## Anthropic：《给长生命周期Agent构建有效Harness》

Anthropic分享了他们自己做长周期Agent的经验：

1. **长生命周期Agent**和一次性Agent完全不一样，需要更完善的Harness
2. 可观测性特别重要——你得知道Agent一天都干了啥
3. 预算控制一定要做好，不然月底哭晕在厕所
4. 一定要留好人类介入的口子，Agent搞不定的时候，人能顶上去

## OpenAI（中文博客）：《Harness Engineering》

OpenAI中文博客也专门聊了，核心意思：**随着Agent变得越来越智能，工程的重心正在从"Agent逻辑"转向"Harness"**。

Agent的智力越来越强，模型帮你搞定了很多思考，你要做的就是把它"套"在一个可靠的Harness里，让它安全稳定地干活。

## 哪些人需要关心Harness？

![图像](https://pbs.twimg.com/media/HE-AcOCagAANIrS?format=png&name=large)

## 更深层次：Harness vs Agent框架，很多人都搞混了

这是我看到最多人问的问题：**LangChain/CrewAI这些不就是Agent框架吗？为什么还要Harness？**

![图像](https://pbs.twimg.com/media/HE-AqpsbgAAhwRY?format=png&name=large)

说白了：

- Agent框架 = 告诉你的Agent**怎么思考**
- Harness = 告诉你的Agent**在哪里、怎么安全地思考**

你用LangChain写了个Agent，这只是写完了Agent逻辑。要放到生产环境给用户用，你还是要搭Harness——沙箱、监控、预算、人类入口，这些LangChain不帮你全包。

> 💡 我的理解：**Agent框架解决"智能"问题，Harness解决"工程"问题**。智能有了，工程也要跟上，才能用。

## Harness设计的五大核心原则

我整理了几位大佬的观点，总结出设计Harness要记住这五个原则：

## 原则一：默认不信任Agent

Agent会犯错，Agent会幻觉，Agent会执行奇怪的操作。Harness从设计一开始，就要假设**Agent一定会出错**。

所以：

- 所有Agent操作都要在沙箱里
- 写操作一定要审批（尤其是删库、打钱）
- 任何操作都有配额，不会失控

> 安全是设计出来的，不是补出来的。

## 原则二：完全可观测

如果Agent跑了一遍，你不知道它每一步都想了啥、干了啥，那这个Harness就是失败的。

生产环境出了问题，你必须能回答：

- Agent这一步为什么这么选？
- Token花在哪了？钱花在哪了？
- 工具调用的输入输出是什么？
- 哪里错了？为什么错了？

完全可观测，才能调试，才能迭代。

## 原则三：预算总是不够的

不管你有多少钱，Agent总能给你花完。Harness必须从一开始就硬限制预算：

- 每个任务有max budget
- 每天账号有max budget
- Token消耗实时统计
- 超过预算立刻切断，喊人

Anthropic那篇文章里说，他们见过一个Agent不小心开了个死循环，一小时花了几千美元——没有预算控制，你月底账单会教你做人。

## 原则四：人永远在回路里

不要相信"完全自主Agent"，至少现在不行。Harness必须设计**人类介入通道**：

- Agent拿不准的时候，自动暂停请求人类
- 人类可以随时查看进度、打断、修改
- 关键操作必须人类审批

不是说要取代Agent，是给Agent上个保险。

## 原则五：设计为失败

Agent一定会失败，任务一定会中断。Harness要能处理失败：

- 失败了能保存状态
- 人修好能断点续跑
- 不会因为一个任务失败，把整个服务弄挂

## 给你看一个极简Harness长什么样（伪代码）

我说了这么多，可能你还是有点懵。我写个极简伪代码，你一看就懂Harness大概干啥：

```text
class SimpleHarness:
    def __init__(self, agent):
        self.agent = agent
        self.budget_remaining = 10.0  # 美元
        self.trajectory = []  # 记录每一步
        self.checkpoint = None  # 断点

    def run_task(self, task, human_approval_required=True):
        # 1. 检查预算
        if self.budget_remaining <= 0:
            raise OutOfBudgetError("预算用完了，请充值")

        # 2. 在沙箱里启动
        sandbox = Sandbox.create()
        logger.info(f"开始新任务：{task[:50]}...")

        try:
            # 3. 逐步执行，每一步都记录
            for step in self.agent.iter_steps(task, sandbox):
                # 记录轨迹
                self.trajectory.append({
                    "thought": step.thought,
                    "tool_call": step.tool_call,
                    "token_used": step.token_used,
                    "cost": step.estimated_cost
                })

                # 扣预算
                self.budget_remaining -= step.estimated_cost

                # 预算检查
                if self.budget_remaining <= 0:
                    self.checkpoint = sandbox.save_state()
                    raise OutOfBudgetError("执行中预算用完了，已保存断点")

                # 关键操作需要人类审批
                if step.is_dangerous() and human_approval_required:
                    self.checkpoint = sandbox.save_state()
                    request_human_approval(step)

                # 执行这一步
                step_result = step.execute()

                # 给Agent结果
                self.agent.give_result(step_result)

            # 4. 完成，返回结果
            final_result = self.agent.get_final_result()
            logger.info(f"任务完成，总花费：${10.0 - self.budget_remaining:.2f}")
            return final_result

        except Exception as e:
            # 出错了保存断点，方便调试
            self.checkpoint = sandbox.save_state()
            logger.error(f"任务出错：{str(e)}，断点已保存")
            raise e

        finally:
            # 不管怎样，都销毁沙箱
            sandbox.destroy()

    def resume_from_checkpoint(self):
        # 从断点继续跑
        sandbox = Sandbox.restore(self.checkpoint)
        ...  # 继续执行
```

看到没？**这里面几乎没有AI逻辑，都是工程逻辑**——这就是Harness。AI逻辑在self.agent里面，Harness就是套在外面这层安全网。

## 不同场景，怎么选Harness方案

不是所有人都需要从头搭一个Harness，看你的场景：

## 🌱 个人项目/爱好者玩一玩

- 需求：能跑就行，不想搞太复杂
- 方案：用LangChain/LlamaIndex自带的基础组件 简单整个try-catch，自己记一下token 预算手动控制，每天看看账单
- 不用搞太复杂，先跑起来再说

## 🚀 创业公司/做产品给用户用

- 需求：稳定，不能出大事，用户多了也可控
- 方案：用Docker容器做沙箱，每个任务一个容器，完了销毁 用LangGraph管状态，断点续跑比较方便 自己加一层预算控制+日志记录 关键操作加人工审批
- 投入1-2人周就能搭个能用的生产级Harness

## 🏢 大厂/大规模生产环境

- 需求：高可靠、可观测、安全合规
- 方案：Kubernetes+Kubeflow做任务隔离 完整的观测栈：Prometheus+Grafana+Jaeger 专门的团队维护Harness基础设施 安全合规审计，所有操作都留痕
- 这本来就是基础设施团队该干的活

## Harness未来会怎么发展？

我个人看法，接下来几年Harness会往这几个方向走：

1. **标准化**：现在大家都是自己搭，未来会出现标准的Harness框架，像Web框架一样，拿来就能用，不用从头搭
2. **无服务器化**：Harness会变成云服务，你只要给Agent逻辑，沙箱、监控、预算这些云帮你搞定
3. **更好的调试工具**：现在Agent出问题调试很难，未来会有专门的Harness调试工具，回放Agent每一步思考
4. **合规原生**：越来越多行业要合规，Harness天生就要支持审计、追踪、权限控制

## 给新手的一句话总结

- 如果你只是做个Demo，跑一次看看效果——不需要复杂Harness，随便写写就能跑
- 如果你要做产品，让用户天天用，让Agent24小时跑——必须搭Harness，不然肯定出事

Harness这个概念火起来，其实标志着一件事：**AI Agent从"实验室Demo阶段"，正式进入"生产落地阶段"了**。

大家开始不满足于"Agent能干活"，开始追求"Agent能稳定干活"。这是好事，说明AI Agent真的要开始在生产环境用起来了。

## 延伸阅读（原文链接）

如果你想读原文深入研究，可以看这些：

- [LangChain: The Anatomy of an Agent Harness](https://blog.langchain.com/the-anatomy-of-an-agent-harness/)
- [Martin Fowler: Harness Engineering](https://martinfowler.com/articles/harness-engineering.html)
- [Anthropic: Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- [OpenAI Harness Engineering (中文)](https://openai.com/zh-Hans-CN/index/harness-engineering/)
- [GitHub: harness/harness](https://github.com/harness/harness)