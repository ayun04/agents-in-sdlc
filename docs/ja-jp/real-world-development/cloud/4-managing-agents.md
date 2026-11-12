---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前のいくつかの演習では、Copilot cloud agent にユーザー体験の改善や機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。このミッション コントロールでは、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を追加したりできます。

このレッスンでは、次の操作を行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を追加し、機能を拡張します。

## シナリオ

エージェントに高コントラスト モードの作成を依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているため、同時に追加するのが合理的です。高コントラストに加えてライト モードも実装されるよう、エージェントの作業に指示を追加します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインのタスク一覧を確認します。次のようなタスクが表示されます。
    - コードのドキュメントを更新する。
    - ゲームを変更するための API を生成する。
    - Web サイトに高コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。リポジトリのチェックアウト、作業環境の作成、MCP サーバーの設定、タスクを完了するための各種手順が含まれる場合があります。

> [!NOTE]
> 一覧に表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認して、作業を監視できます。

## Cloud agent に指示を追加する

実行中のタスクを確認できたので、高コントラスト モードの作業にライト モードも含めるよう Copilot に依頼します。

1. 高コントラスト モードのタスクを示すセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![ミッション コントロールのアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot のエージェント ページにある、作業中のセッションに指示を加えるダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識して作業の流れに追加したことを確認します。

## Copilot に作業を任せる

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が完了すると、新しい依頼が作業の流れに組み込まれます。

作業には数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視して方向付ける中心的な場所であるエージェント ページを確認しました。実行中・完了済みのタスクを追跡し、作業をレビューし、進行中のタスクに追加の指示を出しました。

次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
