---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
description: "了解如何配置 Copilot cloud agent 的工作环境、分配议题、监控会话并审查拉取请求。"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent][about-cloud-agent]** 可在云端异步工作。你在 GitHub 上分配任务后，cloud agent 会在后台探索存储库、进行更改并创建拉取请求，同时你可以处理其他工作。

本工作坊会添加 cloud agent 遵循的自定义说明，然后分配 GitHub 议题并让它实现功能。你将审查并使用自定义智能体来指导它的工作，通过智能体仪表板监控和引导会话，最后审查它创建的拉取请求并迭代完善结果。

## 练习

| 练习 | 主题 | 内容 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加 cloud agent 会遵循的自定义说明 |
| [2. Cloud agent][ex2] | 异步智能体 | 将议题分配给 Copilot cloud agent |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 账户或组织已启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？已验证学生可通过 [GitHub Education][student-plan] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、MCP、代码审查和 Copilot CLI 功能。

> [!NOTE]
> Cloud agent 适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，不适用于 Copilot Free。使用 Copilot Business 或 Enterprise 时，管理员必须为组织启用该功能。

> [!NOTE]
> 本工作坊不介绍 MCP。若要了解如何在 Copilot 中使用 MCP 服务器，请参阅 [CLI 工作坊][cli]或 [VS Code 工作坊][vscode]。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[student-plan]: https://github.com/education/students
[cli]: ../cli/
[vscode]: ../vscode/
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
