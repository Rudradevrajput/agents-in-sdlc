---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が完了すると、人がレビューできるドラフト PR を作成します。手間のかかるタスクを任せることで、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を学びます。

- コード生成環境をカスタマイズする方法
- 安全に操作する方法
- 範囲を明確に定めた issue の重要性
- Copilot に issue を割り当てる方法

## シナリオ

Tailspin Toys は技術的負債を解消しようとしています。最初のサイトを構築した委託先がドキュメントを残しておらず、アプリケーションにはほとんど説明がありません。まず、アプリケーション内のすべてのエクスポート関数に TSDoc コメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示し、利用者が閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度の高い issue が他にあります。

これらは後回しになりがちなタスクの例で、Copilot cloud agent に適しています。非同期で作業を任せている間に別の作業を進め、完了後に結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub issue を Copilot に割り当てます](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。割り当てられると、Copilot は進捗を追跡するドラフト プル リクエストを作成し、環境を準備して、タスクに取り組みます。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プル リクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と Copilot への影響を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、ここで簡単に確認してください。cloud agent に作業を割り当てる前に、このリポジトリに含まれる命令ファイルを読み取り専用で確認しておくと、後で影響を確認できます。

GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターで、命令の適用先が `**/*.test.ts` に設定されていることを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、生成される PR にこれらの命令に基づいた TSDoc コメント、TypeScript 規約、コメント ヘッダーがあるか確認します。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードを作成するには、通常、環境とセットアップ用スクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェア エンジニアのように作業します。

cloud agent は、作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を利用できるようになります。このワークショップでは学習を円滑に進められるよう、あらかじめ設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードが済んだ SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
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

      # ビルドとテストでデータを利用できるよう、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じですが、重要な点がいくつかあります。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot が PR 作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古かったり、まったくなかったりするプロジェクトは少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成に役立つため、cloud agent に依頼するのに適したタスクです。バックグラウンドで必要なドキュメントを作成します。後の演習で結果を確認します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` などのタイトルの新しい PR を開きます。一覧に表示されない場合は、少し待ってからブラウザーを再読み込みします。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot が PR に TODO リストを表示するまで数分かかる場合があります。前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. TODO リストと、Copilot が完了する予定のタスクを確認します。
14. プル リクエストのタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されていない場合は、ブラウザーを再読み込みしてください。

16. ライブ セッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、方針を考え、コードを作成する手順が表示されます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、非同期で作業している間に別の作業へ集中できるようにすることです。もう 1 つのタスクを cloud agent に割り当て、その間にアプリケーションの機能を追加しましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チームのために関連ゲームを追加する作業は短時間で終わるとしても、その時間を別の作業に使えます。cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue ダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

![Copilot が issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 件目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューします。次はカスタム エージェントの作成と使用を学びます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に issue を割り当てて非同期に処理させることができます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに活用できます。

このレッスンでは、コード生成環境のカスタマイズ、安全な操作、範囲の明確な issue の重要性、Copilot への issue 割り当てを学びました。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]とカスタム命令も利用できます。これらは前のモジュールで学びました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
