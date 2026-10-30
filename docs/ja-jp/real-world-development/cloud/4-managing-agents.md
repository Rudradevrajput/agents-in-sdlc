---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザー体験の向上や機能の追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。この mission control では、オープンなプル リクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を出したりできます。

このレッスンでは、次の操作を行います。

- エージェント ページを使って cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに割り当てた後、チームはライト モードも追加することにしました。サイトのスタイルと切り替え機能を変更しているため、ハイ コントラストに加えてライト モードも追加するようエージェントを誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインのタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントの更新
   - ゲーム変更用 API の生成
   - Web サイトへのハイ コントラスト モードの追加
3. 実行中のタスクを選択し、Copilot が実行した操作を確認します。コードのチェックアウト、環境の作成、MCP サーバーのセットアップ、タスクを完了するための各種操作などが表示されます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示される PR ペインも確認します。PR や変更されたファイルを確認できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認したので、ハイ コントラスト モードにライト モードも追加するよう依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選択します。タイトルは Copilot が付けた名前や現在の作業状況によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェント ページで cloud agent タスクの誘導ダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映したことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい要望を作業に反映します。

作業には数分かかります。その間、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視し、誘導するための中心的な場所である Copilot agents ページについて学びました。アクティブなタスクや完了したタスクを追跡し、作業内容を確認して、進行中のタスクに追加の指示を出せます。

この演習では、mission control を使って cloud agent のタスクを監視し、進行中のセッションに機能追加を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は [Copilot が作成したプル リクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
