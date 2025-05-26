---
title: "GitHub、複数アカウントをターミナルでで切り替える方法（Mac,zsh)"
source: "https://qiita.com/TANIOKA_Akihiro/items/206ad5a9619a67043c76"
author:
  - "[[TANIOKA_Akihiro]]"
published: 2023-05-12
created: 2025-05-26
description: "複数のGitHubアカウントを使い分けるために、SSHキーとSSHの設定ファイルを使用してアカウントの切り替えを行う方法を紹介します。会社用と個人用を想定。1. SSHキーの生成まず、各アカウン…"
tags:
  - "clippings"
  - "tool"
  - "git"
  - "ssh"
  - "practical"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

この記事は最終更新日から1年以上が経過しています。

複数のGitHubアカウントを使い分けるために、SSHキーとSSHの設定ファイルを使用してアカウントの切り替えを行う方法を紹介します。会社用と個人用を想定。

## 1\. SSHキーの生成

まず、各アカウント用に異なるSSHキーペアをホームディレクトリーで生成します。  
"アカウントのメールアドレス"は会社用でGitHubアカウントに登録している、emailアドレスに置き換えてください。

```text
ssh-keygen -t ed25519 -C "アカウントのメールアドレス" -f ~/.ssh/id_ed25519_company
```

このコマンドを実行し、会社用のSSHキーペアを生成します。同様に、個人用にもSSHキーペアを生成します。  
"アカウントのメールアドレス"は個人用でGitHubアカウントに登録している、emailアドレスに置き換えてください。

```text
ssh-keygen -t ed25519 -C "アカウントのメールアドレス" -f ~/.ssh/id_ed25519_personal
```

## 2\. SSHの設定ファイルの作成

SSHの設定ファイル（~/.ssh/config）を作成または編集します。

```text
code ~/.ssh/config
```

\[補足\]VSCodeエディターで行っていますが、お好みのエディターで大丈夫かと思います。

以下のような内容を記述します。各項目は自身の環境に合わせて適宜変更してください。

```text
# 会社アカウントの設定
Host github.com-NicName
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_company

# 個人アカウントの設定
Host github.com-NicName
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
```

上記の例では、会社用の設定と個人用の設定を追加しています。  
NicNameやHostNameは実際のGitHubに登録してあるニックネーム、ホストネームに置き換えてください。

## 3\. GitHubに公開鍵を登録

生成したSSHキーの公開鍵をGitHubに登録します。

GitHubのウェブサイトにログインし、右上のアバターをクリックしてドロップダウンメニューを表示します。  
「Settings」を選択し、左側のメニューから、「SSH and GPG keys」を選択します。  
「New SSH key」または「Add SSH key」をクリックし、公開鍵を登録します。  
タイトルに適切な名前を入力(例えば、MyCompanyKeyなど)し、公開鍵の内容を貼り付けます。  
「Add SSH key」または「Add key」をクリックして、公開鍵を登録してGitHubの設定は完了です。

## 4\. アカウントの切り替え方法

.zshrcファイルの編集  
ターミナルを開き、.zshrcファイルを編集します。

```text
code ~/.zshrc
```

以下のコードを.zshrcファイルの適切な場所に追加します。

```text
# 会社アカウント
function gitCo() {
    git config --global user.name "[***]"   # ***には会社アカウントのusername
    git config --global user.email "[***]"  # ***には会社アカウントのemailアドレス
    echo "Switched to Company Account"　　　　　　　　　　　　#アカウントを切り替えると表示される内容
}

# 個人アカウント
function gitPe() {
    git config --global user.name "[***]"   # ***には個人用のusername
    git config --global user.email "[***]"  # ***には個人用のemailアドレス
    echo "Switched to Personal Account"　　　　　　　　　　#アカウントを切り替えると表示される内容
}
```

上記のコードでは、gitCo関数とgitPe関数を定義しています。(この関数は自分でわかりやすい名をつけてよい。ターミナルで切り替えのために入力する関数。)それぞれの関数は、会社用のアカウントと個人用のアカウントの設定を行います。\*\*\*の表示箇所は"会社用のユーザー名"や"会社用のメールアドレス"、"個人用のユーザー名"や"個人用のメールアドレス"は、実際のアカウント情報に置き換えてください。

ファイルを保存してエディタを閉じます。

.zshrcファイルの変更を反映させるために、以下のコマンドをターミナルで実行します。

```text
source ~/.zshrc
```

## 5\. アカウントの切り替え

ターミナルで以下のコマンドを実行して、アカウントを切り替えます。  
職場用アカウントに切り替え。

```text
gitCo
```

"Switched to Company Account"と表示され、切り替え成功。

個人用アカウントに切り替え。

```text
gitPe
```

"Switched to Personal Account"と表示され、切り替え成功。

GitHubのターミナルでのアカウント切り替え方法は、以上となります。  
ご観覧ありがとうございました！

[0](https://qiita.com/TANIOKA_Akihiro/items/#comments)

コメント一覧へ移動

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2FTANIOKA_Akihiro%2Fitems%2F206ad5a9619a67043c76&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2FTANIOKA_Akihiro%2Fitems%2F206ad5a9619a67043c76&realm=qiita)

[![TANIOKA_Akihiro](https://qiita-user-profile-images.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F468139%2Fprofile-images%2F1602671674?ixlib=rb-4.0.0&auto=compress%2Cformat&lossless=0&w=128&s=4f1d24186f5e0e25c837cb746c208ca4)](https://qiita.com/TANIOKA_Akihiro)

[

## @TANIOKA\_Akihiro(akihiro)

](https://qiita.com/TANIOKA_Akihiro)

[RSS](https://qiita.com/TANIOKA_Akihiro/feed)

[2](https://qiita.com/TANIOKA_Akihiro/items/206ad5a9619a67043c76/likers)

いいねしたユーザー一覧へ移動

5