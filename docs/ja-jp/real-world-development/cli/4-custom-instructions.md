---
title: "レッスン 4 - Custom instruction で Copilot をガイドする"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合は、そのガイダンスを Copilot が利用できるようにします。[Instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も説明できます。フィルター機能を実装したので、Copilot が参照した instruction を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instruction とパス単位の instruction file がエージェントに渡る仕組みを確認します。
- コーディング標準に従うよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

優れた開発チームと同様に、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。たとえば、次のような内容です。

- コメントでは、コードを繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` にあるエクスポートされた関数では、`db` の注入可能な引数がある場合はそれも含め、目的、パラメーター、戻り値を TSDoc/JSDoc で文書化します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連するコードを変更するときはコメントも更新します。
- 既存の書式設定と lint のガイダンスを維持します。

Instruction file を使うことで、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

## Instruction file

Custom instruction を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得ることができます。使用するコーディング規約やライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体に適用する instruction も、特定のファイル種別向けの instruction も作成できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信されるファイルです。ほとんどのリクエストに関連するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルには、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。

> [!NOTE]
> その他の instruction 形式やサポート状況は、環境によって異なります。特定の形式を利用する前に、[custom instruction のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには、すでにいくつかの instruction file が含まれています。変更の影響を確認するため、まず既存のファイルを調べます。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディング ガイダンスを確認します。この instruction は、このリポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する instruction があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この値で、instruction が適用されるファイルの glob パターンが定義されています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file やプロジェクトのファイルへのリンクを確認します。これにより、大きな instruction を小さな再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存のファイルはよい出発点ですが、追加のルールが必要です。`copilot-instructions.md` を変更して、新しく生成される TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** という見出しを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction を読み込みます。instruction を保存した後、機能のコンテキストを保ったまま会話を終了して再開し、新しいガイダンスを読み込ませます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択画面が表示されたら、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを得る方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの instruction とパス単位の instruction file がエージェントに渡る仕組みを確認しました。
- コーディング標準を守るため instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、[再利用可能な quality-checks skill をカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI に custom instruction を追加する][instruction-files]
- [Custom instruction のサポート][custom-instructions-support]
- [Custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
