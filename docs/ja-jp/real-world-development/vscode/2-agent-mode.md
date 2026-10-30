---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数のファイルを変更し、テストを実行する必要があります。開発者は通常、必要なファイルを探し、変更し、テストを実行し、問題をデバッグして、変更漏れがあればさらに修正します。この作業は何度も続くことがあります。

ここで Copilot Agent Mode を活用できます。

Copilot Agent Mode は、IDE 内でより自律的に作業します。既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けです。エージェント モードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent はチーム メンバーのように非同期で作業し、issue やプル リクエストを通じてやりとりします。

この演習では、次の操作を学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行う方法
- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる新機能を実装する方法
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリと発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。残りのデータ レイヤー、UI、テストの作業を Copilot Agent Mode で完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者向けテーマを持つボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、対話機能は標準 DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクで、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 絞り込み作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Codespace 内の Web サーバーをホストするプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、次の主な機能を確認します。

- **ホーム ページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのタイトル、説明、発行元、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの最初の実装は動作しますが、さらに機能を追加します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップしたときに issue が作成されています。GitHub Copilot に一覧を表示させます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは、自律的に計画し、複数ファイルに変更を加え、ターミナル コマンドを実行し、ツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨するものではありません。ワークショップ作成時に Claude Sonnet 4.5 を使ったため、体験を揃えやすくするために手順に含めています。ただし、LLM は確率的に動作するため、説明と異なる提案が表示される場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者にとって有用な順に issue を優先順位付けしたことを確認してください。

## 命令ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを与える命令ファイルを確認します。Web サイトへの機能追加方法に関する情報を含む[ユーザー インターフェイス (UI) 命令ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関するガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などのファイル種類別命令へのリンク

> [!TIP]
> 命令ファイルから、他の命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルの場所からの相対パスです。命令を再利用し、複雑な内容を管理しやすい単位に分け、例やテンプレートを示せます。

## 絞り込み機能を実装する

絞り込みを完成するには、少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りの絞り込みロジックを追加または調整する
- 絞り込み動作のテストを追加または更新する
- ゲーム一覧ページに絞り込み UI を追加する

コードベースにマージする前にテストが成功する必要があります。Copilot Agent Mode にこれらの作業を任せます。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、命令ファイルを追加します。

   ![UI 命令ファイルを選択する画面の例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログ内の関連 issue に基づいて機能を実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを特定します。データ レイヤーのヘルパー、UI、テストを探し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって、数分かかる場合があります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。すべての変更に対する操作が表示されるので、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテスト実行の確認ダイアログが表示された画面](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、エラーを検出しなくなるまで進めます。

   ![Copilot Agent Mode の Chat セッション完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を学びました。Agent Mode で UI とデータ レイヤーにまたがる機能を実装し、プロジェクトと関連ファイルを調べて変更を調整し、生成された変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを作成します。

### おまけの演習 — ページ分割を実装する

ゲーム一覧が増えたら、ページ分割が必要になります。この演習で学んだ方法を使って、Copilot にページ分割を実装させてみましょう。

- 既存の命令ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズを利用者が選択できるようにするか、固定にするかを検討します。
- 希望する実装に必要なガイダンスを、プロンプトに含めます。
- Copilot に追加の変更を依頼し、コンテキストを伝えながら反復する必要がある場合があります。これは Copilot を使う通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
