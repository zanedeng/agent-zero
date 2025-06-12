# Agent Zero：MCP 服务器集成指南

本指南解释了如何通过模型上下文协议（MCP）配置和利用外部工具提供商与 Agent Zero 集成。这使 Agent Zero 能够利用由独立的本地或远程 MCP 兼容服务器托管的工具。

## 什么是 MCP 服务器？

MCP 服务器是外部进程或服务，它们暴露了一组 Agent Zero 可以使用的工具。Agent Zero 作为 MCP *客户端*，使用这些服务器提供的工具。集成支持两种主要类型的 MCP 服务器：

1. **本地 Stdio 服务器**：这些通常是本地可执行文件，Agent Zero 通过标准输入/输出（stdio）与之通信。
2. **远程 SSE 服务器**：这些是服务器，通常可以通过网络访问，Agent Zero 使用服务器发送事件（SSE）与之通信，通常通过 HTTP/S。

## Agent Zero 如何使用 MCP 工具

Agent Zero 动态发现和集成 MCP 工具：

1. **配置**：您在配置中定义 Agent Zero 应该连接的 MCP 服务器。主要方式是通过 Agent Zero 设置 UI。
2. **保存设置**：当您通过 UI 保存设置时，Agent Zero 会更新 `tmp/settings.json` 文件，特别是 `"mcp_servers"` 键。
3. **自动安装（重启时）**：保存设置并重启 Agent Zero 后，系统将尝试自动安装任何在其配置中使用 `command: "npx"` 和 `--package` 参数定义的 MCP 服务器包（此过程由 `initialize.py` 管理）。您可以查看应用程序日志（例如，Docker 日志）了解此安装尝试的详细信息。
4. **工具发现**：在初始化时（或设置更新时），Agent Zero 连接到每个已配置和启用的 MCP 服务器，并查询可用工具列表、它们的描述和预期参数。
5. **动态提示**：然后，这些发现的工具信息被动态注入到代理的系统提示中。系统提示模板（例如，`prompts/default/agent.system.mcp_tools.md`）中的占位符如 `{{tools}}` 被替换为所有可用 MCP 工具的格式化列表。这使代理的底层语言模型（LLM）知道它可以请求哪些外部工具。
6. **工具调用**：当 LLM 决定使用 MCP 工具时，Agent Zero 的 `process_tools` 方法（由 `mcp_handler.py` 处理）将其识别为 MCP 工具，并将请求路由到适当的 `MCPConfig` 助手，然后与指定的 MCP 服务器通信以执行工具。

## 配置

### 配置文件和方法

配置 MCP 服务器的主要方法是通过**Agent Zero 的设置 UI**。

当您在 UI 中输入并保存 MCP 服务器详细信息时，这些设置会被写入：

* `tmp/settings.json`

### `tmp/settings.json` 中的 `mcp_servers` 设置

在 `tmp/settings.json` 中，MCP 服务器在 `"mcp_servers"` 键下定义。

* **值类型**：`"mcp_servers"` 的值必须是**JSON 格式的字符串**。这个字符串本身包含服务器配置对象的**数组**。
* **默认值**：如果 `tmp/settings.json` 不存在，或者存在但不包含 `"mcp_servers"` 键，Agent Zero 将使用默认值 `""`（空字符串），表示没有配置 MCP 服务器。
* **手动编辑（高级）**：虽然推荐使用 UI 配置，但您也可以手动编辑 `tmp/settings.json`。如果这样做，请确保 `"mcp_servers"` 值是有效的 JSON 字符串，内部引号正确转义。

**`tmp/settings.json` 中的 `mcp_servers` 字符串示例：**

```json
{
    // ... 其他设置 ...
    "mcp_servers": "[{\\\"name\\\": \\\"sequential-thinking\\\",\\\"command\\\": \\\"npx\\\",\\\"args\\\": [\\\"--yes\\\", \\\"--package\\\", \\\"@modelcontextprotocol/server-sequential-thinking\\\", \\\"mcp-server-sequential-thinking\\\"]}, {\\\"name\\\": \\\"brave-search\\\", \\\"command\\\": \\\"npx\\\", \\\"args\\\": [\\\"--yes\\\", \\\"--package\\\", \\\"@modelcontextprotocol/server-brave-search\\\", \\\"mcp-server-brave-search\\\"], \\\"env\\\": {\\\"BRAVE_API_KEY\\\": \\\"YOUR_BRAVE_KEY_HERE\\\"}}, {\\\"name\\\": \\\"fetch\\\", \\\"command\\\": \\\"npx\\\", \\\"args\\\": [\\\"--yes\\\", \\\"--package\\\", \\\"@tokenizin/mcp-npx-fetch\\\", \\\"mcp-npx-fetch\\\", \\\"--ignore-robots-txt\\\", \\\"--user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36\\\"]}]",
    // ... 其他设置 ...
}
```
*注意：在实际的 `settings.json` 文件中，`mcp_servers` 的整个值是一个字符串，其中数组结构内的引号用反斜杠转义。*

* **更新**：如前所述，设置或更新此值的推荐方式是通过 Agent Zero 的设置 UI。
* **对于现有的 `settings.json` 文件（升级后）**：如果您有来自支持 MCP 服务器之前的 Agent Zero 版本的现有 `tmp/settings.json`，`"mcp_servers"` 键可能缺失。要添加此键：
    1. 确保您运行的是包含 MCP 服务器支持的 Agent Zero 版本。
    2. 运行 Agent Zero 并打开其设置 UI。
    3. 保存设置（即使没有进行更改）。此操作将写入完整的当前设置结构，包括默认的 `"mcp_servers": ""`（如果未填充），到 `tmp/settings.json`。然后您可以通过 UI 或仔细编辑此字符串来配置您的服务器。

### MCP 服务器配置结构

以下是在 `mcp_servers` JSON 数组字符串中配置单个服务器的模板：

**1. 本地 Stdio 服务器**

```json
{
    "name": "My Local Tool Server",
    "description": "可选：此服务器的简短描述。",
    "command": "python", // 要运行的可执行文件（例如，python，/path/to/my_tool_server）
    "args": ["path/to/your/mcp_stdio_script.py", "--some-arg"], // 命令的参数列表
    "env": { // 可选：命令进程的环境变量
        "PYTHONPATH": "/path/to/custom/libs:.",
        "ANOTHER_VAR": "value"
    },
    "encoding": "utf-8", // 可选：stdio 通信的编码（默认："utf-8"）
    "encoding_error_handler": "strict", // 可选：如何处理编码错误。可以是 "strict"、"ignore" 或 "replace"（默认："strict"）。
    "disabled": false // 设置为 true 可临时禁用此服务器而不删除其配置。
}
```

**2. 远程 SSE 服务器**

```json
{
    "name": "My Remote API Tools",
    "description": "可选：远程 SSE 服务器的描述。",
    "url": "https://api.example.com/mcp-sse-endpoint", // MCP 服务器的 SSE 端点的完整 URL。
    "headers": { // 可选：连接所需的任何 HTTP 头。
        "Authorization": "Bearer YOUR_API_KEY_OR_TOKEN",
        "X-Custom-Header": "some_value"
    },
    "timeout": 5.0, // 可选：连接超时（秒）（默认：5.0）。
    "sse_read_timeout": 300.0, // 可选：SSE 流的读取超时（秒）（默认：300.0，即 5 分钟）。
    "disabled": false
}
```

**`tmp/settings.json` 中的 `mcp_servers` 值示例：**

```json
{
    // ... 其他设置 ...
    "mcp_servers": "[{\\\"name\\\": \\\"MyPythonTools\\\", \\\"command\\\": \\\"python3\\\", \\\"args\\\": [\\\"mcp_scripts/my_server.py\\\"], \\\"disabled\\\": false}, {\\\"name\\\": \\\"ExternalAPI\\\", \\\"url\\\": \\\"https://data.example.com/mcp\\\", \\\"headers\\\": {\\\"X-Auth-Token\\\": \\\"supersecret\\\"}, \\\"disabled\\\": false}]",
    // ... 其他设置 ...
}
```

**关键配置字段：**

* `"name"`：服务器的唯一名称。此名称将用作此服务器提供的工具的前缀（例如，`my_server_name.tool_name`）。名称在内部被规范化（转换为小写，空格和连字符替换为下划线）。
* `"disabled"`：布尔值（`true` 或 `false`）。如果为 `true`，Agent Zero 将忽略此服务器配置。
* `"url"`：**远程 SSE 服务器必需。** 端点 URL。
* `"command"`：**本地 Stdio 服务器必需。** 可执行命令。
* `"args"`：本地 Stdio 服务器的可选参数列表。
* 其他字段特定于服务器类型，大多数是可选的，有默认值。

## 使用 MCP 工具

一旦配置完成，成功安装（如果适用，例如基于 `npx` 的服务器），并被 Agent Zero 发现：

* **工具命名**：MCP 工具将出现在代理中，名称前缀是您定义的服务器名称（并规范化，例如，小写，空格/连字符用下划线替换）。例如，如果您的服务器在配置中命名为 `"sequential-thinking"`，它提供了一个名为 `"run_chain"` 的工具，代理将知道它为 `sequential_thinking.run_chain`。
* **代理交互**：您可以指示代理使用这些工具。例如："代理，使用 `sequential_thinking.run_chain` 工具，输入如下..."然后代理的 LLM 将制定适当的 JSON 请求。
* **执行流程**：Agent Zero 的 `process_tools` 方法（逻辑在 `python/helpers/mcp_handler.py` 中）优先在 `MCPConfig` 中查找工具名称。如果找到，执行将委托给相应的 MCP 服务器。如果未找到作为 MCP 工具，则尝试查找具有该名称的本地/内置工具。

这种设置提供了一种灵活的方式来扩展 Agent Zero 的功能，通过与各种外部工具提供商集成，而无需修改其核心代码库。 