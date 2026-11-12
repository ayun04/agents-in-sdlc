---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能の要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、より大きな課題に集中できます。

Copilot cloud agent では、次の操作を行います。

- コード生成の環境をカスタマイズする。
- 操作を安全に行う。
- 範囲を明確にした Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys は、いくつかの技術的負債に対処しようとしています。Web サイトの最初のバージョンを作った請負業者はドキュメントを十分に作成していませんでした。まず、アプリケーションのエクスポート関数すべてに TSDoc doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの別のゲームを表示し、閲覧を続けられるようにします。洗練された実装はまだ必要ありません。UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度がより高い別の Issue があります。

どちらも、優先順位が下がってしまいがちな作業です。Copilot cloud agent に任せれば、エージェントが非同期で作業している間に別の作業を進め、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様に、バックグラウンドでタスクを実行します。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、作業環境をセットアップしてタスクを始めます。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法のような力はありません。考える作業を省略できるわけではありません。一見簡単そうな操作も、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を明確にする][cloud-agent-best-practices]ことが重要です。AI ペア プログラマーとして、段階を踏み、学び、試し、作業を調整してください。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と、Copilot を導く方法を確認しました。カスタム命令を使う演習に取り組んだ場合は、`.github/copilot-instructions.md` が機能することも確認できています。まだ確認していない場合は、ここで内容を簡単に見ておきます。cloud agent に作業を割り当てる前に、リポジトリに用意された命令ファイルを読み取り専用で確認しておくと、後でその効果を見つけやすくなります。

リポジトリの github.com の Web UI、または起動済みの codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターを確認します。この命令は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、命令ファイルが指定する TSDoc doc comment、TypeScript の規約、コメント ヘッダーが反映されます。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

コードの作成には、適切な状態を保つための作業環境とセットアップ用スクリプトが必要なことがあります。Copilot にタスクを割り当てる場合も同様です。cloud agent はソフトウェア エンジニアのように作業します。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を用意します。作業開始前に必要なツールや依存関係を利用できるよう、`.github/workflows/copilot-setup-steps.yml` に専用のセットアップ ワークフローを作成してカスタマイズできます。このワークショップでは、学習を円滑に進められるよう、あらかじめ設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、E2E テスト用のブラウザー、マイグレーションとシードを適用した SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot には操作用の独自トークンが渡されます。
    permissions:
      # セットアップ手順の一部としてリポジトリをクローンする場合 (依存関係のインストールなど) は、contents: read が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro と Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用に、ローカル SQLite データベースをマイグレーションしてシードします
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、次の点を確認してください。

- `copilot-setup-steps` というジョブが 1 つあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の実行を待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

多くのプロジェクトには、古くなった情報があったり、ドキュメントがまったくなかったりします。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成に役立つため、cloud agent に任せるのに適した Issue です。エージェントはバックグラウンドでドキュメントを作成します。後の演習で、その作業をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などです。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot が PR に todo リストを作成するまで数分かかることがあります。Copilot は環境を作成し (前に紹介したワークフローを実行し)、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストを確認し、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するために、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールして Copilot の作業を確認します。コードを調べて状態を理解し、方針を考え、コードを作成する様子を確認できます。

作業には数分かかります。Copilot cloud agent の主な目的は、非同期で作業することにより、開発者が他の作業に取り組めるようにすることです。この機能を使って、もう 1 つのタスクを cloud agent に割り当ててから、アプリの機能を実装するコードを書きます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チームが求める関連ゲームの表示は、それほど時間がかからないかもしれませんが、その間に別の作業も進められます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. 空のテンプレートとして **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が Issue の最初のコメントに表示されます。

![Issue に Copilot が対応中であることを示す目の絵文字が表示されている](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタム エージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、Issue を Copilot に割り当てて非同期に処理できます。技術的負債への対処、新機能の作成、コードの別のフレームワークへの移行などに利用できます。

このレッスンでは、コード生成用の環境のカスタマイズ、安全な操作、範囲を明確にした Issue の重要性、Issue の割り当てを確認しました。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム命令にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
