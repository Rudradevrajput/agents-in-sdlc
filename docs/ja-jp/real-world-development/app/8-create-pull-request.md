---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、命令、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

絞り込み機能の実装、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめてレビューし、プル リクエストを作成します。星評価のプル リクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、命令の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡略化するため、絞り込みと品質確認の一連の作業を 1 つのセッションとブランチで進め、1 つの PR にまとめています。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能 PR 全体と検証の根拠を確認します。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認します。

## シナリオ

絞り込みのワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認権限を開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、GitHub Copilot app でプル リクエストをマージ可能な状態にするための残りの作業を自動化します。有効にすると、アプリのセッションは PR を読み取り、失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要な場合の rebase など、ブロックしている問題に対処します。GitHub が許可するとマージします。バックグラウンドで実行され、アプリを再起動しても継続し、PR がマージされると自動的に停止します。

これまでは自分で **Merge pull request** を選択していました。Agent Merge に任せることもできますが、コードの編集やマージには明示的な承認が必要です。マージ権限を与える前に、許可される操作と作業内容をレビューしてください。

## Agent Merge で PR を管理する

コードの作成とレビューが終わったら、Agent Merge に PR の処理を任せます。

1. エージェント ピッカーで **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択してプロセスを開始します。ドロップダウンの項目を選ぶだけでは開始されません。

Agent Merge が開始され、次の作業を行います。

- タイトルと説明を含むプル リクエストを作成します。
- issue からセッションを開始した場合は、説明文で関連する issue を参照します。
- target ブランチに対して rebase を行い、マージ競合に対処します。
- CI を監視してすべてのチェックが成功することを確認します。
- 他の開発者や Copilot code review からのフィードバックを監視し、コメントを解決するために変更します。
- 必要に応じて、すべて成功した後に PR を自動的にマージします。

すべてのチェックが成功したら、Agent Merge に PR もマージさせます。

5. マージを許可する前に、PR の差分と検証の根拠を確認します。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. エージェントに明示的にマージを依頼します。

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 進行状況を確認し、報告されたブロッカーに対処します。次に進む前に、PR の状態が **Merged** であることを確認してください。マージ権限が有効になったことやエージェントが実行中であることは、マージ完了の確認にはなりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行前にそのブロッカーを解消してください。

## まとめと次のステップ

コードの生成、テストと検証に続き、プル リクエストのプロセスも自動化しました。Agent Merge がマージのライフサイクルを自動化する方法を学び、機能 PR と検証の根拠を確認し、レビュー後にのみマージを許可して PR のマージを確認しました。

次は[既存のキャンバスを使い、トリアージ キャンバスを作成します][next-lesson]。エージェントと一緒に作業を確認、計画、視覚化する方法を学びます。

## リソース

- [GitHub Copilot app で Issue とプル リクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
