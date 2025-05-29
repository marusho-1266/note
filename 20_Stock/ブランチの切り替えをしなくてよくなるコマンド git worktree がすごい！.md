---
title: "ブランチの切り替えをしなくてよくなるコマンド git worktree がすごい！"
source: "https://qiita.com/shibukk/items/80430b54ecda7f36ca44"
author:
  - "[[shibukk]]"
published: 2016-01-12
created: 2025-05-29
description: "masterブランチでちょっとした変更をしたいんだけど、devブランチで作業しているから切り替えるのはめんどくさい。そんな時はgit worktreeを使ってみてください。複数のブランチを切り替え…"
tags:
  - clippings
  - git
  - programming
  - basic
  - article
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

この記事は最終更新日から3年以上が経過しています。

[@shibukk](https://qiita.com/shibukk)

最終更新日 投稿日 2016年01月12日

masterブランチでちょっとした変更をしたいんだけど、devブランチで作業しているから切り替えるのはめんどくさい。  
そんな時は **git worktree** を使ってみてください。複数のブランチを切り替えずに操作できます。  
※git worktreeを使うにはGit 2.7.0以降が必要です。それ以前のバージョンだとgit worktree listがありません。

## ブランチの追加

操作したいブランチはこんな風に持ってくることができます。

```bash
git worktree add <作業ディレクトリ> <ブランチ名>
```

すると、作業ディレクトリ以下に指定したブランチの中身がリンクされます。  
たとえば、 `git worktree add ./worktree-dev dev` とコマンドを打つと、./worktree-devディレクトリが作成され、その下にdevブランチの内容ができるわけです。

git worktreeは今いるブランチに **影響を与えません** 。  
つまり、作業中であっても操作したいブランチを持ってこれます（厳密にはsubmoduleと同じリンクディレクトリができます）。

commitなどはさっきの作業ディレクトリ以下に移動して行うというルールがあるだけで、基本的には今まで通りでOKです。

ちなみに、すでにブランチがある場合は作業ディレクトリを省略すると、ブランチ名のディレクトリで作成してくれます。

```bash
git worktree add dev
```

## ブランチの削除

使い終わったブランチは作業ディレクトリを削除してください。  
そののちに

```bash
git worktree prune
```

とコマンドを打てばキレイになくなります。

## ブランチの一覧

現在リンクされているブランチを確認するにはこんな感じ。

```bash
git worktree list
# こんな感じで一覧が見れます
# /Users/shibukk/git-worktree-test               6d44d5d [master]
# /Users/shibukk/git-worktree-test/worktree-dev  894cec9 [dev]
```

## 応用編

git worktreeを使えば、さくっと修正してpull requestを投げるためのブランチを1行で作れます。  
`-b` は新規ブランチを一緒に作成するオプションです。

```bash
git worktree add hotfix-1 -b hotfix-1
```

なので、bashrcにこんな感じで指定のディレクトリにブランチを作成する関数を用意しておき、`.gitignore` でそのディレクトリを無視するようにしておくと便利です。

.bashrc

```bash
function gwt() {
    GIT_CDUP_DIR=\`git rev-parse --show-cdup\`
    git worktree add ${GIT_CDUP_DIR}git-worktrees/$1 -b $1
}
```

.gitignore

```bash
git-worktrees/*
```

```bash
# これだけでpull request用のブランチができる
gwt <新規ブランチ名>
```

なお、 `git rev-parse --show-cdup` でリポジトリのrootを探すようにしているので、どのディレクトリにいてもgit-worktrees以下に作ってくれます。

ちなみに移動や削除もあると便利なので、以下の記事のシェルスクリプトを参考にすると良さそうです。  
[git worktreeの操作を便利にする](https://qiita.com/mh_mobiler/items/1378eb037d72064772af#zshrc%E3%81%AB%E8%A8%98%E8%BC%89%E3%81%AE%E3%82%B7%E3%82%A7%E3%83%AB%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%97%E3%83%88%E5%85%A8%E6%96%87)

## 注意点

ローカルブランチがないとエラーになります。  
リモートブランチにはあるという場合は、 `-b` を使えば問題ありません。

また、既に作業ツリーが存在しているブランチをチェックアウトしようとすると `fatal: 'hotfix-1' already exists` みたいなエラーになるので、このエラーが出た場合は `git worktree list` を使って作業ツリーがないか確認してみてください。  
作業ツリーがあった場合は上記のブランチ削除を実行すればOKです。

[1](https://qiita.com/shibukk/items/#comments)

コメント一覧へ移動

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fshibukk%2Fitems%2F80430b54ecda7f36ca44&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fshibukk%2Fitems%2F80430b54ecda7f36ca44&realm=qiita)

[![shibukk](https://qiita-user-profile-images.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F37435%2Fprofile-images%2F1473687424?ixlib=rb-4.0.0&auto=compress%2Cformat&lossless=0&w=128&s=2d59eb582a55830153282b408a90136d)](https://qiita.com/shibukk)

[

## @shibukk

](https://qiita.com/shibukk)

東京でCTOとPdM業をちょっとだけやってます。 https://www.instagram.com/shibukkk/

[Web](https://shibukk.github.io/about/) [RSS](https://qiita.com/shibukk/feed)

[763](https://qiita.com/shibukk/items/80430b54ecda7f36ca44/likers)

いいねしたユーザー一覧へ移動

709