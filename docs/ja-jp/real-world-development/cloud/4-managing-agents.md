---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、ユーザー体験を改善して機能を追加する3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[agents ページ][agents-page]など、さまざまなツールで管理できます。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を追加したりできます。

このレッスンでは、次の操作を行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードの作成を依頼した後、チームはライト モードも追加するよい機会だと気付きました。サイトのスタイル更新と切り替え機能を追加する作業をすでに進めているため、ライト モードも含めるのが自然です。エージェントがハイ コントラストに加えてライト モードも実装するよう、作業の方向を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方に表示されるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを更新する API を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを1つ選択し、Copilot が実行した操作を確認します。次のような手順が含まれる場合があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot 用の環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するための各種操作を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される PR ペインも確認します。PR と変更されたファイルを表示し、作業をさらに監視できます。

## Cloud agent に追加の指示を与える

実行中のタスクを確認したので、エージェントがハイ コントラスト モードを作成している間に、ライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選択します。エージェントが付けたタイトルや作業状況によって、表示は異なる場合があります。

   ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始するまで数分間セッションを確認します。以下のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Agents ページで、作業中の Copilot セッションに指示を送るダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加する様子を確認します。

## Copilot に作業を任せる

更新したタスクの作業が再開されます。メッセージを送信した時点で作業中だった手順が完了すると、新しい依頼が作業に組み込まれます。

前回と同様、完了まで数分かかります。その間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視して導く中心的な場所である Copilot agents ページを確認しました。mission control では、実行中や完了済みのタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を送れます。

このレッスンでは、次の操作を行いました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能の作業を進めている間に、[Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
