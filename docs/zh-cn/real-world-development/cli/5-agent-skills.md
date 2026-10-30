---
title: "练习 5 - 自定义并使用 quality-checks 技能"
description: "了解现有的 quality-checks 技能、自定义结果报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是编写代码。我们已手动验证代码，并使用说明文件确保代码符合标准。接下来还要考虑测试、lint 和持续集成（CI）的其他部分。

**智能体技能**适合处理这类任务！技能可以帮助 Copilot 了解如何执行这些操作。

本课中你将：

- 了解现有的 `quality-checks` 技能。
- 自定义结果报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，使结果更易阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和辅助资源打包在一起，供智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含 `SKILL.md`。Markdown 文件的 frontmatter 使用名称和描述定义技能；正文说明技能用途和调用时机。文件夹中还可以包含技能运行时使用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 了解技能

现在查看 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。

1. 返回 Codespace，在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。描述会帮助 Copilot 判断何时调用技能。
3. 阅读说明，留意它如何指导 Copilot 执行测试和 lint。
4. 注意技能目前没有 **Results output formatting** 部分。

## 修改前先运行技能

技能既可以通过 Copilot CLI 直接调用，也可以用自然语言调用。现在请 Copilot 运行该技能中的三项检查。

1. 返回筛选对话并进入 Interactive 模式。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告。

## 自定义报告

现在改进报告，使其说明运行了什么、是否成功，以及工具报告了哪些结果。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

完成更改后，验证其效果。Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看报告，并与第一次的报告比较。

## 总结和后续步骤

你已自定义并使用现有智能体技能。本课中你：

- 了解了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果报告格式。
- 重新加载并运行了技能。

筛选功能 PR 会包含这项更改。接下来，通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

这些社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
