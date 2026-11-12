---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会实时在你的计算机上进行，VS Code 中的 Copilot Chat 会显示每次工具调用和每次文件编辑。你可以直接在聊天中审查每个差异、接受或拒绝单项更改，并通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

本练习将完成以下内容：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导智能体，同时添加浅色模式切换和高对比度模式。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出了一项需求：他们也希望为觉得默认主题太暗的用户添加浅色模式切换。与其从头开始新对话，不如引导现有会话扩展当前工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中内联显示差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容，查看每次工具调用。你会看到文件读取、文件编辑以及需要运行的终端命令。
3. 智能体提出文件编辑时，差异会内联显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的完整工作集。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你确认安全的命令。

5. 等待智能体完成高对比度模式实现。完成时应该会看到总结更改内容的最终消息。

## 引导智能体添加浅色模式

现在高对比度模式已经就绪，可以继续在同一对话中添加浅色模式切换。由于继续使用同一聊天会话，智能体会保留先前工作的上下文，包括修改的组件、使用的样式方案和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会从先前工作继续，探索已作出的更改，并提出添加浅色模式的方案。
3. 新编辑逐步生成时，像处理高对比度模式一样逐一审查。对每项差异使用 **Keep** 或 **Undo**。
4. 如果智能体的方向不符合预期，例如试图以你不喜欢的方式重构切换 UI，可以选择聊天输入框旁边的 **Stop** 按钮停止任务，然后发送澄清后的后续消息。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以使用后续提示要求调整（“简化 CSS——使用 CSS 变量，而不是重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板中”）或扩展范围（“为两个开关都添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有已更改文件。应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并查看差异。确认其中体现了自定义智能体的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并引导正在进行的会话——随着文件编辑逐步生成，你审查每项编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留现有上下文。

本课中，你探索了：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示引导正在进行的会话。

在[下一课][next-lesson]中，你将发布无障碍功能的拉取请求，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
