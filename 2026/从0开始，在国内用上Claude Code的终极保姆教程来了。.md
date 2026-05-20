---
title: "从0开始，在国内用上Claude Code的终极保姆教程来了。"
source: "https://x.com/Khazix0918/article/2046082879109959807"
author:
  - "[[数字生命卡兹克 (@Khazix0918)]]"
published: 2026-04-20
created: 2026-04-20
description: "最近很多朋友都在问我，能不能出一期Claude Code的小白教程。他们也想用上这个世界上最牛逼的Agent产品。而且其实很多人不太知道，Agent产品一般是Agent框架+模型组成的，Claude的模型国内确实会封，会非常的难搞，我也没有任何办法教大家。但Claude Code..."
tags:
  - "clippings"
---
![图像](https://pbs.twimg.com/media/HGUbENnbkAAclyw?format=jpg&name=large)

最近很多朋友都在问我，能不能出一期Claude Code的小白教程。

他们也想用上这个世界上最牛逼的Agent产品。

而且其实很多人不太知道，Agent产品一般是Agent框架+模型组成的，Claude的模型国内确实会封，会非常的难搞，我也没有任何办法教大家。

但Claude Code不会被封，也不会用不了，因为这玩意其实就是个Agent框架，搭配任何模型都可以使用。

虽然Anthropics确实很狗，天天封号，又搞实名认证，但我依然不得不承认，这个世界上目前最好的Agent框架，还是Claude Code。

所以我常年说，能一步到位就一步到位，我当然知道现在比如什么OpenClaw、Hermers Agent等等非常火，但是我还是依然会建议你使用Claude Code，即使用不了Claude的原生模型，你搭配个国产模型，效果也依然很好。

而且也不用担心封号，不需要外国手机号，visa卡，甚至都可以不上魔法。

所以今天，就来一篇Claude Code的从0入门全面新手教程，并且尽可能让所有的朋友，都可以用上，Windows和Mac，有魔法没魔法的操作，我都准备了，大家按需看对应的部分就行。

下面的安装流程，是我一整个周末，跟我们小伙伴一起，折腾了五六台电脑反复安装卸载试出来的。

比如有些场景，像没有魔法，其实还有其他安装方式，像npm，又或者直接curl国内镜像源，这些办法其实也能用，但我在不同电脑上测试的时候并不够稳定。

所以最后，我选了在我看来最简单，并且在极度原始的电脑上测试也不容易翻车的方式。

只希望大家跟着文章，都能顺利地用上世界上最牛逼的Agent框架。

我会把每一步都给大家说得尽量详细清楚，可能会有些啰嗦，大家别介意。

好了，我们直接开始。

**一. Claude Code 安装**

**1\. Mac**

先来看Mac，Windows同学可以直接跳过Mac这一趴去下面找Windows的教程。

我们先在App中找到终端打开。

我们先来安装一下今天的主角，Claude Code。

这里我给自己的电脑新建了一个全新的macOS账号，基本等同于空电脑，方便演示。

**先聊有魔法的情况。**

命令就一句话。

curl -fsSL [https://claude.ai/install.sh](https://claude.ai/install.sh) | bash

我们在终端粘贴这条命令后，按下回车。

等一会，就能看到安装成功

虽然装好了，但这里有可能他也会给出一个提示。

![图像](https://pbs.twimg.com/media/HGUb1CgaYAAfS7J?format=png&name=large)

意思是说，Claude Code已经装好了，但Claude Code的安装位置~/.local/bin还没加到你的PATH环境变量里，所以你直接敲claude可能找不到这个命令。

还说要解决这个问题，请执行下面那条命令巴拉巴拉。

看不懂也没关系，我们就按他说的，把他给出的那一长串echo命令复制到终端里，回车跑一下。

然后输入claude --version，有版本号输出，就表示安装成功了。

![图像](https://pbs.twimg.com/media/HGUb5jUa8AEr9QL?format=png&name=large)

有魔法的情况非常简单，但是我也知道，很多同学是没有魔法的。

**所以，如果没有魔法的话，我们可以通过homebrew安装。**

Homebrew是macOS上最流行的命令行包管理器，作用是让你用一条命令就能安装、更新、卸载各种软件和开发工具。

这里我借了一台我们经纪小伙伴的新电脑，没有魔法，环境非常干净。

我们先来装一下brew，看着可能会有点复杂，但是其实你跟着做，特别简单。

把下面这行命令粘贴到命令行，回车运行。

/bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"

提示之后，我们直接回车.

然后他就会跑啊跑啊。

耐心等待几分钟，等他跑完，出现安装成功就装好了。

下一步，我们需要把homebrew加到路径变量里面去，这样我们在终端使用homebrew的时候他才能找到命令。

![图像](https://pbs.twimg.com/media/HGUkBvEbQAAJFVR?format=jpg&name=large)

把这几行命令粘贴到终端里跑一遍.

![图像](https://pbs.twimg.com/media/HGUcIqRa8AAdbXj?format=jpg&name=large)

这样，我们就可以使用homebrew来管理Mac包了.

接着，用下面这行命令来装Claude code。这里因为公众号编辑器会自动改一些格式，直接复制粘贴会报错。辛苦大家手敲一下，或者发给claude让他改了，再粘贴到终端。

brew install --cask claude-code@latest

![图像](https://pbs.twimg.com/media/HGUcMsHa4AAt9Ne?format=jpg&name=large)

这里的安装速度有点慢，大家可以先去抹灰鱼，忙完了回来看。

等安装成功出现，我们在终端输入claude，就能看到小螃蟹了。

![图像](https://pbs.twimg.com/media/HGUcQWaagAA5SR7?format=jpg&name=large)

但是这里，会显示用不了，可以先不管，一会我们会教大家怎么接上模型

mac说完了，然后在单独说一下Windows，会稍微有一点点不一样，安装好的Mac同学可以直接跳过这一趴。

**2\. Windows**

再来看Windows。

我这里拿了一台刚刷机过的Windows来装了一遍。

因为Claude Code在Windows上内部是用Git Bash来执行命令的，所以要想在Windows上用Claude Code，必须先把Git安装上。

所以第一步，我们先把Git装上。

已经装好的朋友可以跳过这一趴。

用WinGet来装，这个东西是Windows官方的包管理器，可以理解成Windows版本的Homebrew。

我们在任务栏搜索终端打开。

![图像](https://pbs.twimg.com/media/HGUcUk0aEAAQ6jq?format=jpg&name=large)

粘贴下面的命令到终端，这里安装的时候不开魔法速度会快很多。

winget install Git.Git

跑完就会显示成功安装了。

![图像](https://pbs.twimg.com/media/HGUcXk8awAEVDoq?format=jpg&name=large)

**Git装好了之后，老规矩，我们先说有魔法的情况。**

有魔法的朋友，我们还是用他官方的原生安装命令。

粘贴命令到终端。

irm [https://claude.ai/install.ps1](https://claude.ai/install.ps1) | iex

等一会就安装好了。

非常便捷。

![图像](https://pbs.twimg.com/media/HGUhWvSaEAAVBrB?format=jpg&name=large)

**如果没魔法的话，我们就得使用WinGet来安装。**

在终端中运行这行命令。

winget install Anthropic.ClaudeCode

等他安装成功后，同样可以输入claude，就能看到已经安装成功了。

![图像](https://pbs.twimg.com/media/HGUhfcjawAA1Jzu?format=jpg&name=large)

到这一步，从道理上讲，我们在终端里输入claude，就能进到Claude Code页面了

但是。

我们这里光装了框架，还没给他安脑子，所以还没法用。

那接下来，我们就需要把他的脑子接上。

**二. 接模型**

如果是有Claude账号的，直接登录就行了，这里我就不细说了。

因为理论上你已经有Claude账号了，你也不会来看这个保姆教程。。。

所以，为了让国内所有的小伙伴都能用上，这里我用国产模型GLM-5.1来举例子。

因为GLM-5.1是目前我用下来觉得国内效果最好最接近Claude Opus 4.6体验的。

当然，你要是没抢到他们的coding plan的话，用MiniMax M2.7和K2.5也都不错，K2.6 code应该也快出了，感觉kimi也会有一波飞跃，推荐大家可以蹲一下。

然后GLM5.1这块，智谱官方有提供一句话命令来安装，特别简单。

npx [@z\_ai/coding-helper](https://x.com/@z_ai/coding-helper)

但这里，为了方便大家也能接入其他模型并且随意切换，所以，我们教大家一个更通用的方法，也就是，用CC Switch。

还是分Mac和Windows。

**1\. Mac**

在Mac上，他的安装就两行命令。第二行命令同样因为格式原因，直接复制粘贴会报错。辛苦大家手敲一下，或者发给claude让他改了，再粘贴到终端。

brew tap farion1231/ccswitch

brew install --cask cc-switch

在终端粘贴，回车运行。

等他装好就成了。

![图像](https://pbs.twimg.com/media/HGUhlN9bMAA9Byo?format=jpg&name=large)

**2\. Windows**

Windows的话，推荐直接去下面的链接下载安装包。

[https://github.com/farion1231/cc-switch/releases](https://github.com/farion1231/cc-switch/releases)

![图像](https://pbs.twimg.com/media/HGUdCw4aUAA10On?format=jpg&name=large)

**如果你进不去github，那我也给大家准备好了本地安装包，你对着公众号后台回复cc就会自动给你发下载链接了。**

下载后双击运行，然后可以一路next到底，就安装好了。

![图像](https://pbs.twimg.com/media/HGUdF-rbEAA2KsJ?format=png&name=large)

后面的操作Mac和Windows一样，就不分开说了。

装好后，我们打开他。

一进去能看到，这玩意其实不仅适用于Claude Code，Codex、小龙虾这些都能用。

因为我们这里还没配置，所以目前只有Claude官方的模型配置。

在Claude那一栏下面，我们点右上角的加号，新增模型配置。

![图像](https://pbs.twimg.com/media/HGUdJ4JbsAALHOF?format=jpg&name=large)

然后选择我们想要使用的模型，这里我选的是GLM国内版。

![图像](https://pbs.twimg.com/media/HGUdMQSa0AA5-a8?format=jpg&name=large)

接着要填的就两部分，API key（这块如果不知道什么是API Key的，可以去直接问你能用上的任何AI，他们都会帮你解决）和模型配置，其他的他都会自动帮我们填好。

![图像](https://pbs.twimg.com/media/HGUdPZ9bgAAn9GN?format=jpg&name=large)

填好之后，我们点击右下角的添加就行。

他就会切换到我们配置的模型上。

到这一步，Claude Code的安装和GLM-5.1的接入就完成了。

**三. 启动Claude Code**

我们回到终端，输入claude，回车。

![图像](https://pbs.twimg.com/media/HGUhyTkbkAAZCXe?format=jpg&name=large)

就可以正常启动claude code了。

第一次使用，会先有一些初始化设置。

比如颜色模式，模式下面有代码预览，大家根据自己的喜好来就好。选中回车。

以后想改，在Claude Code里运行/theme就行。

接着是安全提示。

![图像](https://pbs.twimg.com/media/HGUh6dZbEAE886L?format=jpg&name=large)

就两点。

一个是Claude会犯错，它生成的代码、它要执行的命令，你都应该过一眼再放行。

一个是只在你信任的代码库里用Claude Code，避免提示词注入攻击。

我们直接回车进入下一步。

是问我们是否使用Claude Code的终端设置。

![图像](https://pbs.twimg.com/media/HGUiEFIbEAAoXq6?format=png&name=large)

这里，直接使用他推荐的终端设置就可以。

其实就是他想帮你启用两个东西。

快捷键实现在终端里换行。

另一个是Visual bell，视觉提示。

也就是Claude跑完任务或者需要你确认的时候，终端窗口的页面会闪一下，Dock图标会弹跳一下，提醒你。

最后一步，就是和你确认当前所在目录，是否可以信任。

![图像](https://pbs.twimg.com/media/HGUiQ7ea4AAm2qH?format=jpg&name=large)

这里选择是，然后回车。

我们就终于来到接入GLM-5.1的Claude Code的对话界面了。

后续想要切换模型，在CC Switch里面配置好，在Claude Code里面用/model切就行。

![图像](https://pbs.twimg.com/media/HGUiOgTa0AAeBvz?format=jpg&name=large)

至此，所有的安装接入操作就完成了。

后续用的话，直接在终端输claude就行了。

这里推荐大家用下面这行命令，特别是开发的时候，不然点各种allow会点到你怀疑人生。

claude --dangerously-skip-permissions

然后我们启动的时候，因为上下文设计，为了让他有约束，更加的专注，所以我们是需要对着一个文件夹进行启动的，而不是直接在根目录启动Claude Code。

我自己就分了一下目录，文稿是我拿来做知识创作了。

![图像](https://pbs.twimg.com/media/HGUiVIhawAAWga_?format=jpg&name=large)

还有一个code文件夹，都是我自己开发的各种各样的产品。

在命令行中对着一个命令行启动Claude Code也特别简单。

就是cd命令，这块Mac和Windows是一样的。

比如我要进入知识库这个文件夹进行创作。

那就打开终端，输入cd，然后一定要记得按一下空格，再把你的文件夹，直接拖进去。

按下回车，就算是进入这个文件夹了。

然后这个时候再用命令启动Claude Code就可以。

你就会进入到这个文件夹下，它默认就只在这个文件夹下工作，默认读取你这个文件夹下的所有文件。

![图像](https://pbs.twimg.com/media/HGUkTwibQAAlsYl?format=jpg&name=large)

这样其实上下文污染更小，也更专注，换句话说，就是更聪明。

**四. 写CLAUDE.md**

学会启动之后，其实你就可以正式的对话了，随便让他干活就行。

但还有个规范和你在深度使用之前，有一个很重要的习惯，我觉得是需要让你先设置的，不要再踩我走过的老坑，先定好你的CLAUDE.md文件。

而在我看来，这甚至是学会启动Claude Code之后，第一件该做的事。

在上具体写法之前，我还是先和大家聊一聊CLAUDE.md是个啥。

这个东西，它不简单是一份文件，它是一个从上往下分层穿透的约束体系。

就像我之前用过的这张图。

刚装完，我们应该去管的两层，就是全局CLAUDE.md和项目CLAUDE.md。

全局CLAUDE.md，放在用户总目录的Claude Code根目录下面，~/.claude/CLAUDE.md。

![图像](https://pbs.twimg.com/media/HGUka80bgAAtspG?format=jpg&name=large)

只要你打开Claude Code，不管你进的是哪个项目，它都会被自动加载和遵守。

这是他的顶层规范。

它可以解决的是你是谁、你做事的原则、你希望他用什么方式跟你协作这一层的问题。

而项目级CLAUDE.md，放在每个项目的根目录下，路径就是项目目录/CLAUDE.md

它只在你打开这个项目的时候才会被加载。

它解决的是这个具体的项目要怎么干，有什么特殊约定这一层的问题。

我们先来聊全局CLAUDE.md，也是第一个需要定好的东西，但是到底该往CLAUDE.md里写什么、怎么写、写多长、放哪里，很多人其实不清楚，对于非开发者来说，我也分享一下我的经验。

关于长度，CLAUDE.md不是越长越好，反而是要尽量精简。

你的CLAUDE.md写得太长，后半段的内容它会直接忽略掉。

具体的红线数字是这样的，超过80行，Claude开始遗漏部分内容，最多最多，一定不要超过200行。

之前的一篇文章，我也给大家看了我的全局CLAUDE.md文件里面都是哪些内容。

在我的内容的基础上，我又迭代了一下，为大家准备了一份模板。

里面都是一些我觉得一份不错的全局CLAUDE.md应该有的东西。

大家只需要在关于我里面，写上自己的内容，其他基本都是可以直接复用的。

\## 关于我\[你的名字 / 身份 / 职业背景，非程序员的话一定要写出来\]。我用 Claude Code 做 \[具体用途 1\] 和 \[具体用途 2\]。 ## 思维原则所有决策从问题本质出发，不因「惯例如此」照搬。回到问题本身：要解决什么？最直接的路径是什么？从零设计会怎么做？不要谄媚。不要夸我的想法好、不要说「这是个很好的问题」、不要开头加「当然可以」。给我真实判断，方案有问题直接指出来。发现更好的做法直接说，不用等我问。 ## 约束先行无论开发项目还是知识管理项目，第一步永远是建规则：新项目先写 CLAUDE.md，新目录先定结构约定（什么放哪、怎么命名、何时清理）。没有规范的工作空间不动手。已有规范的项目，严格遵守其 CLAUDE.md 中的约定。需要调整规范时先改文档、再改实践，不要反过来。 ## 沟通方式- 默认中文，代码、命令、变量名用英文- 结论先行，再给理由，不要先铺垫背景- 遇到模糊需求，先给最合理的方案，再问要不要调整- 不要问「你确定要这样吗」，除非命中下方红线 ## 自主边界（红线，必须先问我）以下操作即使在 auto-accept 模式下也必须停下来问我：- 删除文件、目录或 git 历史- 修改 .env、密钥、token、CI/CD 配置- 数据库 schema 变更或数据迁移- git push、git rebase、git reset --hard、强制推送- 安装新的全局依赖或修改系统配置- 公开发布（npm publish、部署到生产、发文章等） ## 通用工程纪律- 改完主动跑验证（具体命令见各项目 CLAUDE.md），不要只改不验- 不要为了让代码跑起来注释掉报错或加绕过标记，找根本原因- 密钥、token、密码不进代码、不进 commit、不进日志- 大改动前先在 Plan Mode 出方案，我确认后再动手

一共30多行，分成六个部分。

这六个块都有一个共同特征，就是跨项目通用。

全局的CLAUDE.md定好了，我们顶层的规范就有了。

那至于下一层，项目CLAUDE.md。

拿我自己举例。

我主要用Claude Code来做开发和知识管理。

比如我的code/my目录下就有一个CLAUDE.md，这个的作用，主要就是my文件夹下，经常coding的都是一些可能一次性的、或者我实验性质的乱七八糟的东西，其实都非常的小，所以我真的懒得每次都在my下新建一个文件夹。

所以我现在常用的做法，就是直接cd到my文件夹启动，然后愉快的开始说出我自己的需求帮助我coding，那有了这个CLAUDE.md文件之后，他就会自己判断这是不是一个新产品，如果是的话，那就直接帮我新建一个文件夹开始做，这样整个文件管理，就会变得井井有条了。

而这个文件其实也完全不用你自己写，你就直接打开那个文件夹，然后和Claude Code聊就行，你直接把你的需求，和你在意的事情，和他探讨，让他给你写一份就OK了。

![图像](https://pbs.twimg.com/media/HGUkn78b0AAbEe9?format=jpg&name=large)

我自己这几天也在好好跟我的Claude Code制定各种规范，整理我之前遗留下来的各种屎山。

那如果你的电脑上现在还什么都没有的话，那就更方便了。

这也是我一直强调约束先行、规范先行的原因之一。

约束定好了，那就真的可以开始玩起来了。

而skills，plugins，常用命令、功能这些。

我这里就不展开了，我也写过了很多相关的文章，感兴趣的小伙伴可以直接去对应着搜索关键词就行。

**写在最后**

终于把这篇拖了很久很久的保姆级Claude Code教程写完了。

Claude Code，就是我推荐你的当前AI版本的毕业工具。

你根本无需使用各种乱七八糟的那些Agent，用好Claude Code，你就真的能感受到，什么是最牛逼的Agent了。

希望大家都能愉快创造。

做出这个时代。

属于你自己的作品。