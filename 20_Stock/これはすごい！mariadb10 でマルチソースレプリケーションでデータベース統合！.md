---
title: "これはすごい！mariadb10 でマルチソースレプリケーションでデータベース統合！"
source: "https://hit.hateblo.jp/entry/mariadb/replication"
author:
  - "[[Database JUNKY]]"
published: 2018-09-07
created: 2025-05-15
description: "MariaDBでマルチソースレプリケーション 私的に待ちに待ったあの機能が追加されました。それは、マルチデータソースレプリケーション、mariadb10.0から実装されるという話は知っておりましたが、ようやく触る機会がきたので試してみようと思います。 余談ではありますが、弊社では、３０スキーマはあろうデータベースをこれで一台に集約しておりますので、ちゃんと実績はありますのでご安心を"
tags:
  - "#database"
  - "#database/mariadb" 
  - "#article"
  - "#practical"
  - "#advanced"
---
## MariaDBでマルチソースレプリケーション

私的に待ちに待ったあの機能が追加されました。それは、マルチデータソース [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) 、mariadb10.0から実装されるという話は知っておりましたが、ようやく触る機会がきたので試してみようと思います。 余談ではありますが、弊社では、３０ [スキーマ](http://d.hatena.ne.jp/keyword/%A5%B9%A5%AD%A1%BC%A5%DE) はあろうデータベースをこれで一台に集約しておりますので、ちゃんと実績はありますのでご安心を

![f:id:hit10231023:20161121171801j:plain](https://cdn-ak.f.st-hatena.com/images/fotolife/h/hit10231023/20161121/20161121171801.jpg)

f:id:hit10231023:20161121171801j:plain

![f:id:hit10231023:20180309125620p:plain](https://cdn-ak.f.st-hatena.com/images/fotolife/h/hit10231023/20180309/20180309125620.png)

f:id:hit10231023:20180309125620p:plain

## そもそもマルチソースレプリケーションとは何？

異なる２つの [インスタンス](http://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) のデータベースを、１つのSLAVEで [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) しちゃおうという意味です。たぶん言葉で説明するのは難解なので、図を以下に載せます。

![f:id:hit10231023:20161121171801j:plain](https://cdn-ak.f.st-hatena.com/images/fotolife/h/hit10231023/20161121/20161121171801.jpg)

f:id:hit10231023:20161121171801j:plain

え？これって、昔からできるんじゃないの？と思った方（自分）。実は、 [MySQL](http://d.hatena.ne.jp/keyword/MySQL) はこれができなかったのです　(T\_T) [MySQL](http://d.hatena.ne.jp/keyword/MySQL) の [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) を触り始めた時は、マルチマスター [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) がそれだと思っていたのですが、全然違いました。もう一度言いますと、こういう [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) はもとから仕様的にできなかったのです！（しつこい）

できない仕様のものに対して、昔、私は、 [それを無理やりできるよう](http://www.s-quad.com/wordpress/?p=1497) に [魔改造](http://d.hatena.ne.jp/keyword/%CB%E2%B2%FE%C2%A4) を加えできるようにしましたが、正直いって、運用が猛烈にしんどい。そして実運用の場合、ミスをしやすいのが難点でした。

[MariaDB&MySQL全機能バイブル](http://d.hatena.ne.jp/asin/4774170208/quickknowlegd-22)

## メリットは？

すばり分析用のデータベースを作ることかと思います。データベースの実装として、マスタ分割を行っている企業さんは多いと思います。また負荷分散的にわけていることも多いのではないかと思います。さまざまなホストに分割されているデータを統合してトータルに分析したい場合はどうするか？おそらく、ETLなどを利用してデータを加工してフラットなテーブルに変換して誰にでも触れるデータを作成してー・・・とか、本気で面倒なデータ連携をしなければいけないのではないでしょうか？ 正直なところ、中小規模の企業にとってDWHを導入するメリットはあまりなく、純粋なDWHは、こと [SQL](http://d.hatena.ne.jp/keyword/SQL) に関してだけでも色々な制約が多く、１００％活用できないというのが現状です。 また、データの加工をして見やすい形に整形することは仮にあるとしても、データをEXPORTしてDWHにロードして・・とかいう作業は管理がしきれないというデメリットがあります。なので極力したくないですよね？

このようなケースの場合、、マルチソースな [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) があればとくにデータベースエンジニアの手を介さなくても、ほぼリアルタイムに、dwhに同期することができますよね！というメリットは非常に大きいと思っています

また、DWHを構築できれば、 [全文検索エンジン](http://d.hatena.ne.jp/keyword/%C1%B4%CA%B8%B8%A1%BA%F7%A5%A8%A5%F3%A5%B8%A5%F3) 等にも応用できるかと思います

[hit.hateblo.jp](https://hit.hateblo.jp/entry/MySQL/SQL/FULL_TEXT_SEARCH)

## MariaDB 10.0 でマルチソースレプリケーション設定手順

さっそくですが、上記の図に沿ったマルチソース [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) の設定手順について記載していきます。なにぶん初めてなもので、もっとスマートなやり方がありそうですが。そのあたりは許してください。

## 1台目の設定

## master側の作業 (dbsv1)

### マスターのポジションを取得

```
mysql> show master status ;
+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.001961 |  3018345 |              |                  |
+------------------+----------+--------------+------------------+
```

### データベースのバックアップ

```
mysqldump -usysadm -p schema_1 > /var/data/schema_1.dump
```

## slave側の作業(dwh)

### ダンプファイルを適用

```
# mysql -u sysadm -p schema_1 < /var/data/schema_1.dump
```

### レプリケーションの設定（1台目）

```
MariaDB>

SET @@default_master_connection='dbsv1';

CHANGE MASTER 'dbsv1' TO 
MASTER_HOST = '192.168.111.106',
MASTER_USER = 'slave',
MASTER_PASSWORD = 'slavepasswd',
MASTER_PORT = 3306 ,
MASTER_LOG_FILE = 'mysql-bin.001961',
MASTER_LOG_POS = 3018345 ;
```

### コネクション設定が反映されているか確認

```
mysql>
select @@default_master_connection;
+-----------------------------+
| @@default_master_connection |
+-----------------------------+
| dbsv1                       |
+-----------------------------+
```

### スレーブの開始

```
MariaDB>
start all slaves ;
```

### スレーブの確認

SHOW SLAVE STATUS ではなくて、 SHOW ALL SLAVES STATUS であることに注意、新たにConnection\_nameという項目が追加されているのがわかるかと思います。1.rowは、 [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) は動いておりませんが、前述にて設定したdbsv1の [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) については、2. rowで起動しているのがわかるかと思います。

```
MariaDB> SHOW ALL SLAVES STATUS\G
*************************** 1. row ***************************
              Connection_name: dbsv1
              Slave_SQL_State: 
               Slave_IO_State: 
                  Master_Host: 192.168.111.106
                  Master_User: slave
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.001961
          Read_Master_Log_Pos: 3018345
               Relay_Log_File: relay-bin-dbsv1.000001
                Relay_Log_Pos: 4
        Relay_Master_Log_File: mysql-bin.001961
             Slave_IO_Running: No
            Slave_SQL_Running: No
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 3018345
              Relay_Log_Space: 248
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: NULL
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
MariaDB [(none)]> SHOW ALL SLAVES STATUS\G
*************************** 1. row ***************************
              Connection_name: dbsv1
              Slave_SQL_State: Slave has read all relay log; waiting for the slave I/O thread to update it
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.111.106
                  Master_User: slave
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.001961
          Read_Master_Log_Pos: 3018345
               Relay_Log_File: relay-bin-dbsv1.000002
                Relay_Log_Pos: 394
        Relay_Master_Log_File: mysql-bin.001961
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 3018345
              Relay_Log_Space: 691
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 151
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
                   Using_Gtid: No
                  Gtid_IO_Pos: 
         Retried_transactions: 0
           Max_relay_log_size: 1073741824
         Executed_log_entries: 5
    Slave_received_heartbeats: 0
       Slave_heartbeat_period: 1800.000
               Gtid_Slave_Pos: 0-1810151-1308
```

## 2台目の設定

## master側の作業 (dbsv2)

### マスターのデータベース一覧を確認

いっぱいありますね。。。この中から、ttt1だけ、 [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) するように設定してみたいと思います。

```
mysql> 
show databases;

+--------------------+
| Database           |
+--------------------+
| information_schema |
| configurator       |
| mysql              |
| performance_schema |
| sns                |
| ttt1               |
| ttt2               |
+--------------------+
```

### マスターのポジションを取得

```
mysql> show master status ;
+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.001243 | 71613648 |              |                  |
+------------------+----------+--------------+------------------+
```

### データベースのバックアップ

```
mysqldump -usysadm -p ttt1 > /var/data/ttt1.dump
```

## slave側の作業

### ダンプファイルを適用

```
# mysql -usysadm -p ttt1 < /var/data/tl1.dump
```

### レプリケーションの設定（2台目）

```
MariaDB>

SET @@default_master_connection='dbsv2';

CHANGE MASTER 'dbsv2' TO 
MASTER_HOST = '192.168.111.157',
MASTER_USER = 'slave',
MASTER_PASSWORD = 'slavepasswd',
MASTER_PORT = 3306 ,
MASTER_LOG_FILE = 'mysql-bin.001243',
MASTER_LOG_POS = 71613648 ;
```

### コネクション設定が反映されているか確認

```
mysql>
select @@default_master_connection;
+-----------------------------+
| @@default_master_connection |
+-----------------------------+
| dbsv2                       |
+-----------------------------+
```

### レプリケーションフィルタをかける

[レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) は通常 [インスタンス](http://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 単位で行っています。前述の通り、マスターサーバ側から、１つのデータベースしかレプリケートされておりません。どうするのか？dbsv2のコネクションのみフィルターが掛けれるか試してみようと思います。

```
MariaDB>
SET GLOBAL replicate_do_db = "ttt1";
```

### レプリケーションの状態を確認する

以下の通りとなりました。 dbsv2のコネクションのみ、replicate\_do\_dbが適用されるのがわかったかと思います。なぜこのようになったかといいますと、前の作業で、default\_master\_connectionをdbsv2に設定していたからなんですね！（※）

※ 実際は、my.cnfとかに書いたほうがよいような気もしますが、コネクションの設定をどう入れるのかまだ分からないので割愛します。

```
MariaDB [(none)]> SHOW ALL SLAVES STATUS\G
*************************** 1. row ***************************
              Connection_name: dbsv2
              Slave_SQL_State: 
               Slave_IO_State: 
                  Master_Host: 192.168.111.157
                  Master_User: slave
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.001243
          Read_Master_Log_Pos: 71613648
               Relay_Log_File: relay-bin-dbsv2.000001
                Relay_Log_Pos: 4
        Relay_Master_Log_File: mysql-bin.001243
             Slave_IO_Running: No
            Slave_SQL_Running: No
              Replicate_Do_DB: ttt1
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 71613648
              Relay_Log_Space: 248
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: NULL
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 0
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
                   Using_Gtid: No
                  Gtid_IO_Pos: 
         Retried_transactions: 0
           Max_relay_log_size: 1073741824
         Executed_log_entries: 0
    Slave_received_heartbeats: 0
       Slave_heartbeat_period: 1800.000
               Gtid_Slave_Pos: 
*************************** 2. row ***************************
              Connection_name: dbsv1
              Slave_SQL_State: Slave has read all relay log; waiting for the slave I/O thread to update it
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.111.106
                  Master_User: slave
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.001961
          Read_Master_Log_Pos: 9904941
               Relay_Log_File: relay-bin-dbsv1.000002
                Relay_Log_Pos: 6886990
        Relay_Master_Log_File: mysql-bin.001961
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 9904941
              Relay_Log_Space: 6887287
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 151
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
                   Using_Gtid: No
                  Gtid_IO_Pos: 
         Retried_transactions: 0
           Max_relay_log_size: 1073741824
         Executed_log_entries: 23429
    Slave_received_heartbeats: 0
       Slave_heartbeat_period: 1800.000
               Gtid_Slave_Pos: 
2 rows in set (0.00 sec)
```

### スレーブの開始

ここで、上記のステータスを確認すると前述ですでに [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) が開始しているコネクションがありますよね？つまり、今回設定した、dbsv2コネクションの [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) はまだ起動していない状態であることが理解できると思います。

### コネクション毎にスレーブを開始する

コネクション単位でスレーブを開始できるか試してみます。コネクション単位でスレーブを開始するには、コネクション名を指定すればよさそうです。

```
start slave 'dbsv2' ;
```

### スレーブの確認

正常に [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) が開始されているのがわかるかと思います。

```
MariaDB [(none)]> SHOW ALL SLAVES STATUS\G
*************************** 1. row ***************************
              Connection_name: dbsv2
              Slave_SQL_State: Slave has read all relay log; waiting for the slave I/O thread to update it
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.111.157
                  Master_User: slave
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.001243
          Read_Master_Log_Pos: 71613648
               Relay_Log_File: relay-bin-dbsv2.000002
                Relay_Log_Pos: 394
        Relay_Master_Log_File: mysql-bin.001243
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: ttt1
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 71613648
              Relay_Log_Space: 691
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 1010011
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
                   Using_Gtid: No
                  Gtid_IO_Pos: 
         Retried_transactions: 0
           Max_relay_log_size: 1073741824
         Executed_log_entries: 5
    Slave_received_heartbeats: 0
       Slave_heartbeat_period: 1800.000
               Gtid_Slave_Pos: 
*************************** 2. row ***************************
              Connection_name: dbsv1
              Slave_SQL_State: Slave has read all relay log; waiting for the slave I/O thread to update it
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.111.106
                  Master_User: slave
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.001961
          Read_Master_Log_Pos: 9926199
               Relay_Log_File: relay-bin-dbsv1.000002
                Relay_Log_Pos: 6908248
        Relay_Master_Log_File: mysql-bin.001961
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 9926199
              Relay_Log_Space: 6908545
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 151
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
                   Using_Gtid: No
                  Gtid_IO_Pos: 
         Retried_transactions: 0
           Max_relay_log_size: 1073741824
         Executed_log_entries: 23453
    Slave_received_heartbeats: 0
       Slave_heartbeat_period: 1800.000
               Gtid_Slave_Pos: 
2 rows in set (0.00 sec)
```

以上でマルチソース [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) のお話はおしまいにしますが、これを参考にしてくださり、おし！やってみよう！というチャレンジャーは、これから使いそうなコマンドも覚えておくとよさそうなので、メモ程度に載せておきます。

- きっと役に立つであろうコマンド
```
CHANGE MASTER ['connection_name'] ...
FLUSH RELAY LOGS ['connection_name']
MASTER_POS_WAIT(....,['connection_name'])
RESET SLAVE ['connection_name']
SHOW RELAYLOG ['connection_name'] EVENTS
SHOW SLAVE ['connection_name'] STATUS
SHOW ALL SLAVES STATUS
START SLAVE ['connection_name'...]]
START ALL SLAVES ...
STOP SLAVE ['connection_name'] ...
STOP ALL SLAVES ...
```

今回の例は、なんと！MySQL5.5 を [MariaDB](http://d.hatena.ne.jp/keyword/MariaDB) 10.0に [レプリケーション](http://d.hatena.ne.jp/keyword/%A5%EC%A5%D7%A5%EA%A5%B1%A1%BC%A5%B7%A5%E7%A5%F3) してます。 [MySQL](http://d.hatena.ne.jp/keyword/MySQL) の互換性があるって素敵ですね。引き続き検証を進めたいと思います。

現状この機能は、 [MariaDB](http://d.hatena.ne.jp/keyword/MariaDB) だけでなく [MySQL](http://d.hatena.ne.jp/keyword/MySQL) の5.7以降でも利用できます。