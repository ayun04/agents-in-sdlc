---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "Copilot app を使って Pages ワークフローを設定し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開しましょう。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのはユーザーの役割です。

このレッスンでは、次の操作を行います。

- Copilot に GitHub Pages のデプロイ設定を依頼します。
- マージする前にワークフローとプルリクエストをレビューします。
- デプロイの進行状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz は GitHub リポジトリにありますが、まだ簡単に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーや実行時の依存関係を追加せずに、公開 URL から利用できるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Space Quiz のプルリクエストのマージ、Build website、Upload artifact、Deploy to Pages の各ステップを含む GitHub Actions の成功したデプロイ、yourname.github.io/space-quiz/ で表示される公開済み Space Quiz の 3 段階の図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトでは、エージェントにプルリクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを検証する

1. README がまだない場合にだけ追加されていることを確認します。
2. ビルドとデプロイの手順を含め、プルリクエストの変更をレビューします。
3. 必須のプルリクエスト チェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで確認します。
5. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントに Pages の有効化やプルリクエストのマージを行う権限がない場合は、エージェントが示す手順に従ってください。デプロイが成功し、公開サイトが表示されるまでは完了と見なさないでください。

## まとめと次のステップ

Space Quiz をライブサイトとして公開し、エージェントの報告だけに頼らず、実際に確認できました。[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
