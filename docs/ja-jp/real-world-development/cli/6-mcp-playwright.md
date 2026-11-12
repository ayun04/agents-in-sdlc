---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を設定し、ブラウザーからフィルタリング機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを作成するだけではなく、データや外部サービスを扱い、Copilot が利用できる自動化機能を増やすことも必要です。MCP サーバーを使うと、Copilot CLI に組み込まれている機能に加えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びます。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認します。

## シナリオ

単体テストや E2E テストも重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Copilot に Web サイトを使わせ、変更が期待どおりに動作するかを確かめます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。最新の情報にアクセスしたり、エージェントに代わって操作を実行したりできます。

MCP サーバーはエージェントと外部ツールやサービスの間を仲介します。それぞれの MCP サーバーから、異なるツールやリソースを利用できます。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub が公開する [MCP レジストリ][mcp-registry]から、サーバーを探したりエコシステムに貢献したりできます。

> [!CAUTION]
> MCP サーバーは、プロジェクトの他の依存関係と同様に扱ってください。使用前にソース コードと発行元を確認し、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

まだ Playwright MCP サーバーを利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップを省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、次に <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が期待どおりに動作することを手動で確認しました。次は、Copilot の Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを明示しなくても Copilot が適切なサーバーを判断します。ここでは明示することで、利用するツールを指定し、結果の一貫性を高められます。

2. Copilot がブラウザーで機能を確認する様子を見ます。
3. レポートを読み、すべて期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。次は[QA カスタム エージェントを作成し][next-lesson]、スキルとブラウザー ツールを専門的な役割にまとめます。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
