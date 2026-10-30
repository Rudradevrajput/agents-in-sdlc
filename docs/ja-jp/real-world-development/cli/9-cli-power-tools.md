---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュ コマンドが数多くあります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポート、共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローを終えたので、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキスト ウィンドウの上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュ コマンドを使って内容を確認したり、自分で圧縮したりすることもできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリ ルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に圧縮します。自分でタイミングを選びたい場合は `/compact` を使います。関係のない別タスクに移るときは、`/clear` または `/new` で新しい会話を開始します。

## モデルを選ぶ

モデルによって得意なことが異なり、開発者ごとに好みも異なります。Copilot CLI では、モデルの一覧表示と選択ができます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを使うか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して知見を共有すると、AI ツールの活用方法を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` オプションを確認します。
2. 共有する場合は、ローカルの Markdown ファイルに出力する `/share file` など、適切な方法を選択します。
3. 送信または公開する前に、エクスポートした内容を確認します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡張または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には大きなタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行します。
- `/worktree` は、別タスク用の独立した Git worktree を作成します。
- `/delegate` は、非同期に作業してプル リクエストを作成する場合もある Copilot cloud agent にタスクを送信します。

これらのコマンドは、追加の worktree やリモート作業を作成する可能性があるため任意です。試すときは新しい会話で範囲の明確なタスクを開始し、通常のワークフローで結果をレビューしてください。非同期エージェントを詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使って設定を変更し、セッションを共有し、内部の動作を確認しました。`/context` と `/compact` でコンテキストを確認し、`/model` でモデルを調べ、`/share` によるエクスポートや共有、並行作業、worktree、cloud agent への任意の委任を学びました。

Copilot CLI には、ほかにも多くのスラッシュ コマンドがあります。これまでの内容を[振り返り、次の学習方法を確認しましょう][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
