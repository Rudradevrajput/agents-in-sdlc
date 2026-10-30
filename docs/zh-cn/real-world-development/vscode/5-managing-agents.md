---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

在智能体模式中使用 GitHub Copilot 时，它会自主探索代码库、提出更改、编辑文件并运行命令。VS Code 中的 Copilot Chat 会实时显示工具调用和文件编辑。你可以查看每项差异、接受或拒绝单独更改，并通过后续提示引导对话，无需离开编辑器。

本练习中你将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话中引导智能体添加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作过程中，设计团队又提出需求：为觉得默认主题太暗的用户添加浅色模式切换。无需从头开始新对话，可以引导现有会话继续完成此工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体采取的每个步骤，包括探索的文件、运行的命令和建议的编辑。每项编辑都会以内嵌差异形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容查看工具调用，包括文件读取、文件编辑和请求运行的终端命令。
3. 对于每项文件编辑：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中智能体修改的所有文件。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你信任的命令。

5. 等待智能体完成高对比度模式，并查看最终更改摘要。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中添加浅色模式切换。智能体会保留先前工作中的上下文，包括已修改的组件、样式方法和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会检查现有更改，并提出浅色模式的新增内容。
3. 像处理高对比度模式一样审查每项编辑，并选择 **Keep** 或 **Undo**。
4. 如果智能体的方向不合适（例如以不希望的方式重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清提示。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以要求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换项，将浅色模式整合进现有设置面板”）或扩展范围（“还要为两个切换项添加键盘快捷方式”）。

## 审查当前更改

提交前快速审查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查更改文件列表，其中应包括 Astro 组件、样式和相关测试。
3. 打开几个已更改文件查看差异，确认应用了自定义智能体中的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并引导了正在进行的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本课中你了解了：

- 如何实时监控智能体工作。
- 如何接受或拒绝单独的文件编辑。
- 如何通过后续提示引导正在进行的会话。

在[下一课][next-lesson]中，你会将无障碍更改发布为拉取请求，并审查本地自定义智能体在实验期间生成的内容。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
