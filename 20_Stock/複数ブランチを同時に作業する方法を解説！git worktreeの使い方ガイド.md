---
title: "複数ブランチを同時に作業する方法を解説！git worktreeの使い方ガイド"
source: "https://envader.plus/article/408"
author:
  - "[[エンベーダー]]"
published:
created: 2025-05-29
description: "開発作業をしている最中に、チームメンバーからプルリクの依頼をされるということは良くあります。この時、プルリクの内容を確認するためのブランチに移動する前に、作業中の変更をコミットして…という確定していない変更をコミットすることにモヤモヤした経験ありませんか？"
tags:
  - clippings
  - git
  - programming
  - tutorial
  - basic
---
1. [ホーム](https://envader.plus/)
2. [記事一覧](https://envader.plus/article)
3. [複数ブランチを同時に作業する方法を解説！git worktreeの使い方ガイド](https://envader.plus/article/408)
![](https://envader.plus/_next/image?url=https%3A%2F%2Fstorage.googleapis.com%2Fenvader-article-images%2Fgit-worktree%2Fgit-worktree-001.png&w=3840&q=75&dpl=dpl_AuqG8EJftnkwe1ywAEp49uCnyJkW)

2024.06.19

![](https://storage.googleapis.com/envader-article-images/git-worktree/git-worktree-001.png)

## はじめに

開発作業をしている最中に、チームメンバーからプルリクの依頼をされるということは良くあります。この時、プルリクの内容を確認するためのブランチに移動する前に、作業中の変更をコミットして…という確定していない変更をコミットすることにモヤモヤした経験ありませんか？このような場面では `git worktree` コマンドが便利です。 `git worktree` を使うとにより、作業中のブランチとプルリク確認用のブランチを並行して作業する、ということができます。

Gitコマンドは `git commit` などの基本コマンドしか使った事が無い、これからGitを使って開発をスタートする方に向けて、この記事では `git worktree` を解説していきます。

この記事を読むことで、 `git worktree` の基本を学べます！さまざまなGitコマンドを習得して、Gitマスターを目指しましょう！

## git worktreeとは

`git worktree` とは、同じリポジトリ内に複数のワーキングツリーを作成し、管理するための機能です。この機能を使うことで、同じリポジトリ内で複数のブランチを並行してチェックアウトし、作業することができます。

通常、Gitリポジトリには1つのワーキングツリーしかありませんが、 `git worktree` を使用すると複数のワーキングツリーを追加作成することができます。開発者は、これらのワーキングツリーを切り替えて作業することが可能です。あるブランチで作業中に他のブランチの作業に切り替える場合、ブランチの切り替え前に作業中の変更を `git commit` や `git stash` で保存する必要があります。しかし、 `git worktree` を使用すると、作業中の状態を残したまま別のワーキングツリーのブランチにチェックアウトすることが可能です。ワーキングツリーのブランチへのチェックアウトは、 `cd` コマンドでツリーのディレクトリに移動することで、自動的にチェックアウトすることができます。

![](https://storage.googleapis.com/envader-article-images/git-worktree/git-worktree-002.png)

作成したワーキングツリーはそれぞれ独立しているため、変更が他のワーキングツリーに影響を与えることはありません。「はじめに」でお伝えしたような、作業途中にプルリク依頼があった場面では、プルリク確認用のワーキングツリーを追加作成すれば、そのツリー上でコードレビューが可能です。この時、作業していたツリーでの変更はコミットせずに、プルリク用のブランチにチェックアウトすることができます。 `git worktree` は、複数のブランチでそれぞれ独立した並行作業を行いたい場合に非常に便利なツールです。

![](https://storage.googleapis.com/envader-article-images/git-worktree/git-worktree-003.png)

## git worktreeで作成したワーキングツリーについて

ワーキングツリーでは、リポジトリに存在するブランチをそれぞれ紐づけて作業を行います。この時、あるワーキングツリーでチェックアウトされているブランチは、他のワーキングツリーで同時にチェックアウトすることはできません。これは、ブランチを同時に異なる場所で操作することによるコンフリクトを防ぐためです。

ワーキングツリーを追加することを身近なもので例えると、作業デスクを増やすイメージです。同様に、ブランチはそのデスクで作業するために使用する書類に例えられます。作業デスクでは、1つの書類をデスクの上に置いて作業をします。書類は1つしかないので、複数の作業デスクで同時に使用することはできません。

![](https://storage.googleapis.com/envader-article-images/git-worktree/git-worktree-004.png)

## git worktreeの注意点

ここまでの説明で、 `git worktree` は便利なコマンドという印象を持たれた方も多いと思いますが、使用の際にはいくつかの注意点があります。

### リポジトリの管理が複雑化する

複数のワーキングツリーを使用することにより、管理が複雑になります。不要になったワーキングツリーはすぐに削除するなど、ワーキングツリーを増やしすぎないように工夫が必要です。

### コマンドの複雑さ

通常のGitコマンドとは異なり、 `git worktree` は独自のコマンドを使用します。これにより、入力するコマンドが増え、複雑さが増します。入力コマンドについては、「使い方」セクションで紹介します。

## git worktreeコマンドの開発現場での活用例

これまでに `git worktree` の特徴や機能について学びましたが、実際の開発現場でどのように使用されているかを知ることで、さらに理解が深まるでしょう。 `git worktree` を利用すると、複数のブランチを同時に作業することが可能なため、様々な場面での並行作業に活用されています。このセクションでは、その具体的な活用例を紹介します。

### 複数の開発での並行作業

`git worktree` を利用することで、開発者は複数のタスクやブランチを並行して作業することができて、効率的に作業や管理を行うことが可能となります。例えば、チームメンバーのトラブルを解決するために自分の作業を中断する必要がある場合、その問題の解決用にワーキングツリーを別に作成すれば、すぐに対応が可能です。この際、独立したワーキングツリーで作業を行うため、他のワーキングツリーには影響を及ぼしません。

### 緊急のバグ修正の迅速な対応

緊急のバグ修正依頼がある場合も、上記の例と同様に対応できます。開発者は現在の作業を中断し、 `git worktree` を使用してバグ修正用のワーキングツリーを作成すれば、以前の作業をそのままにして直ぐに緊急の修正作業に取り組むことができます。

## git worktreeの使い方

このセクションでは、実際に `git worktree` をどの様に使用するか解説します。前のセクションで少し触れたように、 `git worktree` は他のGitコマンドとは少し異なる、独自のコマンドを使用します。以下は、その基本的なコマンドです。

新しいワーキングツリーを指定したパスに作成し、指定したブランチをチェックアウトします。

```
git worktree add [ワーキングツリーのパス] [ブランチ名]
```

現在のリポジトリに関連付けられているすべてのワーキングツリーを一覧表示します。

```
git worktree list
```

指定したワーキングツリーを削除します。

```
git worktree remove [ワーキングツリー名]
```

使用されていない（削除された）ワーキングツリーに関連する管理ファイルをクリーンアップします。管理ファイルとは、ワーキングツリーの状態などの情報を持つメタデータファイルです。管理ファイルはワーキングツリー削除後、時間が経つと自動で削除されますが、すぐに削除したい場合や徹底的なクリーンアップをする際に、以下のコマンドを利用します。

```
git worktree prune
```

## git worktreeのオプション紹介

| 項目 | 説明 |
| --- | --- |
| \-b | 新しいワーキングツリーを作成する際に、新しいブランチも同時に作成 |
| \--no-checkout | 新しいワーキングツリーを作成するが、ブランチにはチェックアウトしない |

### \-b

新しいワーキングツリーを作成する際に、新しいブランチも同時に作成します。

```
git worktree add -b [新規作成するブランチ名] [ワーキングツリーのパス]
```

以下は `feature#1` のワーキングツリーと、同じ名前のブランチを作成した実行結果です。

```
git worktree add -b feature#1 "/Users/alice/Desktop/worktree/feature#1"
Preparing worktree (new branch 'feature#1')
HEAD is now at f2b5598 test
```

ワーキングツリー `feature#1` に、 `feature#1` ブランチがチェックアウトされました。

```
git worktree list
/Users/alice/Desktop/worktree            f2b5598 [main]
/Users/alice/Desktop/worktree/feature#1  f2b5598 [feature#1] ←新しくワーキングツリーが作成できています
```

### \--no-checkout

新しいワーキングツリーを作成する際に、ブランチにはチェックアウトしません。通常、 `git worktree add` コマンドを使って新しいワーキングツリーを作成すると、ブランチに関連づいているファイルやフォルダが新しいワーキングツリーにコピーされます。しかし、 `--no-checkout` オプションを付けてワーキングツリーを作成すると、ファイルやフォルダのコピーは行われず、新しいディレクトリは空のままになります。このオプションを使用することで、新規にワーキングツリーの作成と指定ブランチの紐付けのみを行うことができます。

```
git worktree add --no-checkout [ワーキングツリーのパス] [ブランチ名]
```

以下は `--no-checkout` オプションを使用して実行した結果です。ワーキングツリーと指定のブランチが紐付きました。

```
git worktree add --no-checkout "/Users/alice/Desktop/worktree/feature#2" feature#2
Preparing worktree (checking out 'feature#2')
```

作成したワーキングツリーのディレクトリ内は空の状態です。このオプションは、メモリ領域の節約や特定のファイルのみを操作したい際に有用です。開発者は必要なファイルやディレクトリのみを `feature#2` にコピーして作業を行うことができます。

![](https://storage.googleapis.com/envader-article-images/git-worktree/git-worktree-005.png)

## この記事で学んだこと

## git worktreeとは

`git worktree` とは、同じリポジトリ内に複数のワーキングツリーを作成し、管理するための機能です。この機能を使うことで、同じリポジトリ内で複数のブランチを並行してチェックアウトし、作業することができます。チーム開発をしている中で、プルリクの依頼やメンバーのトラブル解決用に別のワーキングツリーを作成し、自分の開発中のワーキングツリーと並行して作業を行うことが可能です。

注意点として、ワーキングツリーが増えすぎると管理がしづらくなるため、不要になったワーキングツリーはすぐに削除するなど、ワーキングツリーを増やしすぎないように工夫が必要です。

## 参考資料

以下のリンクは、この記事で説明した手順や概念に関連する参考資料です。より詳しく学びたい方は、ぜひご参照ください。

- Git公式 - git worktree
	[https://git-scm.com/docs/git-worktree](https://git-scm.com/docs/git-worktree)
- Free Developers Hub - git worktreeを使ってプルリクレビューを効率化した話
	[https://developers.freee.co.jp/entry/efficient-pr-review-with-git-worktree](https://developers.freee.co.jp/entry/efficient-pr-review-with-git-worktree)
- ATLASSIAN Blog - Git 2.x シリーズの 6 つの素晴らしいフィーチャー
	[https://www.atlassian.com/ja/blog/cool-features-git-2-x](https://www.atlassian.com/ja/blog/cool-features-git-2-x)
- SCALER Topics - Git Worktree
- GitKraken - Git Worktree
	[https://www.gitkraken.com/learn/git/git-worktree](https://www.gitkraken.com/learn/git/git-worktree)
[![RareTECH 無料体験授業開催中！ オンラインにて実施中！ Top10%のエンジニアになる秘訣を伝授します！ RareTECH講師への質疑応答可](https://envader.plus/_next/image?url=%2Fcommons%2Fbanners%2Fbanner-raretech-trial-lesson01.jpg&w=3840&q=75&dpl=dpl_AuqG8EJftnkwe1ywAEp49uCnyJkW)](https://raretech.site/trial/lesson?from=envader)