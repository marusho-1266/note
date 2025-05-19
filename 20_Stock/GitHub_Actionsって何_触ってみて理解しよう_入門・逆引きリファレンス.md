---
title: "GitHub Actionsって何？触ってみて理解しよう！入門・逆引きリファレンス"
author: "s3i7h"
source: "https://qiita.com/s3i7h/items/b50ceb0008edc3c0312e"
created: 2025-05-16
published: 2022-07-19
tags: #programming #github #ci-cd #article #basic
---

# GitHub Actionsって何？触ってみて理解しよう！入門・逆引きリファレンス

## ある日のこと

「さーて、今日もGitHubにコミットをプッシュしていくぞ〜〜」  
「ローカルでコミットした変更を `git push origin main` して、、」  
「 `github.com` のレポジトリを見にいくと、、お！反映されているな！ `Initial Commit` ってちゃんと出ているぜ！」  
「そういえば、いつも気にしていなかったけど `Actions` タブってのがあるな？これってなんだ？」

これがGitHub Actionsです。 **レポジトリごと** に用意されていて、Actionsタブから管理、確認することができます。

「ほ〜。 `GitHub Actions` っていうのか・・なんのためにあるんだろう？ここで何ができるの？」

## GitHub Actionsとは

GitHub ActionsはGitHubがサービスの一環として提供する、ワークフロー自動化サービスです。

簡単に言えば、「開発している時にやりたいことを、GitHub上で自動化できるサービス」です。

「なるほど・・つまりGitHubを **コード置き場** として使うだけじゃなくて、手元でやるような、よくある手順を **GitHub側で自動で実行できる** んだ？」  
「なんか便利そうだけど、自動化って難しそうだし、あんまり役立つことのイメージがつかないなあ」

## 触ってみよう

簡単にGitHub Actionsに触れてみましょう！

先ほど触っていたレポジトリにGitHub Actionを導入するところから始めてみます

まず、Actionsタブを開きます。レポジトリになにもGitHub Actionsが設定されていない場合、次のような画面が出てきます。既に何かしらのワークフローが存在する場合は左上の `New Workflow` から同じような画面を開くことができます。

下の方にある簡単設定ボタンを使ってもいいのですが、今回は `set up a workflow yourself` を押してみましょう。

このような画面が出てきて、何やらyamlファイルを編集する画面になります。とりあえず中身は読み飛ばして、コミットしてみましょう。

適切なコミットメッセージを追加し、Commit new fileを押すと、GitHub上で新しいコミットを追加することができます。

こうすることで、始めてのGitHub Actionを走らせることに成功しました！  
Actionsタブを改めて開いてみましょう！

先ほど定義したアクションが無事に動いていることがわかります。

「なんかよくわからないけど、思ったより簡単にワークフローを作れたみたいだ」  
「何が起こっているのかはよくわからないけど、左の緑のチェックマークが気持ちいい！」  
「中で何が起こっているのかもうちょっと詳しく見てみたいな」

## 中身の概要

先ほど読み飛ばしたmain.ymlの中身を見てみましょう！

コード

.github/workflows/main.yml

```yaml
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the "main" branch
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v3

      # Runs a single command using the runners shell
      - name: Run a one-line script
        run: echo Hello, world!

      # Runs a set of commands using the runners shell
      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.
```

このyamlファイルは大まかに分けると二つのパートで構成されています。

1. ワークフローについての情報を定義するパート
2. ワークフローの中身を定義するパート

## ワークフローについての情報を定義するパート

ここの部分が該当します。

`name:`ではワークフローの名前を定義しています。ここで定義した名前がActionsタブにも表示されます。

`on:`では、どのタイミングでこのワークフローが走るかが定義されています。今回のワークフローでは

1. 何かしらのコミットがGitHubに追加された時（ `push` ）  
　・ただし、mainブランチのみ（ `branches: ["main"]` ）
2. 新しくプルリクエストが作成された時(`pull_request`)  
　・ただし、mainブランチのみ(`branches: ["main"]`)
3. 明示的に走って！とボタンを押したタイミング（ `workflow_dispatch` ）

の三種類のタイミングで走るよ、と定義されています。

「なるほど、ここでタイミングが定義されているのか」  
「さっき走っていたワークフローのrunをクリックしてみてみると・・」

「確かにmainブランチに対しての `on: push` で走った、って書いてある！」  
「つまり、GitHub Actionはyamlファイルの中身を読んで、 **該当するケースに当たったら** 処理を始めてくれるのね」

## ワークフローの中身を定義するパート

ワークフローでどういう処理が行われるかはyamlの後半部分が該当します。

`jobs:`では一つのワークフローの中に含まれる一つ一つの一連の処理を定義します。現在のワークフローには `build:`というジョブが一つ存在しています。

`(ジョブ名):`の中にそれぞれのジョブの設定、処理内容を書きます。

- `runs-on:`では、どのような仮想環境でジョブの処理を動かすかを定義します。今回はubuntuの最新版を使う、となっています。
- `steps:`に具体的な一つ一つの処理を書いていきます。全部で3つの処理をしています。
	- 最初のステップでは、外部で定義された別のアクションを呼び出しています。このステップでは `actions/checkout@v3` つまり、自分自身のレポジトリのデータを読み込む、という処理が走っています。このステップから始まるワークフローが一番多いと思われます。（こういった外部呼び出しされるアクションはGitHubで公開されています→ [actions/checkout](https://github.com/actions/checkout) ）
	- 次のステップでは単一のシェルスクリプト（ `echo Hello, world` ）が走っています。
	- 最後のステップでは、複数のシェルスクリプトが走っています。

これらのジョブ、ステップはActionsタブから実行結果を確認することができます。

`build` というジョブが見えますね。これをさらにクリックすると、このような画面が表示されます。

前後にセットアップ処理などが含まれていますが、ちゃんと定義した三つのステップが実行されているのが見えます。

「ふむふむ、一つのワークフローの中には **ジョブ** というものとその中で動く **ステップ** というものがあるのか」  
「ためしにジョブを一個増やしてみよう」  
「 `build` ジョブの下に新しく `hoge` ジョブを作ってみた！これでコミットしてみると、、」  
「おお！ジョブが増えたぞ！しかも並列に実行されてるっぽい！」  
「こうやって **並列で動くジョブ** と **順番に動くステップ** を組み合わせて一つのワークフローとまとめていくんだな。完全に理解した！」

## GitHub Actionsでできることの紹介

「でもコミットごとに **あいさつ** をしてもらっても、ちょっと、、って感じだな。。」  
「具体的に何ができるのか知りたいな」

このパートでは、逆引き的にGitHub Actionsで達成できることを紹介していきます

この記事では例としてPythonを使ったプロジェクトでの解説を行いますが、Pythonに限らず他の言語でも共通して利用できるような内容にフォーカスしています

## 自動テストの実行

「最近、プロジェクトに **自動テスト** が導入されたんだよな」  
「手元で実行できるけど、これが自動的に動いてくれたら便利かも」

プロジェクトに自動テストが導入されると、コードの動作についての結果や品質を簡単に担保することができるようになります。この自動テストをローカルだけでなく、GitHub Actions上でも実行することで、継続的にコードの動作をモニタリングすることができるようになります。例として以下のような簡単なpythonのプロジェクトを考えてみます。

```text
project/
  src/
    main.py
  tests/
    test_main.py
  pyproject.toml
```

src/main.py

```python
import sys

def add(a: int, b: int) -> int:
    return a + b

def main():
    if len(sys.argv) <= 2:
        print("a and b is required")
    _, a, b, *_ = sys.argv
    print(add(*map(int, (a, b))))

if __name__ == "__main__":
    main()
```

src/test\_main.py

```python
from unittest import TestCase

from src.main import add

class MainTestCase(TestCase):
    def test_add(self):
        self.assertEqual(5, add(2, 3))
```

この自動テストを実行すると、以下のようになります

これをGitHub Actionsに組み込んでみましょう！ 