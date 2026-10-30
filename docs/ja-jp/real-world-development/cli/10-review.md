---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 件のプル リクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されている絞り込み issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- 絞り込み issue から始めて Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 要件、テスト カバレッジ、スキルの結果、ブラウザーでの根拠を評価する品質保証 (QA) カスタム エージェントを作成して選択しました。
- 絞り込み機能全体をレビューし、Agent Merge を使って絞り込み PR のマージを許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

ワークショップでは、更新済みの `main` から分岐したブランチで、2 つの PR を作成します。

1. **星評価:** ゲーム カードに既存の `starRating` と未評価状態を表示します。
2. **絞り込みと品質ワークフロー:** 絞り込み機能を実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

絞り込みの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルではユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントは PR 作成前に要件とテスト カバレッジをレビューし、これらの結果をまとめました。

テストは実際の不足を補うものである必要があります。カバレッジに不足がなく、QA で新しいテストが不要と判断される場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響する検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールは、作業結果に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。命令は標準を設定し、スキルは繰り返し実行するタスクをまとめ、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。実装前に方針を考えるには **Plan**、小さな変更を対話しながら進めるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を作るか、なぜ必要か、どのように実装するかを明確に説明すると、結果が大きく変わります。

## さらに学ぶ

主要なワークフローを学びました。Copilot CLI には、ほかにも次のような機能があります。

- `/review` でコード レビュー エージェントに変更を分析させる。
- `/rubber-duck` で問題を説明し、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して調整する。
- `/worktree` で別のタスクを独立させる。
- `/delegate` で Copilot cloud agent にタスクを送る。

## 次のステップ

どのツールも、使い続けることで上達します。本番コード、趣味のコード、長年作ろうと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、他のメンバーからも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。他の環境も試す場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
