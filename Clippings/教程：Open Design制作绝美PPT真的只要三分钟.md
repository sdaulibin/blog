---
title: "教程：Open Design制作绝美PPT真的只要三分钟"
source: "https://x.com/yinmin1987/article/2053645190347518115"
author:
  - "[[尹珉 (@yinmin1987)]]"
published: 2026-05-11
created: 2026-05-11
description: "我最近要做一份 Milvus 2.6 的路演 deck。按以前的路径，要么打开 PowerPoint 从空白页开始拼（封面怎么排、目录放哪、章节怎么分，全靠自己想），要么找个模板改（改完发现配色对不上、版式逻辑乱）。Claude Design 能做这事，但它付费、云端、锁死 An..."
tags:
  - "clippings"
---
![图像](https://pbs.twimg.com/media/HIADXD9asAAPdMa?format=jpg&name=large)

我最近要做一份 Milvus 2.6 的路演 deck。按以前的路径，要么打开 PowerPoint 从空白页开始拼（封面怎么排、目录放哪、章节怎么分，全靠自己想），要么找个模板改（改完发现配色对不上、版式逻辑乱）。

Claude Design 能做这事，但它付费、云端、锁死 Anthropic 模型。我电脑上已经有 Claude Code 了，每天用它写代码，凭什么做个路演 PPT 还得再开一个工具、再付一份钱？

后来发现 Open Design 这个项目。它不自带 AI 引擎，直接用你电脑上已有的 coding agent CLI 当设计引擎——装了 Claude Code 就用 Claude Code，装了 Codex 就用 Codex，一共支持 16 种。给它们套上设计系统和流程约束，同一个 agent 就能从写代码切到做 PPT。

![图像](https://pbs.twimg.com/media/HIABnbFbQAA4R52?format=jpg&name=large)

这篇文章记录我用它做 Milvus 2.6 路演 deck 的完整过程，以及它到底解决了什么、解决不了什么。

## 直接让 agent 做 PPT，问题出在哪

用 coding agent 生成演示材料，技术上完全可行。但跑过的人都知道结果：配色随机，排版没章法，同一个 prompt 跑三次出三种风格。封面像海报、目录像文档、正文像网页，前后统一不了。

问题不在模型能力。你让 agent "做一个路演 PPT"，它不知道该用什么视觉节奏、什么配色逻辑、章节之间怎么衔接，只能自由发挥。每次都是一次抽奖。

Claude Design 用一套闭源的 prompt 工程和设计系统解决了这个问题。但代价是全套锁定——模型不能换、本地不能跑、工作流不能接。对于已经有 coding agent 的开发者来说，这个代价没必要付。

## Open Design 的做法：不造 agent，造纪律

Open Design 的核心决策很反直觉：作为一个设计工具，它不带 AI 引擎。

启动时 daemon 扫描你的 PATH，找到哪些 coding agent CLI 已经装好，找到谁就用谁，界面上一键切换。没装任何 CLI 也行，填一个 OpenAI 兼容的 API 地址就能跑。

它真正做的事情是给 agent 加纪律，靠三层东西：

31 个内置 Skill——每个是一个文件夹，里面有模板、排版规则、检查清单。你要做 PPT，daemon 就把 deck 类 skill（比如内置的 guizang-ppt）的全套规范注入 agent 的 prompt——杂志式排版、WebGL 封面背景、P0/P1/P2 质量检查清单。agent 不再自由发挥，而是按规矩出活。

72 套 Design System——Markdown 格式的品牌级视觉规范。选 Claude 体系就出 Claude 风格，选 Stripe 就出 Stripe。配色、字体、间距全锁死，不给 agent 自由发挥配色的空间。

Discovery Form——每次新任务先弹表单锁需求（给谁看、什么调性、什么规模），再从 5 种视觉方向里选一个，每种背后是确定性的调色板和字体栈。填完 agent 才动手。

最后还有一道 5 维自检：agent 生成完自己打分，不及格自己改。

这套东西加起来，agent 的能力是底座，Open Design 提供的是让它稳定出活的纪律。

## 实操：做一份 Milvus 2.6 路演 deck

前置条件

- 至少安装 Claude Code 或 Codex 其中一个 CLI
- 准备好可用的模型 API Key
- Node 24、pnpm

下载和启动

```text
git clone https://github.com/nexu-io/open-design.git
cd open-design
corepack enable
corepack pnpm --version   # should print 10.33.2
pnpm install
pnpm tools-dev run web
```

启动后终端会打印一个本地 URL。整个过程就是标准的 Node 项目启动流程，没有额外的配置文件要改。

![图像](https://pbs.twimg.com/media/HIAB8zJbUAAXr1_?format=jpg&name=large)

配置 agent

打开 Web 界面后，第一步是配置本地 CLI。它自动检测到了我电脑上的 Claude Code，点一下就绑定了，不需要手动填路径。

![图像](https://pbs.twimg.com/media/HIACAAZbwAAktJX?format=jpg&name=large)

选择设计类型：PPT

这一步选了 PPT 类型。界面上还有 web prototype、mobile app、dashboard 等选项，但这次目标明确——路演 deck。

![图像](https://pbs.twimg.com/media/HIACDR1acAAQzOp?format=jpg&name=large)

选择设计体系

这里我选了 Claude 设计体系。72 套里随便挑，选完之后 agent 后续所有输出都遵循这套视觉语言。这一步的好处是：你不需要自己想配色，选一个喜欢的风格就行。

![图像](https://pbs.twimg.com/media/HIACGh7aMAAP8bn?format=jpg&name=large)

一句话需求

输入框里写了一句话：

> 我要一个介绍 Milvus2.6 从概念到精通的路演 PPT

没有写详细大纲，没有指定页数，就这一句。我想看它在最少输入下能做到什么程度。

生成完成

等了大约两三分钟，deck 生成完毕。过程中能看到 agent 的 todo card 在流转——先规划结构，再逐页生成，最后跑自检。

![图像](https://pbs.twimg.com/media/HIACJz7bQAAGb5X?format=jpg&name=large)

查看结果

生成的 deck 有完整的封面、目录、章节分页和结尾页。以下是几页效果：

![图像](https://pbs.twimg.com/media/HIACNB9bQAALNNE?format=jpg&name=large)

![图像](https://pbs.twimg.com/media/HIACPcwa4AA85LD?format=jpg&name=large)

![图像](https://pbs.twimg.com/media/HIACRxLaQAAKCFE?format=jpg&name=large)

![图像](https://pbs.twimg.com/media/HIACUDna4AA9n4i?format=jpg&name=large)

## 生成结果能不能用

先说结论：作为路演 deck 的初稿骨架，能用；作为最终交付稿，不够。

能用的部分：整体结构是合理的——从概念介绍到架构、到核心功能、到实战场景，章节划分符合"从入门到精通"的逻辑。封面、目录、章节分隔页的视觉风格统一，配色和排版遵循了选定的设计体系，不是随机拼凑。作为一份"先把骨架搭好、再往里填内容"的起点，它省掉了最费时间的起稿阶段。

需要改的部分：技术内容的准确性需要人工校验——agent 对 Milvus 2.6 的具体特性了解有限，可能存在版本细节错误或表述不精确的地方。另外，它输出的是 HTML artifact 而不是 .pptx 文件。这意味着你在浏览器里看到的效果很好，但如果要拿去公司模板里用、或者需要 PowerPoint 格式交付，还需要一步导出或手动搬运。

关于输出格式要多说一句：Open Design 的 PPT 模式生成的是单页 HTML，用浏览器横向翻页，视觉上就是一份 deck。它支持导出 PDF，但不直接输出 .pptx。如果你的场景是投屏演示或发 PDF，它的输出可以直接用；如果必须交付 PowerPoint 源文件，这里有一步格式转换的成本。

## 它省掉的到底是什么

从做路演 deck 这个场景看，Open Design 省掉的是三件最费时间的事：

一是起稿。从空白页到有一个完整的章节骨架（封面、目录、N 个章节页、结尾页），以前要花半小时到一小时想结构、调版式。现在一句 prompt 出来了。

二是视觉方向。配色用什么、字体用什么、封面怎么排、章节页怎么分隔——这些以前要么翻模板库挑半天，要么自己试错。现在选一个 Design System 就锁定了，agent 按规矩出活。

三是风格一致性。手动做 deck 最烦的是做到第 8 页发现和第 2 页风格对不上。Open Design 的 Design System 从头锁到尾，每一页都在同一套视觉语言里。

它没省掉的是内容本身。agent 能帮你搭骨架、排版式，但"Milvus 2.6 到底有哪些新特性、路演时该强调什么"这些判断还是你自己的事。

## 边界在哪

几个场景它撑不住：

内容准确性。agent 对特定技术产品的了解有限，生成的技术细节需要人工校验。把它当内容起稿工具，不要当事实来源。

品牌视觉的精确还原。如果你的公司有严格的品牌手册——指定字体、指定色号、指定 logo 间距——Open Design 的 72 套 Design System 里大概率没有你的品牌。它出的是"好看且统一"的通用设计，不是"精确符合品牌规范"的定制设计。

复杂动画和交互。它输出的是静态 HTML deck，没有 PowerPoint 那种入场动画、路径动画、触发器交互。如果你的路演依赖动画叙事，这里是硬伤。

最终交付格式。输出是 HTML / PDF，不是 .pptx。如果对方要求 PowerPoint 源文件可编辑，你需要额外一步格式转换，或者把 Open Design 的输出当作视觉参考、在 PowerPoint 里手动重建。

还有一个隐性边界：agent CLI 本身的能力上限就是 Open Design 的上限。你用的 agent 对 HTML/CSS 生成不稳定，框架再好也救不回来。目前跑下来 Claude Code 和 Codex 效果最稳。

## 写在最后

如果你经常要做技术路演、产品分享、团队汇报这类 deck，而且你电脑上已经有 Claude Code 或 Codex，Open Design 值得一试。

它最适合的角色是"deck 起稿工具"：一句话需求出一份结构完整、视觉统一的初稿，然后你在这个基础上改内容、调细节。比从空白页开始快得多，比翻模板库挑半天靠谱。

它不适合的场景：需要精确品牌视觉、需要 .pptx 源文件、需要复杂动画、需要内容零校验直接交付。

三条命令启动，不需要新 API key，不需要学新工具，不需要付费。门槛几乎为零，值得你花十分钟跑一次看看效果。

![图像](https://pbs.twimg.com/media/HIADRPTaEAABLf6?format=png&name=large)