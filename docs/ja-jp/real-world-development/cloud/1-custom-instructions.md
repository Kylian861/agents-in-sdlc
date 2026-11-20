---
title: "演習 1 - Custom instruction (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、Copilot がそのコンテキストを利用できるようにします。[Instruction file][instruction-files] でガイダンスを伝えると、Copilot は実行する内容だけでなく、その方法も理解できます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の演習とは異なり、ここでは「変更前と変更後」のプロンプトを実行しません。Copilot cloud agent は GitHub issue に対して非同期で作業するため、リアルタイムに並べて効果を示すのは難しいためです。この環境の後半で cloud agent が作成するプル リクエストをレビューするときに、instruction file の効果を確認できます。

## Instruction file

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには、必ずユニット テストを用意します。
- UI はダーク モードで、モダンな印象にします。
- コード内のドキュメントには TSDoc の doc comment を使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

Instruction file を使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に伝えます。

### Custom instruction

Custom instruction を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得ることができます。使用するコーディング規約やライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する instruction も、特定のファイル種別向けの instruction も作成できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の instruction file です。使用する技術スタック、開発中のものの概要、ベスト プラクティス、全般的なガイダンスなど、プロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルには、特定のタスクやファイル種別向けのガイドラインを記載します。TypeScript や Astro などの言語に関するガイドラインや、UI コンポーネントの作成、ユニット テストの追加といったタスクのガイダンスを記載できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat のコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コードを生成するときにリポジトリ レベルの instruction と、`applyTo` frontmatter を持つ `*.instructions.md` file の両方を使用します。
>
> Copilot は、AGENTS.md や CLAUDE.md など、[ほかの規格に基づく instruction file][custom-instructions-support]もサポートしています。

### Instruction file を管理するためのベスト プラクティス

Instruction file の作成に関する詳細な説明は、このワークショップの範囲外です。サンプル プロジェクトには、代表的な方法を示すファイルが用意されています。基本的には、次のようにします。

- `copilot-instructions.md` には、開発中のプロジェクトの説明、構成、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、特定のファイル種別 (ユニット テスト、Astro コンポーネント、データ レイヤー) やタスク向けのガイダンスを記載します。
- 自然な言葉で、明確なガイダンスを書きます。コードの良い例と避けるべき例を示します。

Instruction file に唯一の正解があるわけではなく、AI の使い方にも唯一の正解はありません。試しながら、プロジェクトに最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file のセットを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot は instruction file の作成も支援できます。各環境での利用方法は異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。使用する環境に関係する場面で、このワークショップのレッスンが説明します。
>
> テンプレートや出発点を探している場合は、instruction file、custom agent、その他のリソースを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開き、内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** のガイダンスも確認してください。これらの instruction は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどに関する instruction を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、repo root からの相対 glob を使って、適用対象を指定します。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトのユニット テスト作成に関する instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` など、他の instruction file やプロジェクト内のファイルへのリンクに注目してください。instruction を小さな再利用可能なファイルに分け、Copilot が参考にできる例を示せます。そこにあるパスは repo root ではなく、この instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

この演習で行う編集は、コード内のドキュメントを TSDoc の doc comment とファイル レベルのコメント ヘッダーで記述するルールを追加することです。Cloud agent は、後の演習で割り当てる issue に取り組む際、このルールを参照します。

編集の前にブランチを作成します。Cloud agent は issue の対象ブランチにある instruction を読み込みますが、この演習では変更をきちんと commit します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を伝えるファイルです。コーディング標準を記述し、コード提案の質を高めます。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング標準は記載されていますが、コード内ドキュメントのルールがありません。
3. 既存の標準のすぐ下に、次の行を追加し、ファイル コメント ヘッダーと TSDoc の doc comment を指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> Instruction file は、全体的なガイダンスを記載するリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回追加した doc comment のルールのように、プロジェクト全体に適用する標準にはリポジトリ レベルのファイルが適しています。

## Instruction の変更を commit、push、マージする

Cloud agent は、issue の対象ブランチにある instruction file を読み込みます。次の演習で Copilot に issue を割り当てるとき、Copilot は `main` からブランチを作成します。したがって、cloud agent が instruction の変更を利用できるように、変更を `main` にマージする必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを開き、マージします。次の演習で issue を割り当てたとき、cloud agent がこれらの instruction を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを作成せず `main` に commit してもかまいません。実際のプロジェクトに近い手順を練習するため、ここではブランチを使います。

## まとめと次のステップ

このプロジェクトの instruction file から Copilot がコンテキストを取得する方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。後の演習で、cloud agent が作成するプル リクエストにこの標準が反映されます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [Custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向け custom instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
