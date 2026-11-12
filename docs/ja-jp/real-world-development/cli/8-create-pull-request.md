---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、命令、スキルの更新、QA プロファイル、テストをレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリング機能、命令の更新、スキルの更新、quality assurance (QA) プロファイル、テストが、1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** に作業を任せます。

> [!NOTE]
> 通常は機能、命令の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルタリングと品質確認の一連の作業を 1 つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージ ライフサイクルの自動化方法を学びます。
- 機能全体の変更と検証結果を確認します。
- フィルタリングの PR を作成します。
- レビュー後に Agent Merge を有効にして、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の作業を通じて、Copilot を使って機能を計画、実装、検証しました。次は、開発者の承認権限を保ちながら、PR の残りの作業を自動化します。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗やレビュー コメントなど、PR を妨げているものに対処し、必要に応じてリベースします。その後、リポジトリが許可していれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge に任せることもできますが、有効にする前に変更をレビューし、マージできる状態だと判断してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体をレビューし、PR を作成して、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべてのチェックが成功すると、マージが実行されます。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリの設定、権限不足を回避しません。処理がブロックされた場合は理由を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理を自動化しました。次は、コンテキスト、モデル、共有、クラウドへの任意の委任に使える[スラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
