---
title: "レッスン 7 - QA agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせた QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill を使って自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次に、明確な QA プロセスを定義した custom agent に、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA agent を選び、フィルター機能を issue と照らし合わせて評価します。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫した方法でレビューしたいと考えています。Custom agent を使うと、QA プロセスを調整し、再利用可能なレポートを作成できます。

## Custom agent とは

Custom agent は、Markdown プロファイルで定義する Copilot の専門的な構成です。プロファイルには、エージェントの目的、instruction、使用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリ instruction はチームの標準を説明します。`quality-checks` skill は繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー操作ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、それらをどう使うかを Copilot に指示します。既存の機能を置き換えるものではなく、別の会話を必要とするものでもありません。

## QA プロファイルを作成する

機能の PR を作成する前に、Copilot に再利用可能な QA プロファイルを作成してもらいます。プロファイルには、QA で実行するチェックと、その作業上の制約を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しい custom agent の作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の手順と制約を取り込んだことを確認します。これにより、検証だけを行いたいときに、範囲が不十分または広すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は省略可能ですが、設定するとエージェントを分かりやすい名前で表示できます。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの instruction に従い、`quality-checks` skill を実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードの変更前に確認を求め、変更を commit したりプル リクエストを開いたりしないことを確認します。
5. これらの役割や制約が不足している場合は、続行する前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトの agent を読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開して QA を選択すると、issue と計画の内容を引き継いで作業できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能な agent の一覧に表示されない場合があります。次のコマンドで有効にできます。

2. 次のプロンプトで、QA agent に機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を始めます。
4. 作業が完了したら、レポートを読みます。

## まとめと次のステップ

ワークフローに再利用可能な専門的な役割を追加し、その成果を確認しました。このレッスンでは、次のことを行いました。

- custom agent が instruction、skill、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA agent を選び、フィルター機能を issue と照らし合わせて評価しました。

機能、skill の更新、QA プロファイル、テスト、検証レポートの準備ができました。次は、これらを[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用 custom agent を作成する][create-agents]
- [Custom agent の構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
