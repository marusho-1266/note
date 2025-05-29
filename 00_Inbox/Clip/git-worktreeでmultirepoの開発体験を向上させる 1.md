---
title: "git-worktreeでmultirepoの開発体験を向上させる"
source: "https://tech.yappli.io/entry/introduction-of-git-worktree"
author:
  - "[[shuymn]]"
published: 2022-06-01
created: 2025-05-29
description: "この記事では、複数ブランチを行き来しながら開発をするときに困ったことと、それに対処する方法として利用したgit-worktreeの紹介に加えてmultirepoでの活用例を紹介します。"
tags:
  - clippings
  - git
  - programming
  - practical
  - article
---
[エンジニア募集中！](https://www.wantedly.com/companies/yappli/projects)

![](https://cdn-ak.f.st-hatena.com/images/fotolife/s/shuymn/20220530/20220530170201.png)

サーバーサイドエンジニアの [@shuymn](https://twitter.com/shuymn) です。

2022年5月現在Yappliではサービスのソースコードをサービス単位でリポジトリとして分割するmultirepo [\*1](https://tech.yappli.io/entry/#f-0ae1d79a "polyrepoという呼称もありますが、本記事ではmultirepoで統一します。") スタイルでソースコードを管理しています [\*2](https://tech.yappli.io/entry/#f-3eaf0349 "異なるスタイルとして、1リポジトリで複数のサービスのソースコードを管理するスタイルをmonorepoがあります。") 。

この記事では、複数ブランチを行き来しながら開発をするときに困ったことと、それに対処する方法として利用した [git-worktree](https://git-scm.com/docs/git-worktree) の紹介に加えてmultirepoでの活用例を紹介します。

#### 実行環境について

本記事に記載しているGitコマンドは以下のバージョンで動作確認をしています。異なるバージョンの場合、動作が異なる可能性がありますので実行前に [公式のリファレンス](https://git-scm.com/docs) をご確認ください。

```shell
$ git --version
git version 2.36.1
```

### 困ったこと

Yappliのソフトウェアエンジニアは常にそのとき所属しているプロジェクトで担当しているタスクだけに取り組むのではなく、 [Yappdate Day](https://www.wantedly.com/companies/yappli/post_articles/134748) やインシデント対応などで一時的にそれまで取り組んでいたタスクをストップして別のタスクに取り組むことがあります。

そのような時に良く使われるGitの操作として以下の2つがあります。

- `git stash`: [git-stash](https://git-scm.com/docs/git-stash) を使う
	- `git stash save <message>` でstashエントリに名前を付けることができます
- `git commit -am "wip"`: あとで `git reset HEAD~` する前提で適当にcommitする
	- 絶対に `git reset --hard HEAD~` というように `--hard` を付けて実行してはいけません

これらのサブコマンドも便利ではありますが、どちらも **異なるブランチの作業ディレクトリ [\*3](https://tech.yappli.io/entry/#f-666f5c35 "Working Treeのこと") をローカルに同時に存在させる** ことはできません。そのため、一方のブランチで作業しているときに、そのままターミナルマルチプレクサ [\*4](https://tech.yappli.io/entry/#f-a9d59652 "tmuxなど。最近ではiTerm2などのターミナルエミュレータの機能として存在することもあります。") やエディターで別ブランチの内容を開いて作業することができません。

### git-worktree

では **異なるブランチの作業ディレクトリをローカルに同時に存在させる** ためにはどうしたらよいでしょうか？

簡単に思いつくものとしては、同じリポジトリを別々のディレクトリにcloneするというものがあります。しかしそれらをまとめて管理することはGitコマンドに標準で提供されている機能の範囲では行うことができません。

そこでGit v2.5.0で追加された [git-worktree](https://git-scm.com/docs/git-stash) を使います。git-worktreeは複数の作業ディレクトリを管理するためのコマンドです。

#### add

remoteに存在するブランチをローカルの特定のパスにcheckoutしたい場合は以下のように `add` コマンドを使います。

```bash
git worktree add <path> <branch>
```

ローカルに既に同じ名前のブランチが存在する場合、もしくはローカルでは別名を付けているがupstreamブランチとしてremoteに同名のブランチが存在する場合 [\*5](https://tech.yappli.io/entry/#f-94250887 "git checkout -b branch_b origin/branch_a というようにすると作れます(remoteがoriginの場合)。") はこのコマンドは失敗します。そのため作業途中でgit-worktreeに移動したくなった時は気合いでなんとかする必要があります。気合いの例を書いたのですが、思いのほか長くなってしまったので折りたたみます。

気合いの具体例

※ここでは派生元をmain、作業ディレクトリのブランチをdevelopとします。

##### パターン1: ローカルでブランチを切ってから一度もコミットしていない場合

コミット前のすべての差分をファイルに出力します。

```bash
git diff HEAD > develop.patch
```

mainブランチに移動します。

```bash
git switch main
```

developブランチを削除します。

```bash
git branch -D develop
```

新たなworktreeを追加してブランチ名をdevelopとします。パスは適当です。

```bash
git worktree add ~/Works/awesome-app develop
```

パッチファイルを新たに作成されたディレクトリに移動させます。一度もcommitしたことのないファイル [\*6](https://tech.yappli.io/entry/#f-e862d124 "Untracked Files") がある場合はこのタイミングで移動させておくと良いです。

```bash
mv develop.patch ~/Works/awesome-app/
```

新たに作成したディレクトリに移動した後、パッチを適用します。

```bash
cd ~/Works/awesome-app
git apply develop.patch
```

以上です。

##### パターン2: ローカルでコミットしたことがある場合

パターン1と同じやりかたでコミット前の差分をパッチファイルにする(省略)。

そして、 `git format-patch` を使ってコミットも含めたパッチをつくります。以下の例ではpatchesディレクトリにパッチファイルが出力されます。

```bash
// mainから派生させて一度もpushしたことが無い場合
git format-patch main -o patches

// remoteにある特定のブランチ(origin/develop)をcheckoutした場合
git format-patch origin/develop -o patches
```

mainブランチに移動し、ローカルのdevelopブランチを削除し、worktreeで再度developブランチを作成し、パッチファイルなどを移動させます(コマンドは割愛)。

この段階で「 `git diff` で作成したパッチ」と「 `git format-patch` で作成したパッチ」の2種類が存在しますが、この適用順は「 `git format-patch` で作成したパッチ」→「 `git diff` で作成したパッチ」の順番にします。後者については適用方法は前述の通りのため割愛します。前者については以下の通りです。

```bash
git am patches/*
```

以上です。このように、コミットしてしまったブランチを後からworktreeで復活させるのはまあまあ大変です。

#### remove

worktreeを追加するコマンドを紹介したので、次は削除するコマンドを紹介します。

```bash
git worktree remove <path>
```

もしそのworktreeでコミットしていない変更がある場合は実行に失敗しますので、安心してください。

### multirepoでの活用例

次にmultirepoでの活用例を紹介します。

特定のタスクに取り組む時に、複数リポジトリを横断して開発を行うことがあります。そのような時に、自分は以下のようなディレクトリ構成となるようにしています。

```shell
branch-1
├── repo-a
├── repo-b
└── repo-c
```

このようなディレクトリ構成にすることで、隣り合うディレクトリにあるリポジトリのブランチが統一され、開発がしやすくなります。また、ブランチ名とリポジトリ名が分かればworktreeのパスを決定することができるので、 [git-branch](https://git-scm.com/docs/git-branch) コマンドなどと組み合わせてremoteで削除済みのブランチのworktreeを削除するということもできるようになります。

### さいごに

git-worktreeについて基本のコマンド2つを紹介しました。紹介しなかったコマンドやオプションもいくつかありますので、気になった方やより発展的なユースケースを知りたい方は公式リファレンスの [git-worktree](https://git-scm.com/docs/git-worktree) の項目を確認してください。

Yappliでは全方面でソフトウェアエンジニアを募集中です。もしYappliに興味がありましたら是非カジュアル面談でお話しましょう！

[open.talentio.com](https://open.talentio.com/r/1/c/yappli/pages/59642)

[« 【小ネタ】TerraformでAWS WAFのログ取得…](https://tech.yappli.io/entry/aws_waf_logging_with_terraform) [Goのnet/httpパッケージを理解する »](https://tech.yappli.io/entry/2022/05/16/Go%E3%81%AEnet/http%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8%E3%82%92%E7%90%86%E8%A7%A3%E3%81%99%E3%82%8B)