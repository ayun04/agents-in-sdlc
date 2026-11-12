---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能确实正常。与其亲自逐一点击应用，不如连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器为你测试功能，然后发布分支并创建 PR。

本练习将完成以下内容：

- 了解 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用 Playwright MCP 服务器，在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当智能体模式能够访问编辑器以外的资源时，它的能力会大幅增强。Copilot 使用 Model Context Protocol (MCP) 来实现这一点——MCP 是一种让智能体与外部工具和服务通信的标准方式。

![智能体模式内部工作方式以及它如何与上下文、LLM 和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时与外部工具和服务通信，从而使用资源获取最新信息，并使用工具代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理 AI 智能体与外部工具（例如现有 API 或 NPM 包等本地工具）之间的通信。每个 MCP 服务器都代表 AI 智能体可访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 仓库的 API，让 AI 智能体能够创建新仓库、更新现有仓库、管理议题和拉取请求等。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，让 AI 智能体能够打开网页、填写表单和选择按钮等。

还有许多其他 MCP 服务器，可以提供不同的工具和资源。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 从安全角度看，请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前仔细检查其源代码、验证发布者，并考虑相关安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经为你配置的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板中已包含 `github` 项，因此上一项练习中 Copilot 才能读取待办事项中的议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容——VS Code 通过 HTTP 连接到它，并在 Copilot 首次使用其工具时使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 项，与 `github` 并列，使文件内容如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境的 Codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 进行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器——Copilot 首次需要某服务器提供的工具时，VS Code 会启动它，并要求你确认是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 项上方会出现 **Start** 操作。选择该操作立即启动服务器，确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，下一步中 Copilot 首次需要使用服务器工具时，VS Code 会启动服务器并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了可驱动的真实浏览器。你无需亲自点击应用检查结果，智能体可以打开页面、导航、应用筛选条件、读取结果并向你总结。无需离开对话，就能快速确认功能是否符合预期。

在底层，Playwright MCP 服务器依靠页面的[无障碍树][playwright-mcp-server]而不是屏幕截图工作。这意味着智能体会像辅助技术一样，根据结构化且带标签的元素（按钮、链接、列表项）进行推理——因此，快速功能检查也能顺便进行初步无障碍检查。

连接服务器并启动应用后，让 Copilot 测试刚完成的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成检查，并向你报告结果。请根据议题中的验收标准审查其总结——如果发现异常，可以提出后续问题，或在创建拉取请求前让 Copilot 修复代码。

> [!NOTE]
> 运行此测试时，应用必须正在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做更改的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到仓库。

## 创建拉取请求

可以通过多种方式创建拉取请求，例如使用 github.com 或 GitHub 命令行界面 (CLI)。既然你已经在使用 GitHub Copilot，不妨让它代为创建 PR！它可以找到相关议题并创建与议题关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 为你创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，系统可能会要求你通过 GitHub 身份验证；按照提示授权即可。
5. 留意 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接，审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用该服务器在浏览器中手动测试筛选功能，然后再发布。
- 发布了分支，并为筛选功能创建了拉取请求。

你使用 MCP 服务器**测试**了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等特定任务。

## 资源

- [什么是 MCP，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
