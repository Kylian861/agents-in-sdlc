---
title: "演習 3 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept] を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルで agent を定義し、Copilot の作業に役立つ指示、ベスト プラクティス、コーディング パターン、分野固有の知識を提供します。チームの専門知識を再利用可能な agent にまとめることができます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ agent、安全なコーディング プラクティスに従うセキュリティ agent、一貫したテスト パターンを維持するテスト agent などです。

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

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに作成されています。Copilot をどのようにガイドするかを理解するため、内容を確認します。

Codespace に戻り、アクセシビリティ用の custom agent file を確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> Custom agent には、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。アクセシビリティの高い Web サイトのコード生成に関する中核的な役割、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題や間違いが説明されています。

## issue を作成して割り当てる

Mission control は、環境内のすべての agent を操作するための中心的な場所です。Copilot cloud agent にタスクを割り当てたり、タスクを監視したり、進行中の作業に追加のガイダンスを与えたりできます。まず、Copilot に高コントラスト モードを作成させます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. Custom agent の一覧から **Accessibility agent** を選択します。

   ![Custom agent と Accessibility が選択された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の [custom agent][custom-agents] を確認しました。Custom agent は、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識と標準を再利用可能な agent にまとめ、Copilot の作業をガイドできます。

この演習では、次のことを行いました。

- custom agent の定義方法を確認しました。
- custom agent にタスクを割り当てました。

Copilot が高コントラスト モードを実装する間に、[mission control から agent セッションを監視し、方向を調整します][next-lesson]。

## リソース

- [Custom agent について][custom-agents]
- [組織で custom agent を使う準備][org-custom-agents]
- [Enterprise で custom agent を使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
