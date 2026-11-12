---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体分别处理了三项任务，以改善用户体验并添加功能。云智能体虽然能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，你可以查看所有拥有未关闭拉取请求（PR）的智能体任务。你可以探索智能体执行的操作，甚至可以引导正在进行的会话。

本课将完成以下内容：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以顺便添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，将这项功能纳入同一任务似乎很合理。你将引导智能体，同时要求它添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 留意主面板和左侧面板中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，包括：
   - 从仓库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成所分配任务的各种操作。

> [!NOTE]
> 具体显示的步骤会因 Copilot 的工作状态和执行方案而异。

4. 同时留意右侧显示的拉取请求（PR）面板。通过此面板，你可以查看 PR 和已更改文件，进一步监控任务。

## 引导云智能体

现在你已查看正在进行的任务，可以让 Copilot 在实现高对比度模式时也添加浅色模式切换功能。

1. 选择标题中提到添加高对比度模式的会话。确切标题会因 Copilot 使用的名称和当前工作状态而异。

   ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。看到类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot 智能体页面中的云智能体任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！发送消息时，它正在执行某个步骤；完成该步骤后，它会将新请求加入工作流程。

和之前一样，这需要几分钟。此时可以暂停一下，回顾至今学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，也就是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过此 mission control，你可以跟踪所有正在进行和已完成的任务、审查智能体正在执行的工作，甚至可以重定向正在进行的任务以调整范围或提供更多指导。

你探索了以下概念：

- 使用 mission control 和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
