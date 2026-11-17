---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云智能体练习前，需要完成准备工作。您将创建 Tailspin Toys 存储库的副本，并启动一个 [codespace][codespaces]，以便编辑指令文件并审查云智能体生成的工作。

## 设置实验室存储库

要为练习中的代码创建存储库副本，请根据[模板存储库][template-repository]创建一个实例。新实例将包含实验所需的全部文件，您将在练习过程中使用它。

1. 在新的浏览器窗口中，转到 [Tailspin Toys 模板][tailspin-template]。
2. 在实验室存储库页面上选择 **Use this template**，创建自己的存储库副本。然后选择 **Create a new repository**。

    ![“Use this template”按钮](../../../_images/ex0-use-template.png)

3. 如果您正在参加由 GitHub 或 Microsoft 举办的讲师指导研讨会，请遵循导师提供的说明。否则，可以在您有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下您创建的存储库路径（**organization-or-user-name/repository-name**），稍后会在实验中用到。

## 创建 codespace

接下来，使用 codespace 完成本实验。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让您直接在浏览器中编写、运行和调试代码。它提供功能完整的 IDE，支持多种编程语言、扩展和工具。

1. 转到刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，不过仍比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能，接下来我们会介绍这些功能。

> [!CAUTION]
> 后续练习还会用到此 codespace。现在先在浏览器标签页中保持打开状态。

> [!NOTE]
> 本研讨会既可在 codespace 中运行，也可在本地的 [dev container][dev-containers] 中运行。这两种方式都能确保环境预装了所需的先决条件。如果希望在本地运行，请在 VS Code 中打开克隆的存储库，并在提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜，您已创建实验室存储库的副本！您还启动了 codespace 的创建流程，稍后将使用它与 Copilot 云智能体一起完成练习。

## 下一步

接下来，为云智能体添加自定义指令。继续学习[练习 1：自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [根据模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
