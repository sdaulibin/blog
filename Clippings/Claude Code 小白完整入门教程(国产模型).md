---
title: "Claude Code 小白完整入门教程(国产模型)"
source: "https://x.com/ai_xiaomu/status/2056551528291578288"
author:
  - "[[@ai_xiaomu]]"
published: 2026-05-19
created: 2026-05-20
description: "从零开始，把一个能听懂自然语言、能自己写代码、能自动干活的 Claude code装到你电脑里，并用它做出第一个能跑、能用的产品。新手上路完全新手：从第 1 章到第 5 章按顺序做完，能跑出第一个网页小工具，就算入门。遇到任何报错：直接截图丢给 Claude Code 或豆包，描..."
tags:
  - "clippings"
---
![图像](https://pbs.twimg.com/media/HIo2xQTagAAAOWg?format=jpg&name=large)

> 从零开始，把一个能听懂自然语言、能自己写代码、能自动干活的 Claude code装到你电脑里，并用它做出第一个能跑、能用的产品。

## 新手上路

- **完全新手**：从第 1 章到第 5 章按顺序做完，能跑出第一个网页小工具，就算入门。
- **遇到任何报错**：直接截图丢给 Claude Code 或豆包，描述清楚"我在做哪一步，看到了什么报错"，比自己查 Google 高效 10 倍。

## 第 1 章 掌握基础概念

听不懂直播、看不懂手册，大多不是脑子笨，是名词没串起来。这一章就把后面会反复出现的词一次性掰开。

1.1 三个最核心的概念

```text
模型（Model）     = 大脑（只会思考，没有手脚）
Chatbot（聊天机器人）= 大脑 + 一张嘴（能说不能做）
Agent（智能体）    = 大脑 + 手脚 + 工具箱（能说能做）
```

- **模型**：GPT、Claude、Gemini、DeepSeek、豆包、千问、Kimi、GLM、MiniMax……都是模型，是"大脑"。
- **Chatbot**：ChatGPT 网页、豆包 App、DeepSeek 对话框，你跟它一问一答，它回答你，但活得你自己干。
- **Agent**：你说"帮我建一个网站"，它直接在你电脑上建文件夹、写代码、跑起来、自己调 bug，最后告诉你"做好了"。**Claude Code 就是 Agent**。

同一个模型放在不同工具里，能力天差地别。

同样是 Claude 模型，放在网页里只是聊天，放在 Claude Code 里就能自己写代码。

1.2 Token、上下文、Harness

- **Token**：AI 理解文字的最小单位。1 个中文字约 1-2 个 Token，1 个英文单词约 1 个 Token。Token 是你用 AI 的"电费"。**学习阶段不要省 Token**。
- **上下文（Context）**：你跟 AI 这一轮对话里所有内容。AI 没有长期记忆，桌面上放多少资料它就能参考多少。
- **上下文窗口**：每个模型"桌面"的大小，2026 年主流模型基本都到了 100 万 Token（约 70-80 万字）。
- **上下文污染**：在一个窗口里东聊一句、西聊一句，AI 会乱。**一个窗口聊一件事，切话题就开新窗口**。
- **Harness（挽具）/ 上下文工程**：不是某个软件，而是一套驾驭 AI 的方法——怎么组织信息、管理上下文、引导 AI、验证结果。Claude Code 之所以好用，就是因为它把 Harness 做得最成熟。

1.3 Skill 是什么

Skill 就是给 Claude 看的 **SOP 手册**。

麦当劳全球几万家店巨无霸味道都差不多，不是每个店都有大厨，是每个店都有 SOP。Skill 对 Claude 来说就是这套 SOP——把"先做什么、再做什么、用什么工具、做成什么样算合格"全部写死。

- 写一次，反复调用。
- 自己写的 SOP，等于把自己的工作经验"教"给了 Claude，它从此就是你的"虚拟员工"。

1.4 三种付费方式

![图像](https://pbs.twimg.com/media/HIotT1PbgAA9VFc?format=jpg&name=large)

**最现实的方案：用国产模型 API + cc-switch 一键切换。** 后面第 3 章细讲。

1.5 安装前要懂的辅助工具

![图像](https://pbs.twimg.com/media/HIotUYQacAACYkA?format=jpg&name=large)

1.6 MCP —— 给 Agent 用的"插座标准"

MCP 全称 **Model Context Protocol（模型上下文协议）**。

可以理解为一种**插座标准**——让 Agent 能对接各种外部工具和数据源（飞书、数据库、浏览器、小红书、微信公众号等）。

类比：

- USB 接口让你能给电脑插各种外设，插上音响电脑就多了"播放音乐"的能力。
- MCP 让你能给 Claude Code 插各种工具，插上 Tavily 它就多了"搜实时热点"的能力，插上 xiaohongshu-mcp 它就多了"发小红书"的能力。

默认情况下，Claude Code 只能处理你直接给它的信息，**不会主动上互联网搜资料**。装了 MCP 它才能搜索、读数据库、调外部 API。

第 7 章里的 Tavily MCP、Exa MCP、xiaohongshu-mcp 都是这种"插头"。

1.7 多模态 —— AI 不只会"读文字"

**模态**就是信息的类型。文字是一种模态，图片、声音、视频都是不同的模态。

- **单模态**：只能处理文字，你打字它打字。
- **多模态（Multimodal）**：能同时处理文字、图片、音频、视频、文件、屏幕。

2026 年主流模型支持的模态：

![图像](https://pbs.twimg.com/media/HIotU-qasAAS0ks?format=jpg&name=large)

输入 ≠ 输出

**AI 能"看懂"某种模态，不代表它能"生成"这种模态。**

- 图片：几乎所有主流模型都能**看图**；但只有部分能**画图**（GPT Image、Gemini 图片生成、豆包 Seedance、即梦、Nano Banana 等）。
- 视频：Gemini、豆包、Kimi 能**看视频**；但大模型本身不直接**做视频**，要借专门工具（如 Wan、Seaweed Dance、即梦）。

各家模型的强项

![图像](https://pbs.twimg.com/media/HIotVoHa0AA6swQ?format=jpg&name=large)

**给小白的实操建议**：跟 AI 协作不要只靠打字，**学会把截图、录音、视频直接丢给它**，效率会高很多。

1.8 AI 工具全景图（搞清楚谁是谁）

直播弹幕里最高频的痛点就是"这些工具到底都是啥"。一张图理清。

Agent 工具（帮你干活的）

```text
Agent 工具
├── 命令行类（在终端里用，功能最强）
│   ├── Claude Code —— Anthropic 出品，当前最强，本教程主角
│   └── Codex CLI —— OpenAI 出品，开源
│
├── 图形界面类（有可视化界面，更友好）
│   ├── Codex 桌面版/网页版 —— OpenAI 出品
│   ├── Claude Cowork —— Anthropic 出品，面向非技术用户
│   ├── Trae Solo —— 字节出品，国产，有 Code 和 MTC 双模式
│   ├── WorkBuddy —— 腾讯出品，国产，面向职场非开发
│   └── Kiro —— AWS 出品，规范驱动的 IDE（VS Code 分支）
│
├── 自主运行类（后台持续运行的智能体）
│   ├── OpenClaw —— 原 Clawdbot，开源，可微信/Telegram 远程控电脑
│   └── Hermes —— Nous Research 出品，开源，有持久记忆和自我进化
│
└── 工作流编排类（搭积木式的自动化平台）
    └── COZE（扣子）—— 字节出品，拖拽搭建工作流
```

Chatbot 工具（跟你聊天的）

```text
Chatbot
├── 海外
│   ├── ChatGPT（GPT 模型）
│   ├── Claude.ai（Claude 模型）
│   ├── Gemini（Google 模型）
│   └── Grok（xAI/马斯克，做 X 相关用）
│
└── 国产
    ├── 豆包（字节）
    ├── DeepSeek（深度求索，性价比高）
    ├── 千问（阿里）
    ├── Kimi（月之暗面）
    ├── 微信元宝（腾讯，微信生态数据强）
    └── GLM / 智谱清言（智谱 AI）
```

评论区里的"黑话"

![图像](https://pbs.twimg.com/media/HIotWL1awAAoQTp?format=jpg&name=large)

1.9 一句话总结整套关系

```text
你（人）
  ↕ 用自然语言交流
Claude Code（Agent）—— 装上 MCP 后能连任何外部工具
  ↕ 调用
模型（Claude / GLM / MiniMax / Kimi…）
  ↕ 消耗
Token（电费）
```

## 第 2 章 装好环境变量

按顺序装 4 样东西：

**Git → Node.js → VS Code → Claude Code**。

Mac 和 Windows 流程略有差异，跟着自己的系统走。

**2.1 先确认电脑的芯片类型**

下载安装包要分 ARM 和 x86 两类，先确认自己的电脑属于哪种。

- **Mac**：左上角苹果图标 → "关于本机"，看"芯片"那一栏。M1/M2/M3/M4 这种是 **ARM**；Intel 是 **x86\_64**。
- **Windows**：右键"我的电脑" → 属性，看"处理器"。Intel/AMD 是 **x86\_64**；如果显示 ARM 那就是 ARM。

记住自己是哪种，后面下安装包会用到。

**2.2 装 Git**

Mac 用户

打开"终端"（启动台搜索"终端"），粘贴下面这条命令，回车：

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/happyaicoder/HomebrewCN/raw/master/Homebrew.sh)"
```

- 中途会要你输入电脑开机密码（**输入时不会显示，直接打完回车即可**）。
- 选项让你选 1 就选 1，选 Y 就选 Y。
- 装完 brew，git 会一起装好。

验证：

```bash
brew -v
git --version
```

两条都能看到版本号就 OK。

Windows 用户

1. 打开 <[https://git-scm.com/](https://git-scm.com/)\> ，点首页中间的 **Install for Windows**，下载。
2. 双击安装，**全程默认下一步，不要改安装路径**。
3. 装完桌面或开始菜单会出现 Git Bash 图标。

> **不要自己改 Git 的安装目录**，改了后面 Claude Code 会报错。

**2.3 装 Node.js（必须 18+，推荐 20 或 22）**

Mac 用户

终端粘贴这条命令：

```bash
/bin/bash -c "$(curl -fsSL https://gitee.com/iamzhihuix/nvm-install-cn/raw/main/install.sh)"
```

验证：

```bash
node -v
npm -v
```

Windows 用户

1. 打开 <[https://nodejs.org/](https://nodejs.org/)\> ，下载 LTS 版安装程序（.msi）。
2. 双击安装，**默认下一步**，遇到"是否对设备更改"选"是"。
3. 装完打开 PowerShell，输入：

```powershell
node -v
   npm -v
```

如果 npm -v 报错"无法执行脚本"，先跑一条策略命令再试：

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

提示让你确认就输入 Y 回车。

**2.4 装 VS Code**

1. 打开 <[https://code.visualstudio.com/](https://code.visualstudio.com/)\> ，下载对应系统的安装包。
2. Mac：双击 zip 解压后，把 Visual Studio Code 拖到"应用程序"文件夹。
3. Windows：双击 exe 安装，**勾选"添加到 PATH"和"通过右键打开"等所有选项**。

**2.5 装 Claude Code**

Mac 用户

终端粘贴：

```bash
npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com
```

如果报权限错误，前面加 sudo 再跑一次（要输入开机密码）：

```bash
sudo npm install -g @anthropic-ai/claude-code
```

验证：

```bash
claude -v
```

**Windows 用户**

以**管理员身份**打开 PowerShell，先跑一条策略命令：

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

选 Y 回车。然后安装 Claude Code：

```powershell
npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com
```

验证：

```powershell
claude -v
```

**2.6 常见报错速查**

![图像](https://pbs.twimg.com/media/HIotWupbUAArigE?format=jpg&name=large)

## 第 3 章 给 Claude Code 配上国产模型

Claude Code 是工具（外壳），还需要一个模型（大脑）。官方 Claude 模型需要外网、海外信用卡、有封号风险。

**对国内用户最现实的方案是：用 cc-switch 配国产模型**。

**3.1 选模型**

目前在 Claude Code 上体验和质量比较好的国产模型有三家：

![图像](https://pbs.twimg.com/media/HIotXR6acAASoyw?format=jpg&name=large)

**新手推荐 MiniMax 套餐**：29 元/月起，足够试水。

**3.2 装 cc-switch（模型一键切换工具）**

下载页：<[https://github.com/farion1231/cc-switch/releases](https://github.com/farion1231/cc-switch/releases)\>

下载慢就用国内镜像：<[https://gitee.com/iamzhihuix/cc-switch-mirror](https://gitee.com/iamzhihuix/cc-switch-mirror)\>

- **Windows**：下 .msi 双击安装；如果报错下"Portable"免安装版，解压到固定目录，右键以管理员身份运行。
- **Mac**：下 .dmg，双击后拖到"应用程序"。第一次打开会提示"无法打开"——别慌，**系统设置 → 隐私与安全性 → 拉到最下面 → 仍要打开**，然后输入开机密码即可。

**3.3 拿到模型的 API Key（以 MiniMax 为例）**

1. 打开 minimax官网，购买key

[https://platform.minimaxi.com/subscribe/coding-plan?code=G4YsAHUVMT&source=link](https://platform.minimaxi.com/subscribe/coding-plan?code=G4YsAHUVMT&source=link)。

1. 用手机号注册登录。
2. 选套餐：新手选 29 元/月或 49 元/月，重度使用选 119 元/月。
3. 付完款，进 [https://platform.minimaxi.com/user-center/basic-information/interface-key](https://platform.minimaxi.com/user-center/basic-information/interface-key)，**注意找的是"Coding Plan Key"，不是普通 API Key**。
4. 点"复制"，把 Key 保存到一个文本文件里。

> 其他模型（GLM、Kimi、DeepSeek 等）流程一样：注册 → 进控制台 → 创建 API Key → 复制。

**3.4 在 cc-switch 里配置**

1. 打开 cc-switch，左上角点 Claude 图标。
2. 右上角点 **+**，选模型供应商（如 MiniMax / Zhipu GLM）。
3. 把 Key 粘进去，点"添加"。
4. 回主页面，鼠标移到该模型上，看到"使用中"或点"启用"。

**3.5 验证配置成功**

打开终端（Mac 终端 / Windows PowerShell），输入：

```bash
claude
```

首次启动按回车跳过主题选择，进入对话界面后输入：

> 你是什么模型？

如果回复了正确的模型名（如 MiniMax-M2.7 / GLM 5.1），配置成功。

**3.6 配置成功但还是连官方接口报错**

打开配置文件：

- **Mac**：/Users/\[你的用户名\]/.claude/settings.json
- **Windows**：C:\\Users\\\[你的用户名\]\\.claude\\settings.json（要先开启显示隐藏文件）

确认里面有 ANTHROPIC\_AUTH\_TOKEN 和 ANTHROPIC\_BASE\_URL 两项。

如果没有，回 cc-switch 重新点一次"启用"。

如果还是不行，再打开 .claude.json（在同一个用户目录下），用记事本搜 hasCompletedOnboarding，没有就加这一行（注意逗号是英文）：

```json
"hasCompletedOnboarding": true,
```

不放心格式可以丢去 [https://www.json.cn/](https://www.json.cn/) 验证。

## 第 4 章 Claude Code 基础操作

**4.1 启动 Claude Code 的两种方式**

方式一：VS Code 终端启动（新手首选）

1. 打开 VS Code → 文件 → 打开文件夹 → 新建一个空文件夹（如 我的AI工作区），打开它。
2. 信任项目（弹窗里点"是，我信任作者"）。
3. 点 VS Code 顶部"终端"菜单 → 新终端，或直接按 Ctrl+ \`。
4. 在终端里输入：

```bash
claude
```

1. 看到对话框就可以开始聊了。

方式二：VS Code 插件启动

1. VS Code 左侧扩展面板搜 **Claude Code**（认准 Anthropic 出品）。
2. 安装，重启 VS Code。
3. 右上角会出现 Claude Code 图标，点它就能打开对话框。

> ⚠️ 如果用插件方式打开后出现"订阅"画面，回 cc-switch 设置里勾上 **"应用到 Claude Code 插件"**，重启 VS Code。

4.2 必须记住的快捷操作

![图像](https://pbs.twimg.com/media/HIotX3eagAA8zUj?format=jpg&name=large)

4.3 三种工作模式（一定要分清）

Claude Code 有几个会影响"它怎么干活"的模式。新手最容易混的是这三个：

![图像](https://pbs.twimg.com/media/HIotX3xacAAufIs?format=jpg&name=large)

**Plan 模式特别重要**——Claude 会先把方案想清楚（拆步骤、识依赖、估风险），列出来给你看，你点同意它才动手。复杂功能强烈建议先用 Plan 模式过一遍。

4.4 普通思考 vs 深度思考（ultrathink）

![图像](https://pbs.twimg.com/media/HIotf5nakAEePSg?format=jpg&name=large)

**例子：**

```text
ultrathink 帮我设计一个自动发布到公众号的 skill，先不要写代码，把可能的方案对比一下
```

加了 ultrathink 后，它会消耗更多 Token 但给出更深、更全面的方案。**新手建议默认开普通思考，遇到大问题再加 ultrathink。**

4.5 YOLO 模式（解放双手的"狂飙模式"）

正常用 Claude Code 时，每改一个文件都要你确认一下，写大项目时很烦。YOLO 模式让它自己跑、不打扰你。

启动命令：

```bash
claude --dangerously-skip-permissions
```

第一次会弹个警告，选 yes 回车。看到左下角有 "bypass permissions on" 就说明开了。

**不想每次都打这一长串**，让 Claude Code 帮你配个别名：

```text
请帮我在终端配置一个别名，使得输入 claude 时自动执行 claude --dangerously-skip-permissions 命令
```

> ⚠️ YOLO 模式适合"从 0 到 1 新建项目"。在你不熟悉的项目或重要项目上用，慎重，因为它会自动改任何文件。

4.6 让 Claude 记住你的偏好（CLAUDE.md）

在 ~/.claude/CLAUDE.md（全局）或当前项目根目录的 CLAUDE.md 里写下你的偏好，Claude Code 每次启动都会自动读，相当于"长期记忆"。

例子：

```markdown
- 我是产品经理，不写代码，所有解释用大白话
- 所有生成的文件统一放到 \`outputs/\` 文件夹
- 写注释用中文
```

## 第 5 章 用 Claude Code 做一个"早读神器"

不要急着做大项目，先用一个简单的小工具体验完整流程。

**早读神器**：学生大声朗读时，麦克风音量超过阈值，屏幕上自动出现小动物（emoji），声音越大、动物越多。小红书上这类教育小工具很火，做出来的本身就是个能变现的产品。

**5.1 在 VS Code 里建工作目录**

1. VS Code → 文件 → 打开文件夹 → 新建一个文件夹（如 早读神器） → 打开。
2. 在 VS Code 里新建一个文件 prd.md（PRD = 产品需求文档）。
3. 把下面这段需求贴进去，**保存（Mac：\`Cmd+S\`；Win：\`Ctrl+S\`）**：

```markdown
请用 HTML + CSS + JavaScript 创建一个"早读动物园"应用，具体需求：

1. 主界面布局：
   - 顶部：标题"早读动物园"和副标题
   - 中间：大的天蓝色动物展示区（带浅绿色地面），动物图标随机散落分布
   - 底部三栏：左边显示当前分贝和阈值调节，中间显示计时，右边是设置和暂停按钮

2. 核心功能：
   - 使用 Web Audio API 获取麦克风音量分贝值
   - 当音量超过设定阈值时，每隔一定时间生成一个随机动物 emoji
   - 动物以随机位置、随机大小出现在展示区，带淡入动画
   - 计时器记录累计朗读时间

3. 设置面板（点击设置按钮弹出）：
   - 动物生成速度滑块（1-60 秒）
   - 动物种类选择（多选，emoji）：🐶🐱🐰🐻🐨🦊🦁🐐🐷🐸🐔🐧🦆
   - 全选/全不选按钮
   - 保存和重置按钮

4. 视觉效果：
   - 使用渐变色背景
   - 动物 emoji 大小在 30-60px 间随机
   - 卡片式设计，圆角阴影
   - 按钮用蓝色和红色区分功能

5. 技术要点：
   - 请求麦克风权限
   - 实时计算音量分贝
   - 动态生成和移除 DOM 元素
   - localStorage 保存用户设置
```

**5.2 在 VS Code 终端启动 Claude Code**

```bash
claude --dangerously-skip-permissions
```

**5.3 让它实现**

对话框输入：

```text
@prd.md 实现这个功能
```

回车，等它跑完（一般 2-5 分钟）。期间会自动建文件、写代码、跑测试。

**5.4 打开网页看效果**

1. 项目目录里右键 index.html → 选择"用谷歌浏览器打开"（Mac 在 Finder 里右键，Win 在文件资源管理器里右键 → "用浏览器打开"）。
2. 浏览器弹出"是否允许使用麦克风"，选**允许**。
3. 调节阈值，对着麦克风说话，看小动物有没有蹦出来。

跑通了，恭喜你做出第一个 Claude Code 产品。

**5.5 这一节学到了什么**

- **从小红书挖需求**：找到对标产品，截图问 AI"我想复刻这个，先帮我写好 PRD"。
- **PRD 先于代码**：好的需求文档让 AI 一次写对，省 80% 的来回沟通。
- **\`@文件名\` 是 Claude Code 的核心语法**：让它精准聚焦到某个文件。
- **不会写代码也能做出产品**：你只负责想清楚"要什么"，AI 负责"怎么做"。

## 第 6 章 学会工作流Skills

走完第 5 章，你已经能让 Claude Code 帮你写一次性的代码。

**Skills 让你把"反复要做的事情"固化成可复用的 SOP**——以后只要一句话就能调用。

**6.1 Skills 的本质**

回顾第 1 章：Skill 就是给 Claude 看的 SOP 手册。一个 Skill 包含：

- **SKILL.md**：核心配置文件，定义这个 Skill 干什么、什么时候触发、按什么步骤执行。
- **scripts/**（可选）：可执行脚本。
- **templates/**（可选）：模板文件，规定输出格式。
- **examples/**（可选）：示例文件，AI 会模仿示例的结构。

SKILL.md 长什么样

```markdown
---
name: baoyu-post-to-wechat
description: 把 Markdown 文章一键发布到微信公众号草稿箱。用户说"发到公众号"
             或"上传文章到微信"时触发。
---

# 公众号发布技能

[技能说明]
将本地 Markdown 文章转换为公众号格式并上传草稿箱。

[执行流程]
1. 读取本地 .md 文件
2. 转换为微信支持的 HTML
3. 上传图片到微信服务器换取 media_id
4. 替换文章中的图片占位符
5. 调用微信草稿箱接口发布
6. 返回预览链接

[注意事项]
- AppID 和 AppSecret 必须配对使用
- 图片大小不能超过 1MB
```

最上面 --- 之间的部分叫 **Frontmatter（前置元数据）**，最关键的就两行：

- name：Skill 的唯一标识，调用时用。
- description：**最重要的一行**——Claude 每次启动都会读所有 Skill 的 description，靠这一行判断"这次任务该不该触发这个 Skill"。所以写 Skill 时这行要尽量具体、列出触发关键词。

下面 \[...\] 里的内容是 SOP 主体，Claude 触发后会一步步照做。

**6.2 用万能安装工具 npx skills**

Vercel 出品的 [skills 安装工具](https://github.com/vercel-labs/skills)，一行命令就能给 Claude Code / Codex / Cursor / Gemini-CLI 等 30+ 工具安装 Skill。

用法（以宝玉老师的 Skills 包为例）

1. 新建并进入一个项目文件夹（如 xiaohongshu-jiazhuang），在该文件夹**右键 → 在终端中打开**。
2. 终端输入：

```bash
npx skills add jimliu/baoyu-skills
```

1. 上下方向键浏览，**空格选中你想要的 Skill**，回车。
2. 选择安装到哪个 Coding Agent（多选用空格），选 Claude Code。
3. 安装到哪里：**选 \`project\`（项目级，最推荐）**。
4. 安装方式选默认 symlink，回车，最后选 yes。

安装注意事项

1. **尽量不要全局安装**，特别是带特定业务场景的 Skill，避免模型乱激活。
2. **同一个项目内 Skill 不要超过 10 个**，容易互相干扰。
3. **不要做 Skill 套娃**（一个 Skill 激活另一个），推荐在 CLAUDE.md 里声明使用场景和流程。
4. **来源不明的 Skill 绝对不装**——Skills 有留后门的安全风险，会盗 token、盗账号。

**6.3 推荐的 Skills 市场和仓库**

![图像](https://pbs.twimg.com/media/HIotg4ObYAAJSBS?format=jpg&name=large)

**6.4 常用 Skill 速查**

```bash
# 头脑风暴
npx skills add https://github.com/obra/superpowers --skill brainstorming

# 文档协作
npx skills add https://github.com/anthropics/skills --skill doc-coauthoring

# 网页设计
npx skills add https://github.com/vercel-labs/agent-skills --skill web-design-guidelines

# SEO 审计
npx skills add https://github.com/coreyhaines31/marketingskills --skill seo-audit

# 公众号发布
npx skills add JimLiu/baoyu-skills --skill baoyu-post-to-wechat

# 小红书头图
npx skills add JimLiu/baoyu-skills --skill baoyu-xhs-images

# 文章封面图
npx skills add JimLiu/baoyu-skills --skill baoyu-cover-image
```

**6.5 第一个自定义 Skill：自动配图**

让 Claude Code 帮你做一个 Skill：每次写完文章，自动生成 3 张插画风格的配图。

**步骤 1：拿到图像生成 API Key（APImart）**

1. 打开 [https://apimart.ai/zh/model/nano-banana-2-api](https://apimart.ai/zh/model/nano-banana-2-api)
2. 注册登录（邮箱 / Google / GitHub 都行），完成邮箱验证。
3. 点【API 密钥】→【创建 API 密钥】→ 起名（如 Claude-skill）→ 复制并保存。
4. 进【账单】→ 充值，首次推荐 5-10 美元（每次生成 3 张图约 0.05 美元）。

**步骤 2：让 Claude Code 创建这个 Skill**

启动 Claude Code，在新项目目录里输入：

```text
我需要你帮我设计一个 skill，主要用来帮我实现文章配图。当我写完一篇文章之后，调用这个 skill，帮我文章配图。
我的 API 是 apimart，调用模型选择 gemini-3-pro-image-preview（Nano Banana 2），调用文档：https://docs.apimart.ai/

要求：
- 每篇文章只生成 3 张图片，前 30%、中间 30%、最后 30% 各一张
- 图片中的文字必须是中文
- 图片风格固定为手绘漫画风格，温暖、亲和、有故事感，不偏写实、不偏赛博朋克
- 横向或方形构图，画面干净，主体清晰

这个 skill 的名字叫"插画风格配图生成 skill"。
```

回车，等它跑完（3-5 分钟）。

**步骤 3：把 API Key 给它**

Claude Code 会询问 API Key，直接粘贴：

```text
我的 APImart API Key 是：[粘贴你的 Key]
请把它配置到 skill 中，并测试一下能不能用。
```

步骤 4：测试

```text
列举下现在可以用的 skills 列表
```

然后试着用：

```text
/插画风格配图生成skill "这是一篇关于春天的文章，描述了花开的美景和温暖的阳光。"
```

看到 3 张手绘风格的插画就成功了。

6.6 Skills vs MCP vs Hooks vs Plugin（四种扩展机制别搞混）

经常有人问"我该写 Skill 还是装 MCP"——它们解决的根本不是同一件事。

![图像](https://pbs.twimg.com/media/HIoth4gaIAAWLKV?format=jpg&name=large)

**类比理解**：

- **Skill 像菜谱**——告诉厨师按什么步骤做菜。
- **MCP 像厨房的水电气**——没有这些基础设施，厨师做不了菜。
- **Hooks 像定时器**——到点自动响铃提醒。
- **Plugin 像一整套厨具组合**——锅碗瓢盆食谱一起送给你。

**新手判断标准**：

- 想让 Claude **稳定完成某个流程** → 写 Skill
- 想让 Claude **能用某个外部服务/工具** → 装 MCP
- 想让 Claude **自动响应某种事件** → 配 Hooks
- 想**一站式安装某个能力组合** → 找 Plugin

第 7 章的公众号发布就是 **Skill + MCP 配合**：Skill 负责整体流程，MCP（Tavily/Exa）负责搜热点这个能力。

6.7 Skill 用不好的几个坑

![图像](https://pbs.twimg.com/media/HIotjE-awAA_qKF?format=jpg&name=large)

## 第 7 章 硬件与网络

7.1 硬件怎么选

**Mac 比 Windows 更适合 AI**：M 系列芯片"内存显存共享"架构，同样 32GB 内存，Mac 上能跑的本地模型规模比 Windows 大很多。

系统环境也更干净，少很多奇怪报错。

但**已有 Windows 的不用换**——本教程的所有内容都能在 Windows 上跑。

7.2 网络

- 用国产模型 + cc-switch 不需要外网。
- 用 Claude 官方模型、订阅 Pro/Max 需要稳定外网，且要海外信用卡。
- 网络报错（特别是图生图、MCP）时，把代理改成 **TUN 模式 / 增强模式 / 虚拟网卡模式**。推荐 clash.verge 客户端，老版本 clashx 不推荐。

## 附录 A：核心命令速查表

```bash
# 启动 Claude Code
claude

# 启动 YOLO（自动模式）
claude --dangerously-skip-permissions

# 看版本
claude -v

# 看已装的 Skills
/skills list      # 或在 VS Code 插件里直接说："列举下现在可以用的 skills 列表"

# 刷新 Skills 列表
/skills refresh

# 清空当前对话
/clear

# 清理但保留记忆
/compact

# 检测安装问题
/doctor

# 切换模型
/model

# 装 Skill（万能命令）
npx skills add <github-repo>
```

## 附录 B：常见报错与处理

![图像](https://pbs.twimg.com/media/HIotjz0b0AAE9vr?format=jpg&name=large)

## 附录 C：小白名词速查

教程里一闪而过、但你可能卡住的词，每个一句话讲清楚。

Markdown / 编辑器

![图像](https://pbs.twimg.com/media/HIotkwmaAAASXvC?format=jpg&name=large)

配置文件

![图像](https://pbs.twimg.com/media/HIotsaoawAAY9IW?format=jpg&name=large)

命令行与环境

![图像](https://pbs.twimg.com/media/HIots_jaAAA0_5S?format=jpg&name=large)

网络与认证

![图像](https://pbs.twimg.com/media/HIots_waAAA8Rs6?format=jpg&name=large)

项目流派

![图像](https://pbs.twimg.com/media/HIotryma8AAQeTW?format=jpg&name=large)

## 写在最后

**先上路，再谈车技。**

不用着急一次全搞懂所有概念。

先把第 2-5 章走完，做出第一个能用的小工具，你就比 99% 的人更早摸到了门道。

剩下的——名词、Skill、MCP、Agent 之间的关系——会在你用的过程中自然变清晰。

遇到任何报错、任何看不懂的东西，截图丢给豆包、ChatGPT 或者 Claude Code 本身，描述清楚"我在做哪一步、看到了什么"，比反复读教程快 10 倍。

会用 AI 和不会用 AI 的人，生产力差距已经是 10 倍、100 倍。

从今天开始，慢一点没关系，但要开始。

**黄小木｜T11级架构师｜《30天2万粉2周2k刀》作者｜持续分享 AI 信息、副业赚钱、程序员转型OPC心得｜X：**[@ai\_xiaomu](https://x.com/@ai_xiaomu)