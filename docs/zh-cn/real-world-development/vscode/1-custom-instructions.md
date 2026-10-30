---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应让它能够获取这些上下文。[说明文件][instruction-files]是提供代码生成指导的重要方式。

本练习中你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件提供给 Copilot。
- 在当前说明生效时生成筛选功能的第一部分数据层代码（publishers helper）。
- 向 `.github/copilot-instructions.md` 添加新的全库标准。
- 再次运行相同提示，观察生成代码如何应用新标准。
- 将说明更新和筛选代码提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不完全遵循设置的标准。Copilot 具有概率性；本练习要观察的是更新说明后的行为趋势，而不是要求输出逐字符一致。

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

- `.github/copilot-instructions.md`：存储库级的单一说明文件，会随该存储库的**每个**请求发送给 Copilot。应包含适用于大多数聊天或 CLI 请求的项目级信息，如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件只用于 Copilot Chat 代码生成，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用存储库级说明，以及带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他格式的说明文件，包括 AGENTS.md 和 CLAUDE.md；请参阅[自定义说明支持文档][custom-instructions-support]。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊范围。不过，示例项目展示了一种具有代表性的做法。从整体上看：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供详细说明。
- 使用自然语言，清晰地描述指导，并提供正反示例。

说明文件没有唯一的正确写法，和使用 AI 一样，需要不断试验才能找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护完善的说明文件。本项目提供了针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的说明。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。
>
> 可以在 [awesome-copilot][awesome-copilot] 中查找说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

阅读存储库自带的说明文件：一个核心 `copilot-instructions.md`，以及一组面向不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 指导。这些说明适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，查看针对 Astro、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 决定适用文件，例如匹配 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看为本项目编写单元测试的专门说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部，查看它对其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。较大的说明集可以拆分为较小、可复用的文件，并提供示例供 Copilot 参考。链接路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了编码标准，但尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 创建分支

遵循良好实践，为更改创建分支。

1. 返回上一个练习中的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明*之前*先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的对话。模式和模型选择因练习而异；每一步会在需要时说明。

先在现有说明生效时发送提示，以便观察自定义说明的影响。稍后会更新说明并重新发送相同提示。

1. 关闭之前练习留下的编辑器标签页，避免 Copilot 获取无关上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放置的位置。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以允许 Copilot 修改文件。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)
4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改 helper 文件及其测试。
6. 留意生成的 helper 是带类型的函数，第一个参数接收 `db` 客户端，并返回带类型的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。若出现这种情况也没关系；本练习的重点仍是观察添加说明后行为的一致性提升。

## 添加新的存储库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约位于第 27 行）。这里已经记录了项目编码标准，但尚未包含代码文档规则。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件既可以放在存储库级文件 `.github/copilot-instructions.md` 中以提供全局指导，也可以放在 `*.instructions.md` 文件中以指导特定语言、文件类型或任务。刚才添加的全项目文档标准适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送之前的**相同提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意文件开头现在会出现类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成的函数现在也会包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到下一练习会继续使用的代码中。

## 提交说明并推送分支

说明文件和其他项目资源一样，应通过版本控制管理。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
2. 运行以下命令，确认说明更新和 helper 更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新和 Copilot 为 helper 基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他辅助更改（例如测试），也一并添加。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求，并关联待办事项中的说明文件更新议题。Copilot 可以搜索议题、创建关联 PR 并合并更改。

> [!NOTE]
> 这是首次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。首次使用工具时，VS Code 可能会提示登录 GitHub；按提示授权即可。后续练习会介绍这种连接所用的 Model Context Protocol。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 请 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中包含说明更新和 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 当 Copilot 请求 **Allow** 以代表你执行 GitHub 操作时，审查命令并酌情授权。
6. PR 创建后，请 Copilot 合并 PR 并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 请求授权 GitHub 操作或运行 shell 命令时，先审查命令，再酌情授权。

现在，你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的说明文件，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察现有说明的影响。
- 向 `.github/copilot-instructions.md` 添加新的存储库标准。
- 重新运行相同提示，观察代码如何应用新标准。
- 将说明更新和 helper 基础提交到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中应用这些说明，让 Copilot 跨代码库添加功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义说明][personal-instructions]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
