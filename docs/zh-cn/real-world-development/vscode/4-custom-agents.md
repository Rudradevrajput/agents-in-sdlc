---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot [自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中编写 Markdown 文件，可以为 Copilot 提供聚焦的指令、最佳实践、代码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业经验编码为可复用的智能体，例如执行 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面是用于定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行任务。可以将**自定义智能体**理解为工作者，将**技能**理解为工具。

自定义智能体有独立的上下文窗口，可在工作中协调技能甚至其他智能体。本实验中的无障碍智能体会根据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或测试管理技能。

> [!NOTE]
> 自定义智能体没有唯一的正确写法。与其他 AI 用法一样，应在目标环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好影响。近期用户反馈指出，当前深色主题的文字与背景对比度不足，难以阅读。设计团队因此要求实现可由用户切换的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。现在使用自定义智能体生成实现。

本练习中你将：

- 审查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 审查无障碍自定义智能体

项目已提供无障碍自定义智能体。先查看其内容，了解它如何指导 Copilot。

返回 Codespace，打开终端，从 `main` 创建无障碍功能分支：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带 `name` 和 `description` 的 frontmatter。

3. 阅读后续部分，了解无障碍网站代码生成的核心职责、最佳实践、HTML/CSS/JavaScript 示例，以及常见问题。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的自定义智能体。选择智能体后，该会话会使用其指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的对话。模式和模型选择因练习而异；每一步会在需要时说明。

1. 如果 Chat 视图中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 打开 Chat 视图底部的智能体下拉菜单（默认活动智能体为 **default**）。
3. 从可用智能体中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并修改项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. **Files changed** 指示器会更新，显示智能体已修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库文件，例如 Astro 组件、CSS 和相关测试。

下一练习会审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot [自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。自定义智能体可以将团队专业知识和标准编码为可复用的配置，帮助 Copilot 更有效地完成任务。

本课中你了解了：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
