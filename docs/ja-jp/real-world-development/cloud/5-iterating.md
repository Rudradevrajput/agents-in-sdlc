---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

この演習では、ユーザー体験の向上と機能追加を目的に、GitHub Copilot に複数のタスクを依頼しました。コードへのドキュメント追加、デザイン チームが改善できる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替え機能の実装です。コード変更を確認し、必要に応じて Copilot にフィードバックを送ります。

### シナリオ

生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体のレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ無人でタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みは作業用ブランチに限られます。
- cloud agent は GitHub Actions 内に一時的な環境を作成して実行されます。
- GitHub Actions ワークフローを実行するには、人による承認が必要です。
- MCP サーバーなど、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するために GitHub Copilot cloud agent が作成した最初のプル リクエスト (PR) を、GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。MCP サーバーへのアクセスなど、既定で外部リソースへのアクセスが制限されているためです。必要に応じて [cloud agent のファイアウォールを設定または無効化][agent-firewall]できます。

1. GitHub.com でリポジトリを開きます。
2. **Pull Requests** を選択します。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が付いています。その場合は作業が完了するまで待ちます。

4. **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc コメントやその他のドキュメントが追加されたコードを確認します。変更内容は状況によって異なります。演習 2 で確認したカスタム命令に基づく TSDoc コメント、TypeScript 規約、コメント ヘッダーがあるかを確認してください。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR のコメントやコードへのインライン コメントで、チーム メンバーと同じように Copilot にメンションして変更を依頼できます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確定的ではないため、必ず同じ結果になる指示はありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. `@copilot` にメンションして、生成されたドキュメントの変更を依頼します。上記の例を使うか、コードベースに必要な別の変更を依頼します。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻ります。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで改善を繰り返します。準備ができたら、ドラフト PR をレビュー可能な状態にして `main` にマージできます。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. GitHub.com でリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を開きます。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっているか確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが登録され、単一 Astro アプリのプロジェクト チェックが成功することを確認します。
9. **任意:** Copilot が作成したブランチに切り替え、Codespace で関連ゲーム機能を手動でテストできます。`<branch-name>` を Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、ターミナルで次を実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot は関連ゲーム機能を実装しました。PR の **Conversation** タブにコメントを追加すると、表示件数やコメント ヘッダー、TSDoc コメントの追加など、変更を依頼できます。後者を依頼する場合は、issue の割り当て後にカスタム命令を更新したことを思い出してください。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で追加依頼したライト モードの両方が含まれているはずです。

1. GitHub.com でリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot が作業中の場合は、PR に **[WIP]** が付いています。作業が完了するまで待ちます。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装をレビューします。特に次の点を確認してください。
   - モード切り替え用の UI コンポーネント
   - 利用者の設定を保存する local storage の使用
   - ハイ コントラスト モードとライト モードの CSS またはスタイル変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっているか確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローを確認し、単一 Astro アプリのプロジェクト チェックが成功することを確認します。
10. **任意:** Codespace でブランチに切り替えてアクセシビリティ機能を手動でテストできます。`<branch-name>` を Copilot が作成したブランチ名に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ エージェントがベスト プラクティスに沿って機能を実装するよう Copilot を導いたことを確認します。アクセシビリティ上の懸念や改善点がある場合は、他の PR と同様にコメントで `@copilot` に変更を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent にバックログ項目を渡して別の作業を進めると、効率よく活用できます。習慣として試すために、リポジトリに issue を作成し、Copilot に割り当ててみてください。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンで、次のことを行いました。

- このリポジトリのカスタム命令ファイルを確認し、cloud agent の出力に反映されることを確認しました。
- Copilot cloud agent に issue を割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使用し、ハイ コントラストとライト モードを追加しました。
- Copilot agents ページを mission control として使い、進行中のアクセシビリティ セッションを監視、誘導しました。
- cloud agent の PR をレビューし、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。他のハーネスでは、ここで学んだ内容を別の方法で実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan、Autopilot、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 件リリースします。

自分のリポジトリでは、次の方法も試してください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野に特化したカスタム エージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
