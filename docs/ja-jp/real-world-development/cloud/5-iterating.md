---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的とする複数のタスクを GitHub Copilot と進めてきました。コードにドキュメントを追加し、デザイン チームが改善を検討できる関連ゲーム機能を構築し、高コントラストとライト モードの切り替えなどのアクセシビリティ機能を実装するよう Copilot に依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善します。

### シナリオ

生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに沿って作業します。チームのほかのメンバーにレビューを依頼する前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の変更を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督を受けずに作業するため、安全を確保する目的でいくつかのセキュリティ制約が設けられています。

- Copilot はリポジトリへの読み取りアクセスを持ち、書き込みは作業に使用するブランチに限られます。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されます](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、Copilot cloud agent が作成した最初のプル リクエスト (PR)、コードへのドキュメント追加を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR を開くと、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定どおりの動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. GitHub.com でリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルのプル リクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が含まれます。その場合は、Copilot の作業が終わるまで待ちます。数分かかる場合があるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc の doc comment など、新たに追加されたドキュメントを含むコードを確認します。変更内容は状況によって異なります。演習 2 の最初に確認した custom instruction file による、TSDoc、TypeScript の規約、コメント ヘッダーがあるかを確認します。
6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すメッセージを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. ワークフローは、プル リクエストのチェック セクションにキューとして表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーに依頼するのと同じように、PR やコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する別のセッションを開始します。結果は確率的なため、必ず特定のテキストを使う必要はありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの内容を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc の doc comment を追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを参考に、生成されたドキュメントへの変更を依頼するコメントを追加し、他のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot は更新のために新しいセッションを開始します。
3. **Back to pull request** を選択すると、PR に戻れます。

   ![プル リクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、PR に新しい commit が追加されています。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで作業を繰り返せます。問題がなければ、ドラフト PR をレビュー可能な状態にし、`main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次に、ゲームの詳細ページに関連ゲームを表示する issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示すメッセージを確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. ワークフローは PR のチェック セクションにキューとして表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。これまでと同様に、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を変更したり、この作業は custom instruction の更新**前**に割り当てたため、コメント ヘッダーや TSDoc の doc comment を追加するよう依頼したりできます。前の PR と同様に、**Conversation** タブでコメントを追加すれば、Copilot がそれを受け取って新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ custom agent が実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 で mission control から追加を依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プル リクエストのタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存する local storage の使用。
   - 高コントラスト モードとライト モードの CSS やスタイル変更。
   - アクセシビリティ属性 (ARIA label、キーボード ナビゲーションなど)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すメッセージを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. ワークフローは PR のチェック セクションにキューとして表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ブラウザーで高コントラスト モードとライト モードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ custom agent が、アクセシビリティのベスト プラクティスに沿って Copilot の作業をガイドしたことを確認できました。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## オプションの演習 — 委任を続ける

Cloud agent は、バックログの実作業を割り当て、別の作業に取り組みたいときに便利です。習慣を身に付けるため、リポジトリに issue を追加して Copilot に割り当ててみましょう。次のような例があります。

- ゲームの詳細ページに支援希望者向けのフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent の演習を完了しました。これらのレッスンでは、次のことを行いました。

- **リポジトリに含まれる custom instruction file を確認しました。** 後で cloud agent の出力に及ぼす影響を確認できるようにしました。
- **issue を Copilot cloud agent に割り当てました。** 環境を準備し、計画を立て、非同期に作業する様子を確認しました。
- **アクセシビリティ用の custom agent を作成して使いました。** 高コントラストとライト モードの切り替えを追加しました。
- **Copilot agents ページを mission control として使いました。** アクセシビリティのセッションを監視し、途中で方向を調整しました。
- **Cloud agent のプル リクエストをレビューして改善しました。** `@copilot` に変更を依頼し、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent の演習を完了しました。さらに学びたい場合は、他の環境の演習で今回の内容を補完できます。

- 🖥️ **[VS Code の演習](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を体験します。
- 💻 **[CLI の演習](../../cli/)** — Plan と Autopilot モード、agent skill、QA custom agent、Playwright MCP、Agent Merge、cloud agent につなぐ任意の `/delegate` を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリで、次のような作業も試せます。

- リファクタリングやテスト範囲の改善に関する issue を cloud agent に割り当てる。
- 別の分野に特化した custom agent を作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
