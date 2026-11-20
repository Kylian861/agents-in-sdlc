---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みのフィルター機能の issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルター機能の issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- Custom instruction でエージェントをガイドし、既存の `quality-checks` skill をカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) custom agent を作成して選び、要件、テスト範囲、skill の結果、ブラウザーの証拠を確認させました。
- フィルター機能の変更全体をレビューし、Agent Merge によるフィルター機能 PR のマージを許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## リリースしたもの

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作り、2 つの PR を進めました。

1. **星評価**: 既存の `starRating` と、評価がない場合の状態をゲーム カードに表示します。
2. **フィルター機能と品質ワークフロー**: フィルター機能を実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして、QA プロファイルと関連テストを追加します。

フィルター機能の計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、この作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザーの確認、MCP 経由で Copilot が行うブラウザー操作など、いくつかの方法でコードを確認しました。`quality-checks` skill はユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA agent は要件とテスト範囲のレビューをこれらの結果に加え、PR を作成する前に確認しました。

追加したテストは、実際に不足しているテスト範囲を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと根拠を確認し、変更後は関連する検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の内容に影響します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。Instruction は標準を設定し、skill は繰り返し実行するタスクを説明し、custom agent は専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに応じて **モードとモデル** を選択します。実装前に方針を検討するには **Plan**、範囲を絞った変更を自分で確認しながら進めるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えると、出力が大きく変わります。

## さらに試す

主要なワークフローを終えました。Copilot CLI には、ほかにも役立つ機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点から考えてもらいます。
- `/fleet` で独立したサブタスクを並行して実行します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。実運用のコード、個人のプロジェクト、ずっと作りたいと思っていた小さなアプリなどで活用してください。チームで学びを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境も体験する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
