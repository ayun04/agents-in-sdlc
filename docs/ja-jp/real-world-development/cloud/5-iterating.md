---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能の追加に関するいくつかのタスクを GitHub Copilot に依頼してきました。コードにドキュメントを追加し、デザイン チームが改善を検討する関連ゲーム機能を作成し、高コントラストとライト モードの切り替えなど、アクセシビリティ機能を実装しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って、作業を改善しましょう。

### シナリオ

これまでに何度も確認したとおり、生成 AI を加えても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体によるレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した変更を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ人間の監督なしでタスクを実行するため、安全性を保つためのセキュリティ上の制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセス権はコード用ブランチに限られます。
- Cloud agent は GitHub Actions 内に作成される、個別の一時的な環境で動作します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む外部リソースへのアクセスは、既定で制限されています。[外部リソースへのアクセスをカスタマイズ][agent-firewall]できます。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードにドキュメントを追加する変更を、GitHub.com の標準の PR インターフェイスで確認します。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定される動作**です。必要に応じて [Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. 新しいブラウザー タブで github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc doc comment やその他のドキュメントを含むコードを確認します。具体的な変更内容は状況によって異なります。Exercise 2 の最初に確認したカスタム命令ファイルに基づく、TSDoc doc comment、TypeScript の規約、コメント ヘッダーがあるかを確認します。
6. 更新内容をレビューして問題がない場合は、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローがキューに追加されたことを確認します。すべて順調であれば、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームの他のメンバーと同じように、PR やコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、必ずしも正確な依頼文を示せるわけではありません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの概要を説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc doc comment を追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例から選ぶか、コードベースに追加してほしいドキュメントを考え、**@copilot** をメンションしたコメントで変更を依頼します。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが追加されます。
5. **Files changed** タブを選択して変更をレビューします。

満足するまで作業を続けます。完了したら、PR をドラフトからレビュー可能な状態に変更して `main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示すインジケーターを確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションでワークフローがキューに追加されたことを確認します。すべて順調であれば、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot は関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に変更を依頼して作業を反復できます。たとえば、関連ゲームの表示件数を変えたり、カスタム命令を更新する前に割り当てた Issue であることを考慮して、コメント ヘッダーや TSDoc doc comment を追加したりできます。前と同様に、**Conversation** タブに新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 でミッション コントロールから追加したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は、作業が完了するまで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 特に次の点を確認します。
   - 表示モードを切り替える UI コンポーネント
   - ユーザー設定を保持するための local storage の使用
   - 高コントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローがキューに追加されたことを確認します。すべて順調であれば、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名を `<branch-name>` に入れて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーで高コントラスト モードとライト モードを切り替えます。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタム エージェントが、Copilot にアクセシビリティのベスト プラクティスに沿った機能を実装させたことが分かります。アクセシビリティ上の問題や改善点がある場合は、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 委任を続ける

Cloud agent は、実際のバックログ項目を渡して別の作業に取り組めるときに効果を発揮します。リポジトリに Issue を追加して Copilot に割り当て、習慣として使ってみましょう。次のような作業を試せます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。レッスン全体を通じて、次の操作を行いました。

- このリポジトリに含まれるカスタム命令ファイルを確認し、cloud agent の出力に及ぼす効果を確認しました。
- Issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の作業を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使い、高コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページをミッション コントロールとして使い、アクセシビリティのセッションを監視して途中で指示を追加しました。
- Copilot のプルリクエストをレビューして反復し、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent のハーネスを完了しました。別の作業環境で学ぶ場合は、次のハーネスで関連するシナリオを体験できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skills、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る省略可能な `/delegate` ブリッジを使って、ターミナルから 2 つのレビュー済み変更を提供します。

自分のリポジトリで、次のアイデアを試すこともできます。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野に特化したカスタム エージェントを作成する。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
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
