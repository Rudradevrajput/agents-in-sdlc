---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几节练习中，你要求 Copilot cloud agent 执行了三项改进用户体验和添加功能的任务。Cloud agent 虽然可以自主异步工作，但监控任务仍然很重要。

可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在 Mission control 中，可以查看有未关闭 PR 的智能体任务、查看已执行操作，并引导正在进行的会话。

本课中你将：

- 浏览智能体页面并监控 cloud agent 任务。
- 引导正在进行的会话添加功能。

## 场景

分配高对比度模式后，团队决定同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，可以引导它在实现高对比度模式时一并添加浅色模式。

## 审查 Copilot cloud agent 任务

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表应包含已分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，审查 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 的工作环境。
   - 设置 MCP 服务器。
   - 执行完成指定任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作状态和执行方案而变化。

4. 留意右侧的拉取请求（PR）面板。可在此查看 PR 和更改文件，以便进一步监控。

## 引导 cloud agent

现在请 Copilot 在实现高对比度模式时一并添加浅色模式切换。

1. 选择与高对比度模式相关的会话；具体标题会因 Copilot 采用的名称和当前工作状态而异。

   ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，等待设置完成并开始工作；此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot agents 页面上的 cloud agent 任务，突出显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 确认 Copilot 已接收提示并将其纳入工作流程。

## 让 Copilot 继续工作

Copilot 会在完成发送提示时正在执行的步骤后，将新请求加入任务。这可能需要几分钟；等待期间可以回顾目前学到的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控和引导 cloud agent 任务的中心位置。你可以跟踪活动和已完成的任务、审查工作，也可以在会话进行中调整范围或补充指导。

本课中你浏览了 Mission control 并监控 cloud agent 任务，还引导正在进行的会话添加功能。接下来，[迭代审查 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
