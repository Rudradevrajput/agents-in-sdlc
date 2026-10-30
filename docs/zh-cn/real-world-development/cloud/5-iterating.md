---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你使用 GitHub Copilot 执行了多项任务，以改善用户体验并添加功能：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换的无障碍功能。现在审查这些更改，必要时向 Copilot 提供反馈。

软件设计和 DevOps 的基本原则不会因使用生成式 AI 而改变。始终审查生成的代码，并遵循正常的 DevOps 流程。接下来审查 Copilot 创建的文档、相关游戏功能和无障碍功能，再向团队开放审查。

## GitHub Copilot cloud agent 的安全机制

Copilot cloud agent 会在无人监督的情况下异步工作，因此设置了安全限制：

- Copilot 只能读取存储库，并且只对用于其工作的分支拥有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并创建独立的临时环境。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

首先在 GitHub.com 上查看 Copilot cloud agent 创建的第一个 PR，即添加代码文档的 PR。

> [!NOTE]
> PR 中可能显示 GitHub Copilot 被防火墙阻止的警告，这是预期行为。默认情况下，Copilot 访问外部资源（包括外部 MCP 服务器调用）会受到限制。如有需要，可以[自定义或禁用 cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的存储库，选择 **Pull Requests**。
2. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 会包含 **[WIP]** 标记。请等待任务完成，这可能需要几分钟。

3. 选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

4. 查看新添加的 TSDoc 文档注释和其他文档。具体更改会因 Copilot 生成的内容而异。审查时留意 TSDoc 文档注释、TypeScript 约定和注释头；这些内容来自练习 2 开始时查看的自定义说明文件。
5. 如果更改看起来正常，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准。
7. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 确认工作流已加入 PR 检查队列，并等待单个 Astro 应用的项目检查通过。这可能需要几分钟。

## 请求 GitHub Copilot 修改

与团队协作一样，可以在 PR 评论或代码行内评论中标记 Copilot。它会读取这些评论并启动新会话来处理更改。由于生成结果具有概率性，无法为请求内容给出固定模板；可以考虑要求 Copilot：

- 为每个代码文件添加简短的文件注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建介绍 Astro 应用结构的 README。

1. 在评论中使用 **@copilot** 标记，像标记其他用户一样，请求修改生成的文档。可使用上述任一建议，也可以提出其他文档改进请求。
2. 选择 **View Session** 观察 Copilot 启动新会话并执行更新。
3. 选择 **Back to pull request** 可返回 PR。

   ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，确认 PR 中出现了新的提交。
5. 选择 **Files changed** 选项卡审查更改。

满意后，可以将 PR 从草稿状态改为可审查，然后合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查完成且更改看起来正常后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准。
7. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 确认工作流已加入 PR 检查队列，并等待单个 Astro 应用的项目检查通过。这可能需要几分钟。
9. **可选：**在 Codespace 中手动测试相关游戏功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名称（例如 **copilot/fix-8**），然后运行：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。可以像之前一样继续迭代，例如请求调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释。请记住，分配此任务时尚未更新自定义说明。添加评论后，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后审查无障碍自定义智能体创建的 PR，其中应包含练习 3 中分配的高对比度模式，以及练习 4 中要求的浅色模式。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 会包含 **[WIP]** 标记。请等待任务完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，重点检查：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保留用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 确认更新看起来正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 确认工作流已加入 PR 检查队列，并等待单个 Astro 应用的项目检查通过。这可能需要几分钟。
10. **可选：**在 Codespace 中手动测试无障碍功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名称，然后运行：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在页面重新加载后仍然保留。

无障碍自定义智能体帮助 Copilot 遵循无障碍最佳实践。如果发现问题，可以像处理其他 PR 一样在评论中标记 **@copilot** 请求更新。

## 可选练习：继续委派工作

可以为存储库创建更多议题并分配给 Copilot cloud agent，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表实现分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 工作坊，并：

- 检查存储库的自定义说明，观察其对 cloud agent 输出的影响。
- 将议题分配给 Copilot cloud agent，观察它异步准备环境、规划并执行任务。
- 创建并使用无障碍自定义智能体，添加高对比度和浅色模式切换。
- 使用 Copilot agents 页面监控并引导无障碍会话。
- 审查并迭代 cloud agent 的 PR，通过 **@copilot** 请求更改并批准工作流。

## 审查和后续步骤

如果想继续探索，可学习其他工作坊：

- 🖥️ **[VS Code 工作坊](../../vscode/)**：在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作坊](../../cli/)**：在终端完成两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端委派。

也可以尝试将重构或测试覆盖率议题分配给 cloud agent、创建其他领域的自定义智能体，或为不同技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

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
