# Agent Zero 隧道功能

Agent Zero 中的隧道功能允许您使用 Flaredantic 隧道将本地 Agent Zero 实例暴露到互联网。这使得您可以与他人共享您的 Agent Zero 实例，而无需他们自己安装和运行 Agent Zero。

## 工作原理

Agent Zero 使用 [Flaredantic](https://pypi.org/project/flaredantic/) 库创建安全隧道，将您的本地实例暴露到互联网。这些隧道：

- 是安全的（HTTPS）
- 不需要任何配置
- 为每个会话生成唯一的 URL
- 可以按需重新生成

## 使用隧道功能

1. 打开设置并导航到"外部服务"选项卡
2. 在导航菜单中点击"Flare 隧道"
3. 点击"创建隧道"按钮生成新隧道
4. 创建后，隧道 URL 将显示并可以复制以与他人共享
5. 隧道 URL 将保持活动状态，直到您停止隧道或关闭 Agent Zero 应用程序

## 安全注意事项

通过隧道共享您的 Agent Zero 实例时：

- 任何拥有 URL 的人都可以访问您的 Agent Zero 实例
- 除了您的 Agent Zero 实例已有的认证外，不会添加额外的认证
- 如果您要共享敏感信息，请考虑设置认证
- 隧道只暴露您的本地 Agent Zero 实例，而不是整个系统

## 故障排除

如果您遇到隧道功能的问题：

1. 检查您的互联网连接
2. 尝试刷新隧道 URL
3. 重启 Agent Zero
4. 检查控制台日志中的任何错误消息

## 添加认证

要在使用隧道时为您的 Agent Zero 实例添加基本认证，您可以设置以下环境变量：

```
AUTH_LOGIN=your_username
AUTH_PASSWORD=your_password
```

或者，您可以直接在设置中配置用户名和密码：

1. 在 Agent Zero UI 中打开设置模态框
2. 导航到"外部服务"选项卡
3. 找到"认证"部分
4. 在"UI 登录"和"UI 密码"字段中输入您想要的用户名和密码
5. 点击"保存"按钮应用更改

这将要求用户在访问您的隧道 Agent Zero 实例时输入这些凭据。在未配置认证的情况下尝试创建隧道时，Agent Zero 将显示安全警告。