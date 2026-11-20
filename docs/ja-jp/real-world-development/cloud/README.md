---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を加え、プル リクエストを作成します。その間、別の作業を続けられます。

各演習では、まず cloud agent が従う custom instruction を追加し、GitHub issue を割り当てて実装を依頼します。続いて custom agent を確認して活用し、agents dashboard からセッションを監視して方向を調整し、最後にエージェントのプル リクエストをレビューして成果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Custom instruction][ex1] | コンテキスト | cloud agent が従う custom instruction を追加します |
| [2. Cloud agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てます |
| [3. Custom agent][ex3] | 専門エージェント | custom agent を確認して使用します |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視し、方向を調整します |
| [5. 反復改善][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次の手順を選びます |

## 前提条件

ワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランを利用していませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コード レビュー、Copilot CLI の機能が含まれているため、どの環境の演習も完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** のすべてのプランで利用できます。Copilot Business または Enterprise の場合は、組織の管理者が有効にする必要があります。

> [!NOTE]
> この環境では MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI 環境](../cli/)または [VS Code 環境](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
