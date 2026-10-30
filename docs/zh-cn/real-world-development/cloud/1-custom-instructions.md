---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。[说明文件][instruction-files]可提供指导，让 Copilot 不仅了解要做*什么*，还了解应当*如何*完成。

本练习中你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作坊不同，本练习不会比较更新说明前后的提示结果。Copilot cloud agent 会异步处理 GitHub 议题，难以实时并排展示效果。后续审查它创建的拉取请求时，可以观察说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践准则和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加注释块，说明文件用途。

通过说明文件，可以确保 Copilot 按照这些实践完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它了解编码风格和需求。可以指定编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建说明。

说明文件分为两类：

- `.github/copilot-instructions.md`：存储库级的单一说明文件，会随该存储库的**每个**请求发送给 Copilot。应包含适用于大多数请求的项目级信息，如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> IDE 中的说明文件只用于 Copilot Chat 代码生成，不用于代码补全或下一次编辑建议。Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用存储库级说明，以及带 `applyTo` frontmatter 的 `*.instructions.md` 文件。Copilot 还支持 AGENTS.md 和 CLAUDE.md 等其他说明格式，详情请参阅[自定义说明支持文档][custom-instructions-support]。

### 管理说明文件的最佳实践

- 将 `copilot-instructions.md` 聚焦于项目级指导，如项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型或任务提供详细说明。
- 使用自然语言，清晰描述指导并提供代码正反示例。

说明文件没有唯一的正确写法，和使用 AI 一样，需要在不同环境和场景中测试并不断迭代，找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护完善的说明文件。本项目包含针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的说明。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`；当前工作坊会在相关课程中介绍对应界面的入口。
>
> 可以在 [awesome-copilot][awesome-copilot] 中查找说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

花些时间阅读本存储库随附的说明文件：一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，注意嵌套的 **GitHub Actions Workflows** 指导。这些说明适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览，留意针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 决定哪些文件适用；例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看为本项目编写单元测试的专门说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部，留意它对其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成较小、可复用的文件，并为 Copilot 提供示例。链接路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分尚未要求在代码中编写文档，这也是生成的 helper 没有文档注释的原因。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

本练习中唯一需要编辑的内容是为项目添加标准，要求在代码中使用 TSDoc 文档注释和文件级注释头。后续练习中，cloud agent 会读取这些说明。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 打开 `.github/copilot-instructions.md`，找到 **Code formatting requirements** 部分（大约位于第 27 行）。
3. 在现有标准下方添加：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存文件。

> [!TIP]
> 全局指导应放在 `.github/copilot-instructions.md`；特定语言、文件类型或任务的指导可放在 `*.instructions.md` 文件中。刚才添加的项目级标准适合放在存储库级文件中。

## 提交、推送并合并说明更新

Cloud agent 会读取议题目标分支中的说明。下一练习中，Copilot 会从 `main` 分支创建分支，因此必须先将说明更改合并到 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后 cloud agent 处理议题时即可读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，也可以跳过分支步骤并直接提交。创建分支是为了模拟真实项目的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取项目说明，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续工作坊中，cloud agent 创建的拉取请求会体现该标准。

接下来，将[第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
