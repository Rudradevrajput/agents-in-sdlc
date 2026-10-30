---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "絞り込み、命令、スキル更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能の実装、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめてレビューし、プル リクエストを作成します。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、命令の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは、絞り込みと品質確認の一連の作業を 1 つの会話とブランチで進め、1 つの PR にまとめています。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証の根拠を確認します。
- 絞り込み PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込みのワークフローで Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認権限を開発者が保持したまま、PR の残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プル リクエストをマージ可能な状態にするための残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要な場合の rebase など) に対処し、リポジトリが許可していれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せる場合も、有効にする前に変更をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

作成したコードをまとめてレビューし、PR を作成して、残りの処理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は理由を確認し、リポジトリで許可されていればレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証に続き、プル リクエストのプロセスも自動化しました。Agent Merge とマージ ライフサイクルの自動化方法を学び、機能全体と検証の根拠を確認し、絞り込み PR を作成して、レビュー後に Agent Merge を有効にしました。

次はコンテキスト、モデル、共有、任意の cloud agent への委任に役立つ [Copilot CLI のスラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
