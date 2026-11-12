---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、[codespace][codespaces] を起動します。codespace は、命令ファイルの編集や cloud agent が作成した変更のレビューに使います。

## ラボ用リポジトリをセットアップする

コードの作業用に、[テンプレート][template-repository]からリポジトリのインスタンスを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれます。演習中はこのリポジトリを使います。

1. 新しいブラウザー ウィンドウで [Tailspin Toys のテンプレート][tailspin-template]を開きます。
2. ラボ リポジトリ ページで **Use this template** を選択し、次に **Create a new repository** を選択して、自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成できます。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. ラボの後半で参照するため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

## Codespace を作成する

次に、ラボの演習に使う codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、すべての機能を備えた IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手動でインストールするよりもはるかに短時間で済みます。作成を待つ間に、次に紹介する GitHub Copilot の機能を確認してもよいでしょう。

> [!CAUTION]
> 後の演習で codespace に戻ります。今はブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行するように構成されています。どちらの環境にも、演習に必要なすべての前提条件がインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、確認が表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container が VS Code によって構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成できました。また、Copilot cloud agent と一緒に作業するときに使う codespace の作成も開始しました。

## 次のステップ

次は、cloud agent が従うカスタム命令を追加します。[演習 1 - カスタム命令][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
