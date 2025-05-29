---
title: "Git worktreeを使った効率的なバグ修正とマルチブランチ管理方法｜プロダクト共創組織"
source: "https://co-creation.wesionary.team/n/ne9bc76a05100"
author:
  - "[[プロダクト共創組織]]"
published: 2024-10-21
created: 2025-05-29
description: "最も過小評価されているgitコマンド TL;DR: git worktreeを使用すると、バグのテストや修正をする際にgit stashを使用する必要がありません。🐛  GIT：  ご存知の通り、gitはプロジェクト内のファイルに加えられた変更を時系列で追跡するのに役立ちます。多くの場合、以下の基本的なコマンドを使用します。  基本コマンド：    git init：新しいGitリポジトリを初期化します。    git clone：リモートリポジトリをローカルマシンにコピーします。    git add：変更をコミット用にステージングします。    git commit：ステージングさ"
tags:
  - clippings
  - git
  - programming
  - tutorial
  - practical
---
![見出し画像](https://assets.st-note.com/production/uploads/images/162231119/rectangle_large_type_2_5eba530e67da1d99fa511380ce4d2962.png?width=1200)

## Git worktreeを使った効率的なバグ修正とマルチブランチ管理方法

[プロダクト共創組織](https://co-creation.wesionary.team/)

最も過小評価されているgitコマンド  
**TL;DR**:  
git worktreeを使用すると、バグのテストや修正をする際にgit stashを使用する必要がありません。🐛

## GIT：

ご存知の通り、gitはプロジェクト内のファイルに加えられた変更を時系列で追跡するのに役立ちます。多くの場合、以下の基本的なコマンドを使用します。

## 基本コマンド：

- **git init** ：新しいGitリポジトリを初期化します。
- **git clone** ：リモートリポジトリをローカルマシンにコピーします。
- **git add** ：変更をコミット用にステージングします。
- **git commit** ：ステージングされた変更を説明的なメッセージと共に保存します。
- **git push** ：ローカルのコミットをリモートリポジトリに送信します。
- **git pull** ：リモートリポジトリからの変更でローカルリポジトリを更新します。
- **git merge** ：2つ以上の開発履歴を結合します。
- **git commit** ：リポジトリへの変更を記録します。
- **git worktree** ：複数の作業ツリーを管理します。
- 詳細情報： **man git**

## git worktree：最も過小評価されているgitコマンド

Git worktreeは、ファイルシステム上の異なるディレクトリに複数の作業コピーを持つことができるGitの機能です。  
これは非常に便利で、単一の作業ディレクトリ内でブランチを行き来する必要なく、異なるブランチや異なるバージョンのコードを同時に作業することができます。  
続けてお読みいただければ、私が何を言おうとしているかがわかるでしょう。😋  
worktreeについての詳細情報を得るには、以下のコマンドを使用できます。  
**man git-worktree**

![画像](https://assets.st-note.com/img/1729505535-oHUk3xwlQB761SNRVPJ89ZvF.png?width=1200)

man git-worktree

## 以下のシナリオを考えてみましょう：

**ケース1：**  
機能Aブランチで作業しており、その機能の作業中に多くのファイルを変更したとします。ここで、共同作業者や同僚が機能Bブランチのプルリクエストまたはマージリクエストを送ってきたり、単に機能Bブランチをテストしたいと思ったとします。  
または  
**ケース2：**  
機能Aで作業しており、その機能の作業中に多くのファイルを変更したとします。ここで、ステークホルダーが本番ブランチ（通常はmainブランチ）で重大なバグを発見したとします。  
上記のケースでどうしますか？  
**解決策1：**

- 変更をステージングエリアに追加します。
- 変更をstashします（適切なメッセージを添えて）。
- 機能Bブランチまたは本番ブランチに切り替えます。
- ブランチのコードをレビューするか、バグを修正します。
- ブランチ\[機能Bまたは本番ブランチ\]をプッシュします。
- 最後に機能Aブランチに戻り、stashを適用するかpopします。

**解決策2：**

- はい、その通りです🎉。git worktreeを使用します。

## 始めましょう：

**ケース2** を考えてみましょう。 **ケース1** でどうすればよいかはわかるでしょう。  
以下のような状況を想定してください：

- mainブランチでプロジェクトがv1.0.1で本番稼働しています。

```
// main branch code

// main.go file code
package main

import "fmt"

func main() {
  Feature()
  fmt.Println("Production v1.0.0")
}

// feature.go file code
package main

import "fmt"

func Feature() {
 fmt.Println("feature")
}
```

- mainから分岐して機能Aブランチで作業しています。

```
// feature A branch code
package main

import "fmt"

func main() {
  fmt.Println("Production v1.0.0")
}

func featureA() {
  fmt.Println("feature A")
}
```

mainブランチでは、バージョンはv1.0.1であるべきですが、重大なバグでv1.0.0になっています。

上記の例では1つのファイルだけを変更していますが、実際のプロジェクトでは多くのファイルを変更することになります。変更したファイルをstashする代わりに、以下の操作を行う方が良いでしょう：

**構文：** git worktree add <folder name> <branch name>  
**コマンド** ： git worktree add.worktrees/main main

これにより、現在のフォルダに.worktrees/mainディレクトリが作成されます。

現在のプロジェクトディレクトリをリストアップして確認できます：

**コマンド** ： ls -al

次に、.worktrees/mainディレクトリに移動します。

cd.worktrees/main

このディレクトリに移動すると、すでにmainブランチに入っています。

そのブランチ内でmainブランチのコードを取得できます。

次にコードを変更します：

```
// main branch code
package main

import "fmt"

func main() {
  fmt.Println("Production v1.0.1")
}
```

その後、hotfix後にadd, commitし、mainブランチにコードをpushします。

作業ディレクトリに戻ります：

cd../..  
ワークツリーを削除します：

git worktree remove.worktrees/main

次に機能Aブランチのコードをコミットし、mainブランチをpullして、機能Aブランチにmergeします。

最終的なコード：

```
// final code of feature A branch
package main

import "fmt"

func main() {
  fmt.Println("Production v1.0.1")
}

func featureA() {
  fmt.Println("feature A")
}
```

デモについては以下をご覧ください： [https://youtu.be/fMdiAWgkfDA](https://youtu.be/fMdiAWgkfDA)

**注意** ：.worktreesファイルは残るので、そのフォルダを.gitignoreに追加する必要があります。  
**注意** ：gitで無視されているファイルはワークツリーディレクトリでは利用できないので、手動でそれらのファイルをコピーする必要があります。例 **：.env** ファイル

> 同様に、機能Bブランチをチェックしたい場合は、ワークツリーを追加してそのディレクトリに移動し、コードをテストしてからワークツリーを削除するだけです。

## git worktreeの仕組み：

以下のコマンドを入力すると： **git worktree add <path> <branch>**

- 指定された **<path>** に新しいディレクトリが作成されます。
- このディレクトリがメインのgitリポジトリにリンクされたワークツリーとして初期化されます。
- 指定された **<branch>** がこの新しいワークツリーにチェックアウトされます。

メインgitリポジトリ → プロジェクトのルートgitリポジトリ  
Gitはメインgitリポジトリ内の.**git/worktrees** ディレクトリに記録を保持します。このディレクトリには追加の各ワークツリーのサブディレクトリが含まれ、それぞれに以下が含まれています：

- ワークツリーの現在のコミットを指す **HEAD**
- 変更のステージング用の **index**
- ワークツリー固有の操作ログ用の **logs**
- ワークツリー固有の設定用の **config** ファイル

他に影響を与えることなく、任意のワークツリー内でコミットの作成、ブランチの切り替え、その他のGit操作を実行できます。

## ベストプラクティス：

- **同じブランチをチェックアウトしない**
- **古いワークツリーを削除する：git worktree remove <path>,**

ありがとうございました！

---

この記事は、2024年4月に弊社のエンジニア Mukesh Kumar Chaudhary が執筆した内容を日本語に翻訳したものです。  
英語版はこちらをご覧ください。  
[https://articles.wesionary.team/git-worktree-enhance-your-development-with-git-5434599d720d](https://articles.wesionary.team/git-worktree-enhance-your-development-with-git-5434599d720d)

---

### 採用情報

私たちは **プロダクト共創の仕組み化** に取り組んでいます。プロダクト共創をリードするプロダクト・マネージャー、そして、私たちのビジョンを市場に届ける営業メンバーを募集しています！

[**採用情報 - ITで誰しもがアイディアを市場で検証できる環境を創る** *私ちのビジョンに共感し、能動的に考え行動したい気持ちを感じたら、一歩を踏み出しましょう！ご応募をお待ちしております。* *wesionary.team*](https://wesionary.team/career/product-manager)

[**採用情報 - ITで誰しもがアイディアを市場で検証できる環境を創る** *私ちのビジョンに共感し、能動的に考え行動したい気持ちを感じたら、一歩を踏み出しましょう！ご応募をお待ちしております。* *wesionary.team*](https://wesionary.team/career/sales)

---

### 開発パートナーをお探しの企業様へ

弊社は、グローバル開発のメリットを活かし、高い費用対効果と品質を両立しています。経験豊富で多様性のあるチームが、課題を正しく理解し、最適なシステムと優れた体験を実現します。業務システムの開発、新規事業の開発、業務効率化やDX化に関するお困りごと、ぜひ弊社にご相談ください。

[**お問い合わせ - メール、チャットでお気軽にご相談ください** *貴社の事業開発やDXの課題をお聞かせください。経験豊富なチームですぐに対応致します。オンラインミーティングも承っております* *wesionary.team*](https://wesionary.team/contact)

Git worktreeを使った効率的なバグ修正とマルチブランチ管理方法｜プロダクト共創組織