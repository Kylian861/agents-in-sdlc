---
title: "演習 4 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発ワークフローにおける特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルで agent を定義し、Copilot の作業に役立つ指示、ベスト プラクティス、コーディング パターン、分野固有の知識を提供します。チームの専門知識を再利用可能な agent にまとめることができます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ agent、安全なコーディング プラクティスに従うセキュリティ agent、一貫したテスト パターンを維持するテスト agent などです。

Custom agent は、プロジェクトの `.github/agents` フォルダー内、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter を記述し、その後に agent の動作、専門知識、指示を定義する Markdown プロンプトを続けます。

### Custom agent と agent skill の比較

Custom agent と [agent skill][agent-skills-concept] には共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を指示します。分かりやすく区別するなら、**custom agent は作業者、skill はツール**です。

Custom agent は独自のコンテキスト ウィンドウを持ち、skill や他の agent を調整して作業できます。このラボのアクセシビリティ custom agent は、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プル リクエストのワークフローやテストの実行・管理を行う skill を呼び出すこともできます。

> [!NOTE]
> Custom agent の作成に唯一の正解はありません。AI を使う他の作業と同様に、環境やシナリオに合わせてテストと調整を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。ユーザーからの最近のフィードバックによると、テキストと背景色のコントラストが十分でないため、現在のダーク テーマを読みにくいと感じる人がいます。このアクセシビリティ上の課題に対処するため、デザイン チームはユーザーが切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なので、この機能をすばやく実装します。Custom agent を使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ custom agent を確認します。
- Copilot Chat でアクセシビリティ agent を使い、高コントラスト モードを実装します。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに作成されています。Copilot をどのようにガイドするかを理解するため、内容を確認します。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します (演習 3 のフィルター機能 PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> Custom agent には、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。アクセシビリティの高い Web サイトのコード生成に関する中核的な役割、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題や間違いが説明されています。

## Copilot Chat で custom agent を使う

VS Code では、`.github/agents` に定義した custom agent が Copilot Chat ビュー下部の agents dropdown に表示されます。Custom agent を選択すると、その agent の指示とツールに合わせてチャット セッションを構成できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で指定します。

1. まだ選択されていない場合は、Chat ビューの agents dropdown で **Agent** を選択します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部の agents dropdown を選択します。現在の agent が表示されます。既定では **default** です。
3. 使用可能な agent の一覧から **Accessibility agent** を選択します。
4. アクセシビリティ agent に次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクトのファイルに編集を適用します。各編集はファイル パスと差分とともにチャットに表示されます。
6. 編集が追加されると **Files changed** の表示が更新され、agent が変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリを変更し、作業中に Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、進行中の作業をレビューし、方向を調整します。

## まとめと次のステップ

GitHub Copilot の [custom agent][custom-agents] を確認しました。Custom agent は、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識と標準を再利用可能な agent にまとめ、Copilot の作業をガイドできます。

この演習では、次のことを行いました。

- custom agent の定義方法を確認しました。
- Copilot Chat の agent mode で custom agent を使いました。

次は、同じセッションにライト モードの切り替えを追加しながら、[agent の作業を監視して方向を調整します][next-lesson]。

## リソース

- [Custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [Custom agent の構成][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
