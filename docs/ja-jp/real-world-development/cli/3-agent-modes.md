---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue から絞り込み機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。より大きな変更には、より確かなプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できます。このレッスンから数回にわたり、issue から機能を作成し、コードの妥当性や動作を確認し、プロジェクトにマージする一般的なエージェント主導の開発プロセスを実践します。

> [!NOTE]
> 機能の作業では、同じ会話とブランチを使います。通常、ファイル種類ごとにブランチや PR を分けますが、ここでは中心的な概念に集中するため簡略化しています。

このレッスンでは、次の操作を行います。

- GitHub issue から Copilot の会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この後のレッスンでは、リポジトリ命令の更新、既存の `quality-checks` スキルのカスタマイズ、MCP による検証、QA エージェントの作成、機能 PR の作成を行います。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がゲームをカテゴリや発行元で絞り込めるようにします。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意すべき詳細があります。Plan モードで決定し、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。一般的な開発フローは次のとおりです。

1. 実装内容を記載した issue から始めます。
2. 実装計画を作成します。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、ほとんどの開発フローはこの流れに沿っています。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビュー プロセスを通るようにします。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルは編集できません。
- **Autopilot**: エージェントがコードの作成、テストの実行、反復を自律的に行い、タスクを完了します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

絞り込み機能に取り組む前に、Codespace とリポジトリの準備を確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、絞り込み作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の issue や PR をコンテキストに追加できます。

## 絞り込み機能を計画する

計画を立てると、Copilot に実装を任せる前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、まず計画を作ることをお勧めします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、実装方法を計画します。

3. 絞り込みの仕様について質問された場合は、希望に応じて回答します。唯一の正解はありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して全体を表示します。
5. 計画を読み、内容を確認します。
6. 決定した内容と異なる箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認したら、Autopilot で実装します。Autopilot はタスクが完了したと判断するまで反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様の名前の項目を選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると報告された場合は、セットアップの問題を解決してからチェック結果を確認します。

## 実装をレビューして検証する

生成されたコードは、他のコードと同様にマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替え、**Plan** または **Autopilot** と表示されていないことを確認します。
2. `/diff` を入力し、絞り込みの実装とテストを確認します。
3. 結果を issue と計画時の決定に照らし合わせます。
4. コードのレビューが完了したら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

Codespaces で転送されたポートを使ってアプリを起動し、サイトを確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決めたカテゴリや発行元の絞り込みと、それらの組み合わせを試します。
4. リセットと結果が空の場合の動作が、issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成、レビューしました。GitHub issue から会話を開始し、Plan モードで要件を決め、Autopilot モードで実装し、コードをレビューしてブラウザーで手動検証しました。

次は[カスタム命令を使って][next-lesson]、コードが文書化された開発手法に沿うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
