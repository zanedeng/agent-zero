<div align="center">

# `Agent Zero`


[![Agent Zero 网站](https://img.shields.io/badge/Website-agent--zero.ai-0A192F?style=for-the-badge&logo=vercel&logoColor=white)](https://agent-zero.ai) [![感谢赞助者](https://img.shields.io/badge/GitHub%20Sponsors-感谢赞助者-FF69B4?style=for-the-badge&logo=githubsponsors&logoColor=white)](https://github.com/sponsors/frdel) [![在 X 上关注](https://img.shields.io/badge/X-关注-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/Agent0ai) [![加入我们的 Discord](https://img.shields.io/badge/Discord-加入我们的服务器-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/B8KZKNsPpj) [![在 YouTube 上订阅](https://img.shields.io/badge/YouTube-订阅-red?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/@AgentZeroFW) [![在 LinkedIn 上联系](https://img.shields.io/badge/LinkedIn-联系-blue?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/jan-tomasek/) [![在 Warpcast 上关注](https://img.shields.io/badge/Warpcast-关注-5A32F3?style=for-the-badge)](https://warpcast.com/agent-zero)

[介绍](#一个随你成长和学习的个人有机代理框架) •
[安装](./docs/installation.md) •
[黑客版本](#黑客版本) •
[如何更新](./docs/installation.md#how-to-update-agent-zero) •
[文档](./docs/README.md) •
[使用](./docs/usage.md)

</div>


<div align="center">

> ### 📢 **新闻：Agent Zero 现在包含 MCP 服务器和客户端功能！** 📢
>
> Agent Zero 现在可以作为其他 LLM 工具的 MCP 服务器，并使用外部 MCP 服务器作为工具

</div>



[![展示](/docs/res/showcase-thumb.png)](https://youtu.be/lazLNcEYsiQ)



## 一个随你成长和学习的个人有机代理框架



- Agent Zero 不是一个预定义的代理框架。它被设计为动态的、有机增长的，并随着你的使用而学习。
- Agent Zero 完全透明、可读、可理解、可定制和交互式。
- Agent Zero 使用计算机作为工具来完成其（你的）任务。

# 💡 主要特点

1. **通用助手**

- Agent Zero 不是为特定任务预编程的（但可以是）。它旨在成为一个通用个人助手。给它一个任务，它会收集信息、执行命令和代码、与其他代理实例合作，并尽最大努力完成任务。
- 它具有持久记忆，可以记住以前的解决方案、代码、事实、指令等，以便在未来更快、更可靠地解决任务。

![Agent 0 工作](/docs/res/ui-screen-2.png)

2. **计算机作为工具**

- Agent Zero 使用操作系统作为工具来完成其任务。它没有预编程的单一用途工具。相反，它可以编写自己的代码并使用终端来创建和使用自己的工具。
- 其工具库中唯一的默认工具是在线搜索、记忆功能、通信（与用户和其他代理）以及代码/终端执行。其他一切都是由代理自己创建的，或者可以由用户扩展。
- 工具使用功能是从头开始开发的，以保持最大的兼容性和可靠性，即使使用非常小的模型。
- **默认工具：** Agent Zero 包括知识、网页内容、代码执行和通信等工具。
- **创建自定义工具：** 通过创建自己的自定义工具来扩展 Agent Zero 的功能。
- **工具集：** 工具集是一种新型工具，允许你创建可由 Agent Zero 调用的自定义函数和程序。

3. **多代理协作**

- 每个代理都有一个上级代理给它分配任务和指令。每个代理然后向其上级报告。
- 对于链中的第一个代理（Agent 0），上级是人类用户；代理看不出区别。
- 每个代理都可以创建其下属代理来帮助分解和解决子任务。这有助于所有代理保持其上下文清晰和专注。

![多代理](docs/res/physics.png)
![多代理 2](docs/res/physics-2.png)

4. **完全可定制和可扩展**

- 这个框架中几乎没有什么是硬编码的。没有什么是隐藏的。用户几乎可以扩展或更改所有内容。
- 整个行为由 **prompts/default/agent.system.md** 文件中的系统提示定义。更改这个提示可以显著改变框架。
- 该框架不以任何方式指导或限制代理。没有代理必须遵循的硬编码轨道。
- 每个提示、发送给代理的每个小消息模板都可以在 **prompts/** 文件夹中找到并更改。
- 每个默认工具都可以在 **python/tools/** 文件夹中找到并更改或复制以创建新的预定义工具。

![提示](/docs/res/prompts.png)

5. **沟通是关键**

- 给你的代理一个适当的系统提示和指令，它就能创造奇迹。
- 代理可以与上级和下属沟通，提出问题、给出指令和提供指导。在系统提示中指导你的代理如何有效沟通。
- 终端界面是实时流式传输和交互式的。你可以随时停止和干预。如果你看到你的代理走向错误的方向，立即停止并告诉它。
- 这个框架有很大的自由度。你可以指示你的代理定期向上级报告并请求继续的许可。你可以指示他们在决定何时委派子任务时使用评分系统。上级可以检查下属的结果并提出异议。可能性是无限的。

## 🚀 你可以用 Agent Zero 构建的东西

- **开发项目** - `"创建一个带有实时数据可视化的 React 仪表板"`

- **数据分析** - `"分析上个季度的 NVIDIA 销售数据并创建趋势报告"`

- **内容创作** - `"写一篇关于微服务的技术博客文章"`

- **系统管理** - `"为我们的网络服务器设置监控系统"`

- **研究** - `"收集并总结五篇关于 CoT 提示的近期 AI 论文"`

# 黑客版本
- Agent Zero 还提供基于 Kali linux 的黑客版本，带有针对网络安全任务的修改提示
- 设置与常规版本相同，只需使用 frdel/agent-zero-run:hacking 镜像而不是 frdel/agent-zero-run
> **注意：** 黑客版本及其所有提示和功能将在下一个版本中合并到主分支。


# ⚙️ 安装

点击打开视频了解如何安装 Agent Zero：

[![简易安装指南](/docs/res/easy_ins_vid.png)](https://www.youtube.com/watch?v=L1_peV8szf8)

Windows、macOS 和 Linux 的详细设置指南（带视频）可以在 Agent Zero 文档的[此页面](./docs/installation.md)找到。

### ⚡ 快速开始

```bash
# 使用 Docker 拉取并运行

docker pull frdel/agent-zero-run
docker run -p 50001:80 frdel/agent-zero-run

# 访问 http://localhost:50001 开始
```

## 🐳 完全容器化，支持语音转文本和文本转语音

![设置](docs/res/settings-page-ui.png)

- 可自定义设置允许用户根据需求定制代理的行为和响应。
- Web UI 输出非常清晰、流畅、多彩、可读且交互式；没有任何隐藏内容。
- 你可以直接在 Web UI 中加载或保存聊天记录。
- 你在终端中看到的相同输出会自动保存到 **logs/** 文件夹中的 HTML 文件中，每次会话都会保存。

![时间示例](/docs/res/time_example.jpg)

- 代理输出是实时流式传输的，允许用户随时阅读和干预。
- 不需要编码；只需要提示和沟通技巧。
- 有了可靠的系统提示，该框架即使使用小模型也很可靠，包括精确的工具使用。

## 👀 注意事项

1. **Agent Zero 可能很危险！**

- 有了适当的指令，Agent Zero 能够做很多事情，甚至可能对你的计算机、数据或账户造成危险的操作。始终在隔离环境（如 Docker）中运行 Agent Zero，并小心你的愿望。

2. **Agent Zero 是基于提示的。**

- 整个框架由 **prompts/** 文件夹指导。代理指南、工具指令、消息、实用 AI 函数，都在那里。


## 📚 阅读文档

| 页面 | 描述 |
|-------|-------------|
| [安装](./docs/installation.md) | 安装、设置和配置 |
| [使用](./docs/usage.md) | 基本和高级使用 |
| [架构](./docs/architecture.md) | 系统设计和组件 |
| [贡献](./docs/contribution.md) | 如何贡献 |
| [故障排除](./docs/troubleshooting.md) | 常见问题及其解决方案 |

## 即将推出

- **MCP**
- **知识和 RAG 工具**

## 🎯 更新日志

### v0.8.5 - **MCP 服务器 + 客户端**
[发布视频](https://youtu.be/pM5f4Vz3_IQ)

- Agent Zero 现在可以作为 MCP 服务器
- Agent Zero 可以使用外部 MCP 服务器作为工具

### v0.8.4.1 - 2
默认模型设置为 gpt-4.1
- 代码执行工具改进
- 浏览器代理改进
- 记忆改进
- 与上下文管理相关的各种错误修复
- 消息格式改进
- 调度器改进
- 新模型提供商
- 输入工具修复
- 兼容性和稳定性改进

### v0.8.4
[发布视频](https://youtu.be/QBh_h_D_E24)

- **远程访问（移动端）**

### v0.8.3.1
[发布视频](https://youtu.be/AGNpQ3_GxFQ)

- **自动嵌入**


### v0.8.3
[发布视频](https://youtu.be/bPIZo0poalY)

- ***规划和调度***

### v0.8.2
[发布视频](https://youtu.be/xMUNynQ9x6Y)

- **终端多任务**
- **聊天名称**

### v0.8.1
[发布视频](https://youtu.be/quv145buW74)

- **浏览器代理**
- **用户体验改进**

### v0.8
[发布视频](https://youtu.be/cHDCCSr1YRI)

- **Docker 运行时**
- **新消息历史和总结系统**
- **代理行为变更和管理**
- **文本转语音（TTS）和语音转文本（STT）**
- **Web UI 中的设置页面**
- **SearXNG 集成替代 Perplexity + DuckDuckGo**
- **文件浏览器功能**
- **KaTeX 数学可视化支持**
- **聊天中文件附件**

### v0.7
[发布视频](https://youtu.be/U_Gl0NPalKA)

- **自动记忆**
- **UI 改进**
- **工具集**
- **扩展框架**
- **反思提示**
- **错误修复**

## 🤝 社区和支持

- [加入我们的 Discord](https://discord.gg/B8KZKNsPpj) 进行实时讨论或[访问我们的 Skool 社区](https://www.skool.com/agent-zero)。
- [关注我们的 YouTube 频道](https://www.youtube.com/@AgentZeroFW) 获取实践解释和教程
- [报告问题](https://github.com/frdel/agent-zero/issues) 获取错误修复和功能
