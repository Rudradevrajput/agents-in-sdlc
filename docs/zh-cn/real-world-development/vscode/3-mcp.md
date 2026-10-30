---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应确认它在浏览器中正常运行。本练习将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器测试功能，然后发布分支并创建 PR。

本练习中你将：

- 了解 Model Context Protocol（MCP）如何通过新工具扩展 Copilot。
- 向工作区添加 Playwright MCP 服务器。
- 请 Copilot 使用服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol（MCP）？

智能体模式可以访问编辑器之外的资源，从而变得更加强大。Model Context Protocol（MCP）是一种让 Copilot 与外部工具和服务通信的标准方式。

![示意图展示智能体模式的内部工作方式，以及它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 智能体与外部工具和服务通信提供了途径。通过 MCP，智能体可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。每个 MCP 服务器都代表 AI 智能体可访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP 服务器](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，智能体可以用来创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP 服务器](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，例如浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 应像对待项目的其他依赖项一样对待 MCP 服务器。使用前请仔细审查其源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 检查 MCP 配置

`.vscode/mcp.json` 用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

此条目随项目模板提供，因此上一练习中 Copilot 可以读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用工具时使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是本地服务器：VS Code 会运行 `npx @playwright/mcp@latest` 启动它。`--headless` 表示无头浏览器，因为 Codespace 没有可显示浏览器窗口的桌面环境。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 执行端到端测试，因此通常已安装所需浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器。Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. `.vscode/mcp.json` 中 `playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任。

手动启动是可选的；若跳过，VS Code 会在下一步 Copilot 首次需要该工具时启动服务器并请求信任。

## 测试筛选功能

Playwright MCP 服务器让 Copilot 使用真实浏览器。它可以打开页面、导航、应用筛选并读取结果，然后向你总结观察到的行为，无需离开对话。

Playwright MCP 服务器基于页面的[无障碍树][playwright-mcp-server]而不是屏幕截图工作。智能体会像辅助技术一样，根据带标签的按钮、链接和列表项等结构化元素进行推理，因此功能检查也能顺便进行初步无障碍检查。

确保服务器已连接且应用正在运行，然后请 Copilot 测试筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 启动浏览器、执行上述步骤并报告结果。请对照议题中的验收标准审查报告；如果发现异常，可以继续提问或要求修复代码，再创建 PR。

> [!NOTE]
> 测试时应用必须在 `http://localhost:4321` 运行。如果开发服务器已停止，请先重新启动。首次使用 Playwright MCP 时可能需要下载浏览器；如果提示缺少浏览器，请运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/

## 发布分支并创建拉取请求

确认功能正常后，发布 `filtering-vscode` 分支并创建 PR，供团队审查。

1. 打开 Codespace 的 **Source Control** 面板，审查 Copilot 所做更改。
2. 选择 **+** 暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板显示已更改文件的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行接口（CLI）创建 PR；也可以请 Copilot 查找相关议题并关联 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat** 开始新会话。
2. 在智能体下拉菜单中选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，可能会提示进行 GitHub 身份验证。
5. 确认 Copilot 找到了正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查 PR，但**暂时不要合并**。

## 总结和后续步骤

本练习中你：

- 了解了 Model Context Protocol（MCP）如何通过新工具扩展 Copilot。
- 向工作区添加了 Playwright MCP 服务器。
- 使用它在发布前通过浏览器手动测试筛选功能。
- 发布了分支并为筛选工作创建拉取请求。

MCP 是扩展 Copilot 的一种方式。接下来，创建[自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
