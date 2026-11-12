---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、登録済みのフィルタリング Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールを操作できます。ターミナルからタスクを任せたり、変更を依頼したりできるため、作業に集中できます。最初のステップとして、Copilot CLI をインストールしましょう。

このレッスンでは、次の操作を行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、Copilot CLI で簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が日常的に使うターミナルでエージェントを利用できます。このレッスンでは、Copilot CLI をインストールして認証し、残りのワークショップで使えるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、この演習では npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証が求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、device code の手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub のセッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 使い方を確認する

通常のシェル プロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI を起動すると、自然言語はエージェントに送信され、スラッシュ コマンドは会話を操作します。

1. `/model` を入力します。矢印キーで **Auto** を選択し、<kbd>Enter</kbd> を押して、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストールしたバージョンで利用できるコマンドを確認します。<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の質問を Copilot に送り、正しく動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. Copilot にフィルタリングの Issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開き、Issue の内容を読みます。最初の変更が終わったら、この Issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI セッションでは、ターミナルで現在チェックアウトしているブランチを使用し、自動的に worktree を作成することはありません。変更を始める前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は許可済みの範囲外でツールを使用する前に確認を求めます。このワークショップでは `--yolo` を使って再起動し、Codespace 内でツール使用の確認を省略して演習に集中します。

> [!CAUTION]
> `--yolo` は、`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls` を有効にして、すべての操作を自動的に許可します。Codespace や VM などの隔離された環境でのみ使用し、日常の開発で使う既定の設定にしないでください。詳細は[ツールの使用を許可または禁止する方法][allow-all-warning]を参照してください。

このワークショップでは `--enable-all-github-mcp-tools` を使って、後のレッスンで Issue やプルリクエストを操作するための読み取り・書き込み用 GitHub MCP ツールを有効にします。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 会話が動作することを確認するため、プロジェクトに関する簡単な質問をもう一度送り、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで命令を変更したりエージェントを追加したりした後、`copilot --resume` で同じ会話とブランチに戻ります。

## まとめと次のステップ

GitHub Copilot CLI をインストールして認証し、次のことを行いました。

- npm を使って Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーでフィルタリングの Issue を見つけました。

次は[最初の小さな変更に取り組み][next-lesson]、Copilot CLI でゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
