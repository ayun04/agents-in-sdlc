---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你使用 GitHub Copilot 处理了多个任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换在内的无障碍功能。现在来检查代码更改；必要时，可以向 Copilot 提供反馈，改进其工作。

### 场景

正如前面多次强调的那样，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。基于此，在允许团队其他成员审查之前，先查看 GitHub Copilot 为添加文档、相关游戏功能和无障碍功能所提出的更改。

## GitHub Copilot 云智能体与安全

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此设置了若干安全限制，以确保操作安全：

- Copilot 对仓库只有读取权限；写入权限**仅限**它用于生成代码的分支。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境来工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），也就是为代码添加文档的 PR。你将使用 GitHub.com 上的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。如果需要，可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或更完整描述的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息一下或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

   浏览更改时，留意 TSDoc 文档注释、TypeScript 规范和文件注释头。这些内容来自你在练习 2 开始时查看的自定义说明文件。

6. 审查完更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到有工作流正在等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流进入拉取请求的检查队列。正常情况下，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作处理拉取请求并非单向过程。你可以像对待团队其他成员一样，在拉取请求评论或代码内联评论中提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果具有非确定性，我们无法提供固定的提示文字。可以要求 Copilot 更新的内容包括：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的某项建议，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，查看 Copilot 执行工作。留意 Copilot 会启动新会话来进行更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。确认满意后，可以将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更完整描述的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到有工作流正在等待批准的提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应该会看到工作流进入拉取请求的检查队列。正常情况下，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：** 你也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能！和之前一样，你可以通过迭代向 Copilot 云智能体请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，这项任务是在你更新自定义说明**之前**分配的！）。和之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会看到并启动新的会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的无障碍功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在 mission control 中请求的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更完整描述的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，重点关注：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查完更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到有工作流正在等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流进入拉取请求的检查队列。正常情况下，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：** 你可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确认功能符合预期且在重新加载页面后仍会保留。

请注意，无障碍自定义智能体如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或有改进建议，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

当你能把真实待办事项交给云智能体并转而处理其他工作时，云智能体最有用。可以在自己的仓库中创建更多议题并分配给 Copilot，养成这种习惯。以下是一些想法：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体环境的全部课程。通过这些课程，你：

- **检查了仓库自带的自定义说明文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为 mission control**，监控并引导进行中的无障碍会话。
- **审查并迭代云智能体的拉取请求**，标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体课程。如果想继续探索，其他课程可以补充你在此处练习的内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的仓库中，还可以尝试以下后续练习：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
