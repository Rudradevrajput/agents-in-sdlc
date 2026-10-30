---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys プロジェクト用の Node.js をインストールし、テンプレートからリポジトリの自分用コピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub の両方を一元的に扱うデスクトップアプリです。Issue や pull request にすばやくアクセスでき、GitHub Copilot を使った開発もできます。このワークショップでは、Astro で構築された Tailspin Toys アプリを GitHub Copilot app で更新しながら、ローカル環境で作業します。始める前に Node.js がローカルにインストールされていることを確認し、その後 Copilot app をインストールします。

このレッスンでは、次の操作を行います。

- プロジェクトのテストをコンピューターで実行できるように Node.js をインストールします。
- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を構築させ、Tailspin Toys のテスト スイートをローカルで実行します。そのためには、プロジェクトに必要な唯一のランタイムである **[Node.js][nodejs]** が必要です。現在の **LTS** リリースをインストールしてください。

すべてのプラットフォームで最も簡単なのは、公式インストーラーを使う方法です。

1. Windows Terminal、macOS のターミナル、または普段使っているターミナルを開きます。
2. 次のコマンドを実行し、インストール済みの Node.js と npm のバージョンを確認します。

    ```shell
    node --version
    npm --version
    ```

3. どちらのコマンドも成功し、Node.js がプロジェクトの README と `package.json` に記載された要件を満たしていれば、次のセクションに進めます。

> [!TIP]
> Node.js がまだインストールされていない場合、または更新が必要な場合にのみ、以下の手順を実行してください。

4. [Node.js のダウンロード ページ][node-download]を開きます。
5. 使用しているオペレーティング システム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選択します。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナル ウィンドウを開きます。
8. 新しいターミナルで次のコマンドを実行し、インストールを確認します。

    ```bash
    node --version
    npm --version
    ```

9. インストール済みの Node.js と npm のバージョンが表示されることを確認します。

> [!IMPORTANT]
> Copilot app のエージェントも `node --version` と `npm --version` を実行できる必要があります。普段使っているターミナルで成功しても、アプリから利用できるとは限りません。Node.js のインストール後や PATH の変更後は、アプリを完全に終了して再起動してください。セッション内でどちらかのコマンドが引き続き失敗する場合は、続行する前に[セッション環境のトラブルシューティング][session-environment]を行ってください。

> [!IMPORTANT]
> E2E チェックのために、各 worktree にプロジェクトの依存関係と Playwright Chromium も必要です。worktree を準備する際は Tailspin Toys リポジトリの README に従い、インストールを求められた場合は内容を確認してから承認してください。

## ワークショップ用リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレート リポジトリ][template-repository]から作成してください。新しいリポジトリにはワークショップに必要なファイルがすべて含まれています。アプリのインストール時にこのリポジトリを接続します。

1. 新しいブラウザー ウィンドウで、このワークショップの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` を開きます。
2. ワークショップ用リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して自分用のコピーを作成します。

    ![ドロップダウンで Create a new repository が選択された Use this template ボタン](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成できます。

    ![テンプレートに github-samples/tailspin-toys が設定され、リポジトリ名が入力された Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。後でワークショップ中に参照します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこの issue を使います。自分で issue を作成する必要はありません。

ワークショップ テンプレートの新しいコピーを使ってください。リポジトリの命令、アプリケーション コード、テスト、quality-checks スキル、既存のキャンバス拡張機能が含まれています。ワークショップではスキルをカスタマイズし、QA エージェントを作成します。古いコピーを使う場合は、必要なファイルが含まれているかファシリテーターに確認してください。

## まとめと次のステップ

準備ができました。このレッスンでは、次の操作を行いました。

- プロジェクトをビルド、テストできるように Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成しました。

次は [GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続して、ワークスペースを確認します。

## リソース

- [Node.js をダウンロードする][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
