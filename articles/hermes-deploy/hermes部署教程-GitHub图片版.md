# 零命令行部署 Hermes：腾讯云 + DeepSeek + 飞书搭建 24 小时在线 AI 助手

看完这篇教程，你会拥有一个属于自己的 AI 助手 Hermes——它 24 小时在线，能在飞书里直接和你聊天、帮你干活，而且用得越久越懂你。

整个过程不需要你会命令行，只需要跟着步骤点鼠标就能完成。

跟着做完之后，你会得到：

- 一台运行 Hermes 的腾讯云服务器；
- 一个接入 DeepSeek 的 AI Agent；
- 一个可以在飞书里直接聊天的 Hermes 机器人；
- 一个可以继续安装 Skills 的基础环境。

## 一、Hermes 是什么

Hermes（也叫"爱马仕"）是一款开源的 AI Agent。Hermes 的特点是"越用越聪明"：

- **会总结经验**：做完一件复杂的事之后，会自动把这次的做法记下来，下次遇到类似的事就能直接用上，不用每次都从头摸索。
- **会自我改进**：每次帮你做完事，都在悄悄优化自己，越用越顺手。
- **记得住你们聊过的内容**：不用每次对话都重新介绍背景，它会记住之前聊过什么。
- **会越来越懂你**：用得久了，它会慢慢摸清你的喜好和习惯，回复也会越来越贴合你的需求。

## 二、基础概念

在动手之前，先搞清楚三个问题：Hermes 住在哪、它的大脑是谁、它靠什么和你对话。

### 1. Hermes 住在哪里：本地电脑 or 云服务器

Hermes 可以装在自己的电脑上，也可以装在云服务器上。

- **装在自己电脑上**：不用花钱租服务器，数据也不出本地。但缺点很明显——电脑一关机、一断网，Hermes 就停止工作了，没法一直在线帮你干活。

- **装在云服务器上**：相当于给它租了一间 24 小时通电通网的"办公室"。

  这里简单说一下什么是云服务器——它其实就是一台你可以远程操作的电脑，放在机房里，由服务商保证它一直开机、一直联网，你不用自己买硬件、也不用操心断电断网，按月或按年付费租用就行，很像"租房子"而不是"买房子"。

**推荐装在云服务器上**。

Hermes 越用越聪明这个特点，需要它一直在线才能发挥出来——它要持续接收你交给它的事、不断积累经验，才能越来越懂你。

如果它三天两头掉线，这个效果会大打折扣。而云服务器正好能保证它一直在线，不管你人在哪，只要能打开飞书，就能随时找它。

**云服务商推荐用腾讯云**。

原因有两点：

1. **装起来很简单**：腾讯云提供了现成的 Hermes 镜像，购买服务器时选择镜像，不用自己在服务器里敲命令行。
2. **自带管理平台，接入飞书很方便**：腾讯云配套提供了一个叫 **Lighthouse** 的网页管理平台，后续配置 AI 大模型、接入飞书、微信，都可以在这个平台上点点鼠标完成，不用登录服务器敲命令。比如接入飞书，如果按飞书官方流程走，需要自己申请一个飞书应用、开通机器人权限、配置好几项技术参数，比较麻烦；但用 Lighthouse 管理平台，只需要扫码授权一下就行。

**服务器配置怎么选？**

- 最低配置：2G 内存；
- 推荐配置：4G 内存起步

### 2. Hermes 需要 AI 大模型：负责思考

Hermes 本身更像一个"躯壳"，真正负责理解你说的话、想出该怎么回复的，是它背后接入的 AI 大模型。

本教程使用 **DeepSeek** 作为它的大脑，价格便宜，中文理解能力也不错。

费用主要来自两部分：

- 腾讯云服务器费用：按月或按年付费；
- 大模型 API 调用费用：DeepSeek 按实际使用量计费。

### 3. Hermes 需要聊天渠道：负责对话

光有大脑还不够，还需要一个"嘴巴"——也就是你和 Hermes 聊天的地方，常见的有微信、企业微信、飞书、钉钉等。

本教程用**飞书**做演示。

## 三、准备工作

1. 注册腾讯云并完成实名认证

2. DeepSeek 充值

在 DeepSeek 开发平台（[DeepSeek](https://platform.deepseek.com/usage)）先完成实名认证，然后充值

![](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/01.jpg)

3. 准备飞书账号

## 四、具体操作

### 1.  购买腾讯云服务器，选择 Hermes 镜像

新用户可以选择 109 元/年 的这一款，4核4G，超级划算。

![云服务器](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/02.jpg)

镜像选择 Hermes，这样我们创建服务器之后默认安装好 Hermes，不需要手动敲命令行安装了。

![](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/03.jpg)

完成购买，等一分钟就能看到购买的服务器了。

![](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/04.jpg)

### 2. 进入 Lighthouse Agent 管理平台

这个就是我们购买的服务器，点击“配置”，进入 Lighthouse Agent，这是腾讯云的可视化 Agent 管理平台。

![](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/05.jpg)

左侧点击设置，可以看到配置页面，选择 DeepSeek 模型。

<img src="https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/06.jpg" style="zoom:150%;" />

### 3. 获取 DeepSeek 密钥并填写

在 DeepSeek 开放平台，创建 API Key

![](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/07.jpg)

名字随便取，自己能认识就行。

![](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/08.jpg)



复制 API Key，注意这个 Key 只会出现这一次，不要把 Key 发给其他人。

![](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/09.jpg)

在 Lighthouse Agent 管理页面，粘贴 API Key，设为默认。

![](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/10.jpg)

至此，我们已经填写了模型。

### 4. 接入飞书

在通道（Channels）选择飞书，点击授权接入

![](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/11.jpg)



确认授权接入

![接入飞书](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/12.jpg "飞书")

得到一个二维码，手机飞书扫码

![](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/13.jpg)

在手机上可以给这个 Agent 起个名字

<img src="https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/14.jpg" title="" alt="" style="zoom:50%;">

创建成功，在飞书里发送“你好”
如果它能正常回复，就说明模型和飞书通道都已经跑通了。

<img src="https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/15.jpg" title="" alt="" style="zoom:50%;">

### 5. 添加技能（Skills）

在技能这里，点击“前往 SkillHub”，这里有丰富的技能

![图片16](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/16.jpg)

我们以“文章去 AI 味工具”为例，点击进入

![图片17](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/17.jpg)



右侧复制 Prompt

![图片18](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/18.jpg)



在对话框里粘贴给 Agent，就可以帮我们自动安装了。

![图片19](https://raw.githubusercontent.com/declanliang/image-host/main/articles/hermes-deploy/images/19.jpg)

值得一提的是，Skills 不是非装不可，也不是越多越好。

## 五、常见问题

### 1. 为什么要用云服务器，不直接装在自己电脑上？

首先，Hermes 可以安装在云服务器也可以安装在本地电脑（如 Windows、macOS）。

但 Hermes 的价值在于长期在线、持续积累记忆和经验。如果装在自己电脑上，电脑关机、断网、休眠之后，Hermes 就无法继续工作。

云服务器相当于给 Hermes 租了一间 24 小时在线的办公室，可以确保它一直在线。

### 2. DeepSeek API Key 是什么？

Hermes 本身负责 Agent 的运行逻辑（相当于手和脚），但真正负责理解问题和生成回答的是大模型（相当于大脑）。

DeepSeek API Key 就像一把钥匙，Hermes 通过这把钥匙调用 DeepSeek 模型。

DeepSeek 按照使用量计费，所以需要先在 DeepSeek 开发平台充值。

API Key 相当于你的模型账户密码，不要发给其他人。别人拿到之后，就可以消耗你的账户余额。

### 3. 可以接入其他模型吗？

当然可以，其他模型的接入方式类似，也是创建 API Key 并导入 Hermes。

### 4. Skills 是不是越多越好？

不是。

Skills 可以理解成 Hermes 的工具箱。工具越多，不代表越强；只有你真的会用到的技能，才值得安装。

刚开始建议只安装一两个常用技能，等你熟悉之后再慢慢扩展。

### 5. Hermes 和 OpenClaw 有什么区别？

Hermes 和 OpenClaw 都是开源 AI Agent，都可以接入大模型、工具和聊天渠道，也都适合做个人 AI 助手。

简单理解：

```text
Hermes 更强调 Agent 自我改进、长期记忆和持续积累经验。
OpenClaw 更强调本地优先、连接你已有设备和聊天渠道。
```

但对于普通用户来说，没必要一开始就纠结哪个更好。

重要的是先选一个，坚持用起来，把模型、聊天渠道、常用技能和自己的工作流跑通。

Agent 工具真正有价值的地方，不是装了多少个，而是你能不能把它融入自己的日常工作。

### 6. Lighthouse  起了什么作用？

简单说：Lighthouse 是一个 Channel，严格来说与飞书、微信是平行的channel。

它负责把腾讯云管理页面、飞书等外部入口接到服务器上的 Hermes Gateway。

## 六、选修：理解 Hermes 的内部结构

这一章是技术探讨。如果你只是想把 Hermes 用起来，前面的内容已经够了。

但如果你想真正理解 Hermes，就需要从它的整体运行结构看起，而不是只看可视化页面。

### 1. 总体结构

```text
用户消息
   |
   v
Channel
   |
   v
Gateway
   |
   v
Agent Runtime
   |\
   | \ 读取
   |  v
   | Profile
   |
   v
Model Provider
```

Channel 负责把外部消息接进来，比如飞书、微信、网页入口等。

Gateway 是 Hermes 的统一消息入口，负责接收不同 Channel 的消息，并把消息交给后面的 Agent Runtime。

Agent Runtime 是真正运行 Agent 的部分，负责理解任务、组织上下文、读取 Profile，并调用工具和模型。

Profile 是一个 Agent 的资料夹，保存模型配置、人格设定、长期记忆、会话记录和已安装技能。

Model Provider 是具体的大模型服务，比如 DeepSeek、OpenAI 或其他模型平台。

### 2. 服务器里的结构

**抽象结构**

可以先把 `/home/ubuntu/.hermes/` 理解成 Hermes 的总工作目录。

如果只使用默认 Agent，它大致是这样：

```text
/home/ubuntu/.hermes/
├── default 配置
└── hermes-agent 共享运行代码
```

如果你创建了多个独立 Agent，它更接近这样：

```text
/home/ubuntu/.hermes/
├── default 配置
├── profiles/
│   ├── agent1 配置
│   └── agent2 配置
└── hermes-agent 共享运行代码
```

这里要注意：

```text
default、profiles/agent1、profiles/agent2 是配置和资料；
hermes-agent 是共享运行代码，不是某个具体 Agent。
```

**default 配置**

默认情况下，Hermes 使用 `/home/ubuntu/.hermes/` 作为 default Profile。

```text
/home/ubuntu/.hermes/
├── config.yaml              # 模型、通道、运行参数等配置
├── .env                     # API Key 等环境变量
├── SOUL.md                  # Agent 的人格和长期设定
├── memories/                # 长期记忆
├── sessions/                # 会话记录
├── skills/                  # 已安装技能
├── plugins/                 # 插件
├── platforms/               # 平台相关数据
├── providers/               # 模型服务商相关数据
├── logs/                    # 运行日志
├── state.db                 # 状态数据库
├── kanban.db                # 任务/看板相关数据库
├── workspace/               # 工作区
├── sandboxes/               # 沙盒运行目录
└── hermes-agent/            # Hermes 共享运行代码
```

也就是说，如果你没有创建额外的 Profile，Hermes 就会使用这套默认配置。

**多个 Agent 配置**

如果你想让不同 Agent 拥有不同模型、不同人格、不同记忆，就应该给它们创建不同的 Profile。

```text
/home/ubuntu/.hermes/profiles/
├── agent1/
│   ├── config.yaml          # agent1 的模型和运行配置
│   ├── .env                 # agent1 的环境变量
│   ├── SOUL.md              # agent1 的人格设定
│   ├── memories/            # agent1 的长期记忆
│   └── sessions/            # agent1 的会话记录
└── agent2/
    ├── config.yaml          # agent2 的模型和运行配置
    ├── .env                 # agent2 的环境变量
    ├── SOUL.md              # agent2 的人格设定
    ├── memories/            # agent2 的长期记忆
    └── sessions/            # agent2 的会话记录
```

这样一来，Agent 1 和 Agent 2 就可以真正彼此独立：

```text
Agent 1 可以使用 DeepSeek
Agent 2 可以使用另一个模型

Agent 1 可以是写作助手
Agent 2 可以是编程助手

Agent 1 有自己的记忆
Agent 2 也有自己的记忆
```

如果某个 Agent 没有独立 Profile，或者没有单独配置模型，就可能继续使用 default Profile 里的配置。

**hermes-agent 拆解**

每个 Agent/Profile 不会各自拥有一套 Gateway 代码。

Gateway 是共享入口层，服务器里的代码位于：

```text
/home/ubuntu/.hermes/hermes-agent/gateway/
```

`/home/ubuntu/.hermes/hermes-agent/` 是 Hermes 的共享运行代码目录，大致包含：

```text
/home/ubuntu/.hermes/hermes-agent/
├── agent/           # Agent Runtime 相关代码
├── gateway/         # Gateway 相关代码
├── hermes_cli/      # 命令行入口
├── plugins/         # 插件系统
├── providers/       # 模型服务商适配
├── skills/          # 内置技能
├── tools/           # 工具系统
└── venv/            # Python 运行环境
```

服务器上 Gateway 的启动命令是：

```text
/home/ubuntu/.hermes/hermes-agent/venv/bin/python -m hermes_cli.main gateway run
```

并且环境变量里有：

```text
HERMES_HOME=/home/ubuntu/.hermes
```

所以可以这样理解：

```text
Gateway = 共享入口层
Agent Runtime = 运行 Agent 的逻辑
Profile = 每个 Agent 的配置资料
hermes-agent/ = 共享代码目录
profiles/agent1 = 具体 Agent 配置
```

## 七、写在最后

把 Hermes 跑起来只是第一步。

真正重要的是开始用它：接入你的聊天渠道，安装你确实需要的 Skills，持续调整 `SOUL.md`，让它在一次次对话和任务里积累记忆。

Agent 不是装好那一刻就最聪明，而是越用越懂你。

