---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な変更でも、複数のファイルを更新し、テストを実行するなどの作業が必要になることがあります。開発者は通常、関係するファイルを見つけて変更し、テストを実行して、デバッグし、更新し忘れたファイルを見つけ、さらに変更を加えます。この作業は続くことがあります。

ここで Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE でより自律的に作業します。開発者と同様に、既存のプロジェクト構造を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる使い方を想定しています。Agent mode は IDE で作業し、すばやいフィードバックとやり取りを可能にします。Cloud agent はチームの一員のように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次の操作を行います。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する方法を確認します。
- GitHub Copilot Agent Mode を使い、UI とデータ層にまたがる機能を実装します。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。残りのデータ層、UI、テストの作業を Copilot Agent Mode で完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を確認するため Tailspin Toys の Web サイトを開きます。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細ページを表示します。単一の Astro アプリであり、ビルド時に静的 HTML を生成します。ページは Drizzle ORM を使って frontmatter からローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアント側の UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後であれば、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうかを尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら Astro サーバーのアドレス `http://localhost:4321` を選択し、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、Web サーバーをホストしている codespace へのプライベート トンネルである `https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **Home Page**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を掲載した詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所では各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を加えたいと考えています。まずバックログを確認します。テンプレートからプロジェクトを作成したときに、Issue のバックログも自動的に追加されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** は複数のファイルにまたがって自律的に計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューで agent picker が表示されているスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。提案されるコードに一貫性を持たせ、学習しやすくすることを目的としています。ただし LLM は確率的に動作するため、ワークショップの記載と異なる提案が出る場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot が異なる MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を優先順位付けしたことを確認します。

## 命令ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを伝える命令ファイルを確認します。Web サイトの機能追加に関するガイダンスを含む [ユーザー インターフェイス (UI) 命令ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全般的なガイダンスを確認します。アーキテクチャの概要、コンポーネントの設計・テスト可能性・アクセシビリティに関する原則、ファイルの種類ごとの命令へのリンクが含まれます。
   - Astro
   - Tailwind CSS
   - Drizzle

> [!TIP]
> 命令ファイルから、他の命令ファイルとプロジェクト内のファイルの両方を参照できます。パスは命令ファイルの場所からの相対パスです。命令を再利用でき、より小さなファイルに分けて、Copilot に例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) にフィルタリングのロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

コードベースにマージする前に、テストを実行して成功させる必要もあります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、命令ファイルを追加します。

   ![UI 命令ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューで agent picker が表示されているスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue の要件に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データ層のヘルパー、UI、テストを特定した後、ファイルの変更とテストを行います。

   ![Copilot がプロジェクト ファイルを調べているスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースによって数分かかる場合があります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業が終わると、すべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテストの実行確認ダイアログが表示されているスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクを完了してエラーがないと判断するまで作業します。

   ![Copilot Agent Mode の Chat セッションが完了した状態を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を確認します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode で Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode が UI とデータ層にまたがる機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の探索演習 - ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装させてみましょう。

- 既存の命令ファイルを使うなど、現在のベスト プラクティスに従います。
- ユーザーがページ サイズを選べるようにするか、値を固定するかなど、ページネーションの動作を決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更やコンテキストを追加で伝えながら、何度か調整することがあります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
