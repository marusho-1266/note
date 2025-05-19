---
title: "GitHub Actionsって何？触ってみて理解しよう！入門・逆引きリファレンス"
source: "https://qiita.com/s3i7h/items/b50ceb0008edc3c0312e"
author:
  - "[[Qiita]]"
published: 2022-07-19
created: 2025-05-16
description: "ある日のこと「さーて、今日もGitHubにコミットをプッシュしていくぞ〜〜」「ローカルでコミットした変更をgit push origin mainして、、」「github.comのレポジトリを…"
tags:
  - "clippings #inbox"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

この記事は最終更新日から1年以上が経過しています。

## ある日のこと

「さーて、今日もGitHubにコミットをプッシュしていくぞ〜〜」  
「ローカルでコミットした変更を `git push origin main` して、、」  
「 `github.com` のレポジトリを見にいくと、、お！反映されているな！ `Initial Commit` ってちゃんと出ているぜ！」  
「そういえば、いつも気にしていなかったけど `Actions` タブってのがあるな？これってなんだ？」

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/65a73a03-02f7-c610-c6cd-05851c365d92.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F65a73a03-02f7-c610-c6cd-05851c365d92.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=93a50d04eb967da14577ef534c65e198)

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

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/75579dd8-71d7-8def-ed1e-6fcc74a3d687.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F75579dd8-71d7-8def-ed1e-6fcc74a3d687.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e00bbaa43a3aabdaabe8b702e916a1bf)

下の方にある簡単設定ボタンを使ってもいいのですが、今回は `set up a workflow yourself` を押してみましょう。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/008c334e-2880-4b9d-6c9c-32c805188c83.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F008c334e-2880-4b9d-6c9c-32c805188c83.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=a4e879c3af3740706c0aba0676d22e38)

このような画面が出てきて、何やらyamlファイルを編集する画面になります。とりあえず中身は読み飛ばして、コミットしてみましょう。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/93430eaa-264c-157c-0a36-eaebf6170264.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F93430eaa-264c-157c-0a36-eaebf6170264.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=138643a0fb374f8d43c31779310cc3a7)

適切なコミットメッセージを追加し、Commit new fileを押すと、GitHub上で新しいコミットを追加することができます。

こうすることで、始めてのGitHub Actionを走らせることに成功しました！  
Actionsタブを改めて開いてみましょう！

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/ac2aae12-2fa7-c056-59a0-daf1762af63f.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2Fac2aae12-2fa7-c056-59a0-daf1762af63f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=466f9b3a90325b3b465dde725a22531e)

先ほど定義したアクションが無事に動いていることがわかります。

「なんかよくわからないけど、思ったより簡単にワークフローを作れたみたいだ」  
「何が起こっているのかはよくわからないけど、左の緑のチェックマークが気持ちいい！」  
「中で何が起こっているのかもうちょっと詳しく見てみたいな」

## 中身の概要

先ほど読み飛ばしたmain.ymlの中身を見てみましょう！

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/906928e7-bb5d-01eb-d07d-f541df77cf60.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F906928e7-bb5d-01eb-d07d-f541df77cf60.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2bda5c4766157a83df41f9fe7509bc96)

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

具体的には  
[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/0cd038bf-c3e2-4234-b5c9-9536dce7ea38.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F0cd038bf-c3e2-4234-b5c9-9536dce7ea38.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=68d68883e43e20e87e41155f0069c586)

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

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/2ae7bb58-457c-9da0-0010-1d377029695a.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F2ae7bb58-457c-9da0-0010-1d377029695a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fa9553843550f83a4f78b68015e04a75)

「確かにmainブランチに対しての `on: push` で走った、って書いてある！」  
「つまり、GitHub Actionはyamlファイルの中身を読んで、 **該当するケースに当たったら** 処理を始めてくれるのね」

## ワークフローの中身を定義するパート

ワークフローでどういう処理が行われるかはyamlの後半部分、

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/abf25852-060e-e50f-ed70-a134d4a7600c.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2Fabf25852-060e-e50f-ed70-a134d4a7600c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=df7de41a1a9d6915a675f8d9a7b7fd3a)

ここが該当します。

`jobs:`では一つのワークフローの中に含まれる一つ一つの一連の処理を定義します。現在のワークフローには `build:`というジョブが一つ存在しています。

`(ジョブ名):`の中にそれぞれのジョブの設定、処理内容を書きます。

- `runs-on:`では、どのような仮想環境でジョブの処理を動かすかを定義します。今回はubuntuの最新版を使う、となっています。
- `steps:`に具体的な一つ一つの処理を書いていきます。全部で3つの処理をしています。
	- 最初のステップでは、外部で定義された別のアクションを呼び出しています。このステップでは `actions/checkout@v3` つまり、自分自身のレポジトリのデータを読み込む、という処理が走っています。このステップから始まるワークフローが一番多いと思われます。（こういった外部呼び出しされるアクションはGitHubで公開されています→ [actions/checkout](https://github.com/actions/checkout) ）
	- 次のステップでは単一のシェルスクリプト（ `echo Hello, world` ）が走っています。
	- 最後のステップでは、複数のシェルスクリプトが走っています。

これらのジョブ、ステップはActionsタブから実行結果を確認することができます。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/5357e814-41dc-bb44-1a92-7fbe5aabc553.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F5357e814-41dc-bb44-1a92-7fbe5aabc553.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=b6499d8d9cf877ab084ca56cf75f9ac8)  
緑のチェックマークが付いている場所をクリックしてみるとこのような画面になります。  
[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/76f22b12-9e78-de60-0d19-40b400390097.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F76f22b12-9e78-de60-0d19-40b400390097.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ac4bc83946a0036be39d7464bf1f5fb8)

`build` というジョブが見えますね。これをさらにクリックすると、このような画面が表示されます。  
[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/12901c0d-bdbf-1117-82f7-2b61a728135d.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F12901c0d-bdbf-1117-82f7-2b61a728135d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=77ffa44e2db368e6a48787cd9d95aa7b)

前後にセットアップ処理などが含まれていますが、ちゃんと定義した三つのステップが実行されているのが見えます。

「ふむふむ、一つのワークフローの中には **ジョブ** というものとその中で動く **ステップ** というものがあるのか」  
「ためしにジョブを一個増やしてみよう」  
[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/c0a64378-4a46-9aa1-5b72-677c0fbc74a8.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2Fc0a64378-4a46-9aa1-5b72-677c0fbc74a8.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=586d92a6fd3c1bed9aa6e1230558006f)  
「 `build` ジョブの下に新しく `hoge` ジョブを作ってみた！これでコミットしてみると、、」  
[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/31dc098d-44bd-5075-60db-84f2f2372138.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F31dc098d-44bd-5075-60db-84f2f2372138.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=5c8ea2225d50c4c0216d14d91688ba89)  
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

src/main.py

```python
from unittest import TestCase

from src.main import add

class MainTestCase(TestCase):
    def test_add(self):
        self.assertEqual(5, add(2, 3))
```

この自動テストを実行すると、以下のようになります

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/a2992b88-6efc-a11a-8b35-2f260dce28aa.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2Fa2992b88-6efc-a11a-8b35-2f260dce28aa.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=bb6b35890bbeecbe551e5d9eb9fe1e67)

これをGitHub Actionsに組み込んでみましょう！  
`test.yml` を作成してみます

.github/workflows/test.yml

```yaml
name: Run Tests

on:
  push:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Python Setup
        uses: actions/setup-python@v1
        with:
          python-version: 3.7

      - name: Setup Poetry
        uses: Gr1N/setup-poetry@v7

      - name: Run poetry install
        run: poetry install

      - name: Run tests
        run: poetry run python -m pytest
```

これをプッシュすると次のように、 `Run Tests` というワークフローが新しく実行されていることを確認できます。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/1a8a765b-4f46-94c3-f528-935b2c776ec1.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F1a8a765b-4f46-94c3-f528-935b2c776ec1.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f2710a187658b036c53114da1bc5dff1)

中身をみると、正しくローカルで実行したものと同じ結果が得られていますね！

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/c6b82949-b50a-8958-b876-bc65f782dd92.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2Fc6b82949-b50a-8958-b876-bc65f782dd92.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8723438cd658cbc4a816309698010eca)

「おお〜！こうすれば手元で毎回実行しなくても、 **プッシュした時に毎回自動テストを走らせてくれる** んだ！」  
「これは便利だ」  
「なるほど、手元で実行するときはセットアップが終わってるからコマンド一発だけど、GitHub Actionsだと **まっさらな状態からスタートするから、事前準備が必要** なんだね」  
「まずレポジトリをチェックアウトして」...(1)  
「Pythonをセットアップして」...(2)  
「パッケージマネージャのPoetryをインストールしたら」...(3)  
「プロジェクトに必要なパッケージを `poetry install` で入れて」...(4)  
「...最後にpytestを走らせているのね。面倒臭いけど、考えてみると当たり前のステップだね」  
「新しいメンバーの **環境構築** を手伝ってあげている感覚かも」

## プルリクエストにアノテーションをつける

「そういえば自動テストと一緒に **リンター** も導入されたんだよな」  
「コードのフォーマットをみんなで統一してくれるすごいやつなんだけど、こいつを走らせられると便利かもな」  
「あ〜でも、走らせて落ちたところでActionsの中身まで確認しに行ってどこに問題があるのか探すのは結構骨が折れるかも・・」

そんな悩みも解決できます。 **Annotations** という仕組みを使うことで、コードに対してコメントのような形でツールの処理結果を追加することができます。

先ほどと同じように `lint.yml` を作成します。Pythonのflake8というリンターを使い、コードの問題点をチェックするとします。

.github/workflows/lint.yml

```yaml
name: Lint

on:
  pull_request:

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Python Setup
        uses: actions/setup-python@v1
        with:
          python-version: 3.7

      - name: Setup Poetry
        uses: Gr1N/setup-poetry@v7

      - name: Run poetry install
        run: poetry install

      - name: Lint
        run: |
          echo "::add-matcher::.github/workflows/flake8-problem-matcher.json"
          poetry run python -m flake8 .
```

この時の `poetry run python -m flake8 .`の実行結果は次のようになります。

```bash
$ poetry run python -m flake8 .
./src/main.py:9:24: E222 multiple spaces after operator
./src/main.py:9:31: E261 at least two spaces before inline comment
```

このように問題があるファイルと行番号・列番号とエラーの詳細を出力してくれますが、GitHub Actionsではこのチェックを自動化しつつ、Annotationsを使うことで問題のある箇所をコード上で確認できるようになります。

[Problem Matcher](https://github.com/actions/toolkit/blob/master/docs/problem-matchers.md) という機能を使います。この機能は、出力されるメッセージをパースし、条件にあった場合にその行をハイライトした上で、Annotationsに変換してくれる、というものになります。

今回はこのようなファイルを用意します。

.github/workflows/flake8-problem-matcher.json

```json
{
  "problemMatcher": [
    {
      "owner": "flake8",
      "severity": "warning",
      "pattern": [
        {
          "regexp": "^(.+?):([0-9]+?):([0-9]+?): ([A-Z][0-9]+) (.+)$",
          "file": 1,
          "line": 2,
          "column": 3,
          "message": 5,
          "code": 4
        }
      ]
    }
  ]
}
```

先ほどのエラーメッセージを正規表現でマッチさせ、ファイル名、行番号、列番号、メッセージ、エラーコードに当たる箇所をグループマッチの番号で指定します。

そして、次の行を足すことで、このmatcherを追加してね、という指示を出すことができます。

.github/workflows/lint.yml

```diff_yaml
- name: Lint
        run: |
+         echo "::add-matcher::.github/workflows/flake8-problem-matcher.json"
          poetry run python -m flake8 .
```

これらを設定したものをプッシュし、新しいプルリクエストを開いてみた結果がこちらになります。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/a39b48a4-c913-3701-0885-721dee56a37f.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2Fa39b48a4-c913-3701-0885-721dee56a37f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ec94ebb15ce2fe224373cced1a4886e1)

まず、 **Annotations** の中に、flake8のエラーの結果が含まれていることがわかると思います。さらに詳細を見てみましょう。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/6183732a-2c34-b79f-fc4b-ab5ccd1803c1.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F6183732a-2c34-b79f-fc4b-ab5ccd1803c1.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6f52ea922d54a65018d8fca002249f01)

正しくflake8の出力結果がwarningとして解析されていることがわかります。そしてこの状態でプルリクエストのファイル一覧を開くと・・・

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/ade8bf71-620e-fc7f-2c3f-0230a86650cc.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2Fade8bf71-620e-fc7f-2c3f-0230a86650cc.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d821ccdba50c1676852efaa25d529939)

問題のある箇所が一目瞭然にコメントが付けられます。

「おおお！これを使えばActionの実行を意識しなくても、レビューするときに簡単に問題のある箇所を見つけられる！」  
「これは **自動テスト** とか **静的解析** にも応用できそう・・」

ここでは簡単にアノテーションを作成できることを示すためにProblem Matcherを利用しましたが、今回で言うと [https://github.com/tayfun/flake8-your-pr](https://github.com/tayfun/flake8-your-pr) など、既製のアクションが用意されている場合も多いので一度検索してみてください！

## Artifactsの作成

**テストカバレッジ**  
「自動テストが書いてあると、動作確認が楽だなあ〜」  
「あれ？テストは通ってるのに新機能を使ってみたら落ちちゃった・・」  
「どれどれ、うわ！新機能作ったはいいけどテストを書いてなかったんだ！」  
「あちゃ〜、気づかなかったなあ〜」

こんなことは自動テストを書いていてもよく起こります。このような事態を回避するために使えるのが **テストカバレッジ** です

「テストカバレッジ・・？そういえばテストを書くときにそんなことも言われていたような・・・」

**テストカバレッジ** （あるいは単にカバレッジとも）はテストを実行したときに、具体的にテストされるコードのどの部分が実行されたかを確認できる仕組みです。

```bash
$ poetry add -D pytest-cov
$ poetry run -m pytest --cov --cov-report=html
```

このように実行すると、テスト中にどこの処理が走ったかを可視化することができます

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/2775b2a6-4f45-bece-481a-55513940b0d2.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2F2775b2a6-4f45-bece-481a-55513940b0d2.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=abc93f494b6204411c38c668eccb33a1)

「おおお！こうすることでaddはちゃんとテストが通っているのにmulは通っていないことが一目瞭然だ！」  
「掛け算を期待する場面なのに足し算のままになっているのね・・テストを書かなきゃ！」

**GitHub ActionsのArtifacts**  
「でも、これを自動化しようとすると難しいよな・・」  
「CIの実行画面でも見にくいだろうし、Annotationsだとちょっとたくさん書き出されすぎて、それも辛そう・・」  
「できれば吐き出されるhtmlファイルをそのままみたいな〜」

という要望も [**Artifacts**](https://docs.github.com/ja/actions/using-workflows/storing-workflow-data-as-artifacts) を使うと実現できます。Artifactsを使うことでJobの成果物をActionsの結果に紐づけて後からダウンロードできるようになります。

coverage.ymlを追加してみましょう

.github/workflows/coverage.yml

```yaml
name: Collect Coverages

on:
  pull_request:

jobs:
  coverage:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Python Setup
        uses: actions/setup-python@v1
        with:
          python-version: 3.7

      - name: Setup Poetry
        uses: Gr1N/setup-poetry@v7

      - name: Run poetry install
        run: poetry install

      - name: Collect Coverages
        run: poetry run python -m pytest --cov --cov-report=term --cov-report=html

      - name: Upload Artifacts
        uses: actions/upload-artifact@v3
        with:
          name: coverage
          path: htmlcov/
```

このように書くことで、pytestが生成してくれる `htmlcov` というディレクトリをCI上でアップロードしてくれます。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/39754/e47d639e-0888-31e5-98d6-b0c2ca157202.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F39754%2Fe47d639e-0888-31e5-98d6-b0c2ca157202.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=646266d004657dbe29eb3ac6dd0a65dd)

「お、coverageっていうArtifactができてる！こいつをクリックすると...」  
「zipファイルがダウンロードされた！」  
「中身を見るとちゃんとさっき手元でテストを走らせたときに生成された内容と一致してる！」  
「これによって、 **生成物がある処理** もCI上で走らせて **後からスイスイダウンロード** できるんだ！めっちゃ便利！」

## リリース作成・コミット追加

To Be Written

## アクション・ワークフローの作成・呼び出し

To Be Written

## まとめ

「なるほど、GitHub Actionsを使うことで、 **何回も走らせることになる処理** を **人間の手によらず** に効率的に回せるようになるんだ」  
「そう考えるといろんな使い道が思いつくかも！」

とても簡単に導入できるCIサービス、GitHub Actionsを使いこなし、より快適な開発体験を作ってみましょう！

## レポジトリ

今回使ったデモ用のレポジトリはこちらになります

[2](https://qiita.com/s3i7h/items/#comments)

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fs3i7h%2Fitems%2Fb50ceb0008edc3c0312e&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fs3i7h%2Fitems%2Fb50ceb0008edc3c0312e&realm=qiita)

[554](https://qiita.com/s3i7h/items/b50ceb0008edc3c0312e/likers)

465