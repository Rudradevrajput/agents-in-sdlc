---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要完成准备工作。你将创建 Tailspin Toys 存储库副本、启动用于练习的 [Codespace][codespaces]，并确认 GitHub Copilot Chat 已在编辑器中正常运行。

## 设置实验存储库

通过[模板存储库][template-repository]创建自己的副本。新存储库包含实验所需的全部文件。

1. 在新浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 选择 **Use this template**，然后选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加由 GitHub 或 Microsoft 举办的活动，请遵循导师的说明；否则，请在你有权使用 GitHub Copilot 的组织中创建存储库。

   ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下所创建存储库的路径（`organization-or-user-name/repository-name`），稍后会用到。

> [!NOTE]
> 从模板创建存储库时，系统会自动创建一组 GitHub 议题作为待办事项。整个工作坊都会使用这些议题，无需自行创建。

## 创建 Codespace

接下来使用 Codespace 完成练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可在浏览器中编写、运行和调试代码，并提供支持多种编程语言、扩展和工具的完整编辑器。

1. 打开刚创建的存储库。
2. 选择 **Code**。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮以创建新的 Codespace。

   ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

Codespace 创建可能需要几分钟，但仍比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习会继续使用此 Codespace。现在请将它保留在浏览器标签页中。

> [!NOTE]
> 本工作坊可在 Codespace 或本地 [dev container][dev-containers] 中运行。两者都包含顺利完成练习所需的先决条件。若要在本地运行，请在 VS Code 中打开克隆的存储库，并在提示时选择 **Reopen in Container**；VS Code 会构建与 Codespace 相同的开发容器。

[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和智能体模式

要使用 GitHub Copilot Chat 的智能体模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。GitHub Codespace 通常已预装此扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装，也可以在 Visual Studio Code 的扩展视图中搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装扩展后，可能需要使用 GitHub 账户进行身份验证。

1. 返回 Codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入“Hello world”并按 <kbd>Enter</kbd>，以启动 Copilot Chat。
4. 如果尚未完成身份验证，系统会提示登录 GitHub 账户。按提示完成身份验证。

   ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证后，应能看到 Copilot Chat 窗口。

## 总结

恭喜！你已创建实验存储库副本，并开始创建后续编写代码时要用到的 Codespace。

## 下一步

现在开始使用 Copilot。继续学习[练习 1：自定义说明][next-lesson]，向 Copilot 介绍项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
[codespaces]: https://github.com/features/codespaces
