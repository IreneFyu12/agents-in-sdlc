---
title: "演習 2 - Copilot エージェント モードで新しい機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は必要なファイルを見つけ、変更し、テストを実行し、問題をデバッグし、更新し忘れたファイルを探して、さらに変更を加えます。作業は続きます。

ここで役立つのが Copilot エージェント モードです。

Copilot エージェント モードは、IDE 内でより自律的に作業します。開発者と同様に、既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードを使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けです。エージェント モードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。Cloud agent はチームのペア プログラマーとして、非同期に作業し、issue とプルリクエストを通じてやりとりします。

この演習では、次の操作を行います。

- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する方法を学びます。
- UI とデータ層にまたがる機能を実装します。
- Copilot が生成した変更とテストをレビューしてから、コードベースにマージします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリーと出版社で絞り込めるようにします。前の演習では出版社ヘルパーを追加しました。今度は Copilot エージェント モードを使い、残りのデータ層、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。単一の Astro アプリであり、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、必要なインタラクティブ機能は標準 DOM API を使った小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. 前の演習の続きで Codespace に戻ります。ブランチは `main` になっているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択し、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Web サーバーを実行中の Codespace へ接続するプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明を含むボード ゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、出版社、カテゴリーなどの詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、issue のバックログが自動的に作成されています。GitHub Copilot に内容を表示させます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。エージェント モードは、複数ファイルの変更を自律的に計画して実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェント ピッカーを表示したスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は特定のモデルを推奨していません。作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、学習しやすくすることを目的としています。ただし LLM の動作は確率的なため、ワークショップに記載されたものと異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使うことがありますが、タスクは同様に完了できます。

3. 次のプロンプトを Copilot に送り、バックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者にとって有益だと判断した順に、項目を優先順位付けしていることを確認します。

## 指示ファイルを確認する

コードの生成をエージェントに依頼する前に、作業のコンテキストを与える指示ファイルを確認します。Web サイトに機能を追加する方法の指針がある[ユーザー インターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル形式に関する指示へのリンク。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな内容を管理しやすい単位に分け、例やテンプレートを提示できます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3つの更新が必要です。

- データ層 (`src/lib/`) に残りのフィルター ロジックを追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

コードベースにマージする前に、テストを実行して成功することも必要です。これらの作業を Copilot エージェント モードに依頼します。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェント ピッカーを表示したスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログにある関連 issue の要件に基づいて機能を実装するよう、次のプロンプトを Copilot に送ります。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、目的の機能に関連するファイルを探します。データ層のヘルパー、UI、テストを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって、数分かかる場合があります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。作業中に選択する必要はなく、完了すると変更ごとに **Keep** または **Undo** を選べるようになります。

7. Copilot に促されたら **Continue** を選択してテストを実行します。

   ![テストの実行を確認するダイアログを Copilot Chat ペインに表示したスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot は、タスクが完了し、エラーが見つからなくなるまでコード生成とテストを繰り返します。

   ![Copilot エージェント モードのチャット セッションの完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作を確認し、コードをレビューできたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェント モードを使って Tailspin Toys の Web サイトに機能を追加する方法を学びました。

- エージェント モードで UI とデータ層にまたがる新しい機能を実装しました。
- エージェント モードでプロジェクトを調べ、関連ファイルを特定し、連携して変更できることを確認しました。
- コードベースにマージする前に、エージェント モードが生成した変更とテストをレビューしました。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 - ページ分割を実装する

ゲームの一覧が増えたら、ページ分割が必要になります。この演習で学んだ方法を使って、Copilot にページ分割の実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズを利用者が選択できるようにするか、固定するかを検討します。
- 希望する実装に必要な指示を、プロンプトに含めます。
- Copilot に変更やコンテキストを伝えて、何度かやりとりする場合があります。Copilot を使う際によくある進め方です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
