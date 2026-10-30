---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot [自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库 `.github/agents` 文件夹中编写 Markdown 文件，可以为 Copilot 提供专用指令、最佳实践、代码模式和领域知识。团队可以将专业经验编码为可复用智能体，例如执行 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹或全局 `~/.copilot/agents` 中的 Markdown 文件定义。每个文件都包含至少带有 `name` 和 `description` 的 YAML frontmatter，以及定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行任务。可以将**自定义智能体**理解为工作者，将**技能**理解为工具。自定义智能体有独立的上下文窗口，可协调技能甚至其他智能体。本实验中的无障碍智能体会审查网站，并可调用拉取请求或测试相关技能。

> [!NOTE]
> 自定义智能体没有唯一的正确写法。应在目标环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，不受视力能力或偏好影响。近期用户反馈指出，当前深色主题的文字与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。现在使用自定义智能体生成实现。

本课将了解如何定义自定义智能体，以及如何将任务分配给自定义智能体。

## 审查无障碍自定义智能体

项目已经提供无障碍自定义智能体。先查看它如何指导 Copilot。

1. 在 Codespace 中打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带 `name` 和 `description` 的 frontmatter。

3. 浏览其无障碍网站代码生成职责、最佳实践、HTML/CSS/JavaScript 示例和常见问题。

## 创建并分配议题

GitHub 上的 Mission control 是管理智能体的中心位置，可分配任务、监控任务并提供额外指导。现在将高对比度模式任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

![Cloud agent 分配界面截图，突出显示自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 的[自定义智能体][custom-agents]，它可以将团队专业知识和标准编码为可复用角色。你了解了如何定义自定义智能体并向其分配任务。

接下来，在 Mission control 中[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
