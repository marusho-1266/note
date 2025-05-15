---
title: "Dockerによる開発環境構築のための概念理解と方法解説"
source: "https://zenn.dev/rhothchia/articles/docker-for-dev-setup"
author:
  - "[[Zenn]]"
published: 2024-03-20
created: 2025-05-15
description: "この記事では、Dockerを使った開発環境構築について、概念から手順までを解説しています。コンテナ技術の基本、Dockerの主要コンポーネント、docker-composeによる複数コンテナの管理、そして実際の開発環境構築例を網羅しています。初心者から中級者まで役立つ内容になっています。"
tags:
  - "#tool" 
  - "#tool/docker"
  - "#article"
  - "#tutorial"
  - "#intermediate"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

この記事は最終更新日から1年以上が経過しています。

この記事は [Nuco Advent Calendar 2023](https://qiita.com/advent-calendar/2023) の9日目の記事です。

## はじめに

この記事ではDockerで開発環境を行うために理解してほしい概念と実際の開発環境の構築手順について解説を行います。大きく分けて、

・Dockerの概念理解  
・開発環境の構築

これらの章により構成されています。この記事を読むことで、Dockerファイル、イメージ、コンテナ、Docker compose、compose.ymlを理解できるようになることを目指しています。Dockerに触れてみたい、Dockerの理解があやふやという方は参考にしてみてください！

弊社Nucoでは、他にも様々なお役立ち記事を公開しています。よかったら、Organizationのページも覗いてみてください。  
また、Nucoでは一緒に働く仲間も募集しています！興味をお持ちいただける方は、 [こちら](https://www.recruit.nuco.co.jp/?qiita_item_id=977d28b0eac316915702) まで。

## Dockerとは

[![01-primary-blue-docker-logo.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/fb35b4f7-61b4-2ec1-01ed-46aef1d2b1d7.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2Ffb35b4f7-61b4-2ec1-01ed-46aef1d2b1d7.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=4392047cbecc30dea6b1878287a99785)

まず、Dockerに対する理解をしていきましょう。

Dockerとは「 **コンテナ型の仮想環境を作成、共有、実行するためのプラットフォーム** 」です。クジラのようなアイコンが特徴的です。

私が最初に勉強をした時に、

**「コンテナ型の仮想環境ってなんだ？」**

と思ったので、これを解決するために、仮想環境とコンテナについて解説したいと思います。

### 仮想環境

仮想環境とは、仮想的に構築された環境のことを指します。仮想的という言葉は擬似的という言葉に置き換えられます。例えば仮想環境では、WindowsPC上でLinuxOSを動作させることができます。これは、仮想環境により、LinuxOSが擬似的に再現されているということになります。

[![スクリーンショット 2023-11-27 13.16.02.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/61426301-ecc2-5187-26e9-c632634f2d7a.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F61426301-ecc2-5187-26e9-c632634f2d7a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e582c06f3257277bb711134a24579130)

### コンテナ

コンテナはその名の通り、物流で使用されるコンテナをイメージしてもらって大丈夫です。コンテナという箱の中にアプリケーションに必要なものが詰まっています。

[![スクリーンショット 2023-11-27 13.44.47.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/5e9ac372-6037-3b18-6c8c-5fa40499b485.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F5e9ac372-6037-3b18-6c8c-5fa40499b485.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=85148c27ff1fe7dc021a25887bced3b1)

Dockerでは、コンテナ技術を利用して仮想環境を構築しています（コンテナ利用した仮想環境構築の方法で一番広まっているのがDockerです）。このコンテナ技術を利用すると何が嬉しいのか次のDockerのメリットで解説します。

## Dockerのメリット

### 軽量に起動できる

Dockerは従来の仮想環境に比べて軽量に起動することができます。それはコンテナ技術を利用することで実現されています。

[![スクリーンショット 2023-11-27 14.21.31.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/ebbbeb31-ba89-1017-3c8d-764b4d5edb03.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2Febbbeb31-ba89-1017-3c8d-764b4d5edb03.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=25c1409dd8f15cf58c3f61e134f93f91)

図の左側は従来の仮想環境ソフトウェアを用いた場合になります。従来の仮想技術では、仮想化ソフトウェアによりゲストOSが構成され、その上にミドルウェアやアプリケーションが構築されています。そのため、起動するたびにゲストOSを起動する必要がありました。一方Dockerでは、コンテナエンジンを用いて、ゲストOSがあるかのように環境を構築しています。

**従来の仮想マシン：仮想化ソフトウェア→ゲストOS→ミドルウェア等**  
**Docker：コンテナエンジン→ミドルウェア等**

このようにDockerではゲストOSを起動させる分の処理が簡略化されているため、従来の仮想技術よりも軽量に起動することができます。これにより、従来の仮想技術に比べてハードウェアのリソース削減を行うことができています。

### 共有のしやすさ

後述しますが、DockerではDockerイメージと呼ばれる、テンプレートのようなものを使用して、コンテナ（実行環境）を構築します。Docker HubなどからDockerイメージをダウンロードすれば、誰でも同じ環境を簡単に構築することができます。

[![スクリーンショット 2023-11-27 173728.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/267b420a-bff9-3546-1d9c-de2fd6741683.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F267b420a-bff9-3546-1d9c-de2fd6741683.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=81f68dd3b417abae2095b9e892edc965)

### 開発の一貫性

開発の現場において、プログラミングなどのソースコードはGithubを用いれば簡単に行うことができます。しかし、プログラミングコードを実行するために、実行環境を整える必要があります。Dockerでは、アプリケーションとその依存関係をまとめ、ほかの環境でも同じように動作するように保証します。つまり開発環境と本番環境で同じコンテナを用いれば動作が保証されるため、開発チームと運用チームで発生する環境のずれにより発生する問題を最小限に抑えることが可能です。

### 簡単なバージョン管理

Dockerイメージはバージョン管理が容易で、変更履歴を確認し、特定のバージョンに戻すことができます。これにより、アプリケーションのアップデートやロールバックが簡単に行えます。

### セキュリティの向上

Dockerはセキュリティスキャンツールと統合しやすく、コンテナイメージが脆弱性を含んでいないかを簡単に確認できます。これにより、セキュリティの問題を素早く検知して対処できます。

### 迅速な展開とスケーリング

Dockerは従来の仮想技術よりも軽量に起動可能です。これにより、新しいコンテナを迅速にデプロイし、必要に応じスケールアウトをすることができます。

## Dockerの基本概念

これまでにDockerの全体的構成について解説しました。次により細かく、Dockerを構成する基本概念について解説したいと思います。ここでは特に、イメージ、コンテナ、レジストリ、Dockerfileについて解説します。Dockerfileは環境構築を行う上で重要なものであるため抑えましょう。

### Dockerイメージ

Dockerイメージはコンテナを起動するための設定ファイルをまとめたものとなっています。Dockerイメージを起動することで、コンテナが作成されます。Dockerではこのイメージを共有することで、異なるマシン(PC)上で同じ実行環境（コンテナ）を構築することができます。

より、Dockerイメージの理解を深めるためにDockerイメージの構造について説明します。

Dockerイメージは複数のイメージレイヤと呼ばれる"層"によって構成されています。１つの層に対して1つのミドルウェアが対応します。これらの層は読み取り専用で編集はできません。イメージからコンテナを生成し、コンテナ内でミドルウェアをインストールした場合には、新しい層が追加されることになります。この新しい層は編集可能です。また、コンテナで追加した層を含めて、新たにイメージを作成することもできます。

[![スクリーンショット 2023-11-28 013411.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/b1fb8703-5add-462c-6b6d-9ba2fc5fa349.jpeg)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2Fb1fb8703-5add-462c-6b6d-9ba2fc5fa349.jpeg?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7f6ec658a7a09685925729fc8b1cdb46)

### Dockerコンテナ

今まで説明を行いましたが、コンテナは実際のアプリケーションの動作環境になります。コンテナはイメージを起動することで動かすことができます。私たちはこのコンテナ内で作業を行い、アプリケーションやシステムの開発を行います。

[![スクリーンショット 2023-11-27 231722.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/d43b7957-b71a-7a76-7700-dcba890b6642.jpeg)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2Fd43b7957-b71a-7a76-7700-dcba890b6642.jpeg?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e73d56301dbc96b53c4d18089604d032)

### レジストリとは

Dockerレジストリとは、Dockerイメージを保存する場所になります。Docker Hubなどがレジストリに該当します。レジストリにDockerイメージを保存することで、他の人と共有を行ったりすることができます。Docker Hubの利用方法については、 [こちらの章](https://qiita.com/S4nTo/items/#docker-hub%E3%81%A8%E3%82%A4%E3%83%A1%E3%83%BC%E3%82%B8%E3%81%AE%E5%85%B1%E6%9C%89) で解説します。

### Dockerfile

DockerfileとはDockerイメージを構築するための設計図のようなものです。つまり、Dockerfileを編集することで、環境構築ができるようになります。

[![スクリーンショット 2023-11-27 222811.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/0ecf1f55-b414-1b0e-49eb-b33d5c546203.jpeg)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F0ecf1f55-b414-1b0e-49eb-b33d5c546203.jpeg?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=c17fa2de502b78cd3df4e5286fbdb230)

### Dockerfileの構文

Dockerfileは命令と引数により構成されます。一行につき一つの命令が与えられ実行する際に1行目から順番で実行されます。

```text
{命令}　{引数}
```

こちらが命令の一例です。

| 命令 | 使用用途 |
| --- | --- |
| FROM | ベースイメージを指定する |
| RUN | コマンドを実行する |
| CMD | コンテナの実行コマンド |
| ENV | 環境変数の指定 |
| COPY | ファイルコピー |

##### FROM

FROMではベースイメージの指定を行います。ベースイメージとは、さきほどのDockerイメージの際に説明したイメージレイヤの最初のレイヤに該当します。つまりイメージはベースイメージから積み上げる形で構成されていきます。ベースイメージには、ベースとなるDockerイメージが入ります。最初の内はベースイメージにはUbuntuやCentOSなどが入るという認識でも大丈夫です。

例えば、ベースイメージにUbuntuにした場合次のように書かれます。

```dockerfile
FROM ubuntu:22.04
```

##### RUN

RUNでは、対応するディストリビューション(Ubuntu、CentOS、debianなど）の実行コマンドを入力して必要なライブラリなどのインストールを行います。

```dockerfile
RUN apt-get update && apt-get install -y
```

##### COPY

ファイルのコピーを行います。{src}はコピー元のファイルまたはディレクトリであり、{dest}はコピー先のパスです。

```dockerfile
COPY {src} {dest}
```

##### WORKDIR

作業ディレクトリを指定することができます。

```dockerfile
WORKDIR /app
```

この場合コンテナ起動時にappディレクトリがカレントディレクトリとなっています。

##### CMD

CMDでは"コンテナの"実行コマンドを指定できます。CMDを編集することで、実行コマンドの省略を行えます。

##### 全体

Dockerfileの中身をこちらを用いて確認していきましょう。

```dockerfile
# ベースイメージを指定
FROM ubuntu:20.04

# 作業ディレクトリを/appに設定
WORKDIR /app

# 環境変数を表示するスクリプトをコピー
COPY print_env.sh .

# スクリプトを実行するコマンド
CMD ["./print_env.sh"]
```

このコンテナを実行するとprit\_env.shが実行されて環境変数が表示されます(print\_env.shは別途用意する必要があります)。

### 一連の流れ

[![スクリーンショット 2023-11-27 231944.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/8964792a-5188-c0a3-86a0-a1876d903f77.jpeg)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F8964792a-5188-c0a3-86a0-a1876d903f77.jpeg?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=178ad5dd47c569cd0e2a0cbc4d5b1bc1)

こちらがDockerにおける環境構築の基本的な流れとなっています。まず、Dockerfileを私たちが編集することで、イメージの設計書を作成します。設計書を元に、イメージを作成し、イメージを起動することで、コンテナ（実行環境）を構築します。これがDockerで行われる基本的な流れとなっています。

[![スクリーンショット 2023-11-27 235844.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/71dba689-5c96-0dcd-f62d-9c6c3469611a.jpeg)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F71dba689-5c96-0dcd-f62d-9c6c3469611a.jpeg?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2ea5d22dcab1a60ddb6c6d49f0f50dc3)

また、Dockerfileを編集しなくとも、Docker Hubからイメージをダウンロードすることでコンテナを起動することもできます。

### Dockerの基本コマンド

##### イメージ

・Docker Hubからイメージを取得

```text
docker image pull example_image
```

・Dockerのイメージ一覧の取得

```text
Docker image ls -a
```

\-aはオプションですべてのイメージが表示されます。

・DockerfileからDockerイメージの作成

```text
docker image build -t example_image:1.0 {Dockerfileのパス}
```

\-tのオプションを与えることで、イメージの名前とタグを設定します。タグはバージョンを示したりするのに用います。

・Dockerイメージの削除

```text
docker image rm example_image
```

rmでDockerイメージの削除を行います。

##### コンテナ

・コンテナの一覧を表示

```text
docker container ls -a
```

・コンテナの起動

```text
docker container run -- name example_container
```

・コンテナの停止

```text
docker container stop example_container
```

・停止中のコンテナの起動

```text
docker container start example_container
```

・起動中のコンテナの再起動

```text
docker container restart example_container
```

・コンテナの削除

```text
docker container rm example_container
```

基本となるコマンドの一例を紹介しましたが、コマンドはオプションを含め、たくさんあるのでこちらを参考に調べてみてください。

## ボリュームと永続性

### データ永続性の確保

コンテナ内で管理されているデータはコンテナを削除したときに、消えてしまいます。データを消さないためにも、データの永続性の確保が必要となります。そこで必要となるのがマウントとボリュームです。

##### マウントとは

コンテナの外にあるデータを、コンテナ内で利用できるようにすることをマウントと呼びます。Dcokerにおいてマウントは3種類あります。

**・ボリューム**  
**・バインドマウント**  
**・一時ファイルシステムマウント**

この3種類となります。  
今回はDockerで推奨されるボリュームに焦点を当てて解説します。

##### ボリューム(volume)とは

ボリュームはデータを永続する場所のことを指します（USBなどの外付け記憶装置のようなイメージです）。そのため、ボリュームをコンテナに結び付ける（マウント）ことでデータの永続化を行います。またボリュームは複数のコンテナに接続することもできます。

## Docker Composeの基本

### Dcoker Composeの役割

Dockerを利用した開発では複数のコンテナを起動して、コンテナ間の通信を行う必要があります。例えば、Dockerを利用して、WordPressを実行する場合には、PHPにより構成されたコンテナとデータベースのコンテナを利用する必要があります。いちいち、手動でDockerイメージからコンテナの起動などを行うのは連携するコンテナが増えると面倒になってきます。その手間を解消するために必要となるのが、 **Docker Compose** です。

[![スクリーンショット 2023-11-28 220644.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/5a393f7c-e98b-86a5-504e-06e3e8c64cc5.jpeg)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F5a393f7c-e98b-86a5-504e-06e3e8c64cc5.jpeg?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=930aeca1e0af33bcd543ef57930ece60)

Docker Composeでは、compose.ymlと呼ばれるDockerに対する指示書を作成します。Dockerはこの指示書に基づいて複数のコンテナを同時に起動します。

### YAMLファイルの構文

こちらがWordPressを実現するためのcompose.ymlとなっています。構文の細かな解説は省略しますが、コメントでざっくりと説明を付け加えとくので参考にしてみてください！

```yaml
version: '3' #Docker-composeのバージョンを指定します

services: #起動するコンテナごとに一つのサービスを定義します。
   db: #dbという名前をつけてサービスを定義します
     image: mysql:5.7 #イメージの指定
     volumes: #パスをボリュームとしてマウントします(ホスト側:コンテナ側のように記載します)
       - db_data:/var/lib/mysql
     restart: always #再起動ポリシーの設定(alwaysだとコンテナが停止すると常に再起動します)
     environment: #環境変数の設定を行います。
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on: #サービス間の依存関係を指定します(起動するときはdb→wordpress、停止のときは逆順となります)
       - db
     image: wordpress:latest
     ports: #ポートを公開します（ホスト:コンテナで記載します.コンテナのみを指定した場合はホストはランダムに設定されます）
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
volumes: #ボリュームの設定db_dataという名前のボリュームが定義されます。
    db_data:
```

こちらがリファレンスになるので、他のサービス設定を確認したい方は参考にしてみてください。

### Docker Composeの基本コマンド

Docker Composeの基本コマンドを紹介します。

・copose.ymlで記載された各イメージをまとめてビルド

```text
docker-compose build
```

・コンテナを一括で起動

```text
docker-compose up -d
```

・コンテナの停止

```text
docker compose stop
```

・コンテナの再起動

```text
docker compose start
```

・コンテナをまとめて削除

```text
docker compose down
```

より、詳細なコマンドはこちらを参考にしてみてください。

## 開発環境構築の具体的な手順

ここまで長々と説明しましたが、実際に開発環境を構築してみましょう。

### すでにあるイメージを使用する場合

DockerHubにはDockerイメージが公開されているため、自分でDockerfileをいじらなくてもイメージを引っ張ってきてコンテナを作成することができます。まずはその方法を紹介します(コマンドをぽちぽちしていけば構築できます)

**・Docker Hubからイメージの取得**

```text
docker image pull ubuntu
```

イメージがあることを確認します。

```bash
$ docker image ls
```

**・イメージからコンテナの起動**

```bash
$ docker container run -it -d --name ubuntu-qiita ubuntu
```

\-itはコンテナを対話的なモードで実行するためのオプションです。-dではコンテナがバックグラウンドで実行されるようにします。--nameでコンテナの名前を指定します。

コンテナが作成されていることを確認します。

```bash
$ docker container ls
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
3b35a6bedeb5   ubuntu    "/bin/bash"   9 minutes ago   Up 9 minutes
```

次のコマンドで起動されたコンテナ内に入ることができます。

```text
$ docker exec -it ubuntu-qiita bash
```

コンテナ内はこのようになっています。

[![スクリーンショット 2023-11-30 18.47.37.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/84f2b58c-a1cf-5a38-1236-f3c5ffc86142.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F84f2b58c-a1cf-5a38-1236-f3c5ffc86142.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=648e0b092d2ffbc3873d8db39daea6c7)

これでUbuntuの環境構築が完了しました。

コンテナから出る時はexitでコンテナから出ることができます。

[![スクリーンショット 2023-11-30 20.07.33.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/67fad4c5-a6bd-7900-bc22-eeac98752bdb.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F67fad4c5-a6bd-7900-bc22-eeac98752bdb.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f4db43da825a90496f5f0f60ec9a25ad)

exitでコンテナから出てもコンテナ自体は起動したままなので、コンテナを停止させます。

```text
$ docker container stop ubuntu-qiita
```

このコマンドでコンテナが停止します。

```text
$ docker container rm ubuntu-qiita
```

このコマンドで停止したコンテナを削除することができます。

### Dockerfileから作成

続いて、Dockerfileからイメージを作成して動作させてみましょう。Pythonを実行できるように環境構築を行います。

```text
Docker-Python
└ dockerfile
```

```dockerfile
# ベースイメージを指定
FROM python

RUN apt-get update
RUN pip install --upgrade pip

WORKDIR /Qiita
```

短いDockerfileになりますが、これだけでPython環境を構築できます。Dockerfileをもとにイメージを作成します。

```text
$ docker build -t python-qiita:latest .
```

実際にイメージが生成されました。

[![スクリーンショット 2023-11-30 22.59.29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/839d8cb1-5d02-8a50-0b28-ec78efc4a9ce.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F839d8cb1-5d02-8a50-0b28-ec78efc4a9ce.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8612c46b0c9dd2e713c27124d9fc15f9)

あとは、先ほどと同様にコンテナを起動してコンテナ内に入れば作業ができます。今回はWORKDIRにQiitaを設定しているため、Qiitaディレクトリがカレントディレクトリとなります。

[![スクリーンショット 2023-11-30 23.21.18.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/e2f8bad1-0b80-d4ff-8170-2bac56f51a30.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2Fe2f8bad1-0b80-d4ff-8170-2bac56f51a30.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=f264dc87b0d8d40ad96d94d27bf2e51a)

実際にPythonを実行することができました。

[![スクリーンショット 2023-11-30 23.28.20.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/45cd1741-dde0-14df-7fbe-f44151206994.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F45cd1741-dde0-14df-7fbe-f44151206994.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=82841e6b13b6f3d3b656a7f3e42a3444)

[![スクリーンショット 2023-11-30 23.29.39.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/3242217d-8975-4022-7533-64b1fe25144d.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F3242217d-8975-4022-7533-64b1fe25144d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=a3cf2ca2c68e5a21b02c13ec76facc79)

### Docker Composeを利用した複数のコンテナ管理

Docker composeを利用して、WordPressの環境を構築してみましょう。

以下のようなディレクトリ構成にします。

```text
Docker-WordPress
└ compose.yml
```

```yml
version: '3'

services:
   db:
     platform: linux/x86_64 #M1チップマックの場合はこの行を追加してください
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
volumes:
    db_data:
```

.ymlファイルがあるディレクトリで以下のコマンドを実行します。

```text
docker-compose up -d
```

そうすると、compose.ymlをもとにコンテナを作成、起動します。実際にコンテナが起動していることを確認します。dockerfileがなくとも自動でイメージを引っ張ってきてコンテナを起動してくれます。

```text
$ docker container ls
```

コンテナが起動している状態で [http://localhost:8000](http://localhost:8000/) にアクセスするとWordPressを使用することができます。

[![スクリーンショット 2023-11-30 19.35.18.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/c3a69f12-5b0d-e2b8-07cd-a91f0d211240.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2Fc3a69f12-5b0d-e2b8-07cd-a91f0d211240.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=900741fd410d5ebed0b5c15572b44dd3) [![スクリーンショット 2023-11-30 19.37.08.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/61d1e3eb-be02-0ed8-dcaf-1fdcf4308a32.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F61d1e3eb-be02-0ed8-dcaf-1fdcf4308a32.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=af4463bc25316288491b69224df6c1c3)

コンテナの停止と削除を行ってみましょう。

```bash
$ docker compose stop
[+] Stopping 2/2
 ✔ Container docker-wordpress-wordpress-1  Stopped                                                                                1.3s 
 ✔ Container docker-wordpress-db-1         Stopped
```

```bash
$ docker compose down
[+] Running 3/3
 ✔ Container docker-wordpress-wordpress-1  Removed                                                                                0.0s 
 ✔ Container docker-wordpress-db-1         Removed                                                                                0.0s 
 ✔ Network docker-wordpress_default        Removed
```

これで、作成したコンテナの削除が完了しました。（イメージは残っているため、イメージまで削除したい場合は--rmi allをオプションとして選択してください。)

## Docker Hubとイメージの共有

これまでに散々紹介してきたDocker Hubの利用方法について最後に簡単に説明します。

### Docker Hubの利用方法

Docker Hubにアクセスして、アカウントを作成しましょう。Googleアカウント、GitHubアカウントを使用してアカウントを作成することができます。

このようにDocker Hub上には様々なイメージが保存されています。興味のあるものを引っ張って遊んでみてください。

[![スクリーンショット 2023-11-30 21.30.45.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/874558c0-8057-9e72-15fc-803d926fd762.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F874558c0-8057-9e72-15fc-803d926fd762.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fd879dce4c911ba637d6725c613050ad)

### イメージのビルドと共有

Dockerfileで自分でオリジナルのイメージを作成した場合、そのイメージをDocker Hub上に共有することができます。

Docker Hub上にpushするためには、リポジトリの作成、Docker Hubへのログイン、イメージのタグ付け、Docker Hubへのプッシュのステップとなります。

まず、Docker Hub上でリポジトリの作成を行います。今回はubuntuというリポジトリを作成します。この時通常イメージ名をリポジトリ名とします。

[![スクリーンショット 2023-11-30 21.57.01.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/5e61cb4d-6b6f-2f7e-01e0-88ca820067f5.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F5e61cb4d-6b6f-2f7e-01e0-88ca820067f5.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=57288f551d978d279b76b8e00a4aeb24)  
[![スクリーンショット 2023-11-30 22.07.20.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3125904/7f962b16-221c-9adf-16ba-673e7766bcb5.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3125904%2F7f962b16-221c-9adf-16ba-673e7766bcb5.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=bc3330c80344a14d24017a858cf7e53a)

次にこちらのコマンドでdockerにログインします。

ユーザー名とパスワードを要求されます。ログインが完了すると、「Login Succeeded」と表示されます。

次にpushしたいイメージに次のように名前を変更します。

```text
docker tag <前のイメージ名>:タグ　<ユーザー名>/<リポジトリの名>:タグ
```

```text
docker tag ubuntu user_name/ubuntu:latest
```

pushする際にユーザー名とリポジトリの指定が必要となるため、イメージ名がわかりにくくならないようにリポジトリ名にはイメージ名を設定します（イメージ名と関係のない名前を設定すると何のイメージが入っているのかわかりにくくなる）。

最後にDocker Hubに名前を変更したイメージをプッシュします。

```text
docker image push <ユーザー名>/<リポジトリ名>:タグ
```

```text
docker image push user_name/ubuntu:latest
```

## よりDockerの理解を深めるために、、、

### Dockerチュートリアル

[こちらのサイト](https://github.com/docker/getting-started/tree/6190776cb618b1eb3cfb21e207eefde511d13449) ではDockerを使用するためのチュートリアルが公開されています。チュートリアルを実際に行うことで、Dockerの理解をより深めましょう。

[@Michinosuke](https://qiita.com/Michinosuke "Michinosuke") さんが公開しているDockerチュートリアル和訳された記事も公開されているので参考にしてみてください。

### 書籍

今回できる限り盛り込んで説明を行いましたが、もちろんこの記事がDockerのすべてではありません。より体系的に学びたい場合は書籍から勉強することをおすすめします。

[仕組みと使い方がわかる Docker&Kubernetesのきほんのきほん](https://www.amazon.co.jp/%E4%BB%95%E7%B5%84%E3%81%BF%E3%81%A8%E4%BD%BF%E3%81%84%E6%96%B9%E3%81%8C%E3%82%8F%E3%81%8B%E3%82%8B-Docker-Kubernetes%E3%81%AE%E3%81%8D%E3%81%BB%E3%82%93%E3%81%AE%E3%81%8D%E3%81%BB%E3%82%93-%E5%B0%8F%E7%AC%A0%E5%8E%9F%E7%A8%AE%E9%AB%98/dp/4839972745)

[Docker/Kubenetes 実践コンテナ開発入門](https://www.amazon.co.jp/Docker-Kubernetes-%E5%AE%9F%E8%B7%B5%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E9%96%8B%E7%99%BA%E5%85%A5%E9%96%80-%E5%B1%B1%E7%94%B0-%E6%98%8E%E6%86%B2/dp/4297100339)

## おわりに

いかがでしたでしょうか。今回はdockerの開発環境の構築という点に焦点を当てて解説しました。1人でも多くの人の参考になればと思います。

弊社Nucoでは、他にも様々なお役立ち記事を公開しています。よかったら、Organizationのページも覗いてみてください。  
また、Nucoでは一緒に働く仲間も募集しています！興味をお持ちいただける方は、 [こちら](https://www.recruit.nuco.co.jp/?qiita_item_id=977d28b0eac316915702) まで。

[4](https://qiita.com/S4nTo/items/#comments)

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2FS4nTo%2Fitems%2F977d28b0eac316915702&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2FS4nTo%2Fitems%2F977d28b0eac316915702&realm=qiita)

[629](https://qiita.com/S4nTo/items/977d28b0eac316915702/likers)

785