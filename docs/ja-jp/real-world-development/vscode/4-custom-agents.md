---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成して、対象を絞った命令、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントとしてまとめることで、[WCAG][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディング規約に従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどを定義できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、命令を定める Markdown プロンプトが必要です。

### カスタム エージェントと agent skills の違い

カスタム エージェントと [agent skills][agent-skills-concept]には一部共通する点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルや他のエージェントを調整します。このラボでは、アクセシビリティ用のカスタム エージェントがサイトをガイドラインに照らして確認・更新します。その作業の一環として、プルリクエスト ワークフローのスキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使う他の作業と同様に、利用する環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、見え方や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが十分でないため、暗いテーマを読みづらいと感じるユーザーがいることが分かりました。このアクセシビリティ上の課題に対処するため、デザイン チームはユーザーが切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。アクセシビリティ用のカスタム エージェントに機能を生成させます。

この演習では、次の操作を行います。

- 既存のアクセシビリティ用カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、高コントラスト モードを実装します。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。内容を確認して、Copilot をどのように導くかを理解します。

Codespace に戻り、ターミナルを開いて、アクセシビリティ機能用の新しいブランチを `main` から作成します。Exercise 3 のフィルタリング PR とは別に作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイト用コードを生成する際の主な役割、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題と誤りが記載されています。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` で定義したすべてのカスタム エージェントが表示されます。エージェントを選択すると、そのエージェントの命令やツールに合わせてチャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所では各手順で説明します。

1. まだ選択されていない場合は、Chat ビューのエージェント ドロップダウンから **Agent** を選択します。

   ![Chat ビューで agent picker が表示されているスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. チャット ビュー下部のエージェント ドロップダウンを選択します。現在のエージェント名が表示されます。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べて変更を提案し、プロジェクトのファイルに適用します。各編集は、ファイル パスとレビューできる差分とともにチャットに表示されます。
6. 編集が適用されると、チャットの **Files changed** インジケーターに、エージェントが変更したファイルの一覧が反映されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot は実際にリポジトリを変更します。作業中に Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、進行中の作業をレビューし、エージェントに追加の指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学び、特定のタスクや分野に合わせた AI アシスタントを確認しました。カスタム エージェントを使うと、チームの専門知識や規約を再利用できる形にまとめ、Copilot が適切に作業するよう導けます。

このレッスンでは、カスタム エージェントの定義方法と、Copilot Chat の agent mode でエージェントを使う方法を確認しました。

次は[エージェントの作業を監視して方向付け][next-lesson]ます。変更を確認し、同じセッションにライト モードの切り替えも追加するよう依頼します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
