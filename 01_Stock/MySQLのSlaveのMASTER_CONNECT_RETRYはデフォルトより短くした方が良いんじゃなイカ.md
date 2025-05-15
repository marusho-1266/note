---
title: "MySQLのSlaveのMASTER_CONNECT_RETRYはデフォルトより短くした方が良いんじゃなイカ"
source: "https://yoshinorin.net/2020/06/16/mysql-set-master-connect-retry/"
author:
  - "[[YoshinoriN]]"
published: 2020-06-16
created: 2025-05-15
description: "こんにちわ。PocochaバックエンドでMySQLを担当しています。吉永です。"
tags:
  - "#database"
  - "#database/mysql"
  - "#article" 
  - "#intermediate"
---
MASTER\_CONNECT\_RETRYに関して触れられている記事があまり無かったので、自分メモ。  
最初に結論を言うとMASTER\_CONNECT\_RETRYはデフォルトの60は長すぎるので、3とか5くらいに設定した方が良いんじゃ無いかと。

某クラウドサービスに乗せているサービスのネットワークの関係か、 [MySQL](http://d.hatena.ne.jp/keyword/MySQL) SlaveのIO\_THREADが不安定で、それに付随してちょっと困ったことになったお話。

### 起きたこと

なんか、1分間隔で監視している [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) の監視(IO\_THREAD)アラートがやたら上がってきて深夜眠れない日々を過ごしていました。  
このアラートが上がってくるのは某クラウドサービスに乗っているサービスだから、同居している他のサービスの影響で、ネットワークが時々重くなるのはしようが無いかなと思っていました。  
また、ネットワーク不調で、IO\_THREADのMasterへの接続が切れても、すぐに再接続するはずだから、たまに [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) 監視のアラートにひっかかることがあっても、頻繁に来るのは何かおかしいと思い、ちょっと調べてみることに。

### エラーログ

まず何は無くともエラーログを確認。特に隠す必要はないと思いましたが念為。

```
1301**  8:07:21 [ERROR] Slave I/O: error reconnecting to master 'replication@********:3306' - retry-time: 60  retries: 86400, Error_code: 2003
1301**  8:08:21 [Note] Slave: connected to master 'replication@********:3306',replication resumed in log '****-bin.004731' at position 309391564
```

ふむ、どうやら8時7分21秒にIO\_THREADが止まって、8時8分21秒に再接続しているようですね。1分間きっかりSlave遅延が発生している計算です。再接続が即座に走るはずだという妄想は間違いだということが分かりました。  
Masterの更新が激しいようなサービスだったら、(Slaveの使い方によりますが、)1分の遅延はわりかし致命的だと思います。今回の場合、アプリケーション側からのSlave参照は無い、Active-Stanbyの構成であったため、問題にはなりませんが、ばりばりSlave参照しているサービスだったらきっと目も当てられないことになっていたでしょう。

> Error: 2003 (CR\_CONN\_HOST\_ERROR)
> 
> Message: Can't connect to [MySQL](http://d.hatena.ne.jp/keyword/MySQL) server on '%s' (%d)
> 
> [http://dev.mysql.com/doc/refman/5.1/ja/error-messages-client.html](http://dev.mysql.com/doc/refman/5.1/ja/error-messages-client.html)

ちなみにエラーコード2003はサーバへのコネクションが確立できなかった際のエラーです。

さて、この60秒というのはどこから来ているのでしょうか。勘が良ければ、"retry-time: 60 "にあたりがつくと思います。  
何は無くとも、show variablesで現在の設定を確認することに。

```
[root@localhost] (none)> show variables like '%retry_time%';
Empty set (0.00 sec)
```

…えっ。  
いろいろ調べた結果SHOW SLAVE STATUSで出力されるConnect\_Retryがそのものらしいです。

```
[root@localhost] (none)> show slave status\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: **********
                  Master_User: replication
                  Master_Port: 3306
                Connect_Retry: 60  <= これ
(後略)
```

#### で、この値は何なのか。

> \--master-connect-retry=seconds
> 
> マスタがダウンするか接続不可の場合にマスタへ再接続を試行する前に、スレーブ スレッドがスリープ状態になる秒数。master.info ファイルの値が読み込れる場合、その値が優先される。設定しなければ、デフォルトで 60 秒。--slave-net-timeout の値に基づくマスタからのデータ読み込みに対してタイムアウトするまで、スレーブによる再接続の自動呼び出しは行われない。再接続の試行の回数は、--master-retry-count で制限する。
> 
> (中略)  
> \--slave-net-timeout=seconds
> 
> スレーブが読み取りを中止する前に、マスタからのデータを待つ秒数。スレーブが接続切断と判断して再接続を試行するときのもの。最初の接続試行はタイムアウト直後に行われる。再試行のインターバルは、--master-connect-retry オプションでコントロールできる。再接続の試行回数は --master-retry-count オプションで設定する。デフォルトでは、3600 秒 (1時間)。
> 
> [http://dev.mysql.com/doc/refman/5.1/ja/replication-options.html](http://dev.mysql.com/doc/refman/5.1/ja/replication-options.html)

```
時系列
===+============================+===============================+====>
   |                            |                               |
   |<= net-slave-timeout(sec) =>|<= master-connect-retry(sec) =>|
   |                            |                               |
   |                        timeout(IO_THREAD切断)
ネットワーク不調
   |<==========================================================>|
           ここの部分をmaster-retry-count回繰り返す
```

ドキュメントを読み解いた限りではこのように自分は解しました。net-slave-timeout [\*1](https://blog.masasuzu.net/entry/2013/01/18/#f1 "今回の場合、my.cnfで1秒に設定してあった") 秒後にtimeoutして、master-connect-retry秒後に再接続試行して、それをmaster-retry-count回数繰り返すと。ちなみに、master-retry-countは先にエラーログにあった"retries: 86400"ですね。  
要は、master-connect-retryがデフォルトの60秒だと長すぎるので、短くした方が良いということが分かりました。

### 変更

MASTER\_CONNECT\_RETRYの値はmaster.infoに記述されているので、CHANGE MASTER TOで変更します。

> サーバが、記述したばかりのスタートアップ オプションよりも、既存 master.info ファイルを優先するため、これらの値をスタートアップ オプションに使用するよりも、CHANGE MASTER TO ステートメントを使用して値を指定する方が賢明です。詳細は、「CHANGE MASTER TO 構文」 を参照してください。
> 
> [http://dev.mysql.com/doc/refman/5.1/ja/replication-options.html](http://dev.mysql.com/doc/refman/5.1/ja/replication-options.html)

CHANGE MASTER TOを打つ前にSlaveを止めておく必要があります。Slave参照をしている場合は順次、サービスから抜いてから実行する必要があるでしょう。最後にSHOW SLAVE STATUSして値が変わっているか確認しておきます。

```
STOP SLAVE;
CHANGE MASTER TO MASTER_CONNECT_RETRY=3;
START SLAVE;
SHOW SLAVE STATUS\G
```

この対策を入れたことで、深夜に監視アラートが(ほとんど)飛んでこなくなり安心、安眠です!  
(根本の問題はま っ た く解決してませんが。

これからは新規でSlave作る際もMASTER\_CONNECT\_RETRYを明示的に指定するようにしたいと思います。

余談ですが、master.infoに書かれた設定は再起動してもリセットされないので、安心です。

[\*1](https://blog.masasuzu.net/entry/2013/01/18/#fn1):今回の場合、my.cnfで1秒に設定してあった

[« innotopがsegmentation faultで落ちる件](https://blog.masasuzu.net/entry//2013/02/05/innotop_segmentation_fault) [エンジニアサポートCROSS2013に行ってきた… »](https://blog.masasuzu.net/entry/2013/01/18/cross2013)