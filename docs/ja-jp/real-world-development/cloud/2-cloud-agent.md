---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom instruction][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、時間がなくバックログに残った機能要望などがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せることで、開発プロセスを速め、開発者がより大きな課題に集中できるようにします。

Copilot cloud agent では、次のことを学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確にした issue の重要性を理解する。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを開発した委託先がドキュメントを適切に整備しておらず、実際にはほとんどありません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc の doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、閲覧を続けられるようにします。現時点では完成度の高い実装ではなく、UX の受け入れテストに使えるものを求めています。この作業はブロッカーですが、さらに優先度の高い issue がほかにあります。

これらは優先度が下がりやすいタスクの例で、Copilot cloud agent に適しています。Copilot cloud agent に非同期で作業を依頼している間、開発者は別の作業に取り組み、完了後に戻って成果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などがあります。割り当てると、Copilot は進行状況を追跡するドラフト プル リクエストを作成し、環境を準備して、タスクに取り組み始めます。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プル リクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単そうな作業にも複雑な部分があるため、[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

これまでの演習では、custom instruction と Copilot への指示方法を学びました。まだ `.github/copilot-instructions.md` を使っていない場合は、この機会に確認してください。Cloud agent に作業を割り当てる前に、このリポジトリに含まれる instruction file を読み取り専用で確認しておくと、後でその影響を見つけやすくなります。

リポジトリの GitHub Web UI または起動中の Codespace で、次のファイルを確認します。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc の doc comment の要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この instruction は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、生成された PR で TSDoc の doc comment、TypeScript の規約、コメント ヘッダーを確認します。後の演習で PR をレビューするときにも、これらの点を確認します。

## Copilot cloud agent の開発環境をセットアップする

コードの作成には、環境を整え、必要な開発ツールや依存関係をインストールする作業が必要です。Copilot にタスクを割り当てる場合も同様です。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップ ワークフロー][setup-workflow]を作成すると、必要な開発ツールや依存関係を利用できるようになります。このラボの流れを円滑にして学習機会を提供するため、事前に設定されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー、移行と seed が済んだ SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub Actions ワークフローと同様ですが、次の点を確認してください。

- `copilot-setup-steps` という 1 つの job が含まれています。この job は、Copilot がプル リクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が実行するのを待たずに、正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、ドキュメントが不足していたりします。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成に役立つため、cloud agent に依頼するのに適したタスクです。Copilot はバックグラウンドで作業し、必要なドキュメントを生成します。後の演習で作業結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot に issue を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` などのタイトルの新しいプル リクエスト (PR) を開きます。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo list を作成したことを確認できます。

> [!NOTE]
> Copilot の todo list が PR に表示されるまで数分かかる場合があります。Copilot は環境を作成し (前述のワークフローを実行し)、プロジェクトを分析して、問題に取り組む方法を検討しています。

13. todo list と実行する作業を確認します。
14. プル リクエストのタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション ビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、適切な計画を考え、コードを作成する手順を確認できます。

完了まで数分かかる場合があります。Copilot cloud agent の主な目的は、タスクを非同期に実行し、その間にほかの作業を進められるようにすることです。この機能を使い、別のタスクを cloud agent に割り当てた後、アプリに機能を追加するコード作成に取り組みます。

## ゲームの詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チーム向けに関連ゲームのセクションを追加する作業は短時間で終わるかもしれませんが、その時間を別の作業に使えます。Copilot cloud agent に依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot に issue を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が issue の最初のコメントに表示されます。

![issue に Copilot が目の絵文字で作業中と示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように作業するため、常に監視する必要はありません。完了後にレビューします。次は custom agent の作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を確認しました。Cloud agent に issue を割り当てると、非同期に作業を実行できます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに利用できます。

このレッスンでは、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に実行する方法を確認しました。
- 範囲を明確にした issue の重要性を学びました。
- issue を Copilot に割り当てました。

Cloud agent がバックグラウンドで作業している間に、custom agent の作成と利用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認した custom instruction も適用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agent →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
