---
title: "练习 10 - 回顾和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已使用 GitHub Copilot CLI 完成 Tailspin Toys 的连续工作流。本次工作中你：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中确定方案、使用 Autopilot 模式构建，再在 Interactive 模式下审查。
- 使用自定义说明指导智能体，然后自定义现有 `quality-checks` 技能并运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol（MCP）服务器，在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义智能体，以评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选功能更改，并为该 PR 授权 Agent Merge。
- 探索用于上下文、模型、共享和可选云端委派的斜杠命令。

## 交付内容

本工作坊分为两个 PR 阶段，每个阶段都从更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选功能、更新说明并应用于功能、更改 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建其 PR，你始终使用同一对话和分支。为简化工作坊，这些更改合并在一个 PR 中。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 在 PR 创建前将这些结果与需求和测试覆盖审查结合起来。

新增测试应解决实际缺口；QA 运行后无需添加测试也可能是正确结果。授权合并前请审查代码和证据；更改之后应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。本工作坊中，你更新了说明、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。说明设定标准，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择**模式和模型**。使用 **Plan** 在构建前确定方案，使用 **Interactive** 协作完成范围明确的更改，使用 **Autopilot** 处理边界清晰的任务。日常修改可选择响应更快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清楚说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响结果。

## 更多探索

核心工作流已完成，还可以了解一些 CLI 功能：

- `/review`：请代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获取另一种观点。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

提高工具使用能力的最佳方法就是继续使用它。可以在生产代码、个人项目或一直想做的小应用中使用 Copilot。与团队分享经验，也向团队成员学习。也可以继续探索相关文档。

如果要继续使用终端扩展 Tailspin Toys，可选修 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [了解 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
