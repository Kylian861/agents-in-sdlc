---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザー体験の改善と機能追加を目的とする 3 つの別々のタスクを Copilot cloud agent に依頼しました。Cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

Cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com の agents ページ][agents-page]もその 1 つです。この mission control では、オープンなプル リクエスト (PR) があるすべての agent タスクを確認できます。実行された操作を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

高コントラスト モードの作成を agent に依頼した後、チームはライト モードも追加するとよいと気付きました。サイトのスタイル更新と切り替え機能をすでに実装しているため、一緒に追加するのが自然です。高コントラスト モードに加えてライト モードも実装するよう、agent の作業を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左側のペインにあるタスク一覧を確認します。割り当てたタスクとして、コードのドキュメント追加、ゲームを変更する API の生成、Web サイトへの高コントラスト モード追加などが表示されます。
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。リポジトリの checkout、作業環境の作成、MCP サーバーのセットアップ、タスクを完了するための各手順などが含まれます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプル リクエスト (PR) ペインも確認します。ここでは、PR と変更されたファイルを確認し、進行状況を監視できます。

## Cloud agent に追加の指示を送る

実行中のタスクを確認できたら、高コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. 高コントラスト モードを追加するセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

   ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、agent が作業を開始したことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![agents ページの cloud agent タスクで、作業中のセッションに指示するダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業計画に反映する様子を確認します。

## Copilot の作業を続ける

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい依頼が作業に反映されます。

作業には数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視し、案内する中心的な場所である Copilot agents ページを確認しました。この mission control では、進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、進行中のタスクの範囲やガイダンスを変更できます。

このレッスンでは、次のことを行いました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成したプル リクエストを[反復的に改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [Custom agent について][custom-agents]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
