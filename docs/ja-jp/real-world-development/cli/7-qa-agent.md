---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーのフィルタリング動作を確認しました。これらの機能を、明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次の操作を行います。

- カスタム エージェントが命令、スキル、MCP ツールを使う方法を確認します。
- 再利用可能な quality assurance (QA) プロファイルを作成して調べます。
- QA エージェントを選択し、フィルタリングの Issue に照らして結果を確認します。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫して確認したいと考えています。カスタム エージェントを使えば、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、目的、命令、利用可能なツールを記述する Markdown プロファイルで定義する、Copilot の専門的な構成です。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

ここまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの規約を示します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは要件を評価し、結果を報告するための使い方を Copilot に伝えます。既存の機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用できる QA プロファイルを Copilot に作成してもらいます。どのチェックを行うかだけでなく、守るべき境界も指定します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のプロンプトを送信します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

エージェントを使う前に、意図した QA の流れと制約が記載されていることを確認します。検証だけを依頼したのに、プロファイルが不完全だったり範囲を広げすぎたりして、機能に変更を加えることを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略可能ですが、記載するとエージェント名が分かりやすくなります。
3. 要件から確認を始め、リポジトリの命令に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. 結果の根拠を報告し、実装コードの変更前に確認を求め、変更をコミットしたり PR を開いたりしないことを確認します。
5. いずれかの役割や制約が抜けている場合は、続行する前に既定のエージェントへ修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後に同じフィルタリングの会話を再開し、QA を選択すると、それまでの Issue と計画の内容を引き継げます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 新しく作成したエージェントは、利用可能なエージェントの一覧にまだ表示されない場合があります。その場合でも、上記のコマンドで有効にできます。

2. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を進める様子を確認します。
4. 完了したら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割を追加し、その結果を確認しました。次は[機能の PR に変更をまとめて Agent Merge を使い][next-lesson]ます。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
