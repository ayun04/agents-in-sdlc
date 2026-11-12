---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。タスクを特定の方法で行う必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパスを対象とした命令ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメントの規約が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する規約を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムに並べて効果を示すのが難しいためです。後の演習で cloud agent が作成するプルリクエストをレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。

- データ層には必ず単体テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- TSDoc doc comment を使って、コードにドキュメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使い、Copilot がこれらの開発方針に沿ってタスクを進めるための情報を提供します。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件に関するコンテキストや好みを Copilot に伝えられます。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定することで、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定のタスクやファイルの種類に命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の命令ファイルです。技術スタック、開発対象の概要、ベスト プラクティス、全般的なガイダンスなど、Copilot Chat や CLI に送る多くのリクエストに関係するプロジェクト情報を記載します。
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

## リポジトリ全体に適用する規約を追加する

ここで行う編集は、TSDoc doc comment とファイル レベルのコメント ヘッダーでコードを文書化するという、プロジェクト全体の規則を追加することです。後の演習で Issue を割り当てると、cloud agent がこの規則に従います。

編集の前に、作業用のブランチを作成して切り替えます。cloud agent は Issue の対象となるブランチから命令を読み込みますが、変更を整理してコミットできるよう、ここではブランチを作成します。

1. Codespace のターミナルで、次のコマンドを実行します。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に提供します。コーディング規約を文書化し、コード提案の品質を高めます。

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

## 命令の変更をコミット、プッシュ、マージする

Cloud agent は、Issue が対象としているブランチの命令ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチを作成します。そのため、命令の変更を `main` にマージしておく必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で Issue に割り当てると、cloud agent はこの命令を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せず、変更を `main` にコミットしてもかまいません。ここでは実際のプロジェクトで行う方法に近づけるために、ブランチを使っています。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の規則を追加しました。残りのハーネスで cloud agent が作成するプルリクエストをレビューするときに、この規則の効果を確認します。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 用のよりよいカスタム命令を作成するための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
