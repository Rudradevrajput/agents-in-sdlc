---
title: "练习 4 - 使用自定义说明指导 Copilot"
description: "了解存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相关指导。[说明文件][instruction-files]不仅描述想要*什么*代码，也会说明代码应当*如何*组织。筛选功能已经完成，现在将了解 Copilot 使用的说明、添加文档标准并将其应用到代码中。

本课中你将：

- 了解存储库说明和按路径限定范围的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践准则和要求：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值，包括适用时可注入的 `db` 参数。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码变更时，注释也应保持更新。
- 保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 按照这些实践完成任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以指定编码约定、库以及注释风格，既可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为任务提供上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：单个存储库级说明文件，会随该存储库的**每个**请求发送给 Copilot。应在此文件中记录适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。使用特定格式前，请参阅[自定义说明支持参考][custom-instructions-support]。

## 浏览本项目中的自定义说明文件

初始项目已包含一组说明文件。先查看现有内容，再修改文件并观察影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。它们适用于此存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件，包括针对 Astro、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它指定哪些文件适用这些说明。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它对其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分成较小、可复用的文件，并提供示例供 Copilot 参考。

## 更新说明以符合团队指导

现有文件已经提供了良好起点，但仍有缺漏。现在修改核心 `copilot-instructions.md`，确保新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约位于第 35 行。
2. 在该部分最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 应用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何读取项目说明，并将新的标准应用于功能代码。本课中你：

- 了解存储库说明和按路径限定范围的说明如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
