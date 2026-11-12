---
title: "演習 3 - カスタム エージェント"
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

この演習では、次の操作を行います。

- カスタム エージェントの定義方法を確認します。
- カスタム エージェントにタスクを割り当てます。

## シナリオ

Tailspin Toys は、見え方や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが十分でないため、暗いテーマを読みづらいと感じるユーザーがいることが分かりました。このアクセシビリティ上の課題に対処するため、デザイン チームはユーザーが切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。カスタム エージェントに機能を生成させましょう。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用カスタム エージェントはすでに用意されています。内容を確認して、Copilot をどのように導くかを理解します。

Codespace に戻り、アクセシビリティ用カスタム エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイト用コードを生成する際の主な役割、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題と誤りが記載されています。

## Issue を作成して割り当てる

ミッション コントロールは、環境内のエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、リダイレクトしたり追加のガイダンスを与えたりできます。高コントラスト モードを作成するタスクを Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントの選択画面で、エージェントの一覧から Accessibility が強調表示されているスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学び、特定のタスクや分野に合わせた AI アシスタントを確認しました。カスタム エージェントを使うと、チームの専門知識や規約を再利用できる形にまとめ、Copilot が適切に作業するよう導けます。

次はミッション コントロールから[エージェントのセッションを監視して方向付け][next-lesson]ます。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使用する準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
