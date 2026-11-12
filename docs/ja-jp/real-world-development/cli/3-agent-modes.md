---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で Issue に基づくフィルタリングを構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しましたが、規模の大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できます。これから複数のレッスンにわたり、Issue から機能を作成し、コードが有効で、期待どおりに動作することを確かめ、プロジェクトへのマージを完了する一般的なエージェント駆動型の開発プロセスに取り組みます。

> [!NOTE]
> 機能の作業では、同じ会話とブランチを使い続けます。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次の操作を行います。

- GitHub Issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送したブラウザーで機能を手動で検証します。

この機能の作業を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えてきたため、訪問者がゲームをカテゴリやパブリッシャーで絞り込めるようにします。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装前に合意が必要な点があります。Plan モードで決定し、範囲を明確にした実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発に取り入れても、基本は変わりません。一般的な開発フローは次のとおりです。

1. 必要な作業を記載した Issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって具体的な手順は異なりますが、多くの場合は上記の流れを基にしています。

この標準的な進め方に従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューと検証を受けるようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと共同で作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集は行いません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行して、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot モードで実装します。

## Issue から始める

フィルタリングの作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続行します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** という Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に任せる前に、アプローチを決められます。複雑な作業では計画に時間をかけることをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切なアプローチを決めて計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に応じて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を上から下まで確認します。
6. 決めた内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認したので、実装に進みます。Autopilot を使うと、Copilot は完了したと判断するまで問題に取り組み続けます。

1. **Accept plan and build on autopilot (recommended)** またはインストールしたバージョンに表示される同様の項目を選択します。
2. プロンプトの下に **Autopilot** と表示されることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。計画を先に確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、他のコードと同様にマージ前にレビューします。コードを確認し、サイトを起動して、動作を確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードにします。モード表示に **Plan** または **Autopilot** と表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画中に決めた内容と照らし合わせます。
4. コードレビューが終わったら、<kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードに問題がないことを確認したら、アプリを起動し、Codespaces で転送したポートを通じてサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces からポート `4321` が利用可能と通知されたら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリとパブリッシャー、およびその組み合わせで絞り込みます。
4. リセット時の動作と、結果がない場合の動作が Issue と決定事項に合うことを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って、機能を構築しレビューしました。このレッスンでは、次の操作を行いました。

- GitHub Issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送したブラウザーで機能を手動で検証しました。

次は[カスタム命令を使って][next-lesson]、生成されたコードが文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
