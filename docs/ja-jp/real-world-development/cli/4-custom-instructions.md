---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合、その指針を Copilot が利用できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、その構造も指定できます。フィルター機能を構築したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用しましょう。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパス別の指示がエージェントに適用される仕組みを確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインがあります。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数を注入できる場合は、その引数も記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明します。関連するコードを変更するときは、コメントも最新の状態に保ちます。
- 既存の書式設定と lint の指針を維持します。

指示ファイルを使って、Copilot がこうしたプラクティスに沿ってタスクを実行するための情報を提供します。

## 指示ファイル

カスタム指示は、Copilot がコーディング スタイルや要件を理解するためのコンテキストと希望を提供します。リポジトリ全体や特定のファイルに対して、コーディング規約、ライブラリ、コメントの書き方などを指定できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md`: リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル形式、タスクのガイドラインを記載します。

> [!NOTE]
> ほかの指示形式への対応状況は環境によって異なります。形式に依存する前に、[カスタム指示の対応状況に関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに指示ファイルが含まれています。変更を加える前に、内容を確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/copilot-instructions.md` を開きます。ターミナルではなく、エディターで操作してください。
2. プロジェクトの概要とコーディング ガイドラインを確認します。この指示は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。冒頭の `applyTo` フィールドが、適用対象のファイルを決める glob を指定しています。
5. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分け、参考例にリンクできます。

## チームのガイドラインに合わせて指示ファイルを更新する

既存のファイルは出発点として十分ですが、まだ改善できます。`copilot-instructions.md` に新しく生成する TypeScript の TSDoc に関する指示を追加します。

1. `.github/copilot-instructions.md` を開き、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きに次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新した指示を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルター機能のコンテキストを保持したまま新しい指示を読み込むため、会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替えてから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択画面が表示された場合は、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に次のように依頼し、更新した指示を適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI がコンテキストを読み込む仕組みを確認し、新しい標準を機能に適用しました。具体的には、次の操作を行いました。

- リポジトリの指示とパス別の指示がエージェントに適用される仕組みを確認しました。
- 指示ファイルを更新し、新しい標準を追加しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な品質チェックを実行するために、[quality-checks スキルをカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [効果的なカスタム指示の作成に関するベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
