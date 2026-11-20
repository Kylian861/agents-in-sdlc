---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体处理了三项任务，以改善用户体验并添加功能。云智能体可以异步、自主地工作，但仍需要监控这些任务。

有许多工具可用于管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有带有未关闭拉取请求（PR）的智能体任务，了解它们执行的操作，甚至可以引导正在进行的会话。

本课将学习如何：

- 浏览智能体页面并监控云智能体任务。
- 在会话进行期间引导智能体，要求它添加其他功能。

## 场景

将创建高对比度模式的任务分配给智能体后，团队意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，将浅色模式也纳入其中很合理。现在引导智能体，让它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应包含分配给 Copilot 的任务，例如：
   - 更新代码库文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各种步骤。

> [!NOTE]
> 具体列出的步骤取决于 Copilot 当前的工作状态和执行方案。

4. 还要查看右侧显示的拉取请求（PR）面板。通过此面板可以查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

现在已了解当前任务，请求 Copilot 在实现高对比度模式时，也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。看到类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    添加高对比度模式时，也请添加浅色模式。浅色模式也应有一个开关，供用户选择所需的显示模式。
    ```

    ![Copilot 智能体页面中的任务截图，突出显示了“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。它会先完成发送消息时正在执行的步骤，再将新请求加入工作流程。

这同样需要几分钟。此时可以暂停一下，回顾迄今为止学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过此控制中心，可以跟踪所有正在进行和已完成的任务、审查已执行的工作，甚至可以重定向正在运行的任务以调整范围或补充指导。

你探索了以下概念：

- 使用控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求智能体添加其他功能。

Copilot 正在完成无障碍功能。接下来，继续[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
