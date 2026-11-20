---
title: "レッスン 3 - Agent mode: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で issue に基づくフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。しかし、より大きな変更には、さらに確実なプロセスが必要です。GitHub Copilot CLI は組織の既存の作業方法に沿って動作するよう設計されており、適切な方法で適切なものを構築できます。このレッスンから、エージェントを使った一般的な開発プロセスに沿って複数の作業を進めます。issue から新機能を実装し、コードの妥当性と機能の動作を確認して、最終的にプロジェクトへマージします。

> [!NOTE]
> この機能のワークフローでは、同じ会話とブランチを引き続き使います。通常は、作業するファイルの種類ごとに異なるブランチや PR を使用しますが、ここでは中核となる概念に集中するため簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送したブラウザーで機能を手動で検証します。

この機能の作業を続ける中で、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加し、QA agent を作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリやパブリッシャーでゲームを絞り込む必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意しておく必要があります。Plan モードで方針を決め、その後、範囲を定めた実装を Autopilot に許可します。

## 背景

AI コーディング エージェントを開発ワークフローに導入しても、基本は変わりません。むしろ、基本がこれまで以上に重要になります。多くの開発者は、次のような流れで作業します。

1. 実装する内容を記述した issue から始めます。
2. 実装計画を作成します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードのレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、多くの場合は上記の流れを調整したものです。

この標準的な方法に従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューのプロセスを通るようにできます。

## 会話モード

**会話モード**によって、エージェントにどの程度の自律性を与えるかを制御できます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントが自律的に作業します。コードを作成し、テストを実行して、タスクが完了するまで反復します。

Plan モードで開始して計画をレビューし、その後 Autopilot で実装します。

## issue から開始する

フィルター機能の作業を始める前に、Codespace とリポジトリ、ターミナルの準備をします。

1. Codespace に戻ります。停止している場合は、再起動してから続行します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーを使い、**Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の issue やプル リクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てることで、Copilot に実装を依頼する前に、機能の実装方法やタスクの進め方を決められます。複雑な作業には、時間をかけて計画を立てることをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot が計画の作成を始めます。プロジェクトを調べて、最適な方法を検討します。

3. フィルターの動作について質問された場合は、希望に応じて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上から下まで確認します。
6. 決定した内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を依頼します。

Autopilot では、Copilot がタスクを完了したと判断するまで問題に取り組み続けます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンにある同等の選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って作業し、コードを作成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画を確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と見なす前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージする前にレビューする必要があります。コードをレビューし、サイトを起動して動作も確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** や **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. 変更結果を issue と計画中に決めた内容と照らし合わせます。
4. コードのレビューが終わったら、<kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題がなくても、実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces で転送したポートを使ってサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリとパブリッシャーのフィルター、およびその組み合わせを試します。
4. リセットと結果が空の場合の動作が、issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能の構築とレビューを行いました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot の会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送したブラウザーで機能を手動で検証しました。

次は、[custom instruction を使って][next-lesson]、コードが文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
