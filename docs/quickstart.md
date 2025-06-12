# 快速入门
本指南提供了使用 Agent Zero 的快速介绍。我们将介绍启动 Web UI、开始新聊天以及运行简单任务。

## 启动 Web UI
1. 确保您已安装 Agent Zero 并正确设置环境（如果需要，请参考[安装指南](installation.md)）。
2. 在 Agent Zero 目录中打开终端并激活您的 conda 环境（如果您使用的话）。
3. 运行以下命令：

```bash
python run_ui.py
```

4. 终端中将显示类似以下消息，表明 Web UI 正在运行：

![](res/flask_link.png)

5. 打开您的网页浏览器并导航到终端中显示的 URL（通常是 `http://127.0.0.1:50001`）。您应该能看到 Agent Zero Web UI。

![新聊天](res/ui_newchat1.png)

> [!TIP]
> 如您所见，Web UI 有四个不同的按钮用于轻松管理聊天：
> `新聊天`、`重置聊天`、`保存聊天`和`加载聊天`。
> 聊天可以单独以 `json` 格式保存和加载，并存储在
> `/tmp/chats` 目录中。

    ![聊天管理](res/ui_chat_management.png)

## 运行简单任务
让我们让 Agent Zero 下载一个 YouTube 视频。方法如下：

1. 在聊天输入框中输入"为我下载一个 YouTube 视频"并按回车键或点击发送按钮。

2. Agent Zero 将处理您的请求。您将在 UI 中看到它的"思考"和采取的行动。它将找到一个默认的现有解决方案，这意味着使用 `code_execution_tool` 运行一个简单的 Python 脚本来执行任务。

3. 然后代理将询问您想要下载的 YouTube 视频的 URL。

## 交互示例
以下是您在第 3 步可能在 Web UI 中看到的内容示例：
![1](res/image-24.png)

## 下一步
现在您已经运行了一个简单任务，您可以尝试更复杂的请求。尝试让 Agent Zero：

* 执行计算
* 在网络上搜索信息
* 执行 shell 命令
* 探索 Web 开发任务
* 创建或修改文件

> [!TIP]
> [使用指南](usage.md)提供了有关使用 Agent Zero 各种功能的更深入信息，
> 包括提示工程、工具使用和多代理协作。