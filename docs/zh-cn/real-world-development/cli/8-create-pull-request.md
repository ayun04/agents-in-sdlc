---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证 (QA) 配置和测试都已保存在同一分支上。现在，一起审查这些更改并创建拉取请求。之前，你亲自合并了星级评分拉取请求（PR）；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分到几个 PR 中。为了简化研讨会，你将完整的筛选和质量工作流保留在同一对话和分支中，并通过一个 PR 提交所有更改。

本课将完成以下内容：

- 了解 Agent Merge 及其如何自动化 PR 合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理剩余 PR 工作，同时由开发者掌控合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决阻止 PR 合并的问题——修复失败的持续集成 (CI) 检查、回应审查意见，并在需要时变基——然后在仓库允许的情况下启用 GitHub 自动合并。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代为处理这部分工作。启用 Agent Merge 前，先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码都已完成，现在一起审查、创建 PR，并让 Agent Merge 处理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认看到 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、仓库设置或权限不足等限制。如果操作受阻，请阅读报告的原因，并在仓库允许时手动完成已审查的合并。

## 总结和后续步骤

你已将代码生成、测试和验证以及拉取请求处理等多个开发步骤自动化。本课中，你：

- 了解了 Agent Merge 及其如何自动化 PR 合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
