---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードで絞り込み機能を作成しました。プル リクエストを作成する前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーで絞り込み機能を手動テストさせます。
- ブランチを公開して、絞り込み機能のプル リクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部にあるツールを利用できると、さらに便利になります。Copilot がそれを可能にするのが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェント モードの内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやりとりを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスと通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

ツールやリソースには MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部ツールの通信を管理します。既存の API や npm パッケージなどのローカル ツールと接続できます。各サーバーは、エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、issue、プル リクエストを管理する API にアクセスできます。リポジトリの作成、更新、issue やプル リクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。エージェントが Web ページを開き、フォームに入力し、ボタンを選択できます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促すために [MCP registry](https://github.com/mcp) を公開しています。

> [!CAUTION]
> セキュリティの面でも、MCP サーバーはプロジェクトの他の依存関係と同様に扱ってください。利用する前にソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が最初にツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるため、機能のテストに適しています。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモート サーバーの `github` と異なり、`playwright` はローカル サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は、画面を表示できない Codespace 内でブラウザーを可視ウィンドウなしで実行するための指定です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストに Playwright を使うため、必要なブラウザーはすでにインストールされている場合がほとんどです。ブラウザーが見つからないと Copilot が報告した場合は、`npx playwright install chromium` を実行させてから再試行してください。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がそのサーバーのツールを初めて使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に表示される **Start** 操作を選択し、サーバーを起動して構成を確認します。
2. VS Code からサーバーを信頼するか確認された場合は、構成を確認し、起動を許可します。

手動での起動は任意です。省略した場合は、次の手順で Copilot がツールを必要としたときに VS Code が起動し、信頼するか確認します。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。利用者の代わりにページを開き、移動し、フィルターを適用し、結果を読み取って、確認内容をまとめます。会話を離れずに機能の動作を確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。そのため、エージェントは支援技術と同じように、ボタン、リンク、リスト項目などの構造化されたラベル付き要素を使って判断します。簡単な機能確認が、アクセシビリティの簡易チェックにもなります。

サーバーを接続し、アプリを実行した状態で、次のプロンプトを Copilot に送ります。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件に照らして要約を確認してください。問題がある場合は、PR を作成する前に追加の質問をするか、コードを修正させます。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリが実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要な場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行させて再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能の動作を確認したので、チームがレビューできるようにプル リクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プル リクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、複数の方法で PR を作成できます。ここでは GitHub Copilot に作成させ、関連 issue と紐付けます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択されていることを確認し、Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を許可します。GitHub ツールを初めて使うときは、GitHub 認証を求められる場合があります。指示に従ってください。
5. Copilot が issue を検索し、適切な issue を見つけて PR を作成したことを確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、MCP と MCP サーバーが Copilot にツールを追加する方法を学び、Playwright MCP サーバーをワークスペースに追加して、ブラウザーで絞り込み機能を手動テストし、ブランチを公開して PR を作成しました。

MCP サーバーを使って機能を**テスト**しましたが、Copilot を拡張する方法は MCP だけではありません。次は[カスタム エージェントを作成し][next-lesson]、アクセシビリティ レビューなどの作業に活用します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
