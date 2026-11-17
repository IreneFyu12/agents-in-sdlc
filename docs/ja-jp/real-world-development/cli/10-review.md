---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みのフィルター issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の issue から始め、Plan モードで方針を決め、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を試しました。
- QA カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの確認結果を評価しました。
- フィルター機能の変更全体をレビューし、フィルター機能の PR で Agent Merge を承認しました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使えるスラッシュ コマンドを確認しました。

## リリースしたもの

ワークショップでは、更新済みの `main` からそれぞれ独自のブランチを作り、2つの PR を作成します。

1. **星評価**: 既存の `starRating` と未評価状態をゲーム カードに表示します。
2. **フィルター機能と品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの変更を1つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルは、単体テスト、lint、型チェックを実行し、更新した形式で結果を報告しました。QA はこれらの結果に要件とテスト カバレッジのレビューを加えて、PR の前に確認しました。

追加するテストは、実際に不足しているカバレッジを補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを承認する前に、コードと検証の証拠をレビューしてください。変更があった場合は、影響する証拠を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果をレビューしてください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は構築前の検討、**Interactive** は範囲を絞った変更への継続的な関与、**Autopilot** は明確に範囲を定めたタスクに適しています。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

コンテキストは、作業環境と同じくらい重要です。構築するもの、その理由、方法を明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローは完了しました。Copilot CLI で試せる機能をいくつか紹介します。

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に実行します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送信します。

## 次のステップ

ツールの使い方を上達させるには、実際に使い続けるのが一番です。本番コード、個人のコード、ずっと作りたかった小さなアプリなどで活用してください。チームと学んだことを共有し、メンバーからも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張するには、省略可能な[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比べるには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または[Copilot cloud agent ワークショップ][cloud]を確認します。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
