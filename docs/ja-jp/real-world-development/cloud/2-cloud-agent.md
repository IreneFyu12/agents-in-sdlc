---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織が技術的負債に悩んでいます。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残っている機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。時間のかかるタスクを委任し、開発を加速して、開発者がより大きな課題に集中できるようにします。

Copilot cloud agent では、次の内容を確認します。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- issue の範囲を明確に定義する重要性。
- Copilot への issue の割り当て。

## シナリオ

Tailspin Toys は、いくつかの技術的負債に対応したいと考えています。最初の Web サイトを作成した委託先がドキュメントを整備しておらず、ドキュメントがまったくないことに気付きました。まず、アプリケーションのエクスポート関数すべてに TSDoc ドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーのほかのゲームを表示して、利用者が次のゲームを探せるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。現在はブロッカーですが、ほかに優先度の高い issue があります。

どちらも優先順位が下がりやすいタスクであり、Copilot cloud agent に適しています。Copilot cloud agent が非同期に作業している間、開発者は別の作業に取り組めます。後から戻って作業をレビューし、期待どおりの結果になったか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を設定してタスクを開始します。作業中も完了後もセッションを確認できます。提案された解決方法のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、必要に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を紹介しました。カスタム指示の実習を済ませている場合は、`.github/copilot-instructions.md` の動作を確認したはずです。まだ確認していない場合は、cloud agent に作業を割り当てる前に、このリポジトリにある指示ファイルを読み取り専用で確認しておきましょう。後で影響を見つけやすくなります。

GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript 規約と TSDoc ドキュメント コメントに関する内容を確認します。
- `.github/instructions/unit-tests.instructions.md`: `**/*.test.ts` ファイルに指示を適用する `applyTo` フロントマターを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てると、作成されたプルリクエストで TSDoc コメント、TypeScript 規約、ファイル ヘッダーを確認できます。これらの指示ファイルの影響は、後の演習で PR をレビューするときにもう一度取り上げます。

## Copilot cloud agent の開発環境を設定する

コードの作成では、通常、適切な環境とセットアップ スクリプトが必要です。タスクを Copilot に割り当てる場合も同様です。Copilot はソフトウェア エンジニアのように作業します。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を用意できます。ラボを円滑に進めるため、このワークフローは事前に設定されています。Node.js、プロジェクトの依存関係、E2E テスト用のブラウザー バイナリ、単一 Astro アプリ用にマイグレーションとシードを適用した SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な範囲で最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
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

      # ビルドとテストに必要なデータを用意するため、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあり、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、ワークフローが正常に動作するかをテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、まったく存在しなかったりするプロジェクトは少なくありません。こうした技術的負債が放置されると、生産性が低下し、コードベースの保守や新しい開発者の受け入れが難しくなります。Copilot はドキュメントの作成を得意としているため、cloud agent に割り当てるのに適した issue です。Copilot がバックグラウンドで必要なドキュメントを作成します。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの、新しく作成されたプルリクエスト (PR) を開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が作成した TODO リストが表示されます。

> [!NOTE]
> Copilot が PR に TODO リストを表示するまで数分かかる場合があります。前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題への最適な取り組み方を判断しているためです。

13. リストを確認し、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されるよう、ページを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot が問題に取り組む様子を確認します。コードを調べて状態を理解し、方針を考え、コードを作成する手順が表示されます。

完了まで数分かかる場合があります。Copilot cloud agent の主な目的は、非同期にタスクを実行し、その間にほかの作業を進められるようにすることです。この機能を活用するため、もう1つタスクを cloud agent に割り当ててから、アプリへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の1つは作業を分けられることです。デザイン チーム向けに関連ゲームの表示を追加する作業は短時間で終わるかもしれませんが、その時間を別の作業に充てることもできます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. 空のテンプレートを使うため **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、最初の issue へのコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![目の絵文字で issue の作業中であることを示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。Copilot に issue を割り当てて、非同期に作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

この演習では、次の内容を確認しました。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- issue の範囲を明確に定義する重要性。
- Copilot への issue の割り当て。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
