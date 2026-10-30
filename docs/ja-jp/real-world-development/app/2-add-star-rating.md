---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認してクイックチャットを試したので、**エージェント セッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さくしましょう。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次の操作を行います。

- エージェント セッションを開始し、セッションの構成を学びます。
- エージェントに小さく対象を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプル リクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードには、タイトル、カテゴリ、発行元、説明だけが表示されています。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。小さく自己完結した、最初のセッションに適した変更です。

## セッションの構成

**セッション**は、専用の独立したワークスペースで実行するエージェントとの会話です。各セッションに**専用の git worktree とブランチ**が割り当てられます。そのため、一方で機能を追加しながら、別のセッションでバグを修正するなど、変更を競合させずに複数の作業を同時に進められます。セッションはリポジトリごとにサイドバーにまとめられ、選択すると切り替えられます。

セッション内には、エージェントとの**会話**、ファイルを調査、編集するときの**ツール操作**、差分付きの**変更ファイル**一覧が表示されます。

## セッションを開始して変更を依頼する

プロジェクトを調べて機能を実装する新しいセッションを始めます。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを新たに作成し、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリに `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選択し、次のプロンプトで変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は、最初にプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調べ、新機能の追加に必要なファイルを見つけてコードを作成します。これで Copilot app を使って機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものであってもマージ前にレビューします。Copilot app 内で変更を確認します。

1. アプリの右上隅にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更ファイルと差分を確認します。

    ![Create PR ボタンの右側に Toggle review panel がある GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム詳細の表示に使われる中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` の場合は "No rating yet" と表示する、次のような小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、実際のコードは上記と異なる場合があります。似た実装であれば問題ありません。

## 変更を確認する

ブラウザーを開く前に、エージェントの自動チェック結果をレビューします。数値の `starRating` と、`null` の場合の代替表示の両方をテストしていることを確認します。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認してください。

コードを読んだだけで動くと判断せず、Copilot に Web サイトを起動させ、ブラウザー キャンバスで更新された UI を確認します。

> [!TIP]
> キャンバスは、Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンではカスタム キャンバスを調べて作成しますが、ここでは組み込みのブラウザー キャンバスを使います。

1. 次のプロンプトでアプリを起動し、ブラウザー キャンバスでページを開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価済みのゲーム カードに 5 点満点の値が表示されることを確認します。
4. 確認が終わったら、次のプロンプトで、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプル リクエストを作成してマージする

機能を作成できました。次は、新しいコードを既存のコードベースにマージするためのプル リクエスト (PR) を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、指示に従って認証します。
3. PR を作成する前に対応する Issue が必要であるというリポジトリ ポリシーを Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビュー ペインで PR を開きます。ここで PR を確認できます。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースしました。エージェント セッションを開始して構成を学び、エージェントにゲーム カードの小さな変更を依頼し、ワークスペースの差分ビューでレビューしました。アプリをローカルで実行してブラウザーで星評価を確認し、PR 1 を作成してチェックをレビューし、明示的にマージしました。

次は[絞り込み Issue から開始し、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェント セッションを使う][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプル リクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#ワークスペースを確認する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
