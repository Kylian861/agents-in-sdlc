---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。任务完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发进程，让开发人员专注于更重要的工作。

本课将探索 Copilot 云智能体的以下用途：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解清晰定义议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初为网站开发第一版的承包商没有留下完善的文档，事实上，代码完全缺少文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页上显示相关游戏，即同一类别中的其他游戏，让用户可以继续浏览。当前不需要精致的实现，只需要可用于 UX 验收测试的版本。虽然这项工作目前受阻，但还有优先级更高的议题。

这两项任务都可能很快被搁置，因此很适合交给 Copilot 云智能体异步处理。开发人员可以继续处理其他工作，之后再回来审查 Copilot 的成果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。可以通过多种方式让它开始工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。会话进行期间或完成后，都可以查看 Copilot 会话。提议的解决方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 明确指令范围的重要性

GitHub Copilot 并非魔法，不能替代思考。即使看似简单的操作，也可能包含复杂细节，因此请谨慎[界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为 AI 结对编程助手：分阶段工作、不断学习和试验，并根据结果调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果已经完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。将工作分配给 Copilot 云智能体前，先只读查看存储库中的指令文件，稍后便能观察它们的影响。

在 GitHub 网页界面或正在运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给云智能体时，注意观察它创建的拉取请求是否包含 TSDoc 文档注释、TypeScript 规范和注释头。这些内容来自上述指令文件，稍后在审查 PR 时还会再次提到。

## 设置 Copilot 云智能体的开发环境

编写代码通常需要特定环境并运行一些设置脚本，以确保一切就绪。将任务分配给 Copilot 时也是如此，因为它会像软件工程师一样处理任务。

云智能体通过 [GitHub Actions][github-actions] 环境执行工作。可以创建[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 中，让工作流在智能体开始工作前运行，以便安装所需开发工具和依赖项。为方便本实验，这项配置已预先准备好，可作为学习示例。它会为单体 Astro 应用提供 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为执行*这些步骤*所需的最低级别。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果希望在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流相似，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先执行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，以便测试工作流是否成功，而不必等待 Copilot 运行。

## 添加文档

虽然人人都知道文档的重要性，但大多数项目的信息都已过时或根本不存在。这种技术债务常常被忽略，会降低效率，也会增加维护代码库或引入新开发人员的难度。Copilot 擅长生成文档，因此很适合将这项任务交给云智能体。在后续练习中，你会回来审查它生成的成果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    我们组织要求在适当的地方为函数和方法添加 TSDoc 文档注释。遗憾的是，近期更新没有遵循此标准。我们需要更新现有代码，在能帮助说明行为的地方添加文档注释。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求（PR）。其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定处理任务的方案。

13. 查看待办事项列表及 Copilot 准备完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定方案，以及生成代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们可以处理其他工作。接下来利用这一特性，再分配一项任务，然后转而编写代码，为应用添加功能。

## 为游戏详情页添加相关游戏部分

GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一项任务时，它可以处理另一项。为设计团队添加相关游戏部分也许不会花太久，但这段时间仍可用于其他工作。现在将任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    我们希望在每款游戏的详情页上显示相关游戏，帮助用户发现更多游戏。设计团队希望探索 UX 并进行一些验收测试。需求如下：

   - 在 `src/lib/` 中添加一个数据访问帮助函数：给定一款游戏，返回同一类别中的其他游戏，但不包括该游戏本身
   - 在游戏详情页添加“Related games”部分，并使用此帮助函数
   - 处理没有相关游戏的情况
   - 为新帮助函数创建单元测试
   - 创建 PR 前，确保所有测试通过
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题中的第一条评论旁会出现一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它的工作方式与软件工程师类似，因此无需一直监控；等它完成后再审查即可。接下来了解如何创建和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理任务。它可以解决技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

你探索了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解清晰定义议题范围的重要性。
- 将议题分配给 Copilot。

云智能体在后台继续工作时，可以开始创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并能读取前面模块介绍过的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
