---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、クラウドへの任意の委任に使うコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には操作用のスラッシュ コマンドが多数あります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` で Copilot のコンテキスト管理を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、クラウド エージェントへの委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主なワークフローを終えたので、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキスト ウィンドウの上限に近づく場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュ コマンドを使って、自分で内容を確認したり圧縮したりすることもできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き容量へのコンテキストの割り当てを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合、大きな違いが見られないことがあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に圧縮します。圧縮するタイミングを選ぶには `/compact` を使います。関係のない別タスクを始める場合は、`/clear` または `/new` で新しい会話に切り替えます。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、利用できるモデルの一覧を表示し、使用するモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使うか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学んだことを共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンに表示される `/share` のオプションを確認します。
2. セッションを共有する場合は、`/share file` によるローカルの Markdown へのエクスポートなど、目的に合った方法を選びます。
3. エクスポートした内容を送信または公開する前に確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 省略可能: 作業の拡張や委任

ワークショップの主要な内容は完了です。Copilot CLI には、大規模なタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行します。
- `/worktree` は、別のタスク用に分離された Git worktree を作成します。
- `/delegate` は、タスクを Copilot cloud agent に送り、非同期で作業させます。プルリクエストが作成される場合があります。

これらのコマンドは追加の worktree やリモート作業を作成することがあるため、任意の操作です。試す場合は、範囲の明確な新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶには、[クラウド エージェントのワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

スラッシュ コマンドを使うと、Copilot CLI を構成したり、セッションを共有したり、内部の動作を確認したりできます。次は[ここまでの内容を振り返り][next-lesson]、学習を続ける方法を確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
