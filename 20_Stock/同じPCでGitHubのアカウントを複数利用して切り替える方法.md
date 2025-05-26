---
title: "同じPCでGitHubのアカウントを複数利用して切り替える方法"
source: "https://qiita.com/manzoku_bukuro/items/c6fc9cbc069fe4a1b776"
author:
  - "[[manzoku_bukuro]]"
published: 2022-12-01
created: 2025-05-26
description: "はじめに私は1つのPCで5つのGitHubのアカウントを切り替えています。何故そんなSNSの裏垢のような使い方を…？ と思われるかもしれませんが、諸事情があるので触れないでください。(?)冗談…"
tags:
  - "clippings"
  - "tool"
  - "git"
  - "ssh"
  - "practical"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

この記事は最終更新日から1年以上が経過しています。

## はじめに

私は1つのPCで5つのGitHubのアカウントを切り替えています。

*何故そんなSNSの裏垢のような使い方を…？* と思われるかもしれませんが、 **諸事情があるので触れないでください。** (?)

冗談はさておき、社用アカウントと私用アカウントで分けている等GitHubのアカウントを複数持つ人も少なくないと思います。複数アカウントがある場合、同じPCでGitHubアカウントを切り替えるには少し設定が必要だったのでこちらで手順を紹介します。

## この記事の対象者

- GitHubで複数のアカウントを所持していて、リポジトリ毎にアカウントを切り替えたい人
- GitHubのアカウントをTwitterの裏垢のように運用したい人
- **GitHubのアカウントを切り替えて怪しい活動をしたい人**

## GitHubのアカウントの切り替え手順

### 前提

companyというGitHubアカウントとpirvateというGitHubアカウントを所持していると想定します。

## 1つのアカウントに紐付ける秘密鍵と公開鍵を生成する

まずはcompanyアカウントの秘密鍵と公開鍵を生成します。鍵周りは `~/.ssh` ディレクトリを利用しますので、存在していないのであれば生成してください。

```bash
$ mkdir ~/.ssh # .sshディレクトリが存在しない場合
$ cd ~/.ssh # ~/.sshディレクトリに移動
$ ssh-keygen -t rsa -f <ファイル名> # ex) ssh-keygen -t rsa -f company
$ ls # <ファイル名> と <ファイル名>.pub が生成されているか確認
```

以上のコマンドを実行すると秘密鍵と公開鍵が生成されます。companyというファイル名を指定した場合、 `company` という秘密鍵ファイルと、 `company.pub` という公開鍵ファイルが生成されます。

## ~/.ssh/configファイルに設定を書き込む

※.gitconfigファイルとは別物なので注意  
次に~/.sshディレクトリにconfigというファイルを作成し、そこに先ほど生成した秘密鍵の情報を登録します。

```bash
# カレントディレクトリ　~/.ssh
$ touch config # ~/.ssh/configファイルがない場合
$ vi config
```

以下configファイルの編集内容です

```bash
Host <ホスト名>
    HostName github.com
    IdentityFile ~/.ssh/<秘密鍵のファイル名>
    User git
    AddKeysToAgent yes
    UseKeychain yes

# 以下設定例
Host company
    HostName github.com
    IdentityFile ~/.ssh/company
    User git
    AddKeysToAgent yes
    UseKeychain yes

Host private
    HostName github.com
    IdentityFile ~/.ssh/private
    User git
    AddKeysToAgent yes
    UseKeychain yes
```

以上のように設定を保存してconfigの設定は完了です。  
こちらで設定したホスト名を指定することで、GitHubとのSSH認証をする際に秘密鍵のパス指定等を省略することが出来ます。

## 公開鍵をGitHub上で登録する

### 公開鍵のコピー

公開鍵の中身をコピーします。以下コマンドです。

```bash
pbcopy < ~/.ssh/<ファイル名>.pub
```

### GitHubのサイト上で公開鍵登録

続いて [GitHubの設定画面](https://github.com/settings/profile) にアクセスして、SSH keysの設定メニュー上のNew SSH keyというボタンを押します。

以下手順画像です。

[![無題のプレゼンテーション (1).jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2849606/8875c18b-d9e5-af8a-af44-f9b1c23b3ec8.jpeg)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2849606%2F8875c18b-d9e5-af8a-af44-f9b1c23b3ec8.jpeg?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7b1961faac558e82bc8d7d7a517778a4)

[![無題のプレゼンテーション (2).jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2849606/c53ac8b0-5039-12bd-1d55-1ab71d673157.jpeg)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2849606%2Fc53ac8b0-5039-12bd-1d55-1ab71d673157.jpeg?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=40dbe883980fc8fdc0542106427b0eca)

以上の手順で公開鍵の登録が完了し、SSH接続が出来るようになりました。GitHubとのSSH接続が正常に行われているか確認するコマンドは以下になります。

```bash
$ ssh -T <ホスト名>
```

接続が完了している場合、以下のようなメッセージが表示されます。

```text
Hi <ホスト名>に紐付くGitHubアカウント名! You've successfully authenticated, but GitHub does not provide she
ll access.
```

## GitHubとのSSHを利用したやりとり

GitHub上に登録されているリポジトリとやりとりを開始する際に、 `git remote add` や `git clone` 等で接続設定をすると思います。  
このときに先ほど `~/.ssh/config` に登録したホスト名を指定します。

### cloneするとき

例えばsample-userというユーザーもしくは組織が持つsample-repositoryというリポジトリをクローンするとします。  
こちらのリポジトリに対してはprivateというホスト名で登録したGitHubアカウントでやりとりしようとする場合は、以下のようなコマンドになります。

```bash
$ git clone git@private:sample-user/sample-repository.git
```

### リモートリポジトリを登録するとき

cloneした後にファイル編集をして `git add` や `git push` をするにはリモートリポジトリを登録する必要があります。その時も先ほどのようにホスト名を指定します。以下コマンドになります。

```bash
$ git remote add origin git@private:sample-user/sample-repository.git
```

## ローカルリポジトリのGitのconfig設定

先ほどGitHubとのSSHを利用した接続が完了したのですが、 `git add` をした際にemailとnameの設定を求められると思います。  
その際は `--local` オプションを指定することで、そのプロジェクト限定の設定になります。アカウントを複数切り替える場合は `--local` オプションを指定した方が良いでしょう。

```bash
$ git config --local user.name "<ユーザー名>"
$ git config --local user.email "<メールアドレス>"
```

## おわりに

私が複数アカウントを切り替える際に利用しているコマンドは以上になります。アカウントを切り替えて、表の自分と裏の自分、第三の自分と切り替えてソースコードにコミットしていきましょう！

[0](https://qiita.com/manzoku_bukuro/items/#comments)

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fmanzoku_bukuro%2Fitems%2Fc6fc9cbc069fe4a1b776&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fmanzoku_bukuro%2Fitems%2Fc6fc9cbc069fe4a1b776&realm=qiita)

[15](https://qiita.com/manzoku_bukuro/items/c6fc9cbc069fe4a1b776/likers)

10