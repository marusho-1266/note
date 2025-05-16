---
title: "【初心者向け・図解】Docker Composeとは？Dockerとの違いを現役エンジニアがわかりやすく解説"
source: "https://o2mamiblog.com/docker-beginner-2/"
author:
  - "[[エンジニア女子の自習室]]"
published: 2022-07-14
created: 2025-05-15
description: "Docker Composeとは、複数のDockerコンテナを定義し実行する Docker アプリケーションのためのツールです。本記事ではIT初心者向けにDocker Composeの仕組みを図解でわかりやすく解説しています。Docker Composeの全体像を掴みましょう！"
tags:
  - "#tool"
  - "#tool/docker"
  - "#tool/docker-compose"
  - "#article"
  - "#basic"
---
【初心者向け・図解】Docker Composeとは？Dockerとの違いを現役エンジニアがわかりやすく解説 – エンジニア女子の自習室

![IT初心者](https://o2mamiblog.com/wp-content/uploads/2021/11/pose_atama_kakaeru_man.png)

IT初心者

Docker Composeって何？Dockerとの違いが分からないなぁ..

![おつまみ](https://o2mamiblog.com/wp-content/uploads/2021/11/SNS-icon-1.jpg)

おつまみ

そのお悩みを解決します！

エンジニアの皆さん。  
日々の業務、そして学習お疲れ様です！学習は順調に進んでいますか？  
  
Dockerを学習していると一緒に出てくる「Docker Compose」  
自分の言葉で「Docker Compose」が何か説明することはできますか？

  
  
……。  
  
………………。  
  
説明できましたでしょうか？  
  
本記事では「Docker Compose」について、 **初心者にわかりやすいよう図解付き** で解説していきます！  
  
結論、こちらがDocker Composeでコンテナを起動する仕組みです！

![](https://o2mamiblog.com/wp-content/uploads/2022/07/Docker-compose_page-0001.jpg)

コンテナを起動する仕組み DockerFileの作成 docker-compose.ymlの作成 docker-composeコマンドでコンテナ起動

コンテナを起動する仕組み

1. **DockerFileの作成**
2. **docker-compose.ymlの作成**
3. **docker-composeコマンドでコンテナ起動**

今は全てを理解できなくて全然大丈夫です！  
  
本記事を読んだ後に「Docker Composeの仕組みってこうなんだなぁ」ってざっくり理解して頂ければ嬉しいです。  
  
それではどうぞ！！

本記事で解説していること

- **Docker ComposeとDockerの違い**
- **Docker Composeのメリット**
- **Docker Composeによるコンテナ起動方法**

本記事で解説していないこと

- **ハンズオンのような詳細なコマンド例**
- **データマウント,Docker network,Docker-machine,Docker Swamについて**

## Docker Composeって何？Dockerとの違いも解説

**Docker Composeとは複数のDockerコンテナを定義し実行するDockerアプリケーションのためのツールです。**  
  
一方、Dockerとはコンテナ型仮想環境を作成・実行・管理するためのプラットフォームのことです。  
  
つまり **Dockerコンテナを楽にまとめて起動したい時はDocker Compose** を使います！  
違いはこちらの通りです。

![](https://o2mamiblog.com/wp-content/uploads/2022/07/Docker-compose_page-0004-1.jpg)

参考： Docker Compose入門 (1) ～アプリケーションをコンテナで簡単に扱うためのツール～

**Docker（Docker Engine）は1度に1つのコンテナしか操作できません。**  
  
ここで、Dockeイメージを冷凍炒飯と例えてみましょう。  
すると、 **Docker（Docker Engine）は一般的な一段タイプの電子レンジ** と表現することができます。  
  
一方、 **Docker Composeは同時に複数のコンテナを操作することができます。**  
  
そのため、 **Docker Composeは複数段あって温め順等を設定できる高機能な電子レンジ** と表現することができます。

![おつまみ](https://o2mamiblog.com/wp-content/uploads/2021/11/SNS-icon-1.jpg)

おつまみ

Docker Composeはまとめてコンテナを操作できるんだね！

この例えはこちらの記事を参考にさせて頂きました。  
これ以外にも冷凍炒飯を例にしたコマンド紹介があり、クスッと笑えて面白かったので、ぜひ参考に読んでみてください！  
[Dockerについてなるべくわかりやすく説明する – Qiita](https://qiita.com/rawHam/items/80ba13d2d2d56dba411e)

![IT初心者](https://o2mamiblog.com/wp-content/uploads/2021/11/pose_atama_kakaeru_man.png)

IT初心者

違いについてはわかった！  
でもDockerコンテナを起動させる仕組みって何だっけ？忘れちゃったなぁ・・

まず、Docker Composeでコンテナを起動する仕組みを理解するために、Dockerでコンテナを起動する流れをおさらいしましょう！

### Dockerでのコンテナ起動方法について

Dockerでコンテナを起動させる方法はこちらの通りです。

![](https://o2mamiblog.com/wp-content/uploads/2022/07/Docker-compose_page-0002.jpg)

コンテナを起動する仕組み DockerFileの作成 docker-compose.ymlの作成 docker-composeコマンドでコンテナ起動

以下の流れでDockerコンテナを起動させます。

Dockerコンテナ起動までの仕組み

1. **Docker Hub等のリポジトリから公式のイメージを取得（pull）**
2. **取得イメージをもとにDockerFileを作成し、自作のイメージを作成（build）**
3. **イメージからコンテナを作成（create）**
4. **コンテナを起動（start）**

詳細についてはこちらの記事で詳しく解説しているので、良ければ参考にしてみて下さい。

Dockerだけでもコンテナを起動させることはできます。  
しかし、その場合にこのような悩みが出てきました。

![](https://o2mamiblog.com/wp-content/uploads/2022/07/Docker-compose_page-0003.jpg)

コンテナを起動する仕組み DockerFileの作成 docker-compose.ymlの作成 docker-composeコマンドでコンテナ起動

先ほどDockerとの違いでお話したように、Dockerは1度に1つのコンテナしか操作できません。  
  
そのため、コンテナを起動させるためには、Dockerクライアントから何回もdockerコマンドを発行する必要がありました。  
  
1つのコンテナならそこまで手間ではないですが、複数のコンテナを起動させたい場合はかなり手間がかかりますよね。。  
  
そこで **Docker Compose** が活躍します！

### Docker Composeはコンテナを一度に操作するコマンド

冒頭でお見せしたように、Docker Composeでコンテナを起動させる方法はこちらの通りです。

![](https://o2mamiblog.com/wp-content/uploads/2022/07/Docker-compose_page-0001-1.jpg)

コンテナを起動する仕組み DockerFileの作成 docker-compose.ymlの作成 docker-composeコマンドでコンテナ起動

コンテナを起動する仕組み

1. **DockerFileの作成**
2. **docker-compose.ymlの作成**
3. **docker-compose **コマンド** でコンテナ起動**

Docker Composeではdocker-composeコマンドで、同時に複数のコンテナを操作することができます。  
その元になる設計書の役割を果たすファイルのことを「 **docker-compose.yml** 」と呼びます。  
  
実際に起動するまでの流れは後続の章で解説しますね。  
  
次にDocker Composeを使うメリットについて解説します。

## Docker Composeを使うメリット

Docker Composeを使うメリットはこちらの通りです。

Docker Composeを使うメリット

1. **実行コマンドが簡潔になり、環境差異によるミスを起こしにくい**
2. **Dockerネットワークの作成が不要**
3. **インフラ構成の可視化、バージョン管理が可能**

### ①実行コマンドが簡潔になり、環境差異によるミスを起こしにくい

**Docker Composeは実行コマンドが簡潔になり、環境差異によるミスを起こしにくいです。**  
  
なぜなら、 **コンテナ操作に必要なdockerコマンドをオプション付きでdocker-compose.ymlに全て記述できる** ためです。  
  
例えば、dockerコンテナを起動する場合は、環境変数の設定やディスクマウント、ポートの設定等色々なオプションを指定する必要があるとします。  
  
これらを毎回オプション付きのdockerコマンドで操作する場合、どうでしょう？

![IT初心者](https://o2mamiblog.com/wp-content/uploads/2021/11/pose_atama_kakaeru_man.png)

IT初心者

環境によって指定するオプションが別だったらミスしちゃうかも..  
あとは手順書の管理が大変になりそう..

その通りです。  
  
手順書の管理を簡素化・コマンドミスを起こさないために、 **docker-compose.ymlをdockerコマンドの手順書** として事前にまとめておくことができます。  
  
そして実行する際には、docker-composeコマンドで一発で実行することができます！

### ②Dockerネットワークの作成が不要

Docker ComposeはDockerネットワークの作成が不要です。

![IT初心者](https://o2mamiblog.com/wp-content/uploads/2021/11/pose_atama_kakaeru_man.png)

IT初心者

Dockerネットワークって何？

**Dockerネットワークはコンテナが安全に通信を行うために必要なネットワーク** のことです。  
  
DockerコンテナとDocker Composeで複数のコンテナを起動させる場合でネットワークがどう違うか確認してみましょう！

#### DockerコンテナとDocker Composeのネットワークの違い

![](https://o2mamiblog.com/wp-content/uploads/2022/07/Docker-compose_page-0005-1.jpg)

参考： Docker Compose入門 (4) ～ネットワークの活用とボリューム～

**Dockerコンテナの場合、ネットワーク指定のオプションなしでは デフォルトでbridgeネットワーク に接続されます。**  
  
このbrigdeネットワークは、IPアドレスでの通信は可能ですが、プロセス名（ホスト名）での通信はできないという特徴があります。  
  
よって、アプリケーションのスケール等によりIPアドレスが変わってしまった場合は対処することができません。

![おつまみ](https://o2mamiblog.com/wp-content/uploads/2021/11/SNS-icon-1.jpg)

おつまみ

ホスト名で通信を行うには、事前にネットワークを作成して、オプションで指定する必要があるってことね。（めんどくさい）

一方、 **Docker Composeの場合、 プロジェクト専用のブリッジ・ネットワーク が自動で作成されます** ！  
  
この専用のネットワークにより、IPアドレスでもホスト名でも互いに通信することが可能になります。  
  
そのため、dockerと異なり、 **事前にネットワークを作成する必要はありません** 。

詳細についてはこちらの記事がわかりやすかったので、参考にしてみてください。  
[Docker Compose入門 (3) ～ネットワークの理解を深める～ | さくらのナレッジ](https://knowledge.sakura.ad.jp/23899/)  
[Docker Compose入門 (4) ～ネットワークの活用とボリューム～ | さくらのナレッジ](https://knowledge.sakura.ad.jp/26522/)

### ③インフラ構成の可視化、バージョン管理が可能

Docker Composeはインフラ構成の可視化、バージョン管理が可能です。  
  
なぜなら、docker-compose.ymlにあらかじめ記述しておくことができるためです。  
  
インフラ構成やバージョン情報がわかると、 **複数人で開発をしているときに大変便利ですよね** 。  
  
そのため、docker-compose.ymlを正しく記述し、内容を理解できるようようにしておきましょう！  
  
最後に、もう一度Docker Composeでコンテナ起動する仕組みを見てみましょう！

## Docker Composeでのコンテナ起動方法について

冒頭でお見せしたように、Docker Composeでコンテナを起動させる方法はこちらの通りです。

![](https://o2mamiblog.com/wp-content/uploads/2022/07/Docker-compose_page-0001-1.jpg)

コンテナを起動する仕組み DockerFileの作成 docker-compose.ymlの作成 docker-composeコマンドでコンテナ起動

コンテナを起動する仕組み

1. **DockerFileの作成**
2. **docker-compose.ymlの作成**
3. **docker-compose **コマンド** でコンテナ起動**

1つ1つ何を行っているか押さえていきましょう！

#### ①DockerFileの作成

まずはDockerFileを作成します。  
  
DockerFileは **コンテナの元になるDockerイメージの設計図** でしたね。  
Dockerイメージからコンテナが作成されます。  
  
具体的にDockerFileはこのように記述します。([公式ドキュメント](http://docs.docker.jp/compose/rails.html) から引用)

```
FROM ruby:2.3.3　#レジストリから取得するイメージを指定
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs　#ミドルウェアをインストールするコマンドを指定
RUN mkdir /myapp
WORKDIR /myapp　＃ワークディレクトリを設定する
ADD Gemfile /myapp/Gemfile　＃ファイルを追加する
ADD Gemfile.lock /myapp/Gemfile.lock
RUN bundle install
ADD . /myapp
```

#### ②docker-compose.ymlの作成

次にdocker-compose.ymlを作成します。  
  
このファイルにはDocker Composeプロジェクトで作成するコンテナの初期設定状態を記述します。  
  
具体的にdocker-compose.ymlはこのように記述します。([公式ドキュメント](http://docs.docker.jp/compose/rails.html) から引用)

```yaml
version: '3'　#バージョンを指定
services:
  db:
    image: postgres　#レジストリから取得するイメージを指定
  web:
    build: .　＃DockerFile のあるディレクトリのパスを指定（左記はカレントディレクトリを指定）
    command: bundle exec rails s -p 3000 -b '0.0.0.0' ＃docker-compose up を実行した時に実行するコマンドを指定
    volumes: ＃マウントする設定ファイルのパスを指定
      - .:/myapp
    ports: ＃DBのDockerImageを立ち上げる際のポート番号を指定
      - "3000:3000"
    depends_on: #サービス同士の依存関係を指定（左記はdbが作成されてからwebを作成するように指定）
      - db
```

#### ③docker-composeでコンテナ起動

最後にコマンドを入力してコンテナを起動させます。

```
$ docker-compose up
```

このコマンドにより、DockerFileからDockerイメージを作成し、コンテナを起動させることが一発でできます！  
  
1つ1つdockerコマンドを発行していた時よりも簡単にできましたよね？  
  
以上が、Docker Composeによるコンテナ起動方法です！

## まとめ

今回はDocker ComposeとDockerの違い、メリット、そしてDocker Composeによるコンテナの起動方法についてご紹介しました。

![](https://o2mamiblog.com/wp-content/uploads/2022/07/Docker-compose_page-0001.jpg)

コンテナを起動する仕組み DockerFileの作成 docker-compose.ymlの作成 docker-composeコマンドでコンテナ起動

はじめに見た時よりも理解できましたでしょうか？  
  
Docker Composeによるコンテナ起動は、複数のコンテナ管理では必ず使用されます。そのため、エンジニアとして避けて通ることができません。

![少し自信がついたIT初心者](https://o2mamiblog.com/wp-content/uploads/2021/11/gutspose_man.png)

少し自信がついたIT初心者

少し自信がついたIT初心者

もっとDocker Composeについて学びたい！

こう思った方はUdemyの動画講座で学習を始めましょう！  
こちらの記事で初心者におすすめの講座をご紹介しているので、よければ参考にしてみて下さい！

[

![](https://o2mamiblog.com/wp-content/uploads/2022/07/udemy-docker-beginner-320x198.png)

](https://o2mamiblog.com/udemy-docker-beginner/ "【2023年】初心者おすすめUdemyのDocker講座2選！")

![おつまみ](https://o2mamiblog.com/wp-content/uploads/2021/11/SNS-icon-1.jpg)

おつまみ

私もまだまだ勉強中です！一緒にがんばりましょう！！

それでは、これからも一緒に学んで、自己価値を高めていきましょう〜！  
  
最後まで、お読み頂きありがとうございました！

## 参考文献

こちらの記事を作成するにあたり、公式ドキュメントやこちらのブログ記事を参考にさせて頂きました。 よければ参考にしてみて下さい！

タイトルとURLをコピーしました