---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクを特定の方法で行う必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中でも重要なのが、コード生成の指針を伝える[命令ファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパスを対象とした命令ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメントの規約が Copilot に伝わる方法を確認します。
- 既存の命令を使い、フィルタリング機能の最初のデータ部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加します。
- 同じプロンプトを再実行し、生成コードが新しい規約に従うことを確認します。
- 命令の更新とフィルタリングの最初の実装を `main` にコミットし、次の演習で Copilot が最新のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した規約に従わない場合があります。Copilot は確率的に動作します。この演習では、出力が文字単位で同じになることではなく、命令を更新した後の動作の**傾向**を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には開発方法に関するガイドラインと要件があります。

- データ層には必ず単体テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- TSDoc doc comment を使って、コードにドキュメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使い、Copilot がこれらの開発方針に沿ってタスクを進めるための情報を提供します。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件に関するコンテキストや好みを Copilot に伝えられます。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定することで、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定のタスクやファイルの種類に命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の命令ファイルです。技術スタック、開発対象の概要、ベスト プラクティスなど、多くのチャットや CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に対する命令ファイルです。たとえば、TypeScript など特定の言語や、UI コンポーネントまたは単体テストの作成に関するガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、命令ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方をコード生成時に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格を使った命令ファイルにも対応しています。詳細は[命令ファイルのサポート][custom-instructions-support]を参照してください。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成に関する詳細な説明はこの演習の範囲外です。サンプル プロジェクトの例から、基本的な方針を確認します。

- `copilot-instructions.md` には、開発対象やプロジェクトの構造、全般的なコーディング規約など、プロジェクト全体のガイダンスを記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ層など、特定のファイルの種類やタスクに関するガイダンスを記載します。
- 自然な言葉を使い、明確なガイダンスと、コードのよい例・悪い例を示します。

AI の使い方に唯一の正解はありません。プロジェクトで試しながら、適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した命令ファイルを用意してください。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスクに対応する命令ファイルがあります。
>
> Copilot を使って命令ファイルを作成することもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、それぞれの環境で作成方法が異なります。各ハーネスでは、関係する箇所で手順を説明します。
>
> テンプレートや出発点を探している場合は、命令ファイル、カスタム エージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** のガイダンスもあります。これらの命令は Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象をリポジトリ ルートからの glob で定義しています。この例では `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトで単体テストを作成するときに従う命令を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾までスクロールし、`unit-tests.instructions.md` などの命令ファイルや、既存のプロジェクト ファイルへのリンクを確認します。相対パスはリポジトリのルートではなく、命令ファイルの場所からのパスです。命令を小さく再利用可能なファイルに分け、コード生成の例を Copilot に示せます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc doc comment とファイルのコメント ヘッダーの規則を追加します。

## 変更用ブランチを作成する

ここではベスト プラクティスに従い、作業用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行し、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な箇所では各手順で説明します。

既存の命令ファイルを使ってプロンプトを送信し、カスタム命令の効果を確認します。後で命令を更新し、同じプロンプトを再送信します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューで agent picker が表示されているスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパー ファイルとテストなど、複数のファイルを変更する場合があります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 提案されたコードには TSDoc doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示しなくても doc comment を追加する場合があります。その場合も問題ありません。ここでは命令の更新後に規約への準拠が一貫することを確認します。

## リポジトリ全体に適用する規約を追加する

`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に提供します。コーディング規約を記載して、コード提案の品質を高めます。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存のコーディング規約が記載されていますが、コード内のドキュメントに関する規則はまだありません。
3. 既存の規約のすぐ下に次の Markdown 行を追加し、ファイル ヘッダーと TSDoc doc comment を Copilot に求めます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したとおり、命令ファイルはリポジトリ全体に適用される `.github/copilot-instructions.md` と、特定の言語、ファイルの種類、タスクに適用される `*.instructions.md` として作成できます。doc comment の規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルを使います。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが付いていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト規約に従うよう Copilot を導き、次の演習で使う実際のコードに適用できました。

## 命令をコミットしてブランチをプッシュする

命令ファイルも他のアセットと同様に、ソース管理で管理します。命令の更新とヘルパーの変更をコミットして、リポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルで `git status` を実行し、命令とヘルパーの変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. 命令の更新と、ヘルパーの基礎作成にあたり Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の更新 (テストなど) が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、命令の規約に関する Issue に関連付けたプルリクエストを作成します。手動で行うこともできますが、GitHub ツールがプロジェクトに接続されているので、Copilot に依頼します。Issue を検索し、終了させる PR を作成してから、マージを依頼します。

> [!NOTE]
> GitHub 上で Copilot に操作を依頼するのは今回が初めてです。プロジェクト テンプレートでは GitHub のツールがあらかじめワークスペースに接続されています。Copilot が初めてツールを使うときに、VS Code から GitHub へのサインインを求められることがあります。プロンプトに従ってください。後の演習でこの接続の仕組み (Model Context Protocol) を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 命令ファイルの更新と、次のフィルタリング作業に向けた publishers ヘルパーの基礎作成の両方を説明する PR を、現在のブランチから作成するよう Copilot に依頼します。

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot から GitHub 操作の実行許可を求められた場合は、コマンドを確認して、適切な操作のみ **Allow** を選択します。
6. PR の作成後、次のプロンプトを使って PR をマージし、ローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot から GitHub 操作とシェル コマンドの実行許可を求められた場合は、コマンドを確認し、適切な操作のみ **Allow** を選択します。

GitHub Copilot の支援を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを取得する方法を確認し、Copilot Chat を使って次の作業を行いました。

- 既存の命令を使ってコード生成プロンプトを送り、Copilot の提案を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい規約に従うことを確認しました。
- 命令の更新とヘルパーの基礎作成を `main` にコミットし、次の演習に備えました。

次は[agent mode][next-lesson]で命令を使い、Copilot にコードベース全体にまたがる機能を追加させます。

## リソース

- [GitHub Copilot のカスタム命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 用のよりよいカスタム命令を作成するための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
