---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を体験します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を繰り返し行います。

## シナリオ

架空の会社 Tailspin Toys に新しく加わった開発者として、ボードゲームのクラウドファンディングを提供するサービスに取り組みます。チームのバックログは GitHub Issue として登録済みです。フィルターやページネーションなどの機能開発に加え、アクセシビリティやコーディング規約などの品質改善にも取り組みます。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。開発方法に合った環境を選んでください。各ワークショップには専用のセットアップ手順があり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使用します。Plan モードと Autopilot モード、命令、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を導き、進捗を監視し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターを離れずに、Copilot Chat の agent mode、MCP サーバー、カスタム エージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
