---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。作業の方法に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム命令とパス別命令ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する方法

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、リアルタイムで並べて効果を示すのが難しいためです。命令ファイルの影響は、この後の演習で cloud agent が作成したプル リクエストをレビューするときに確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- データ レイヤーには必ずユニット テストを用意します。
- UI はダーク モードで、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使って Copilot に適切な情報を伝え、これらの開発手法に沿った作業を行えるようにします。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。リポジトリ全体、または特定の種類のファイルに対して命令を設定できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に渡される単一のファイルです。技術スタック、プロジェクトの概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に適用する命令ファイルです。たとえば、TypeScript、Astro、UI コンポーネントの作成、ユニット テストに関するガイドラインを記載できます。

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

## プロジェクトのカスタム命令ファイルを確認する

このリポジトリに含まれる命令ファイルを確認します。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリ ルートからの相対パスで、適用対象を指定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルに適用されます。
5. ユニット テストの作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の命令ファイル (`unit-tests.instructions.md` など) やプロジェクト ファイルへのリンクを確認します。大きな命令セットを再利用可能な小さなファイルに分け、Copilot が参照できる例を示せます。ここにあるパスはリポジトリ ルートではなく命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc コメントとファイル ヘッダーの規則を追加します。

## リポジトリの標準を追加する

この演習で編集するのは、プロジェクト全体の規則を記載する `.github/copilot-instructions.md` です。コード内に TSDoc コメントとファイル レベルのコメント ヘッダーを追加するよう指示します。

編集する前に、作業ブランチを作成します。cloud agent は issue の対象ブランチから命令を読み取りますが、変更を整理してコミットするためにブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き、既存の標準に続けて次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、命令ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイル種類、タスク用に `*.instructions.md` として作成することもできます。今回追加したドキュメント コメントの規則など、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## 命令の変更をコミット、プッシュ、マージする

cloud agent は、対象 issue のブランチにある命令ファイルを読み取ります。次の演習で Copilot に issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、命令の変更を `main` に反映してください。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプル リクエストを作成し、マージします。これで cloud agent は次の演習で割り当てられた issue に取り組む際、この命令を読み取れます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成せず、`main` にコミットしてください。ここでは実際のプロジェクトでの作業方法に近づけるため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの命令ファイルを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスで cloud agent が作成するプル リクエストをレビューするときに、この標準が反映されているか確認します。

次は [Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
