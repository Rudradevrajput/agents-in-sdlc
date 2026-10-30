---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、用意されている絞り込み issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンド ラインから実行できるため、タスクを任せたり、変更を依頼したりしながら作業に集中できます。まずツールをインストールしましょう。使い慣れたツールでインストールできます。

このレッスンでは、次の操作を行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーで絞り込み issue を見つけます。

## シナリオ

チームは、増え続けるバックログに取り組むために AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を利用できるようにします。このレッスンでインストールと認証を済ませ、ワークショップの残りで使えるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js が事前にインストールされているため、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、device code の手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本操作を確認する

通常のシェル プロンプトで入力したコマンドは Codespace で実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、スラッシュ コマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選び、<kbd>Enter</kbd> を押します。もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力し、インストール済みのバージョンで利用できるコマンドを確認してから <kbd>Esc</kbd> を押してヘルプを閉じます。
3. 次の質問を Copilot に送り、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が回答前にリポジトリを調べる様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のプロンプトで絞り込み issue を探すよう Copilot に依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて issue を読みます。最初の小さな変更を完了した後に使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使い、自動的に worktree を作成しません。変更ごとに機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は、設定済みの権限外でツールを使う前に通常は確認を求めます。このワークショップでは `--yolo` を使って再起動します。ユーザーが承認したこのショートカットは Codespace 内で承認プロンプトを省略し、演習に集中できるようにします。

> [!CAUTION]
> `--yolo` を使うと、完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) が有効になります。Codespace や VM などの分離された環境でのみ使用し、日常の開発で既定の設定として alias にしないでください。詳細は[ツールの使用を許可または拒否する方法][allow-all-warning]を参照してください。

このワークショップで `--enable-all-github-mcp-tools` を指定すると、後のレッスンで issue やプル リクエストを操作するための GitHub MCP の読み取り / 書き込みツールが有効になります。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 会話が動作することを確かめるため、プロジェクトについて簡単な質問をもう一度送り、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で命令を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

GitHub Copilot CLI を npm でインストールし、GitHub アカウントで認証しました。リポジトリを信頼して簡単な会話を試し、組み込みの GitHub MCP サーバーで絞り込み issue を見つけました。

次は[最初の小さな変更を行い][next-lesson]、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
