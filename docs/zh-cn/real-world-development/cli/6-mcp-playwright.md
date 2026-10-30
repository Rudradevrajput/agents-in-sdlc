---
title: "练习 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中查看或配置 Playwright MCP，并通过浏览器体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码还涉及数据、外部服务，以及让 Copilot 使用更多自动化工具。MCP 服务器可以扩展 Copilot CLI 的内置能力，为其提供其他工具和服务。

本课中你将：

- 了解 Model Context Protocol（MCP）及 Copilot CLI 如何使用它。
- 如果尚未配置，则添加 Playwright MCP 服务器。
- 请智能体通过浏览器体验筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证界面更新还需要实际操作界面。你希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化更改流程，并确认功能符合预期。

## 什么是 Model Context Protocol（MCP）？

[Model Context Protocol（MCP）][mcp-blog-post]为 AI 智能体与外部工具和服务通信提供了途径。通过 MCP，智能体可以实时访问最新信息，并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，每个服务器代表一组可供智能体使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP 服务器][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP 服务器][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化能力。

此外还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助用户发现并参与生态系统。

> [!CAUTION]
> 应像对待项目的其他依赖项一样对待 MCP 服务器。使用前请审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 使用 Playwright 请 Copilot 体验功能

之前已手动确认功能行为正确。现在使用 Copilot 中的 Playwright MCP 服务器执行同样的检查。

1. 使用以下提示，请 Copilot 使用 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确要求 Copilot 使用 MCP 服务器，它一般会自行判断。不过，说明预期使用的工具有助于保持结果一致，也能节省一些 token。

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告，确认功能行为符合预期。

Copilot 会启动服务器、使用 Playwright 操作网站、停止服务器并提供报告。

## 总结和后续步骤

恭喜！你已使用 Playwright MCP 服务器，通过 Copilot CLI 在真实浏览器中体验功能。本课中你：

- 了解了 Model Context Protocol（MCP）及 Copilot CLI 如何使用它。
- 如果尚未配置，则添加了 Playwright MCP 服务器。
- 请智能体通过浏览器体验筛选功能。

接下来，将[创建 QA 自定义智能体][next-lesson]，把技能和浏览器工具整合到专门角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
