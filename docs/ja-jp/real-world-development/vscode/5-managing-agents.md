---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agent][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、コードベースの探索、変更の提案、ファイルの編集、コマンドの実行を自律的に行います。VS Code の Copilot Chat では、その作業がローカルでリアルタイムに実行され、各ツールの呼び出しやファイルの編集を確認できます。差分をインラインでレビューし、個々の変更を受け入れたり取り消したりできます。また、エディターを離れずに追加のプロンプトを送信し、作業を改善したり広げたりできます。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムで監視します。
- 提案されたファイル編集をレビューして受け入れます。
- セッションの途中で指示を追加し、高コントラスト モードとともにライト モードの切り替えを実装します。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティ custom agent は、高コントラスト モードを実装しています。作業中にデザイン チームから、既定のテーマが暗すぎると感じるユーザー向けにライト モードの切り替えも追加してほしいと依頼がありました。新しい会話を最初から始めるのではなく、既存のセッションに指示を追加して作業を広げます。

## Copilot の進行状況を監視する

Copilot Chat ビューでは、agent が実行する各手順を確認できます。調べているファイル、実行するコマンド、提案する編集が表示されます。ファイル編集は差分と **Keep** / **Undo** の操作とともにチャットに表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. agent の作業中にチャットをスクロールし、ツールの呼び出しを確認します。ファイルの読み取りと編集、実行を求められたターミナル コマンドなどが表示されます。
3. agent がファイル編集を提案すると、差分がインラインで表示されます。
   - **Keep** を選ぶと変更を受け入れます。
   - **Undo** を選ぶと変更を取り消し、agent に再度作業させます。
4. チャット上部の **Files changed** を開くと、このセッションで agent が変更したファイル全体を確認できます。

> [!NOTE]
> ツールの呼び出し (例: ターミナル コマンドの実行) に許可が必要な場合、agent は一時停止し **Allow** を表示します。実行してもよいコマンドだけを承認してください。

5. agent が高コントラスト モードの実装を終えるまで待ちます。最後に、変更内容をまとめたメッセージが表示されます。

## Agent にライト モードの追加を依頼する

高コントラスト モードができたら、同じ会話でライト モードの切り替えも追加します。前の作業を続けるため、agent は編集したコンポーネント、スタイルの方針、local storage の使い方などのコンテキストを保持しています。

1. Copilot Chat の入力欄に、次の追加プロンプトを送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. Agent は既存の変更を調べ、ライト モードの追加を提案します。
3. 高コントラスト モードのときと同じように、新しい編集を確認します。差分ごとに **Keep** または **Undo** を選択します。
4. Agent の方向性が希望と異なる場合 (例: toggle UI のリファクタリングを始めた場合) は、チャット入力欄の横にある **Stop** ボタンで停止し、追加の説明を送ります。

> [!TIP]
> 進行中のセッションに指示を追加する方が、最初から始めるよりも効率的です。追加のプロンプトで修正を依頼できます。たとえば、「CSS を簡潔にし、selector の重複ではなく CSS variable を使う」「3 つ目の toggle を追加せず、既存の設定パネルにライト モードを統合する」「両方の toggle にキーボード ショートカットを追加する」などと指示します。

## 変更されたファイルをレビューする

作業を commit する前に、agent が変更したファイルをひととおり確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイルをすべて確認します。Astro コンポーネント、スタイル、関連テストなどが更新されています。
3. 変更されたファイルをいくつか開き、差分を確認します。ARIA 属性、キーボード ナビゲーション、セマンティック HTML、local storage による設定の保持など、custom agent が指示したアクセシビリティのパターンが反映されていることを確認します。
4. 問題がなければ **Source Control** パネルから変更を stage して commit します。変更は[次のレッスン][next-lesson]で公開します。

## まとめと次のステップ

Copilot Chat の agent mode を使い、進行中のセッションを監視して方向を調整しました。編集を確認して受け入れるか取り消し、コンテキストを失わずに追加のプロンプトで作業を広げました。

このレッスンでは、次のことを行いました。

- Agent の作業をリアルタイムで監視しました。
- 個々のファイル編集を受け入れたり取り消したりしました。
- 追加のプロンプトでセッションの進行中に方向を調整しました。

[次のレッスン][next-lesson]では、アクセシビリティの作業をプル リクエストとして公開し、このラボで custom agent が作成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode の使用][agent-mode]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
