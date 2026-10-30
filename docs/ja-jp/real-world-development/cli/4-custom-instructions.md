---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント標準を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際は、コンテキストが重要です。作業方法に決まりがある場合は、Copilot がそのガイダンスを参照できるようにします。[命令ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。絞り込み機能を実装したので、Copilot が使った命令を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの命令とパス別命令ファイルがエージェントに適用される方法を確認します。
- コーディング標準が守られるように命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記載します。該当する場合は、注入可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載し、関連コードが変更されたときにコメントも更新します。
- 既存の書式設定と lint のガイダンスを維持します。

命令ファイルを使って Copilot に適切な情報を伝え、これらの開発手法に沿った作業を行えるようにします。

## 命令ファイル

カスタム命令を使うと、コーディング スタイルや要件を Copilot に伝えられます。リポジトリ全体に適用する命令と、特定のファイル種類やタスクに適用する命令を設定できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に渡される単一ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> 他の命令形式への対応状況は環境によって異なります。特定の形式に依存する前に、[カスタム命令のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには命令ファイルが含まれています。変更前に内容を確認し、後で影響を比較します。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この命令は、リポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ レイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob で命令の適用対象を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の命令ファイルや既存のプロジェクト ファイルへのリンクを確認します。命令を再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて命令を更新する

既存のファイルは有用ですが、生成される TypeScript に TSDoc コメントを追加する標準がまだありません。`.github/copilot-instructions.md` を更新します。

1. **Code formatting guidance** セクション (35 行目付近) を見つけます。
2. 最後の箇条書きに次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。命令を保存したら、絞り込みの会話を終了して再開し、機能のコンテキストを保ったまま新しいガイダンスを読み込ませます。

1. 絞り込みの会話が開いている場合は `/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. リポジトリ ルートの `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は絞り込みの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に次の変更を依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認してください。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを読み取る方法を確認し、新しい標準を機能に適用しました。リポジトリの命令とパス別命令がエージェントに届くことを確認し、命令ファイルを更新してコードへの影響を見ました。

次は再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
