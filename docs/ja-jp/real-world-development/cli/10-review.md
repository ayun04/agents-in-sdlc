---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の作業に取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトと登録済みのフィルタリング Issue を確認しました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から開始し、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- quality assurance (QA) カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの証拠を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge による PR のマージを許可しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

ワークショップでは、更新済みの `main` から別々のブランチを作成し、2 つの PR をマージします。

1. **星評価:** ゲームカードに既存の `starRating` と未評価であることを示す表示を追加します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、命令を更新して機能に適用します。`quality-checks` レポートをカスタマイズして QA プロファイルを作成し、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、関連作業を 1 つの PR にまとめています。

## 異なる方法での検証

自動テスト、自分で行うブラウザーでの確認、MCP を通じた Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルでは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は要件とテスト範囲のレビューをこれらの結果に加え、PR を作成する前に確認しました。

新しいテストは、実際に不足しているカバレッジを補うものである必要があります。QA の実行で追加テストが必要ない場合もあります。マージを許可する前に、コードと検証結果を確認してください。変更後は、影響を受ける検証結果を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業結果に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。命令は規約を定め、スキルは繰り返し実行できるタスクを記述し、カスタム エージェントは専門的な役割を定め、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選びます。**Plan** は実装前に方針を検討するとき、**Interactive** は範囲を絞った変更に取り組みながら状況を確認するとき、**Autopilot** は範囲が明確なタスクを任せるときに使用します。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

ツールの設定と同様に、コンテキストも重要です。何を、なぜ、どのように作成するかを明確に伝えると、出力が大きく変わります。

## さらに試す

主要なワークフローを完了しました。次の CLI 機能も試せます。

- `/review` でコード レビュー エージェントに変更を分析させる。
- `/rubber-duck` で問題を説明し、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して進める。
- `/worktree` で別のタスクを分離する。
- `/delegate` でタスクを Copilot cloud agent に送る。

## 次のステップ

ツールを上達させるには、使い続けることが一番です。製品コード、趣味のコード、長い間作ろうと思っていた小さなアプリなどで活用してください。チームで学びを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張するには、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も確認する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
