---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot cloud agent 练习前，需要完成准备工作。你将创建 Tailspin Toys 存储库副本并启动 [Codespace][codespaces]，用于编辑说明文件和审查 cloud agent 的工作。

## 设置实验存储库

通过[模板存储库][template-repository]创建自己的副本。新存储库包含实验所需的全部文件。

1. 在新浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 选择 **Use this template**，然后选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加由 GitHub 或 Microsoft 举办的活动，请遵循导师的说明；否则，请在你有权使用 GitHub Copilot 的组织中创建存储库。

   ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)
4. 记下所创建存储库的路径（`organization-or-user-name/repository-name`），稍后会用到。

## 创建 Codespace

接下来使用 Codespace 完成练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可在浏览器中编写、运行和调试代码，并提供支持多种编程语言、扩展和工具的完整 IDE。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 创建新的 Codespace。

   ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

Codespace 创建可能需要几分钟，但比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习会继续使用此 Codespace。现在请将它保留在浏览器标签页中。

> [!NOTE]
> 本工作坊可以在 Codespace 或本地 [dev container][dev-containers] 中运行，两者都包含所需先决条件。若要在本地运行，请在 VS Code 中打开克隆的存储库，并在提示时选择 **Reopen in Container**；VS Code 会构建与 Codespace 相同的开发容器。

## 总结

恭喜！你已创建实验存储库副本，并开始创建后续与 Copilot cloud agent 协作时要用到的 Codespace。

## 下一步

接下来添加 cloud agent 会遵循的自定义说明。继续学习[练习 1：自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
