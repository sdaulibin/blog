---
title: "从「帮我做」到「做完记住」，我的Agent记忆升级实录！"
source: "https://x.com/berryxia/status/2059825347538104400"
author:
  - "[[@berryxia]]"
published: 2026-05-21
created: 2026-05-28
description: "申明：本文古法手艺实战的心得撰写，并且文章比较长，如果你没有耐心看完，可以直接拉到第二章让AI帮你安装也可以。或者，转身离开！昨晚看罗振宇的「得到大脑」发布会，有一个点一直在我脑子里转--他说 Agent 最关键的能力，是「主动性」。系统不是你喊它一下它动一下，而是它自己知道什么..."
tags:
  - "clippings"
---
![图像](https://pbs.twimg.com/media/HJX4KmIaYAEwqMC?format=jpg&name=large)

> 申明：本文古法手艺实战的心得撰写，并且文章比较长，如果你没有耐心看完，可以直接拉到第二章让AI帮你安装也可以。或者，转身离开！

![图像](https://pbs.twimg.com/media/HJX0Fm9a4AANAQd?format=jpg&name=large)

昨晚看罗振宇的**「得到大脑」**发布会，有一个点一直在我脑子里转--他说 **Agent 最关键的能力，是「主动性」**。系统

不是你喊它一下它动一下，而是它自己知道什么时候该做什么。

我听完一愣。因为我自己的 AI 助手 Berry 小跟班，重要的事儿需要被动进行加强记忆。

上周告诉它的偏好，对话一旦上下文爆，压缩后可能就会有丢失的风险。

刚配置好的工作流，下一个 Session 得从头说。每次对话，都像在训练一个「零基础新人」。

问题不在模型不够聪明Claude 、GPT等这些都已经很强了。问题在于：它们没有**「记忆」，只有「上下文」**。

**上下文有窗口上限，会截断；记忆可以持久，可以进化。**

最近我一直在用 **Bloome，也是给大家疯狂案例Bloome。如果没有安装的强烈去安装一个。**

这里我手动@ Bloome 老板给我打钱吧，注册要邀请码：[https://bloome.im](https://bloome.im/) 邀请码：K049zmo0

应该还可以注册几个名额，自己去试试吧，不好用去打他们老板😄

我的Berry 小跟班陪我干活已经有一阵子了。它自带的记忆方案是MEMORY.md、每日日志、用户画像。

不能说不好用。

但用得越深，越觉得它跟不上我的需求了。

倒不是说它不好，而是既然有更好的选择，在提供服务的时候，是不是可以考虑给它做一次升级和改装，把这个功能也融入进去？

我前阵子还转了一篇帖子就是关于这个开源记忆 MemOS [@MemOS\_dev](https://x.com/MemOS_dev) 项目，于是我就是将它接入到我的Bloome中去。

> 5月21日
> 
> 兄弟们，MemOS 2.0 开源项目又更新了！ Github 已经斩获9.3K Star ~ 这次直接把“AI记忆”从高级剪贴板升级成了真·执行即学习。 以前很多记忆方案，就是把聊天记录存下来，加个语义检索，看起来有记忆，实际上还是RAG那一套。 这次MemOS Local Plugin 2.0最狠的功能，叫“执行即学习”。

于是有了这篇文章，就是我把 **MemOS Local Plugin 2.0 装进 Bloome Agent 的完整实战记录**。

从「遇到问题」到「打通架构」，以及这次升级后，Berry 小跟班到底变了什么。

## 一、Bloome 自带的记忆系统，够用吗？

Bloome Agent 默认的记忆方案，本质上是**文件系统 + 手动管理：核心靠 MEMORY.md、每日日志 memory/YYYY-MM-DD.md 和用户画像文件来存储信息。**

不能说不能用，但是我发现有更好的选择的时候，我就忍不住想折腾。一旦时间一长，记忆越积越多，几个问题就冒出来了：

![图像](https://pbs.twimg.com/media/HJX0LfuakAABd4o?format=jpg&name=large)

① **记的是结论，不是过程。**

只保存**「我帮用户生成了一张图」**，没有保存「为什么这样做、遇到了什么问题、下次如何更快」。经验无法积累，每次相似任务都要重新推导。

**②没有反馈闭环，缺乏主动性。**

用户说**「这个不对」，我记下来了，**但这条信息不会自动影响我下次的决策。**学习是单向的，没有强化。缺乏主动性。**

**③检索靠读文件。**

回忆靠 Read 工具逐文件扫描，没有语义搜索。**「上次做类似任务用了什么工具？」，Berry小跟班**无法快速回答。

**④无法跨 Session 复用，多个对话就需要单独的记忆。**

每次新对话，能拿到的只有 MEMORY.md 里的静态文本。没有可调用的**「技能」结构，能力无法结晶化**。

说白了，这些问题的根源就一个：**它在「存」，不在「学」**。

罗振宇说的 **Agent 主动性，其实也是这个意思。**

我们会实时动态主动地记忆我们的内容，而不是**被动每次「帮我记一下这个XX」**。

当大模型已经具备通用推理能力，下一步真正影响 Agent 好不好用的，不是模型参数本身，而是它能不能在真实用户的本地世界里**持续学习、沉淀经验、记住反馈、复用能力**。

**我们的 Agent 的记忆，不就是自己的数字资产嘛。**

## 二、MemOS是什么？ 不是聊天记录，是记忆操作系统

**MemOS（Memory Operating System）**是专门为 AI Agent 设计的记忆基础设施。它不是「把对话存下来」，是把 Agent 执行任务的全过程，系统化转化为**可审计、可归因、可复用**的学习资产。

![图像](https://pbs.twimg.com/media/HJX0Nv5aQAA8MJo?format=jpg&name=large)

1. **官网：**[https://memos.openmem.net](https://memos.openmem.net/)
2. **Github项目地址：**[https://github.com/MemTensor/MemOS](https://github.com/MemTensor/MemOS)
3. 论文：[https://arxiv.org/pdf/2507.03724](https://arxiv.org/pdf/2507.03724)

说白了，就是 Berry 小跟班做完一件事之后，不只是记下**「我做完了」**，而是能说清楚**「我为什么这么做、哪里可以更好、下次遇到类似的事我直接用」。**

MemOS Local Plugin 2.0 的核心是**「执行即学习」**——每次 Agent 完成任务，不只是记下「做了什么」，而是把整个执行链路拆解成可学习的单元，自动评分、归因、入库。

它的**架构由四层认知资产组成**。我用 Berry小跟班 学会一个新技能的过程来解释：

> **L1 Trace（执行轨迹）**——Berry 第一次帮我部署一个 Docker 环境，它记下了每一步：用了什么命令、返回了什么报错、怎么解决的、这条经验值多少分。这是原材料。

> **L2 Policy（策略归纳）**——Berry 小跟班帮我部署了三次类似的环境之后，它从三次 Trace 里归纳出一条规律：「遇到 Docker 部署任务，先检查端口占用，再拉镜像，最后配环境变量。」经验从点连成了线。

> **L3 World Model（世界认知）**——Berry小跟班 记住了：我是谁、我常用的技术栈是什么、我的项目当前什么状态、我有哪些工具可用。这是它的「背景知识」，不用每次重新问。

> **Skill（结晶化技能）**——那条「Docker 部署」的 Policy 被反复验证有效，最终结晶成一个可以直接调用的 Skill。下次我说「帮我部署一个新服务」，Berry 不用从头推导，直接调用这个 Skill 就行。经验从线凝成了工具。

![图像](https://pbs.twimg.com/media/HJX0VM0aIAENUio?format=jpg&name=large)

## 三、怎么装？一行命令搞定！

MemOS Local Plugin 2.0 目前首发支持 Hermes Agent 和 OpenClaw，未来应该会支持和兼容更多 Agent 平台。

**一份记忆核心，跨 Agent 共享，换工具不用重新「训练」你的 AI。**

PS：需要大家提前可以注册一个OpenAI或者其他的Embedding 模型的API，用于云端的嵌入模型使用。也可以自己本地部署安装都可以，我这里建议大家可以使用GLM智谱的免费的就行。

注册地址：[https://bigmodel.cn/console/overview](https://bigmodel.cn/console/overview) 你告诉大模型KEY就行，不用自己捣鼓。

![图像](https://pbs.twimg.com/media/HJX0oWUagAAs6ux?format=jpg&name=large)

方式一：Hermes Agent（推荐新手入手）

Hermes Agent 是目前用户最多的本地 AI Agent，安装流程最为成熟。三步走：

**1\. 安装 Hermes Agent**

打开终端，一行命令完成安装：

```plaintext
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

**2\. 安装 MemOS Local Plugin（Hermes 模式）**

```plaintext
bash <(curl -fsSL https://memos-claw.openmem.net/install.sh) --agent hermes
```

**3\. 启动并打开 Memory Viewer**

安装完成后，在浏览器中打开 \[http://127.0.0.1:18800，即可看到你的记忆全貌。\](http://127.0.0.1:18800，即可看到你的记忆全貌。)

📸 **Hermes Agent + MemOS 安装成功。**

![图像](https://pbs.twimg.com/media/HJX09tBbYAAidsC?format=jpg&name=large)

**方式二：Bloome Agent（OpenClaw 模式，本文重点）**

Bloome Agent 运行在云端沙箱，跟 Hermes 的本地模式不太一样。安装命令相同，只需替换 agent 参数：

```plaintext
bash /tmp/memos_install.sh --agent openclaw
```

装完之后我发现一个问题——**Memory Viewer 默认只能在沙箱内部访问（127.0.0.1:18799）**，我的 Mac 浏览器根本打不开。

这是 **Bloome 用户集成 MemOS 时遇到的最典型问题**，下一节专门讲怎么解决。

比如你的是云端龙虾或者Hermes 就会遇到这样的问题，不要着急慢慢来给你解决这个问题。

## 四、踩坑：云端沙箱的 Viewer 打不开怎么办

装好插件，兴冲冲想看 Memory Viewer——结果发现它跑在沙箱的 127.0.0.1:18799，我的 Mac 浏览器根本访问不到。

这是 Bloome 用户或者云端沙盒的龙虾集成 MemOS 时遇到的最典型问题。

解法很简单—我的Bloome小家伙直接给我推荐**ngrok 内网穿透**，三步搞定：

![图像](https://pbs.twimg.com/media/HJX1DEAawAAGbNy?format=jpg&name=large)

**1\. 注册 ngrok，获取免费 authtoken**

访问 [ngrok.com](https://ngrok.com/) 注册账号（免费），在 Dashboard 复制你的 Authtoken。

这个面版的地址：[https://dashboard.ngrok.com/authtokens](https://dashboard.ngrok.com/authtokens)

![图像](https://pbs.twimg.com/media/HJX1IZmboAED64n?format=jpg&name=large)

**2\. 在沙箱中启动 ngrok 隧道**

```plaintext
/tmp/ngrok config add-authtoken YOUR_TOKEN
/tmp/ngrok http 18799 --log=stdout &
```

![图像](https://pbs.twimg.com/media/HJX1OFBbMAAyfGS?format=jpg&name=large)

**3\. 在本地浏览器打开公网地址**

ngrok 会生成一个 [https://xxxx.ngrok-free.app](https://xxxx.ngrok-free.app/) 地址，在 Mac 浏览器中打开即可。

搞定。从这以后，我随时可以在本地浏览器里查看 Berry 的记忆全貌。

![图像](https://pbs.twimg.com/media/HJX1SdPaEAEqe2F?format=jpg&name=large)

## 五、记忆迁移：过去的经验记忆+技能不能丢啊！

插件装好了，Viewer 也能访问了。

但我面临一个现实问题，Berry 小跟班之前已经积累了大量工作记录（MEMORY.md + 日志文件），这些怎么办？

总不能全扔了吧。

答案是**批量迁移**。通过 Python 脚本直接写入 MemOS 的 SQLite 数据库，把历史任务、用户偏好、工具配置全部转化为结构化的认知资产：

![图像](https://pbs.twimg.com/media/HJX1aQfbMAAVLOP?format=jpg&name=large)

![图像](https://pbs.twimg.com/media/HJX1dXnaIAAxENf?format=jpg&name=large)

迁移完成后，打开 Memory Viewer，World Model 页面里已经能看到我的项目状态和工具配置，Traces 页面里 15 条历史记录全部入库。过去的经验，一个都不会少。

![图像](https://pbs.twimg.com/media/HJX1hDFbkAA3kTB?format=jpg&name=large)

![图像](https://pbs.twimg.com/media/HJX1lEPa8AAYeaM?format=jpg&name=large)

## 六、实时 Trace：让每次任务都留下可复用的记忆

光有历史记忆还不够——我需要让之后每一次对话都能实时写入 MemOS。

这里有个架构层的限制：**Bloome Agent 走 IM 通道，不经过 OpenClaw CLI 的 hook 机制，所以 MemOS 没法像在 Hermes 上那样自动拦截所有对话。**

解法是：**在 Agent 每次完成重要任务后，主动调用** push\_trace() **函数，将这次任务的「用户说了什么 → 我做了什么 → 任务摘要 → 用到了哪些工具」写入 MemOS。**

不是所有对话**都值得记住—Berry 需要判断哪些经验值得沉淀，哪些只是闲聊**。这里就是展示Agent的能力的时候，就是聪明的Agent就是自我感知上下文和内容。

标准是这样的：

**🔴** **完成可交付物** **🔴** **配置工具/定时任务** **🟡** **用户确认新偏好** **🟡** **重要技术决策** **⚪** **简单问答不记录**

> **实时 Trace 注入已在 Berry 小跟班上运行。每次完成文件生成、脚本配置、方案撰写等任务，记忆会自动同步到 MemOS Viewer，随时可以在公网地址查看最新的执行记录。**

![图像](https://pbs.twimg.com/media/HJX1q5paoAAlom_?format=jpg&name=large)

## 七、升级前后：哪里不一样了？

先说一个我自己的体会。

升级前，我让 Berry 小根本帮我写一篇技术文档。它写完了，我改了几处说「风格不对，要更口语化」。Berry 把这条记在了 MEMORY.md 里。

我不需要一次次的强调记住，自我感知主动去记住。

下一次我让它写文档，它又从零开始——上次的修改意见躺在文件里，但它不会主动去读、去用。

![图像](https://pbs.twimg.com/media/HJX1u4caoAAb9EC?format=jpg&name=large)

升级后，同样的场景。Berry 写完文档，我给了反馈。这次反馈被写入了 Trace，自动归因到**「文档撰写」**这个任务类型。下次我再让它写文档，它会先调出相关的 Policy，「用户偏好口语化风格，避免学术腔」，直接按这个方向写。不用我再说一遍。

这就是从**「记了但不用」到「记了就会用」**的区别。主动记忆，无需强调和说明。

![图像](https://pbs.twimg.com/media/HJX103Pa4AA4nF1?format=jpg&name=large)

下面是系统层面的对比：

![图像](https://pbs.twimg.com/media/HJX2DRdbYAAEjyL?format=jpg&name=large)

## 八、我有多个Agent，跨Agent记忆共享可以吗？

MemOS 2.0 最令人兴奋的能力之一，是支持**跨 Agent 记忆共享。**

同一个用户的多个 AI Agent，可以共享同一套 World Model、Skills 和 Traces。 换工具不清零，不同 Agent 的经验可以互相学习。

> **「一份核心，多 Agent 共用：记忆资产不会因工具切换而清零。」**

Hub-Client 架构和MemOS 2.0 的跨 Agent 共享基于 Hub-Client 架构：

![图像](https://pbs.twimg.com/media/HJX2MHpa4AE7ymc?format=jpg&name=large)

实际配置（Berry小跟班 + BuLeng）

在我们的实战配置中，Berry小跟班作为 Hub，**BuLeng 作为 Client：**

![图像](https://pbs.twimg.com/media/HJX2PjXagAA7LnG?format=jpg&name=large)

**Hub Agent 的 config.yaml 配置：**

```plaintext
hub:
 
  enabled: true
 
  role: hub        # 这个 Agent 是记忆中心
 
  port: 18912   # Hub 监听端口
 
  teamName: berry-team
 
  nickname: berry
 
  teamToken: your-shared-token
 
  address: ""         # 空 = 自身是 Hub
```

**Client Agent 的 config.yaml 配置：**

```plaintext
hub:
  enabled: true
  role: client    # 这个 Agent 是接入方
  address: https://your-hub.trycloudflare.com
  teamToken: your-shared-token  # 必须与 Hub 一致
```

> **公网暴露方案：** Hub 的 18912 端口需要通过隧道暴露到公网才能让 Client 连接。

> 推荐使用 **Cloudflare Tunnel**（免费，比 ngrok 更稳定）： cloudflared tunnel --url http://localhost:18912

![图像](https://pbs.twimg.com/media/HJX2ZBgbgAAJOtP?format=jpg&name=large)

![图像](https://pbs.twimg.com/media/HJX2g-Fa4AAVeI7?format=jpg&name=large)

**共享后的效果**

1. 两个 Agent 的 Trace 合并
2. Skills 互相可见
3. World Model 共享
4. 记忆越用越丰富

## 九、写在最后

**当大模型已经够聪明，下一步比拼的不是参数，是谁能记住你。**

而这一切就是你的数字分身，你留给这个世界最宝贵的东西，**记忆**。

记住你，不是为了下次聊天时显得更贴心——而是为了不再等你开口，就知道该做什么。

MemOS Local Plugin 2.0 做的事情，就是让 Agent 从**「被动存档」变成「主动学习」**。一行命令，让你的 AI 开始真正记住你。

**现在就为你的 Agent 装上 MemOS**

支持 Hermes Agent 和 OpenClaw / Bloome，开源免费。

![图像](https://pbs.twimg.com/media/HJX3U3baoAAhVyv?format=jpg&name=large)

⭐ [GitHub Star](https://github.com/MemTensor/MemOS/tree/main/apps/memos-local-plugin) · 📖 [查看文档](https://memos-docs.openmem.net/openclaw/hermes_local_plugin) · 🌐 [官网](https://memos.openmem.net/)