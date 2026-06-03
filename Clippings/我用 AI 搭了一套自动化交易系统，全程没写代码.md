---
title: "我用 AI 搭了一套自动化交易系统，全程没写代码"
source: "https://x.com/axichuhai/status/2058830451167367560"
author:
  - "[[@axichuhai]]"
published: 2026-05-25
created: 2026-06-02
description: "一周前我搭了一套自动化模拟交易系统，现在它每天自己在跑、自己复盘、还能实时参考聪明钱的买卖信号。全程没有写代码。用的是 Codex + OKX Agentic Wallet 这个组合。先说一下这两个工具的分工：CodexOpenAI 的 AI 编程 Agent，可以理解成一个会自..."
tags:
  - "clippings"
---
![图像](https://pbs.twimg.com/media/HJIxpDhWwAEnpYY?format=jpg&name=large)

一周前我搭了一套自动化模拟交易系统，现在它每天自己在跑、自己复盘、还能实时参考聪明钱的买卖信号。

![图像](https://pbs.twimg.com/media/HJIx6ZRW4AArxB2?format=jpg&name=large)

全程没有写代码。

用的是 Codex + OKX Agentic Wallet 这个组合。

先说一下这两个工具的分工：

- **Codex**

OpenAI 的 AI 编程 Agent，可以理解成一个会自己写代码、自己执行工作流的 AI 助手

- **OKX Agentic Wallet**

专门面向 Agent 的钱包。普通钱包是给人操作的，这个是让 AI Agent 去调用的——你的 AI 可以通过它直接操作链上资产、扫聪明钱动向、执行交易

两个放在一起：Codex 是大脑，Agentic Wallet 是它伸出来的那双手。

## 第一步：创建 Agentic Wallet

我先在 Codex 里装了一个叫 OnchainOS 的 skill，一条命令：

> Run npx skills add okx/onchainos-skills to install Onchain OS skills.

![图像](https://pbs.twimg.com/media/HJItq0SW0AAo4Dp?format=jpg&name=large)

**OnchainOS 是什么？**

是 OKX 提供的一套让 Agent 能直接跟区块链对话的能力集合。

装了这个 skill 之后，Codex 就有了调用链上功能的能力，包括查余额、扫聪明钱、发起交易。

装好之后，我直接给 Codex 发这句话：

> Log in to Agentic Wallet with email

它问我要邮箱地址。填完去邮箱取验证码，把验证码发回对话框，完成。

整个过程1分钟不到。

![图像](https://pbs.twimg.com/media/HJIuS36XwAAbwrR?format=jpg&name=large)

## 第二步：搭交易策略，先跑模拟盘

钱包建好，我让 Codex 帮我设计交易系统。

指令很简单：帮我设计一套模拟交易策略，能自己跑，能看胜率，根据结果不断优化。

为什么先做模拟盘？

策略没跑稳之前不动真金白银。先在模拟盘里把胜率跑上来，再考虑切实盘。

它设计了策略、写好了执行逻辑，然后根据每次模拟交易的结果迭代参数。这个反馈循环是整个系统的核心。

我没有写一行代码，最后拿到了一套能自己运转的交易系统。

## 第三步：接进自动化，每天收复盘

光能跑还不够。我还希望每天早上能自动收到交易动作的汇报和复盘，不用主动去看，正好Codex自带了自动化定时任务的功能。

![图像](https://pbs.twimg.com/media/HJIumy5WsAAyrIe?format=jpg&name=large)

我让 Codex 把这两件事做成定时任务，放进它的自动化功能里：

- 每天自动记录交易动作
- 每天自动生成复盘报告

![图像](https://pbs.twimg.com/media/HJIuwCkXcAAvlJi?format=jpg&name=large)

设置完成，它每天主动推送提醒过来。这个感受和之前用 Claude Code 做日报是一样的——**规则你设，之后它自己转**。

![图像](https://pbs.twimg.com/media/HJIu36_XUAAiGak?format=jpg&name=large)

## 第四步：自动扫描聪明钱的实时动向

配好之后我顺手问了一下 Agentic Wallet 还能干什么。

它有一个我没想到的功能：**扫描聪明钱的实时买卖动向**。

简单解释一下聪明钱扫描是什么意思，就是跟踪链上那些历史盈利率高的钱包地址，看它们最近在买什么、卖什么——这是判断资金流向的有效信号。我

让它跑了一下，直接吐出来了当前实时的扫描结果，格式很清晰。

![图像](https://pbs.twimg.com/media/HJIvIeqXgAAth1v?format=jpg&name=large)

我当时想：这个能不能并入模拟盘？

就给了它这条指令：

> 把聪明钱的信号并入这个模拟盘自动扫描，你在做投资的时候可以考虑聪明钱的信号。

Codex 理解了意图，把信号接入逻辑写进了策略里。

![图像](https://pbs.twimg.com/media/HJIvPZhXEAAHUr8?format=jpg&name=large)

现在这套系统跑着模拟交易，同时参考聪明钱信号。等胜率跑稳了我会再出一篇实盘结果。

整个搭建过程，从零到跑起来，大概只花了十几分钟。