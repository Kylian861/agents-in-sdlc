---
title: "演習 1 - Custom instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、Copilot がそのコンテキストを利用できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。その 1 つが[Instruction file][instruction-files]です。コード生成の指針を伝えます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- *現在の* instruction を使って、フィルター機能の最初のデータ部分 (publishers helper) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 次の演習で Copilot が最新のガイダンスを使えるよう、instruction の更新とフィルター機能の基盤を `main` に commit します。

> [!CAUTION]
> 生成コードが、設定した標準に従わない場合があります。Copilot は確率的に動作します。この演習の目的は、更新後の instruction で振る舞いが変わる**傾向**を確認することであり、出力を 1 文字ずつ一致させることではありません。

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

## ブランチを作成する

ここでは、変更をブランチにまとめて作業します。

1. 前の演習で使った Codespace に戻ります。
2. Codespace で新しいターミナルを開きます。<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## Instruction を更新する*前に* Copilot Chat を使う

Custom instruction の効果を確認するため、まず現在の instruction を使ってプロンプトを送信します。その後、instruction を更新し、同じプロンプトをもう一度送信して違いを確認します。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は、各手順で指定します。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習で開いたエディター タブを閉じます。
2. Copilot が helper の場所を認識できるよう、`src/lib/publishers.ts` を開きます。
3. ファイルを編集できるよう、Chat ビューの agents dropdown で **Agent** を選択します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、helper とテストなどのファイルに変更を適用します。
6. 提案された helper が型付き関数で、最初の引数として `db` client を受け取り、型付きの publisher 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードに TSDoc の doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、明示的に指示しなくても doc comment を追加する場合があります。その場合も問題ありません。重要なのは、instruction を更新した後に一貫性が向上することです。

## リポジトリ全体に適用する標準を追加する

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

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をクリックして、Copilot に対象ファイルを認識させます。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc の doc comment が含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しい標準を指示し、次の演習で使う実際のコードに適用しました。

## Instruction とブランチを commit して push する

Instruction file も他の資産と同じように、ソース管理で管理します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
2. 次のコマンドを実行して、instruction の更新と helper の変更を確認します。

   ```bash
   git status
   ```

3. instruction の更新と helper の基盤、および Copilot が helper 用に変更したすべてのファイルを stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチを push したら、バックログのドキュメント標準に関する issue と関連付けたプル リクエストを作成します。GitHub ツールはすでにプロジェクトに接続されているため、Copilot に issue を探し、PR を作成してマージするよう依頼できます。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートでは GitHub のツールが workspace に接続済みのため、セットアップは不要です。Copilot が初めて GitHub ツールを使うときに、VS Code でサインインを求められる場合があります。画面の案内に従ってください。接続の仕組み (Model Context Protocol) は後の演習で学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. instruction file の更新に関する issue を探し、instruction の変更と publishers helper の基盤を説明する PR を現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 操作の **Allow** を求めたら、コマンドを確認して必要に応じて **Allow** を選択します。
5. PR が作成されたら、次のプロンプトでマージを依頼し、ローカル ブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が GitHub 操作や shell command の **Allow** を求めたら、コマンドを確認して必要に応じて **Allow** を選択します。

GitHub Copilot の支援でプル リクエストを作成し、マージできました。

## まとめと次のステップ

Instruction file から Copilot がコンテキストを得る方法を確認し、Copilot Chat を使って次のことを行いました。

- *既存の* instruction を使ってコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認しました。
- 次の演習で使えるよう、instruction の更新と helper の基盤を `main` に commit しました。

次は、[agent mode][next-lesson]で instruction を使い、コードベース全体にわたる新機能を Copilot に追加してもらいます。

## リソース

- [GitHub Copilot 用の instruction file][instruction-files]
- [Custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向け custom instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
