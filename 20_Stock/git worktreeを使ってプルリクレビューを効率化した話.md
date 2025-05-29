---
title: "git worktreeを使ってプルリクレビューを効率化した話"
source: "https://developers.freee.co.jp/entry/efficient-pr-review-with-git-worktree"
author:
  - "[[u5surf]]"
published: 2024-04-12
created: 2025-05-29
description: "共通マスタ基盤チームにおけるソフトウェアエンジニアのyugoです。 共通マスタ基盤チームは、従業員、商品、取引先といった製品横断で利用できるマスタデータを一元管理し、ユーザーにfreeeプロダクトにおける統合体験を提供できる基盤開発をミッションとしております。 そんな共通マスタ基盤チームチームですが、製品横断で利用されるとだけあり、日々の開発フローでPRレビューの割り込みが多いです。そんな中で、開発フローにgit worktreeを導入してみて、個人的にはPRレビューの割り込み作業時に割と使いやすかったので紹介します。 git worktreeを使うに至る背景 実はfreeeで働く以前、前職で…"
tags:
  - clippings
  - git
  - programming
  - tutorial
  - practical
---
共通マスタ基盤チームにおけるソフトウェアエンジニアのyugoです。

共通マスタ基盤チームは、従業員、商品、取引先といった製品横断で利用できるマスタデータを一元管理し、ユーザーにfreeeプロダクトにおける統合体験を提供できる基盤開発をミッションとしております。

そんな共通マスタ基盤チームチームですが、製品横断で利用されるとだけあり、日々の開発フローでPRレビューの割り込みが多いです。そんな中で、開発フローにgit worktreeを導入してみて、個人的にはPRレビューの割り込み作業時に割と使いやすかったので紹介します。

### git worktreeを使うに至る背景

実はfreeeで働く以前、前職で先輩シニアエンジニアが「レビューするときにgitのstagingにあげていない自分の変更を、stashしたり、テキトーにcommitしてからrebaseするなりするの嫌だったら、worktree使ったほうがいいよ」と言われて、当時は「ふーん」くらいで聞いていたのですが、 **PRレビューが多いfreeeなら使い道があるかも?**と思ってgit worktreeを使ってみました。

### git worktreeとは

複数のブランチを並行して運用することが多い時に、指定したブランチを任意のディレクトリにチェックアウトできるgitの機能です。gitのサブコマンドで、freeeで標準的に使っているというか、世の中で一般的に使われているgitコマンドさえあれば誰でも使うことができます。

公式マニュアル　 [git-scm.com](https://git-scm.com/docs/git-worktree)

### 使ってみる

#### 某マイクロサービス開発編

私の所属する共通マスタ基盤チームでは、現在、とあるマスタのマイクロサービス化を絶賛実施中で、開発レポジトリには絶賛開発中につき毎日たくさんのPRが飛んできます。開発レポジトリは、ここではmicro-serviceとして話を進めましょう。

#### 開発中にレビューがやってきた

例えば、私が、とあるマスタデータの更新APIを開発するタスクで、APIを実装している最中に、マスタデータの検索APIのクエリに関する修正PR のレビュー依頼がやってきましたとしましょう。（伏せ字が多いですが、実際にあった話です。） まず、更新APIを開発をというupdate\_apiブランチで開始する時に、毎日PR依頼が来るたびに、自分が手元でいじっているコードをstashしたりcommitしたりして、兎も角退避する生活にうんざりしていた私は、背景の件を思い出しgit worktreeを使い始めることにしました。

```
[yugo@host:/home/yugo/micro-service]% git worktree add update_api update_api
[yugo@host:/home/yugo/micro-service]% cd update_api
[yugo@host:/home/yugo/micro-service/update_api]%

変更ゴニョゴニョ

[yugo@host:/home/yugo/micro-service/update_api]% git status
On branch update_api
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   xxxxxxx
        modified:   yyyyyyy
        modified:   zzzzzzz
        ...
no changes added to commit (use "git add" and/or "git commit -a")
```

update\_apiというブランチをあらかじめチェックアウトしており、それをgit worktree addで追加するとmicro-service配下に「update\_api」というディレクトリができます。change directoryでupdate\_apiに移動します。で、update\_apiには、チェックアウトしたブランチの内容そのものがまるでコピーされたかのように出来上がります。つまりここで開発をしていけば良いということです。 開発を続けていくと、当たり前ですが、上のように差分が出来上がります。 ちょうどAPIも形になってきた頃、案の定、micro-serviceには、anonymousさんからPR依頼が来るわけです。(ここではPR番号238番が来たとしましょう。)

```
[yugo@host:/home/yugo/micro-service/update_api]% cd ../
[yugo@host:/home/yugo/micro-service]%
[yugo@host:/home/yugo/micro-service]% git fetch origin pull/238/head:pr-238
[yugo@host:/home/yugo/micro-service]% git worktree add pr-238 pr-238
[yugo@host:/home/yugo/micro-service]% cd pr-238
[yugo@host:/home/yugo/micro-service/pr-238]% git show
commit 01234567890abcdef01234567890abcdef (HEAD -> pr-238)
Author: anonymous <anonymous@freee.co.jp>
Date:   Wed Mar 27 13:17:27 2024 +0900

    :sparkles: [xxxx-123] fix xxxx query for search api.
    ref: https://anonymous-ticket-system.invalid/tickets/xxxx-123

diff --git a/foo/bar/baz b/foo/bar/baz
index deadbeef..beefdead 100644
--- a/foo/bar/baz
+++ b/foo/bar/baz
...
```

私は、一旦mainブランチのあるディレクトリに戻って、git fetchでoriginからPRの差分を取得してきます。ここで引っ張ってきた **pr-238に対して、git worktree addします。** すると、pr-238の変更をチェックアウトしたpr-238というディレクトリができるので、そのディレクトリに移動して、レビューを開始します。 つまり、 **ディレクトリの移動=ブランチの移動** となるわけです。

#### レビューが終わったら

動作確認もOK！コードも問題なさそうでLGTMをつけました。さて手元はどうするかというと、 **mainブランチのあるディレクトリに戻って、さっきのディレクトリをrmして、git worktree pruneします。** あとはいつものようにgit branch -D pr-238でもしておけば良いでしょう。

```
[yugo@:/home/yugo/micro-service/pr-238]% cd ../
[yugo@host:/home/yugo/micro-service]% rm -rf pr-238
[yugo@host:/home/yugo/micro-service]% git worktree prune
...
[yugo@host:/home/yugo/micro-service]% cd update_api
[yugo@host:/home/yugo/micro-service/update_api]% git status
On branch update_api
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   xxxxxxx
        modified:   yyyyyyy
        modified:   zzzzzzz

no changes added to commit (use "git add" and/or "git commit -a")
[yugo@host:/home/yugo/micro-service/update_api]%
```

そしてupdate\_apiのブランチ（すなわちディレクトリ）に戻れば、さっきまで作業していた変更がそのままで再び作業を開始できます。 これでgit stash listがえらいことになったり、stagingしたものが泣く泣く降ろされたり、とりあえずgit add., git commitしたがために要らない変更まで取り込んでしまったみたいなことも減らせます。普段からgit add -p でstagingするときも吟味したりする慎重派には良さそうです。

#### 某バックエンドフォーフロントエンド開発でgit worktreeを使ってハマったポイント

共通マスタのフロントエンドとなるバックエンドフォーフロントエンドサービス（ここでは便宜上bffというレポジトリ名にしましょう）も開発を進めており、最近ではこちらにも、PRがドシドシ飛んできます。 こちらも同様にgit worktreeを使っていたのですが、yarn lintで呼び出すeslintが動かせませんでした。 当初はyarn lintできないとCIが落ちるし開発できなくて、git worktree諦めなきゃならんのか？と焦りました。

```
[yugo@host:/home/yugo/bff/devlop-123]% yarn lint
yarn run vx.y.z
$ eslint ./front && prettier --check ./front

Oops! Something went wrong! :(

ESLint: x.y.z

ESLint couldn't find the plugin "eslint-plugin-react".

(The package "eslint-plugin-react" was not found when loaded as a Node module from the directory "/home/yugo/bff".)

It's likely that the plugin isn't installed correctly. Try reinstalling by running the following:

    npm install eslint-plugin-react@latest --save-dev

The plugin "eslint-plugin-react" was referenced from the config file in "../.eslintrc.js".

If you still can't figure out the problem, please stop by https://eslint.org/chat/help to chat with the team.

error Command failed with exit code 2.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
[yugo@host:/home/yugo/bff/devlop-123]%
```

よくよく、見てみるとどうもこれはmainブランチの配下に作業ブランチ用のディレクトリdevlop-123を切ってしまい、git管理されている.eslintrc.jsがdevlop-123にも含まれてしまったため発生しておりました。 任意の位置にディレクトリとしてブランチを切れるのなら、ホームディレクトリとかテキトーなところに切っても良さそうですが、なんとなくmainの配下に切りたくなるのが人情。でも、そんな時でも回避方法はあります。 親ブランチの.eslintrc.jsとnode\_moduleディレクトリを一時的に削除しておきます。

参考資料： [zenn.dev](https://zenn.dev/aiya000/articles/d527fbfe288c3f)

```
[yugo@host:/home/yugo/bff]% rm -rf .eslintrc.js
[yugo@host:/home/yugo/bff]% 
[yugo@host:/home/yugo/bff]% cd devlop-123
[yugo@host:/home/yugo/bff/devlop-123]% yarn lint
yarn run vx.y.z
$ eslint ./front && prettier --check ./front
Checking formatting...
...
All matched files use Prettier code style!
Done in xx.yys.
[yugo@host:/home/yugo/bff/devlop-123]%
```

これでbffでも開発できそうです。

#### 制約事項

個人的には推していけそうな開発フローだと思い布教していこうと思ったのですが、vscodeで使えないという悲しいissueを見ました。 [github.com](https://github.com/microsoft/vscode/issues/68038)

これどうなんですかね？私自身vscode民ではないので真偽も分からず…。

### まとめ

一旦上のはまりポイントさえどうにかなれば、個人的には割り込み作業でstashしたりcommitするために作業効率が落ちたり、誤ってテキトーにcommitしてしまったみたいな操作ミスは防げて安全性は高まった気がしてます。ディレクトリに分かれていてrmで捨てれば良い後腐れがない感じも心理的には良いです。ただし、ディレクトリが分かれていることでgit管理対象外のファイル問題などでハマるポイントはありますが…。

皆さんも時間がございましたら、トライしてみてください。

[« 新メンバーを受け入れる際に大事だなと思…](https://developers.freee.co.jp/entry/mindset-for-new-comers) [5月31日（金）・6月1日（土）、freee 技術… »](https://developers.freee.co.jp/entry/freee-tech-day-2024)