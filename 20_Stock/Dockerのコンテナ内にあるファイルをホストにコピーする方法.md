---
title: "Dockerのコンテナ内にあるファイルをホストにコピーする方法"
source: "https://qiita.com/gorou_178/items/a93c6f7b440b1c5890bc"
author:
  - "[[Qiita]]"
published: 2018-03-12
created: 2025-05-15
description: "docker container cp コマンドを使います。コンテナからホストへ docker container cp コンテナID:コンテナ内のコピー元 ホスト側のコピー先 $ docker container cp c…"
tags:
  - "#tool"
  - "#tool/docker"
  - "#reference"
  - "#basic"
---
14[tech](https://zenn.dev/tech-or-idea)

## はじめに

Laravelで、CSVファイルを出力する機能を実装した際に、出力したCSVファイルの内容を確認したいことがありました。  
最初、 `cat` コマンドで確認したのですが、ホストに直接コピーして確認する方法はないかなー？と調べたトコロ、まさに求めていた方法があったのでメモしておきます。

## 手順

### DockerコンテナのコンテナIDを確認

以下コマンドで、対象のコンテナIDを確認します。

```
$ docker ps -a

CONTAINER ID   IMAGE   COMMAND   CREATED   STATUS   PORTS   NAMES
************   .....   .......   .......   ......   .....   .....
```

### コンテナからローカルにファイルをコピー

以下コマンドで、コピーしたいファイルをホストにコピーします。

```
$ docker cp [コンテナID]:[コピーするファイルパス] [コピー先のファイルパス]

// $ docker cp ************:/usr/local/test.csv /Desktop
```

コピー先にファイルがコピーされていると思います！

#### 逆（ホストからコンテナへコピー）もあります。

こちらは実際に使用したことないですが、逆（ホストからコンテナへコピー）もありました。

```
$ docker cp [コピーするファイルパス] ************:[コピー先のファイルパス]

// $ docker cp /Desktop/test.csv ************:/usr/local
```

## 参考

[GitHubで編集を提案](https://github.com/DatchLive/Zenn/blob/main/articles/2effe192d1568f.md)

14

### Discussion

![](https://static.zenn.studio/images/drawing/discussion.png)

ログインするとコメントできます