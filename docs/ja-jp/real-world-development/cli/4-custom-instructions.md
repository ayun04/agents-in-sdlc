---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント作成の規約を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際は、コンテキストが重要です。タスクを特定の方法で行う必要がある場合、そのガイダンスを Copilot が参照できるようにします。[命令ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルタリング機能を構築したので、Copilot が参照した命令を確認し、ドキュメント作成の規約を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの命令とパスを対象とした命令ファイルがエージェントに適用される方法を確認します。
- コーディング規約を守るために命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発方法に関するガイドラインがあります。

- コメントでは、コードの言い換えではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を付けます。`db` 引数を注入する関数では、その引数も説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードが変更された際にコメントも最新に保ちます。
- 既存の書式と lint のガイダンスを維持します。

命令ファイルを使って、Copilot がこうしたガイドラインに沿って作業するための情報を提供します。

## 命令ファイル

カスタム命令を使うと、コーディング スタイルや要件に関するコンテキストを Copilot に渡せます。リポジトリ全体、または特定のファイルやタスクに命令を適用できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に渡されます。多くのチャットや CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、言語、ファイルの種類、タスクに応じたガイドラインを記載します。

> [!NOTE]
> その他の命令形式とサポート状況は環境によって異なります。特定の形式を使用する前に、[カスタム命令のサポート][custom-instructions-support]を確認してください。

## プロジェクトのカスタム命令を確認する

スターター プロジェクトには命令ファイルが用意されています。変更前に内容を確認します。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。これらの命令は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに関する命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、適用対象のファイルを決める `applyTo` フロントマターを確認します。
6. `.github/instructions/drizzle.instructions.md` の他の命令ファイルやプロジェクト ファイルへのリンクを確認します。命令を小さく再利用可能なファイルに分け、Copilot が参照できる例を指定できます。

## チームのガイダンスに合わせて命令を更新する

既存の命令に、追加で TypeScript に TSDoc コメントを付けるという要件を加えます。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。およそ 35 行目にあります。
2. 最後の箇条書きに次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。フィルタリングのコンテキストを保ったまま新しい命令を読み込むため、会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートから `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に次を依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを取得する方法を確認し、新しい規約を機能に適用しました。具体的には、リポジトリの命令とパス単位の命令ファイルを確認し、命令を更新して、フィルタリングのコードへの影響を確認しました。

次は[再利用できる quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI 用のカスタム命令の追加][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
