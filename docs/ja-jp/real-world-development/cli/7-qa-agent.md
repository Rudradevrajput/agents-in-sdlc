---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上の絞り込み動作を確認しました。これらの機能を、明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次の操作を行います。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、絞り込み issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys は、プル リクエスト (PR) の作成前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザー動作を一貫して確認したいと考えています。カスタム エージェントで QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルにはエージェントの目的、命令、利用可能なツールを記載します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの標準を示します。`quality-checks` スキルは繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。これらを置き換えたり、別の会話を作成したりするものではありません。

## QA プロファイルを作成する

機能 PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成させます。プロファイルにはチェック内容と守るべき境界の両方を定義します。

1. Codespace に戻り、絞り込みの会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA の手順と境界がプロファイルに含まれていることを確認します。これにより、検証だけを依頼したのに不完全、または範囲の広すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須で、`name` は任意ですが、含めるとエージェントの表示名が明確になります。
3. 要件から確認を始めること、リポジトリの命令に従うこと、`quality-checks` スキルを実行すること、Playwright MCP を使うことを確認します。
4. QA が根拠を示して結果を報告し、実装コードを変更する前に確認を求め、変更をコミットしたり PR を作成したりしないことを確認します。
5. 必要な責務や境界が欠けている場合は、続ける前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクト エージェントを読み込みます。プロファイルを作成したら同じ絞り込み会話を再開し、QA エージェントを選択します。これにより、issue と計画の決定をコンテキストとして使えます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は、利用可能なエージェントの一覧に表示されない場合があります。コマンドを実行すると有効になります。

2. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業が始まります。
4. 作業が完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに加え、その動作を確認しました。カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認し、品質保証プロファイルを作成、選択して、絞り込み issue に照らした結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次はそれらを[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
