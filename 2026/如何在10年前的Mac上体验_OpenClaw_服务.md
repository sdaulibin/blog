# 如何在10年前的Mac上体验 OpenClaw 服务

> 作者: **Mr Gafish** (@MrGafish)

> 📅 发布时间: 2026/02/03 09:36

---

看大家玩 OpenClaw 都玩的不亦乐乎，心痒痒了，于是搬出我那2015款的 Mac Pro，斑驳的屏幕，褪色的键盘，一个退休的老家伙又被我强制上班，我的系统是 macOS Big Sur 11.7.10，真的够老了。

我将尽量以技术小白的视角来讲解这次的安装过程，本教程将使用 Qwen 的免费模型，并在 Telegram 中跟 AI 交互。

根据 openclaw 官网的提示，Mac 有4种安装方法
- One-liner：一行代码安装所有的服务，包括 Node.js
- npm：基于已安装的 Node.js 服务安装 openclaw
- Hackable：从源文件编译安装
- macOS：直接用安装包，要求 macOS 14+ 以上版本

第 4 种版本要求太高，第 3 种不适合小白用户，第 1 种我亲测安装失败

最后选择第 2 种方式，不太懂技术的小白也可以跟着我的方式一起操作

▍准备工作
- 安装并找开代理软件，如我演示用的 Clash Verge
- 安装 vscode： https://code.visualstudio.com/download
- 注册 Qwen：注册并登录 https://chat.qwen.ai/ 保持页面打开状态

▍第一步：打开终端并配置代理
- 在 「应用程序」 - 「其它」 中找到并打开 「终端」
- 在你的代理软件中复制环境变量，以 Clash Verge 为例，在 「设置」- 「Verge 基础设置」- 「复制环境变量类型」，点一下旁边的复制按钮
- 在终端中粘贴一下，会显示类似 export https_proxy=http://127.0.0.1:7890 .... 的命令，回车
- 如果不小心关闭了终端，则重复第一步的操作配置代理

▍第二步：安装 Node.js
- OpenClaw 要求最低的 Node.js 版本是 22
- 进入 Node.js 官网 https://nodejs.org/en/download ，下载安装 Node 22.22.0 版本
- 这里我直接给出了下载地址： https://nodejs.org/dist/v22.22.0/node-v22.22.0.pkg
- 安装完后，在终端输入 node -v 回车，如果显示 v22.22.0 表示 Node.js 安装成功

▍第三步：安装 openclaw
- 在终端中输入 npm i -g openclaw  回车，等待安装完成
- 当终端上显示 added 1 package, and changed 692 packages in 3m 说明安装完成
- 输入 openclaw -v 回车，如果显示 2026.2.1 这样的日期说明安装成功

▍第四步：获取 Telegram 机器人 token
- 打开 Telegram 搜索 @BotFather 并点打开
- 点 Create a New Bot，为机器人设置一个以 bot 结尾的用户名
- 复制类似 123456:ABC... 这样的 token 和 机器人用户名 一起放在一边备用

▍第五步：初始化 openclaw

在终端输入 openclaw onboard 回车，当出现以下提示时，按方向键左右或上下选择

◆  I understand this is powerful and inherently risky. Continue?

选 Yes

◆  Onboarding mode

选 QuickStart

◆  Model/auth provider

选 Qwen (OAuth)

◆  Qwen auth method

选 Qwen OAuth

会跳到 Qwen 网站去授权，点 Confirm ，提示 Authentication successful 回到终端

◆  Default model

选 Keep current (qwen-portal/coder-model)

◆  Select channel (QuickStart)

选 Telegram (Bot API)

◆  Enter Telegram bot token

输入前面获取的 Telegram 机器人 token

◆  Configure skills now? (recommended)

选 Yes

◆  Preferred node manager for skill installs

选 npm

◆  Install missing skill dependencies

按空格选 Skip for now

◆  Set GOOGLE_PLACES_API_KEY for goplaces?

◆  Set GOOGLE_PLACES_API_KEY for local-places?

◆  Set GEMINI_API_KEY for nano-banana-pro?

◆  Set NOTION_API_KEY for notion?

◆  Set OPENAI_API_KEY for openai-image-gen?

◆  Set OPENAI_API_KEY for openai-whisper-api?

◆  Set ELEVENLABS_API_KEY for sag?

上面7个问题都选 No

◆  Enable hooks?

按空格选 Skip for now

◆  How do you want to hatch your bot?

选 Hatch in TUI (recommended)

◆  Install shell completion script?

选 Yes

▍第六步：修改配置文件
- 在 访达 左侧点击你的用户名，按 Command + Shift + 句点 快捷键显示隐藏目录
- 用 vscode 或 文本编辑器打开文件 .openclaw/openclaw.json
- 找到 "channels": { "telegram": {
- 在后面添加一行输入以下代码后保存 "proxy": "http://127.0.0.1:7890",
- 这里的端口号跟之前在代理软件中复制环境变量中的端口号一致
- 新打开一个终端，输入 openclaw gateway restart 回车

▍第七步：在 Telegram 中发送指令

打开 Telegram 搜索刚才申请的机器人用户名，发送 Hello 试试，跟如图一样的效果就可以开始玩了

![图片](https://pbs.twimg.com/media/HAMlX2VbQAAelXa?format=png&name=large)

---

**互动数据:** 💬 5 | 🔁 3 | ❤️ 16 | 👁️ 3037

[🔗 查看原文](https://x.com/MrGafish/status/2018498787711627772)
