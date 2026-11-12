---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已经手动验证代码能够运行，也使用说明文件确保代码遵循团队标准。但测试呢？Lint 呢？持续集成 (CI) 中的其他环节呢？

对于这类任务，**智能体技能**最适合！技能可以帮助 Copilot 了解如何正确执行这些操作。

本课将完成以下内容：

- 探索现有的 `quality-checks` 技能。
- 自定义技能结果的报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，使结果更易阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和辅助资源打包在一起，供智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。该 Markdown 文件包含定义技能名称和说明的 frontmatter、技能用途概述，以及说明何时应调用该技能的指导。文件夹中还可以包含供技能调用时使用的脚本和其他资源子目录。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，成为团队共享和复用的仓库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

来看看 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明可帮助 Copilot 判断何时调用该技能。
3. 阅读说明，留意它如何引导 Copilot 执行测试和 lint 流程。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言请求。现在让 Copilot 运行该技能中的三项检查。

1. 返回处于 Interactive 模式的筛选功能对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后生成的报告。

## 自定义报告

我们希望报告能清楚说明运行了什么、是否成功，以及工具实际报告的结果。现在更新技能，让它生成这样的报告！

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

现在看看更改效果！Copilot CLI 无需重启对话即可重新加载编辑后的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后的报告，并与第一份报告比较。

## 总结和后续步骤

你自定义并使用了现有智能体技能。本课中，你：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能结果的报告格式。
- 重新加载并运行了技能。

该更改会随筛选功能一并提交到功能 PR。接下来，[使用 Playwright MCP 服务器][next-lesson]，让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，并非额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
