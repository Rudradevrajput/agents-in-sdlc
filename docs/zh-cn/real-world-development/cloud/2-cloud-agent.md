---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务，例如待解决的安全问题、需要更新的旧代码，或长期搁置的功能请求。GitHub Copilot cloud agent 可以自主更新代码和添加功能。完成任务后，它会创建草稿拉取请求，供开发者审查。这样可以将繁琐任务交给智能体处理，让开发者专注于更重要的工作。

本练习将介绍如何：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受聘开发网站的承包商没有留下完善的文档，事实上几乎完全没有文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验：在每款游戏的详情页显示同类别的其他游戏，方便用户继续浏览。他们暂时不需要完善的实现，只需要一个可用于用户体验验收测试的版本。此任务目前受阻，不过还有优先级更高的议题。

这类工作容易被推迟，很适合交给 Copilot cloud agent 异步处理。开发者可以继续其他工作，之后再审查结果。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent][cloud-agent] 可以像开发者一样在后台执行任务。可以通过多种方式启动工作，例如[将 GitHub 议题分配给 Copilot][assign-issue]。分配后，Copilot 会创建草稿 PR 跟踪进度、准备环境并开始工作。你可以在会话进行中或完成后查看；方案准备好供审查时，Copilot 会在 PR 中标记你。

## 明确任务范围的重要性

GitHub Copilot 并非魔法，使用它并不能省去思考。即使看似简单的任务也可能有复杂细节，因此请遵循[为 Copilot cloud agent 界定任务范围的建议][cloud-agent-best-practices]。将它视为 AI 结对程序员，分阶段工作、学习、试验并调整；加入生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

之前的练习介绍了自定义说明及其如何指导 Copilot。如果已经完成相关实践，就会看到 `.github/copilot-instructions.md` 的作用；如果还没有，可以借此快速查看。在将任务分配给 Copilot cloud agent 前，先只读查看存储库中已有的说明文件，以便稍后观察它们的影响：

- `.github/copilot-instructions.md`：查看 **Code standards**，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将说明限定到 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给 cloud agent 时，留意生成的 PR 是否包含 TSDoc 文档注释、遵循 TypeScript 约定并添加文件注释头；这些要求来自上述说明文件。后续练习还会再次检查 PR。

## 配置 Copilot cloud agent 的开发环境

无论由谁编写代码，通常都需要特定环境和安装脚本，以确保项目状态正常。将任务分配给 Copilot 时也一样，因为它的工作方式类似软件工程师。

Copilot cloud agent 使用 [GitHub Actions][github-actions] 作为工作环境。可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]，在 Copilot 开始工作前安装所需的开发工具和依赖项。本实验已提前配置该工作流，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库。该项目是单个 Astro 应用：

```yaml
name: "Copilot Setup Steps"

# 允许在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅授予步骤所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置：Node.js（应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有数据可用。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几点值得注意：

- 它只有一个名为 `copilot-setup-steps` 的作业，会在 Copilot 开始处理拉取请求前通过 GitHub Actions 执行。
- `workflow_dispatch` 触发器允许在存储库的 **Actions** 选项卡中手动运行工作流。这便于提前测试，而不必等待 Copilot 启动工作流。

## 添加文档注释

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表，因为它正在准备环境（运行前面介绍的工作流）、分析项目并确定处理方案。

13. 审查任务列表和 Copilot 即将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session**。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新浏览器才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何探索代码、理解当前状态、暂停思考并确定合适计划，然后编写代码。

这一过程可能需要几分钟。Copilot cloud agent 的主要优势之一是能够异步执行任务，让你处理其他工作。接下来利用这一点，将另一项任务分配给 Copilot，同时转去为应用编写其他功能。

## 为游戏详情页添加相关游戏

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一组任务时，它可以处理另一组。即使为设计团队添加相关游戏区块不一定需要很长时间，也可以把时间用于其他工作。因此，将这项任务交给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理第二项请求。

![Copilot 使用眼睛表情符号表示正在处理议题的截图](../../../_images/ex4-issue-eyes-emoji.png)

Copilot cloud agent 的工作方式类似软件工程师，因此无需主动监控；任务完成后再审查即可。现在可以开始创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][cloud-agent]，它可以异步处理议题，帮助解决技术债务、创建功能或迁移代码。你了解了环境配置、安全操作、明确界定任务以及分配议题。

接下来创建并使用自定义智能体。Cloud agent 也可以使用 [MCP 服务器][cloud-agent-mcp]和自定义说明。

## 资源

- [关于 Copilot cloud agent][cloud-agent]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
