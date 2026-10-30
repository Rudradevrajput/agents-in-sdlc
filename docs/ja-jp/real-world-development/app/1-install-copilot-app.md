---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップ アプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。複数のエージェントをそれぞれ独立したワークスペースで並列に指示し、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js をインストールし、プロジェクトのコピーを用意できたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次の操作を行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートで用意されたバックログを含め、ワークスペースを確認します。
- クイックチャットを試して、アプリについて質問します。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app を使うと、Issue の選択、エージェントの実行、変更のレビュー、プル リクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、またはいずれかの有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise では、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディング ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データ所在地を設定した GitHub Enterprise Cloud を使う場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリを選択する場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリ選択をスキップし、オンボーディング後に追加することもできます。
6. テーマを選択するよう求められたら選び、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを探します。すでに表示されている場合は、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を入力します。
9. リポジトリがサイドバーに表示されていることを確認します。このレイアウトでは、`tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にまとめられ、それぞれのリポジトリの下に個別のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには次の領域があります。

- **New** — ここから Copilot との新しいチャット セッションを開始できます。
- **Pull requests** — アプリの GitHub ネイティブ統合で表示されるプル リクエストです。アプリを離れずに参照、絞り込み、CI 状態の確認、レビューを行えます。
- **Issues** — 同じように表示される Issue です。参照や絞り込みのほか、Issue からセッションを開始できます。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェント タスクです。やることリストの管理、定期的なプロジェクト保守、その他の手間のかかる作業を任せるのに役立ちます。振り返りでは次のステップとして紹介しますが、ワークショップ内の演習には含まれません。
- **Customize** — MCP サーバー、プラグイン、スキルなどを使って Copilot app に機能を追加します。Playwright MCP の設定で使用します。
- **Chats** — ブランチや専用ワークスペースが不要な質問やブレインストーミング向けの軽い会話です。最初のチャットを開始すると表示されます。表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。
- **Sessions** — リポジトリ フォルダーと、その中のエージェント セッションをまとめます。`tailspin-toys` などのリポジトリ名を選択するとリポジトリ ビューが開き、その下のセッションを選択すると会話に戻ります。セッションごとに独立したワークスペースが用意されるため、変更を競合させずに複数のセッションを同時に実行できます。星評価を追加するときに最初のセッションを開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問してください。操作方法がわからない場合や、何かが可能かどうか知りたい場合に案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどありません。Tailspin Toys も例外ではありません。テンプレートを作成したときに用意されたバックログを確認します。

1. サイドバーの **Sessions** で、個別のセッションではなく Tailspin Toys のリポジトリ フォルダーを選択します。リポジトリの上部ナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue からエージェント セッションを開始できます。簡単な最初の変更が終わったら、絞り込み機能の Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。他のリポジトリの作業項目を確認するには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリ自体について質問すると、アプリに慣れることができます。その用途には **quick chat** が適しています。ブランチや worktree を作成せずに質問したりブレインストーミングしたりできるため、セッションを必要としない、その場限りの質問に便利です。

1. サイドバーの **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを始めます。その後 **Chats** が表示されます。
2. セッションの仕組みについてアプリに質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。セッションごとに分離された独自の git worktree が使われるため、変更を競合させずに複数のエージェントを並列実行できることがわかります。会話は続けることも、新しく始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールしてプロジェクトを接続し、ワークスペースを確認しました。次の内容を学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットで短い質問をする。

次は[最初のエージェント セッションを開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app を使い始める][getting-started]
- [GitHub Copilot app でエージェント セッションを使う][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
