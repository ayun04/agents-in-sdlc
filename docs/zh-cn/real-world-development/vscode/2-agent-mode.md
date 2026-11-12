---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使对应用进行最简单的更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，你通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件，再次更新……后续工作还有很多。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式能够在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。现在来探索如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但它们针对两种不同的体验。智能体模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你交流。

本练习将介绍：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望让用户按类别和发行商筛选游戏。上一项练习中已经添加了发行商辅助函数；现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解其当前功能。

该网站是一个以开发者为主题的棋盘游戏众筹平台，可让用户浏览游戏列表并查看详情。它是一个单体 Astro 应用，页面会在构建时渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库——没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能使用小型、限定作用域的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

为方便运行网站，项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。可以按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一项练习结束后，你应该处于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否要为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 后，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道，网站服务器正在其中运行！

### 浏览网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和简介。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、简介、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

网站的初始实现已经可以正常使用，但我们希望添加新功能来改进它。先查看待办事项。你从模板创建项目时，系统已经自动生成了一组议题；让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令及调用工具。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某一种模型。制作本研讨会时使用了 Claude Sonnet 4.5，因此将其列入说明。这样希望能让你收到的代码建议相对一致，获得良好体验。不过，由于 LLM 具有概率性，你收到的建议可能与研讨会中描述的内容不同。这完全正常，也在预期之中。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意，Copilot 还根据它认为对用户最有用的内容为你排列了优先级。

## 查看说明文件

让智能体开始生成代码前，先查看将用于向 Copilot 提供工作上下文的说明文件。接下来会使用[用户界面 (UI) 说明][ui-instructions]，了解如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向多种文件类型说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件既可以引用其他说明文件，也可以引用项目中的文件。路径相对于说明文件所在位置。这样便于重复使用内容、将复杂说明拆分成更易管理的部分，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

完成筛选至少需要对应用进行三项单独更新：

- 在数据层 (`src/lib/`) 添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前需要运行并通过测试。Copilot 智能体模式可以为你完成这些任务！现在添加筛选功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。你应该会看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有 **Keep** 或 **Undo** 选项，因此不必在它工作时选择这些按钮。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有几次暂停，甚至出现测试失败。这没有关系！Copilot 会在代码生成和测试之间反复迭代，直到任务完成且未发现错误。

   ![Copilot 智能体模式完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

现在使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 - 实现分页

游戏列表不断增长后，就需要启用分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用已有的说明文件。
- 考虑分页的实现方式，是否允许用户选择每页大小，还是使用固定大小。
- 编写提示时，提供必要指导，让 Copilot 按照你的设想实现分页。
- 你可能需要与 GitHub Copilot 反复协作，提出更改要求并提供上下文。这是使用 Copilot 的正常流程！

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
