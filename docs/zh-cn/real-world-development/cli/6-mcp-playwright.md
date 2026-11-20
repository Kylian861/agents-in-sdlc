---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并通过浏览器体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码只是开发工作的一部分。我们还需要使用数据和外部服务，并允许 Copilot 使用其他自动化工具。MCP 服务器可以扩展 Copilot CLI 的内置功能，为它提供更多工具和服务。

本课将学习如何：

- 了解 Model Context Protocol（MCP）及 Copilot CLI 如何使用它。
- 如果尚未提供 Playwright MCP 服务器，则添加它。
- 让智能体操作浏览器并体验筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，还需要实际与 UI 交互。让 Copilot 像用户一样使用正在开发的网站，可以进一步自动化更改流程，并增强对功能行为的信心。

## 什么是 Model Context Protocol（MCP）？

[Model Context Protocol（MCP）][mcp-blog-post]让 AI 智能体能够与外部工具和服务通信。使用 MCP 后，智能体可以实时获取最新信息，并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当智能体与外部工具、服务之间的桥梁，每个 MCP 服务器都提供一组可供智能体使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：提供基于 Playwright 的浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 维护了一个 [MCP 注册表][mcp-registry]，帮助社区发现并贡献 MCP 服务器。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样谨慎对待 MCP 服务器。使用前先检查源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未提供 Playwright MCP 服务器，请添加它，以便 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 在名称字段中输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 通过 Playwright 体验功能

之前你已手动确认功能行为符合预期。现在使用 Copilot 提供的 Playwright MCP 服务器再次检查。

1. 使用以下提示，要求 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   启动应用，并使用 Playwright MCP 对照议题和计划检查筛选功能。告诉我哪些行为正常、哪些不正常，不要进行任何更改。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 通常 Copilot 会自行判断何时使用 MCP 服务器，因此不必明确要求它使用 MCP。不过，既然已知应使用哪项工具，明确指出也有助于保持结果一致，同时节省一些 token。

2. 观察 Copilot 在浏览器中执行的步骤，确认功能行为。
3. 阅读报告，确保一切按预期运行。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器，然后给出报告。

## 总结与后续步骤

恭喜你使用 Playwright MCP 服务器从 Copilot CLI 操作真实浏览器，体验了新功能。回顾一下，你已经：

- 了解 Model Context Protocol（MCP）及 Copilot CLI 如何使用它。
- 在尚未配置 Playwright MCP 服务器时添加了它。
- 让智能体操作浏览器并体验筛选功能。

接下来，创建一个[自定义 QA 智能体][next-lesson]，将技能与浏览器工具结合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
