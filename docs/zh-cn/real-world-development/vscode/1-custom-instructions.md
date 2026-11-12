---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种关键方式是使用[说明文件][instruction-files]指导代码生成。

本练习将完成以下内容：

- 了解项目上下文、编码指南和文档标准如何通过仓库自定义说明及按路径限定的说明文件提供给 Copilot。
- 在当前说明文件的指导下，为筛选功能创建第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加一项新的仓库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能切片提交到 `main`，以便 Copilot 在下一项练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合你设置的部分标准。Copilot 具有非确定性。本练习的重点是观察更新说明后行为的*整体变化趋势*，而不是要求输出逐字一致。

## 说明文件

### 场景

像所有优秀的软件开发团队一样，Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 界面应采用深色模式并具有现代感。
- 应在代码中通过 TSDoc 文档注释添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过说明文件，可以确保 Copilot 掌握相关信息，并按照上述实践完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。通过这些说明，你可以指定偏好的编码规范、库，甚至注释风格。你可以为整个仓库创建说明文件，也可以为特定文件类型创建说明，提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个说明文件。文件内容应包含适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如所用技术栈、构建内容概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建说明文件，为特定语言（如 TypeScript 或 Astro），或特定任务（如创建 UI 组件或一组新的单元测试）提供指导。

> [!NOTE]
> 在 IDE 中，说明文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用仓库级说明文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

全面讨论如何创建说明文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 在 `copilot-instructions.md` 中集中说明项目级指导，例如构建内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，让指导清晰易懂，并提供代码应如何编写（以及不应如何编写）的示例。

创建说明文件没有唯一的方式，正如使用 AI 也没有唯一的方法一样。通过试验，你会找到最适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的说明文件。浏览本项目中的文件时，你可能会发现其中有针对多种任务的说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作方式各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应环境的课程会在相关处介绍这些功能。
>
> 想寻找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

花些时间阅读此仓库自带的说明文件，其中有一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览该文件，留意项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用相对于仓库根目录的 glob 指定适用文件。任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 留意其中针对本项目单元测试创建方式的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并提供生成代码时可参考的示例。（其中的路径相对于说明文件，而非仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

按照最佳实践，为本次更改创建一个分支。

1. 返回上一项练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

为了了解自定义说明的影响，先在当前说明文件的指导下发送提示。稍后更新说明，再次发送相同提示并比较结果。

1. 关闭之前练习中打开的编辑器选项卡，确保 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是一个有类型的函数，它将 `db` 客户端作为第一个参数，并返回有类型的发行商数组——这是 `.github/instructions/drizzle.instructions.md` 中数据层规范的作用（该说明适用于 `src/lib/*.ts`）。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有相关要求，它也可能添加文档注释。如果发生这种情况也没关系——更新说明后提升行为*一致性*仍然是本练习的重点。

## 添加新的仓库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为仓库编码标准添加文档要求，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应该位于第 27 行附近。留意其中记录的项目编码标准——目前还没有代码文档要求，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在仓库级文件 (`.github/copilot-instructions.md`) 中设置全局指导，也可以在 `*.instructions.md` 文件中设置适用于特定语言、文件类型或任务的指导。仓库级文件适合放置刚才添加的文档注释规则等项目级标准。

## 重新运行提示并观察更改

1. 返回 Copilot Chat，选择 **New Chat** 清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一项练习会以这些代码为基础继续推进。

## 提交说明并推送分支

说明文件和仓库中的其他资源一样，需要通过源代码管理进行维护。现在将更改提交并推送到仓库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改已保存：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求并将其关联到待办事项中的文档标准议题。虽然可以手动操作，但也可以让 Copilot 使用已连接到项目的 GitHub 工具代为处理！让 Copilot 查找议题、创建可关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已经将 GitHub 工具连接到工作区，无需额外设置——Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。稍后的练习会介绍这种连接的工作方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明其中既有说明更新，也有新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果 Copilot 提示允许其代表你执行 GitHub 操作，请审查命令，并酌情选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并将当前分支切回 main，提示如下：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示允许其代表你执行 GitHub 操作和 shell 命令，请审查命令，并酌情选择 **Allow**。

现在，你已在 GitHub Copilot 的帮助下创建并合并了拉取请求！

## 总结和后续步骤

你探索了 Copilot 如何获取本项目说明文件中的上下文，然后在 VS Code 中使用 Copilot Chat 完成了以下内容：

- 发送代码生成提示，并观察 Copilot 如何根据*现有*说明生成内容。
- 在 `.github/copilot-instructions.md` 中添加一项新的仓库级标准。
- 再次运行相同提示，观察建议的代码如何遵循新标准。
- 将说明更新和辅助函数基础提交到 `main`，以便下一项练习继续使用。

接下来，在[智能体模式][next-lesson]中实际应用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更有效 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
