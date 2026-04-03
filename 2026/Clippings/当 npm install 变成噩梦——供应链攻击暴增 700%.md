---
title: "当 npm install 变成噩梦——供应链攻击暴增 700%"
source: "https://x.com/MinLiBuilds/article/2039319515830608216"
author:
  - "[[实践哥MinLi (@MinLiBuilds)]]"
published: 2026-04-01
created: 2026-04-02
description:
tags:
  - "clippings"
---
当你执行 npm install 的那一刻，你真的知道自己安装了什么吗？

时间线ClaudeCode代码泄漏刷屏了，最近经常刷到供应链攻击，加上我中招过，我想梳理一下。

> 2026 年 3 月 25 日我装了 browser-use，上午装的，一打开就有无数 python 进程启动电脑卡死。claude 一启动就自动加载mcp 就会启动无限个 python。后来查到是litellm被注入了木马。

> 2026 年 3 月 31 日，全球最流行的 HTTP 客户端库 Axios 被攻破。攻击者劫持维护者 npm 账号，在每周 8300 万次下载的包中植入跨平台远程控制木马。这是 2026 年 Q1 第三起重大供应链攻击——在它之前，AI 框架 LiteLLM 和安全工具 Trivy 已相继沦陷。

本文基于 XCrawl Skill + Claude Code，对真实数据采集与分析，梳理供应链攻击的趋势全貌。

## 一、数据采集方法

本报告的真实数据采集全部基于**XCrawl**完成。

> 1\. 对Claude Code说：安装[https://github.com/xcrawl-api/xcrawl-skills](https://github.com/xcrawl-api/xcrawl-skills) 2. 简单配置后，对 CC 说： /xcrawl-search 搜索供应链攻击，整理出近期代表性的，并整理供应链攻击的趋势，形成一个趋势图表

整个采集流程消耗 **25 credits**（Search 20 + Scrape 3 + Map 1 + Crawl 1），从搜索到拿到结构化数据不到 1 分钟。以下分析结论均基于这些真实抓取数据。

## 二、一条陡峭的上升曲线

根据 Sonatype 时间线页面的抓取数据和多家机构报告，全球软件供应链攻击呈指数级增长：

![图像](https://pbs.twimg.com/media/HE0aG5MaQAA409P?format=jpg&name=large)

来自 GitHub 官方博客的抓取数据显示：仅 2024 年一年，GitHub 就检测到 **3,900 万个泄露的 secret**。DeepStrike 的统计数据进一步指出：第三方供应链导致的数据泄露占比已达 30%（DBIR），平均泄露成本 444 万美元（IBM）。

经济损失方面，2025 年全球损失达 600 亿美元，预计 2031 年攀升至 1,380 亿美元。

## 三、AI 普及加速供应链攻击的三条路径

搜索和抓取的数据清晰揭示了 AI 与供应链攻击之间的三条因果链：

**路径一：AI 幻觉抢注**

这是 AI 普及带来的最直接威胁。研究显示，**每 5 段 AI 生成的代码中就有 1 段包含虚构的包名**。当 ChatGPT 或 Copilot "幻觉"出一个不存在的包（如 flask-jsonschema），攻击者只需在 npm 或 PyPI 上抢注该名称并植入恶意代码，就完成了一次完美的供应链攻击。

更可怕的是，攻击者已经开始**系统性地监控各模型高频幻觉的包名**，预先注册并埋入恶意代码。IIM Calcutta 的研究估计，这一风险的潜在经济损失达 **2,500 亿美元**。

**路径二：AI 基础设施本身成了目标**

AI 普及意味着大量组织在生产环境中部署了 LLM 代理、向量数据库、模型编排框架。这些系统通常持有多个云服务的高权限凭据（OpenAI API key、AWS secret、数据库连接串）。

**路径三：AI 武装了攻击者**

Google Mandiant 2026 年 3 月的报告披露：到 2025 年底，威胁行为者已经将 LLM API 直接集成到恶意软件中，实现"即时代码生成"——每次执行时动态生成变体，绕过签名检测。

OpenSSF 在 2025 年 1 月的预测中警告：AI + 国家级行为者 + 供应链 = 2025-2026 年最大的开源安全威胁。

## 四、代表性事件复盘

**2024：XZ Utils 后门。** 攻击者潜伏两年获得维护权，植入绕过 SSH 认证的后门。同年 [Polyfill.io](https://polyfill.io/) CDN 被收购后植入恶意脚本，影响 10 万+ 网站。

**2025：npm 蠕虫大爆发。** tj-actions/changed-files 泄露 23,000 仓库的 CI/CD secrets。9 月 npm 蠕虫感染 debug、chalk 等基础库，180+ 个包被投毒，CISA 发布全国警告。

**2026 Q1：AI 基础设施沦陷。** Trivy 被篡改 76 个历史标签；LiteLLM 被植入三阶段后门；Axios 维护者账号被劫持。攻击目标全面转向 AI 和安全工具。

## 五、应对建议

![图像](https://pbs.twimg.com/media/HE0eF5HaUAAkT_K?format=png&name=large)

1. **验证 AI 生成的每一个依赖。** 不要盲目 pip install AI 推荐的包名。先去 npm/PyPI 确认包的真实性、维护者和下载量。
2. **锁定依赖版本。** lock file + 哈希值，每次更新像代码一样 review。
3. **限制 AI 应用的运行权限。** AI agent 持有的凭据越多，被攻破后危害越大。遵循最小权限原则。
4. **启用安全特性。** npm provenance attestation、GitHub secret scanning、push protection。
5. **持续监控供应链情报。** 利用 XCrawl 等工具对安全公告页面建立自动化抓取告警，确保关键依赖出现事件时能小时级响应。

## 结语

AI 普及与供应链攻击的关系，是多重因果：AI 制造了新的攻击面，AI 基础设施扩大了攻击价值（LiteLLM），AI 增强了攻击者的能力（自动化社工和代码生成）。三条路径同时作用，构成了一个正反馈循环。

当越来越多的代码由 AI 编写、越来越多的基础设施由 AI 驱动、越来越多的攻击由 AI 辅助——供应链安全的挑战不是在线性增长，而是在指数级恶化。

也许下一次你让 Claude Code 帮你选一个库的时候，值得多花 30 秒用 XCrawl去验证一下：这个包，真的存在吗？

本次数据采集使用XCrawl Search 共花费 25 credits，体验非常丝滑。