---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码不只是写代码。我们还需要处理数据、外部服务，并让 Copilot 使用更多自动化工具。这正是 MCP 服务器的作用所在。MCP 服务器可以让 Copilot 使用 CLI 内置功能以外的工具和服务。

本课将完成以下内容：

- 了解 Model Context Protocol (MCP) 是什么，以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加它。
- 让智能体驱动浏览器并体验筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证界面更新，还需实际与界面交互。你希望 Copilot 能像用户一样使用正在开发的网站，从而进一步自动化更改，并更有信心地确认更新符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时与外部工具和服务通信，从而访问最新信息并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都代表智能体可访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 仓库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先检查其源代码、验证发布者，并考虑相关安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过这些设置步骤，继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 将名称设为 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能行为符合预期。现在使用 Copilot 中配置的 Playwright MCP 服务器完成同样的操作！

1. 使用以下提示，让 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确要求 Copilot 使用 MCP 服务器，它一般会自行判断。但既然你知道应该使用哪个工具，明确指出也很有帮助！这样结果会更一致，也能节省一些令牌。

2. 观察 Copilot 在浏览器中执行各项步骤，确认功能是否正常。
3. 阅读报告，确保一切行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并向你提供报告。

## 总结和后续步骤

恭喜！你使用 Playwright MCP 服务器，让 Copilot CLI 在真实浏览器中体验了功能。回顾一下，你已经：

- 了解了 Model Context Protocol (MCP) 是什么，以及 Copilot CLI 如何使用它。
- 在尚未配置 Playwright MCP 服务器时添加了它。
- 让智能体驱动浏览器并体验筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专业角色中。

## 资源

- [什么是 MCP，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
