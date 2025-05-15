---
title: "SQL Server へ他のPCからも接続できるようにする"
source: "https://www.usagi1975.com/2020_04May/02_0000/"
author:
  - "[[usagi1975]]"
published: 2020-04-02
created: 2025-05-15
description: "いくつかの手段があるようなのですが、よく使われている方法で設定しました。まとめると下記の手順です。SQL Server 設定SQL ServerSQL Server Browser設定Windowsファイアウォール設定TCPポートを開くSQL Server Brows…"
tags:
  - "#database"
  - "#database/sqlserver"
  - "#tutorial"
  - "#basic"
---
![](https://image.jimcdn.com/app/cms/image/transf/dimension=539x10000:format=png/path/s64d21b00408982fc/image/i45e80f1f435bf79d/version/1689664391/image.png)

インストールしたSQL Server へ他のPCから接続できるようにします。

SQL Server のデフォルト設定は、他のPCから接続できないようになっています。設定変更やファイヤウォールを設定することでExpress Editionもサーバーとして使用できるようになります。

  

**目次**

[SQL Server の インスタンス名を確認](https://www.tpics.co.jp/2023/06/13/sql-server-%E3%81%B8%E4%BB%96%E3%81%AEpc%E3%81%8B%E3%82%89%E3%82%82%E6%8E%A5%E7%B6%9A%E3%81%A7%E3%81%8D%E3%82%8B%E3%82%88%E3%81%86%E3%81%AB%E3%81%99%E3%82%8B/#cc-m-header-7915543611)

[既定のインスタンス(MSSQLSERVER)へ他のPCから接続可能にする](https://www.tpics.co.jp/2023/06/13/sql-server-%E3%81%B8%E4%BB%96%E3%81%AEpc%E3%81%8B%E3%82%89%E3%82%82%E6%8E%A5%E7%B6%9A%E3%81%A7%E3%81%8D%E3%82%8B%E3%82%88%E3%81%86%E3%81%AB%E3%81%99%E3%82%8B/#cc-m-header-7914528811)

[名前付きインスタンス(SQLEXPRESS)へ他のPCから接続可能にする](https://www.tpics.co.jp/2023/06/13/sql-server-%E3%81%B8%E4%BB%96%E3%81%AEpc%E3%81%8B%E3%82%89%E3%82%82%E6%8E%A5%E7%B6%9A%E3%81%A7%E3%81%8D%E3%82%8B%E3%82%88%E3%81%86%E3%81%AB%E3%81%99%E3%82%8B/#cc-m-header-7914530911)

[接続できないとき](https://www.tpics.co.jp/2023/06/13/sql-server-%E3%81%B8%E4%BB%96%E3%81%AEpc%E3%81%8B%E3%82%89%E3%82%82%E6%8E%A5%E7%B6%9A%E3%81%A7%E3%81%8D%E3%82%8B%E3%82%88%E3%81%86%E3%81%AB%E3%81%99%E3%82%8B/#cc-m-header-7916296511)

**事前に必要な、SQL Server のインストールやTPiCSのテーブル作成については**

[SQL Server Express Edition でTPiCSデモ環境作成](https://www.tpics.co.jp/2023/06/13/tpics%E3%83%87%E3%83%A2%E7%89%88%E3%81%AE%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9-sql-server-%E3%82%92%E6%A7%8B%E7%AF%89%E3%81%99%E3%82%8B/ "SQL Server Express Edition でTPiCSデモ環境作成") を参考にしてください。

大まかに、事前に必要な作業は以下です。

　・SQL Server の構成マネージャー　SQL Server サービスの自動起動

　・Management Studio でWindows 認証→混合認証  
　・Management Studio でsa ユーザーの有効化  
　・Management Studioから sa ユーザー接続  
　・TPiCSのデータベース設定ツールから接続  
　・TPiCS本体からの接続

外部から接続に必要なサービスや設定は、インスタンスによって変わります。

![](https://image.jimcdn.com/app/cms/image/transf/none/path/s64d21b00408982fc/image/iceb97926466875e1/version/1688051535/image.png)

**SQL Server の構成マネージャ**

Windowsのスタートメニューから \[SQL Server 2022 構成マネージャー\] をクリック。

SQL Serverをインストールしたコンピュータで確認します。

クライアントPCから、サーバーの構成マネージャーは開けません。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=511x1024:format=png/path/s64d21b00408982fc/image/ia63684390f87073b/version/1689324467/image.png)

**SQL Server 名前 が "SQL Server (MSSQLSERVER)"ならば、**  
**既定のインスタンス**

[既定のインスタンス(MSSQLSERVER)へ他のPCから接続可能にする](https://www.tpics.co.jp/2023/06/13/sql-server-%E3%81%B8%E4%BB%96%E3%81%AEpc%E3%81%8B%E3%82%89%E3%82%82%E6%8E%A5%E7%B6%9A%E3%81%A7%E3%81%8D%E3%82%8B%E3%82%88%E3%81%86%E3%81%AB%E3%81%99%E3%82%8B/#cc-m-header-7914528811)

  

  

![](https://image.jimcdn.com/app/cms/image/transf/dimension=519x1024:format=png/path/s64d21b00408982fc/image/i745824bfd1906a8b/version/1689324570/image.png)

**SQL Server 名前 が "SQL Server (SQLEXPRESS)"** **ならば、  
名前付きインスタンス**

他に、SQL Serverのインストール時に、インスタンス名を"SQL2022"のように設定した場合も、"SQL Server (SQL2022)"のように表示され、これも名前付きインスタンス。

"MSSQLSERVER"以外は名前付きインスタンス。

[名前付きインスタンス(SQLEXPRESS)へ他のPCから接続可能にする](https://www.tpics.co.jp/2023/06/13/sql-server-%E3%81%B8%E4%BB%96%E3%81%AEpc%E3%81%8B%E3%82%89%E3%82%82%E6%8E%A5%E7%B6%9A%E3%81%A7%E3%81%8D%E3%82%8B%E3%82%88%E3%81%86%E3%81%AB%E3%81%99%E3%82%8B/#cc-m-header-7914530911)

  

## 既定のインスタンス(MSSQLSERVER)へ他のPCから接続可能にする

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i22d5e0c5a2258a9e/version/1689648169/image.png)

**SQL Server のサービスの設定  
**

デフォルトの状態でよい。

・SQL Browser サービス ：停止(接続には不要)

・SQL Server (MSSQLSERVER) ：実行中

・SQL Server エージョント (MSSQLSERVER) ：停止(接続には不要)（ExpressEdtion は有効にできません）

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/id2dd722b00e5254d/version/1689648187/image.png)

**SQL Server ネットワークの構成の設定  
**

・名前付きパイプ ：無効

・TCP/IP ：有効(必須)

  

![](https://image.jimcdn.com/app/cms/image/transf/dimension=566x1024:format=png/path/s64d21b00408982fc/image/i869af7c3fe65bd46/version/1689850580/image.png)

**サーバー側のWindowsファイヤウォールを設定**

通常許可されてない。

**ファイヤーウォールの許可**

・TCPプロトコルのポート：1433　通信許可が必須。

（UDPプロトコルのポート：1434 通信できなくてよい。）

**サービス名で指定する方法**

　ポートで設定の方法の他にサービスの指定して通信を許可する方法もある。

　サービス名：SQL Server MSSQLSQRVER を許可

※ファイアウォールの設定は\[コントロールパネル\]-\[システムとセキュリティ\]-\[Windows Defender ファイアウォール\]から設定を行います。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/ib8c61018090f2091/version/1689850096/image.png)

**Windows Defender ファイヤウォールで新しい規則追加**

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i885f723dfa5a11f4/version/1689850580/image.png)

**規則の種類**

●ポート を選択。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i3e149da2ce0b74ee/version/1689850580/image.png)

**プロトコルおよびポート**

プロトコル

●TCP を選択

特定のローカルポート：1433

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i25c7c77b1a91cf7f/version/1689850580/image.png)

**操作**

接続の許可（デフォルト）。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i6c72a4549c0ac403/version/1689850580/image.png)

**プロファイル**

☑ドメイン

☑プライベート

☑パブリックをチェック（デフォルト）。

ドメインネットワーク環境内のみ通信を許可することや、ドメインとプライベートネットワーク環境のみ許可する設定をします。

後からも変更できるので、接続検証のために、３つとも（どこでのネットワーク接続でも）通信許可でよいです。

接続が確認できてから、パブリックはOFFのような設定をしていくとよいです。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/if1bad95935a7da0a/version/1689850581/image.png)

**規則の名前**

規則の名前は自由に付けてください。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=887x1024:format=png/path/s64d21b00408982fc/image/icb65e9d3530e5484/version/1689850581/image.png)

### 既定のインスタンスへ接続するクライアントPC側の設定

 ●SQL Server 　クライアントソフト不要。

 ●SQL Server Management Studio

   ローレベル接続の検証には、SQL Server Management Studio をインストールします。

   PCからの接続検証ができれば、大概は同様に接続できます。

   ※TPiCSのクライアント使用には不要です。

●ファイヤーウォール

   設定不要。

## 名前付きインスタンス(SQLEXPRESS)へ他のPCから接続可能にする

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i5e7967a23094543a/version/1689648223/image.png)

**SQL Server のサービス設定  
**

デフォルトから、SQL Browserの起動が必要。

・SQL Browser サービス ：実行中（必須）

・SQL Server (SQLEXPRESS) ：実行中

・SQL Server エージョント (SQLEXPRESS)：停止（ExpressEdtion は有効にできません）

  

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i50730ebcc44b9ee0/version/1689648230/image.png)

**SQL Server ネットワークの構成の設定  
**

・名前付きパイプ ：有効（必須）

・TCP/IP ：無効

  

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i86e1334411783635/version/1689648236/image.png)

**SQL Server (SQLEXPRESS)を再起動**

［SQL Server 構成マネージャー（ローカル）］‐［SQL Server のサービス］‐「SQL Server (SQLEXPRESS)」を右クリックし、再起動をクリック。

名前付きパイプの設定を有効にするため、SQL Server の再起動が必要。

  

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i00e422e4b19257ce/version/1689839320/image.png)

**サーバー側のWindowsファイヤウォールを設定**

通常許可されてない。

**ファイヤーウォールの許可**

（TCPプロトコルのポート：1433　通信できなくてよい。）

・UDPプロトコルのポート：1434　通信許可が必須。

**サービス名で指定する方法**

　ポートで設定の方法の他にサービスの指定して通信を許可する方法もある。

　サービス名：SQL Browser　を許可

※ファイアウォールの設定は\[コントロールパネル\]-\[システムとセキュリティ\]-\[Windows Defender ファイアウォール\]から設定を行います。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i531b816f2f684f75/version/1689850095/image.png)

**Windows Defender ファイヤウォールで新しい規則追加**

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i9842ef7d81e399d7/version/1689850126/image.png)

**規則の種類**

●ポート を選択。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i691ebd0cb729aea1/version/1689850187/image.png)

**プロトコルおよびポート**

プロトコル

●UDP を選択

特定のローカルポート：1434

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i478cb08aea119981/version/1689850264/image.png)

**操作**

接続の許可（デフォルト）。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/i6e4dc63dc2ec27ef/version/1689850313/image.png)

**プロファイル**

☑ドメイン

☑プライベート

☑パブリックをチェック（デフォルト）。

ドメインネットワーク環境内のみ通信を許可することや、ドメインとプライベートネットワーク環境のみ許可する設定をします。

後からも変更できるので、接続検証のために、３つとも（どこでのネットワーク接続でも）通信許可でよいです。

接続が確認できてから、パブリックはOFFのような設定をしていくとよいです。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=524x1024:format=png/path/s64d21b00408982fc/image/ia8fa0a2044131898/version/1689850378/image.png)

**規則の名前**

規則の名前は自由に付けてください。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=907x1024:format=png/path/s64d21b00408982fc/image/if1667e6ec901705f/version/1689850538/image.png)

### 名前付きインスタンスへ接続するクライアントPC側の設定

 ●SQL Server 　クライアントソフト不要。

 ●SQL Server Management Studio

   ローレベル接続の検証には、SQL Server Management Studio をインストールします。

   PCからの接続検証ができれば、大概は同様に接続できます。

   ※TPiCSのクライアント使用には不要です。

●ファイヤーウォール

   設定不要。

## 接続ができなとき

![](https://image.jimcdn.com/app/cms/image/transf/none/path/s64d21b00408982fc/image/i0d1b87afeadae811/version/1689663592/image.png)

**TCP/IPが無効のため接続できないケース**

クライアントからサーバーへの接続で

SQL Server のTCP/IP無効、名前付きパイプ無効の場合

接続できない。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=413x1024:format=png/path/s64d21b00408982fc/image/i44abf73277d43c7a/version/1689663895/image.png)

**名前付きパイプが無効のため接続できないケース**

クライアントからサーバーへの接続で

SQL Server のTCP/IP有効、名前付きパイプ無効の場合

接続できない。

![](https://image.jimcdn.com/app/cms/image/transf/dimension=413x1024:format=png/path/s64d21b00408982fc/image/ica52462e3331bf53/version/1689663630/image.png)

**ファイヤウォールによって接続てきないケース  
**

### 関連

- [**SQL Server Express Edition でTPiCSデモ環境作成**](https://www.tpics.co.jp/2023/06/13/tpics%E3%83%87%E3%83%A2%E7%89%88%E3%81%AE%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9-sql-server-%E3%82%92%E6%A7%8B%E7%AF%89%E3%81%99%E3%82%8B/ "SQL Server Express Edition でTPiCSデモ環境作成")
- **[インストールした SQL Server Express に接続できないときには](https://www.tpics.co.jp/2023/06/13/%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%97%E3%81%9F-sql-server-express-%E3%81%AB%E6%8E%A5%E7%B6%9A%E3%81%A7%E3%81%8D%E3%81%AA%E3%81%84%E3%81%A8%E3%81%8D%E3%81%AB%E3%81%AF/ "インストールした SQL Server Express に接続できないときには")**