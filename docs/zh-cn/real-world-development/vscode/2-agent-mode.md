---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使最简单的应用更新，通常也需要修改多个文件并运行测试。开发者往往需要找出相关文件、完成更改、运行测试、调试、发现遗漏后再次修改，流程可能不断重复。

GitHub Copilot 智能体模式可以在 IDE 中更自主地工作。它会先探索项目结构，再执行必要更改、运行测试等任务，并自动修复发现的问题。现在看看如何使用智能体模式为网站添加功能。

> [!NOTE]
> 智能体模式和 cloud agent 名称相似，但适用于不同场景。智能体模式在 IDE 中执行任务，便于快速反馈和交互；cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习中你将了解：

- Copilot 智能体模式如何探索项目、找到相关文件并协调更改。
- 如何使用 GitHub Copilot 智能体模式跨 UI 和数据层实现功能。
- 合并前如何审查智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，希望用户可以按类别和发行商筛选。上一练习已添加 publishers helper；现在使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

先了解 Tailspin Toys 网站当前的功能。该网站是面向开发者的桌游众筹平台，可列出游戏并显示详情。它是一个 Astro 应用，构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能使用范围明确的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 命令，用于启动 Astro 开发服务器。

1. 返回 Codespace；上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 更新主分支并创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后会显示类似以下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 `http://localhost:4321` 地址打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接，会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道。

### 浏览网站

网站运行后，可以探索主要功能：

- **主页**：显示游戏列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后查看标题、描述、发行商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的对话。模式和模型选择因练习而异；每一步会在需要时说明。

项目运行正常后，查看模板自动创建的议题待办事项。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。Agent 可以自主规划并修改多个文件、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此示例使用该模型，以便尽量保持体验一致。由于大型语言模型具有概率性，实际建议可能不同，这是正常现象。

> [!NOTE]
> 由于模型具有概率性，Copilot 可能会调用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，请 Copilot 查看议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审查 Copilot 生成的议题列表和优先级建议。
 
Copilot 还会根据它认为对用户最有用的事项为议题排序。

## 审查说明文件

让智能体生成代码前，先查看用于提供上下文的说明文件。本练习将使用 [用户界面（UI）说明][ui-instructions]，了解如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的整体指导，包括：
   - 项目架构概览。
   - 组件设计、可测试性和无障碍原则。
   - 针对 Astro、Tailwind CSS 和 Drizzle 等文件类型的说明链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用内容、拆分复杂指导，并为 Copilot 提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

完成筛选至少需要对应用进行三项更改：

- 在数据层（`src/lib/`）添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

合并前还必须运行并通过测试。Copilot 智能体模式可以代为执行这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并找到相关数据层、UI 和测试文件，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库复杂度，这可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo**，完成后会为所有更改提供相应选项；工作尚未结束时无需立即选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板中的对话框，询问用户是否允许运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，或有测试暂时失败。这是正常的；Copilot 会在生成代码和运行测试之间迭代，直至任务完成且未发现错误。

   ![包含 Copilot Agent Mode 完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了功能：

- 使用智能体模式跨 UI 和数据层实现新功能。
- 让 Copilot 探索项目、识别相关文件并协调更改。
- 合并前审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并为其创建拉取请求。

### 额外探索：实现分页

随着游戏列表增长，可能需要分页。运用本练习学到的技能，请 Copilot 为网站添加分页。考虑分页方式、是否允许用户选择每页大小、提示中需要提供哪些指导，以及如何通过迭代和补充上下文完善结果。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途及使用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
