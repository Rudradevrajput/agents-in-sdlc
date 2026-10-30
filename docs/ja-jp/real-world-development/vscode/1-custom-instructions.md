---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。作業方法に決まりがある場合や、Copilot に伝える背景情報がある場合は、参照できるようにします。コード生成の指針を伝える方法の 1 つが[命令ファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパス別命令ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 既存の命令を使って、絞り込み機能の最初のデータ部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 次の演習で Copilot が更新したガイダンスを使えるよう、命令と絞り込みの一部を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、命令の変更後に傾向がどう変わるかを確認します。文字単位で同じ出力を期待するものではありません。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- データ レイヤーには必ずユニット テストを用意します。
- UI はダーク モードで、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使って Copilot に適切な情報を伝え、これらの開発手法に沿った作業を行えるようにします。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件を Copilot に伝えられます。リポジトリ全体または特定の種類のファイルに適用できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に渡される単一ファイルです。技術スタック、プロジェクトの概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種類に適用します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニット テストなどに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の命令ファイルにも対応しています。[命令ファイルのサポート][custom-instructions-support]を参照してください。

### 命令ファイルを管理するためのベスト プラクティス

サンプル プロジェクトには、命令ファイルの一般的な使い方が示されています。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、ユニット テスト、Astro コンポーネント、データ レイヤーなどのファイル種類や特定のタスクに関するガイドラインを記載します。
- 自然な言葉で、明確なガイダンスとコード例を記載します。

命令ファイルの作成方法や AI の使い方に唯一の正解はありません。プロジェクトに合わせて試行し、調整してください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した命令ファイルを用意してください。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに関するファイルがあります。
>
> Copilot に命令ファイルを生成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、利用する環境によって方法が異なります。必要に応じて各ハーネスのレッスンで説明します。
>
> テンプレートや出発点を探す場合は、命令ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリに含まれる中心的な `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルを確認します。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリ ルートからの相対パスで適用先を決めます。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルに適用されます。
5. ユニット テストの作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の命令ファイル (`unit-tests.instructions.md` など) やプロジェクト ファイルへのリンクを確認します。命令を再利用可能な小さなファイルに分け、Copilot が参照できる例を示せます。ここにあるパスはリポジトリ ルートではなく命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトの標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc コメントとファイル ヘッダーの規則を追加します。

## 変更用ブランチを作成する

変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

命令の効果を確認するため、まず現在の命令ファイルを使ってプロンプトを送信します。その後ファイルを更新し、同じプロンプトを再送信して違いを見ます。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習で開いたエディター タブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。ヘルパー ファイルとテストを変更する場合もあります。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取り、発行元の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案コードに TSDoc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。命令の更新後に一貫性が向上することが、この演習の目的です。

## リポジトリの標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を伝えるファイルです。コード提案を改善するため、コーディング標準を追加します。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** セクション (27 行目付近) を見つけます。既存の標準にコード内ドキュメントの規則がないため、ヘルパーにドキュメント コメントがなかったことを確認します。
3. 既存の標準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、命令ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイル種類、タスク向けの `*.instructions.md` のどちらにも作成できます。ドキュメント コメントの規則など、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目できるようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習で使う実際のコードに適用しました。

## 命令とブランチをコミット、プッシュする

命令ファイルも他のプロジェクト ファイルと同様に、ソース管理で管理します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで命令とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 命令の変更と、ヘルパーの基礎として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチをプッシュしたら、命令標準の issue に関連付けたプル リクエストを作成します。GitHub ツールを使える Copilot に、issue を探して PR を作成し、マージするよう依頼します。

> [!NOTE]
> この演習では Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートには GitHub ツールがすでに設定されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められる場合があります。指示に従ってください。接続方法 (Model Context Protocol) は後の演習で学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 命令ファイル更新の issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。命令の更新と発行元ヘルパーの基礎の両方を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot が GitHub 操作の **Allow** を求めたら、コマンドを確認して適切な場合に選択します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ローカル ブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作とシェル コマンドの **Allow** を求められたら、内容を確認して適切な場合に選択します。

GitHub Copilot の支援でプル リクエストを作成し、マージしました。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを読み取る方法を確認し、VS Code の Copilot Chat を使って、既存の命令でコードを生成し、リポジトリ全体の標準を追加して同じプロンプトを再実行し、命令更新とヘルパーの基礎を `main` にコミットしました。

次は[エージェント モード][next-lesson]で、コードベース全体にまたがる機能を Copilot に追加させます。

## リソース

- [GitHub Copilot の命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
