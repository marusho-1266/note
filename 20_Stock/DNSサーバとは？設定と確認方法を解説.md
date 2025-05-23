---
title: DNSサーバとは？設定と確認方法を解説
source: https://hnavi.co.jp/knowledge/blog/dns-server/
author:
  - "[[システム開発のプロが発注成功を手助けする【発注ラウンジ】]]"
published: 2025-01-06
created: 2025-05-02
description: DNSサーバとは？設定と確認方法を解説｜発注ラウンジは、システム開発・Webサイト制作の発注に役立つノウハウや、「発注ナビ」で実際にシステム開発を発注された方のインタビューなど、ITでお困りの皆様をサポートする情報を掲載しています。
tags:
  - dns
  - network
  - practical
  - reference
  - tutorial
association:
---
[発注ラウンジTOP](https://hnavi.co.jp/knowledge/) \> \> DNSサーバとは？設定と確認方法を解説

## DNSサーバとは？設定と確認方法を解説

![DNSサーバのイメージ図](https://hnavi.co.jp/_next/image/?url=https%3A%2F%2Fwordpress.hnavi.co.jp%2Fwp-content%2Fuploads%2F2021%2F12%2F211157_DNS-server2.jpg&w=640&q=75)

DNSサーバとは、ドメイン名とIPアドレスを変換する仕組みを提供するサーバのことです。DNSサーバがあることで、Webサイトの閲覧やメールの送受信ができるようになります。

この記事では、インターネットを使ううえで欠かせない **DNSサーバの仕組みや種類、設定方法や選び方** について解説します。また「DNSサーバは応答していません」といったエラーが出た際の対処法も紹介しているので、ぜひ参考にしてください。

## 目次

## ピッタリの制作会社が見つかる完全無料で全国6000社以上からご提案

[![即戦力のシステム開発会社を探すなら「発注ナビ」](https://hnavi.co.jp/_next/image/?url=https%3A%2F%2Fwordpress.hnavi.co.jp%2Fwp-content%2Fuploads%2F2022%2F12%2Fcta_hp.jpg&w=640&q=75)](https://hnavi.co.jp/lp/hp3/?argument=bzcUT99h&dmai=a639b5374da89e)

・最短1営業日以内に4～5社ご紹介  
・ITに詳しいコンシェルジュがサポート  
・10年以上、16500件以上の紹介実績  
・ご相談～ご紹介まで完全無料

＼完全無料・たった1日で回答／

[ご相談はコチラ](https://hnavi.co.jp/lp/hp3/?argument=bzcUT99h&dmai=a639b5374da89e)

## DNSサーバとは？

DNSサーバとは、ドメイン名とIPアドレスを変換する仕組みを提供するサーバのことで、「Domain Name System」の略語です。

DNSサーバは、インターネットを支える大切な技術の1つで、これがないとWebサイトやメールの利用ができなくなってしまいます。というのも、ふだん使っている英数字のドメイン名と、Webサイトやメールのサーバ機器にアクセスするIPアドレス（番号）は別物です。そのため、 **ドメイン名とIPアドレスを紐づけて変換する必要があります。** その変換する情報をDNSサーバが保管し、Webサイトやメールを使えるように提供しているのです。

## DNSサーバの仕組みとは？

DNSサーバの仕組みを理解するために、実際にWebサイトが表示されるまでの動きを確認しながら解説していきます。

例えば、パソコンやスマートフォンなどのブラウザで気になるWebサイトのドメイン名を入力したとします。すると、そのドメイン名を世界中に無数にあるDNSサーバが連携しIPアドレスを特定して、一瞬でブラウザ上にWebサイトが表示されるようになるのです。

また、メールの場合もWebサイトを表示する流れと一緒で、特定のメールアドレス宛に送受信した際にDNSサーバを経由して届けられます。DNSサーバによってWebサイトが表示される順番は、以下のとおりです。

1. ユーザーがデバイスからドメイン名を入力する
2. デバイスからキャッシュDNSサーバへリクエストが送信される
3. キャッシュDNSサーバが権威DNSサーバ（ルートサーバ）へリクエストを送信する
4. リクエストを受けた権威DNSサーバが該当するIPアドレス情報を探し、キャッシュDNSサーバへ送信する
5. キャッシュDNSサーバがデバイスへ情報を発信し、Webサイトが表示される

この時の処理は、あまりに短い間に完了します。そのため、普段インターネットを利用している際に、この動きを意識している方はほとんどいません。しかし、裏側では複数の処理が行われて、Webサイトが表示されるのです。

また、Webサイトが表示される手順を見て「キャッシュDNSサーバ」と「権威DNSサーバ」は何が違うのだろうと思った方も多いのではないでしょうか。次項で詳しく解説します。

## DNSサーバの種類

DNSサーバには、大きく分けて「キャッシュDNSサーバ」と「権威DNSサーバ（コンテンツサーバ）」の2種類があります。

| 種類 | 特徴 |
| --- | --- |
| キャッシュDNSサーバ | インターネットに接続する端末が名前解決をするために問い合わせるDNSサーバ |
| 権威DNSサーバ（コンテンツサーバ） | 「ドメイン名（ホスト名）とIPアドレスとのゾーン情報（変換表）」を回答するDNSサーバ |

### ●キャッシュDNSサーバ

![キャッシュDNSサーバのイメージ図](https://hnavi.co.jp/_next/image/?url=https%3A%2F%2Fwordpress.hnavi.co.jp%2Fwp-content%2Fuploads%2F2024%2F02%2Fcache-dns.png&w=640&q=75)

キャッシュDNSサーバ（DNSキャッシュサーバ）は、 **ユーザーのデバイスが直接やりとりをするサーバ** で、名前解決（ドメイン名からコンピューターが通信時に使用するIPアドレスを検索すること）のための問い合わせ先にあたります。

主な役割は、ユーザーがブラウザで入力したドメイン名をIPアドレスに変換し、デバイスに返信すること。キャッシュDNSサーバに情報がない場合は、権威DNSサーバへの問い合わせを行います。この過程は以下のステップで進行します。

1. ドメイン名の入力と探索：ユーザーがドメイン名を入力すると、キャッシュDNSサーバが該当するIPアドレスを探し始める
2. キャッシュの利用：キャッシュDNSサーバにすでにそのドメイン情報がキャッシュされている場合、その情報を利用してデバイスに返信する
3. 権威DNSサーバへの問い合わせ：キャッシュDNSサーバに情報がない場合、権威DNSサーバに問い合わせを行う
4. デバイスへの返信：権威DNSサーバから受け取った情報をデバイスに返信する

権威DNSサーバから受け取った情報は、キャッシュDNSサーバに一時的に保持されます。ユーザーがそのWebサイトに再びアクセスする際は、権威DNSサーバに問い合わせることなく、保持した情報を返信できるため、サーバへの負荷が軽減されます。

権威DNSサーバとキャッシュDNSサーバは密接に連携しており、インターネット接続において重要な役割を担っているのです。

キャッシュDNSサーバは、通常インターネット・サービス・プロバイダーやネットワーク管理者が用意して使用しますが、Googleが無償提供しているサーバが使われることもあります。

### ●権威DNSサーバ

「コンテンツサーバ」とも呼ばれる権威DNSサーバは、 **インターネット上のドメイン（Webサイトのアドレス）に関する正確な情報を保持するサーバ** です。キャッシュDNSサーバからの問い合わせを受けて、「ドメイン名（ホスト名）とIPアドレスとのゾーン情報（変換表）」を回答します。あるいは、ゾーン情報を持っていそうな、より下位の権威DNSサーバに問い合わせるよう指示して、ドメイン名とIPアドレスのゾーン情報を探し出すものです。

キャッシュDNSサーバとは異なり、ユーザーが使用するデバイスと直接やりとりすることはありません。簡単にいうと、権威DNSサーバはインターネット上のドメインに関する「情報の保管庫」であり、キャッシュDNSサーバはその情報を効率的に取り出し、ユーザーが求めるWebサイトへ迅速にアクセスできるようにする「仲介者」の役割を果たしています。

権威DNSサーバは複数の階層に分けて構成されており、最上位に位置するのがルートDNSサーバです。ルートサーバは世界中に13ヶ所あり、「.com」や「.jp」などのトップレベルドメインに関するIPアドレス情報を保持しています。

ルートDNSサーバに続いて.co.jpの.coの部分に該当するセカンドレベルドメインを管理するDNSサーバがあり、さらに下位層を管理するDNSサーバがあるのです。

## DNSサーバの設定方法は2パターン

DNSサーバの設定方法は以下の2パターンです。

- ドメイン登録業者のDNSサーバを利用する
- レンタルサーバ業者のDNSサーバを利用する

2つのパターンそれぞれの設定方法とあわせて、設定の確認手順やDNSサーバの設定に必要なDNSレコードについて詳しく解説します。

### ●ドメイン登録業者のDNSサーバを利用する場合

ドメイン登録業者のDNSサーバを利用する場合のは、ドメインを提供する業者のコントロールパネルを通じて設定を行います。ドメインを新規取得した場合やほかの業者からドメインを移行した場合など、状況によって設定方法が異なることがありますが、基本的な手順は以下のとおりです。

1. コントロールパネルを開く：ドメイン登録業者のコントロールパネルにログインする
2. DNSサーバ名の入力：コントロールパネル内のDNSサーバ設定欄に、ドメイン登録業者のDNSサーバ名を入力する
3. DNSレコードの入力：ドメイン登録業者のコントロールパネルにDNSレコード（ドメインに関連する設定情報）を入力する

これらの手順に従って設定することで、ドメインが正しくインターネット上で機能するようになります。ただし、各業者によって具体的な設定方法や必要な情報が異なる場合があるため、詳細はその業者のマニュアルや指示に従ってください。

### ●レンタルサーバ業者のDNSサーバを利用する場合

レンタルサーバ業者のDNSサーバを使用する場合は、以下のステップに従って設定を行います。

1. コントロールパネルを開く：ドメイン登録業者のコントロールパネルにログインする
2. DNSサーバ名の入力：コントロールパネル内のDNSサーバ欄に、レンタルサーバ業者が指定するDNSサーバ名を入力する
3. DNSレコードの入力：レンタルサーバ業者のコントロールパネルにアクセスして、DNSレコード（ドメインに関連する設定情報）を入力する

基本的な設定方法は上記のとおりですが、新規にドメインを取得した場合や、ほかの業者からドメインを移行する場合など、状況に応じて異なることがあります。そのため、正確な手順については各業者の指示やマニュアルを確認してください。

### ●設定の確認手順

DNS設定を行っても、すぐには反映されません。これは、キャッシュDNSサーバが情報を24時間から72時間程度一時的に保存し、そのデータをもとにドメイン名とIPアドレスを紐づけるためです。キャッシュが書き換わるまでは、単にブラウザにドメインを入力してサイトを確認するだけでは、設定が正しく反映されているかどうかを知ることができません。

DNS設定を確認する方法は次の2つです。

#### ・時間を置いて確認する

DNS設定後、72時間以上待ってから再度確認します。この時間が経過すると、キャッシュされた古い情報が更新される可能性が高くなります。

#### ・コマンドプロンプトを使う

Windowsではコマンドプロンプトを使って直接DNS設定を確認できます。なお、Macの場合も同様のコマンドで確認が可能です。

具体的には下記の手順で確認を行います。

1. コマンドプロンプトを開く：「スタートメニュー」から「すべてのアプリ」→「Windowsツール」→「コマンドプロンプト」を選ぶか、「スタートメニュー」で「コマンドプロンプト」と検索してコマンドプロンプトを開く
2. nslookupコマンドの使用：コマンドプロンプトで「nslookup」に続けてドメイン名を入力。例えば、ドメイン名がwww.example.coの場合「nslookup www.example.com」と入力する
3. 実行と確認：Enterキーを押してコマンドを実行し、「Address」に設定したIPアドレスが表示されているか確認する

これらの方法を用いることで、DNS設定が正しく行われているか確認できます。

### ●DNSサーバの設定に必要なDNSレコードの概要

DNSサーバを設定し正常に動かすためには、ドメイン名とIPアドレスを関連付ける情報を含む「ゾーンファイル」の設定が必要です。このゾーンファイルには、様々な種類の「DNSレコード」が記載されます。DNSレコードは、インターネット上でのドメイン名の解決やルーティングに不可欠な役割を果たします。主要なDNSレコードの種類は以下のとおりです。

| レコード種類 | 概要 |
| --- | --- |
| Aレコード | ドメイン名に対応するIPv4形式のIPアドレスを指定する   （IPアドレスがIPv6形式の場合は「AAAAレコード」） |
| CNAMEレコード | 1つのドメイン名（ホスト名）に別名を定義する |
| NSレコード | ゾーン情報を管理するネームサーバの名前を定義する |
| MXレコード | 対象ドメイン宛のメールサーバのホスト名を指定する |
| TXTレコード | ホスト名に関連付ける任意のテキスト情報を定義する |

このようなDNSレコードがゾーンファイルの中にあることを理解しておきましょう。

## DNSサーバを選ぶ際のポイント4つ

DNSサーバを選ぶ際のポイントは、以下の4つです。

- 高いセキュリティ性
- アクセススピードの速さ
- 運営会社の信用度
- サポート体制の充実度

それぞれ詳しくみていきましょう。

### ●高いセキュリティ性

DNSサーバを選ぶ際は、セキュリティの高さを確認することが大切です。インターネットに接続する際、セキュリティが不十分なDNSサーバは、 **DNSキャッシュポイズニング攻撃** や **カミンスキー攻撃** といった脆弱性を突いた攻撃に晒されるリスクがあります。これらの攻撃は、DNSサーバを悪用して偽の情報を送り込むことで、ユーザーを危険なサイトに誘導するものです。

したがって、重要なデータを守るためには、外部攻撃に対して強固な防御機能を持ち、脆弱性の低いDNSサーバが推奨されます。また、プライバシー保護の観点から、短期間でログを削除するDNSサーバが適切とされています。

このようにセキュリティ面で信頼でき、強固な防御機能と高いプライバシー保護性を備えたDNSサーバを選択することが、インターネット接続の安全性を高めるうえで非常に重要です。

### ●アクセススピードの速さ

DNSサーバを選ぶ際に重要なポイントは、アクセススピード、すなわち **通信速度** です。DNSサーバの処理速度が速いほど、インターネットの通信速度も向上します。特にWebサービスを提供している場合、ユーザーの満足度を高めるためにも、通信速度に優れたDNSサーバの選定が重要です。

DNSサーバの通信速度は、処理可能な問い合わせの量と、ソフトウェアのパフォーマンスによって変わります。

多くのDNSレコードを登録し、頻繁にアクセスされるドメインの場合、レコードの検索処理を迅速に行えるDNSサーバが必要です。

DNSサーバを選択する際は、その通信速度が求める基準をクリアしているか、Webサービスのニーズに適しているかをしっかりと確認しましょう。

### ●運営会社の信用度

DNSサーバ選びは運営会社の信用度も重要です。通信が不安定で信頼性の低いDNSサーバを選んだ場合、通信障害が頻発したり、サーバ攻撃を受けてサービスを停止せざるを得なくなったりする可能性があります。

通信障害やサーバ攻撃によって発生するユーザー満足度の低下、業務停止、社会的信用の損失などを避けるためには、事前にDNSサーバの安定性と信頼性を確認することが不可欠です。具体的にはDNSサーバの運営会社の評判や知名度、信用度をあらかじめチェックしておきましょう。

信頼性の高い運営会社であれば、万が一の事態にも迅速に対応してくれる可能性が高く、安心してサービスを利用できます。

### ●サポート体制の充実度

セキュリティ上の問題が生じた時、適切なサポートがなければ安心してサービスを使用することはできません。十分なセキュリティ対策がとられていても、100％安全とは言い切れないため、サポート体制が充実しているDNSサーバを選びましょう。

充実したサポート体制には、迅速な問題解決、技術的な支援、そして適切な対応策の提供が含まれます。特に、DNSサーバ関連の問題は、Webサイトのアクセス障害やセキュリティの脅威に直結するため、専門家による迅速な対応が不可欠です。したがって、サポート体制の詳細や対応時間、対応範囲などを確認しておくことが重要です。サポート体制が充実しているDNSサーバを選ぶことで、万一の問題が発生した際にも、適切かつ迅速に対処できるようになります。

## エラーが起こる原因

インターネットを利用中に「DNSサーバは応答していません」と表示されたことがある方もいるのではないでしょうか。「DNSサーバは応答していません」というエラーは、「WebサイトのIPアドレスを取得できなかった」、つまり、開こうとしたWebサイトのIPアドレスを取得しようとしてDNSサーバにアクセスを試みたものの、応答が返ってこなかったという意味になります。

では、なぜ IPアドレスを取得できなかったのでしょうか。考えられる原因は複数ありますが、特に多いのは下記4つです。

- インターネットに接続できていない
- DNSサーバがダウンしている
- DNSサーバに接続できない
- Webサイトが閉鎖してしまっている

それぞれ詳しく解説します。

### ●インターネットに接続できていない

インターネットに接続できていない場合、「DNSサーバは応答していません」と表示されます。DNSとはインターネット上でドメイン名とIPアドレスを翻訳して紐づける仕組みです。インターネットにつながっていなければ、DNSサーバにもアクセスできません。そのため、まずは自身のデバイスがインターネットに接続しているか確認してみましょう。

### ●DNSサーバがダウンしている

「DNSサーバは応答していません」というエラーが出る理由として、DNSサーバがダウンしている可能性も挙げられます。サーバダウンはアクセスが集中したり、DDoS攻撃（対象のサーバに対して複数のコンピューターで大量に情報を送る攻撃のこと）を受けたりすることで起こります。

インターネットに問題なく接続されているのにもかかわらず、エラーメッセージが出る場合には、DNSサーバがダウンしている可能性もあります。重大なサーバダウンでない限りは、数時間で復旧することが多いため、しばらく時間を置いて改めて接続し直してみてください。

また、あまりにも復旧しないようであれば、プロバイダーに問い合わせてみるのも1つの手です。

### ●DNSサーバに接続できない

インターネットは接続できているのに「DNSサーバは応答していません」と表示される場合は、DNSサーバに接続できていないケースも考えられます。その場合は、少し知識と技術が必要になりますが、コンピューターに対して対象のコマンドを入力することでDNSサーバに接続できているか確認できます。どうしてもDNSサーバの状況を把握したいという場合に、対象コマンドを調べて確認してみてください。

### ●Webサイトが閉鎖してしまっている

Webサイトが閉鎖している場合にも、エラーメッセージが表示されることがあります。Webサイトが閉鎖していると、DNSサーバは応答しませんし、当たり前ですがWebサイトも表示されません。閉鎖されたWebサイトを確認したい場合には、「インターネット・アーカイブサービス」を使ってみてください。また、サービスの提供元がわかるようであれば、問い合わせて運営状況を確認してみてはいかがでしょうか。

## エラーが発生した時の対処法

DNSサーバのエラーが発生した時の対処法は以下のとおりです。

1. ネットワーク診断で原因を確かめる
2. ルーターの再起動
3. ルーターをリセット
4. Windowsを再起動
5. Wi-Fi子機を再接続してみる
6. ネットワークドライバーの再インストール
7. ISPの障害の確認
8. ルーターを交換する
9. 配信の最適化機能をオフに設定する
10. PCやスマホなどのデバイスを再起動してみる
11. パブリックDNSを設定する
12. IPv6の機能を無効化してみる
13. 不要なネットワークアダプタを無効にしてみる

それぞれ詳しく解説します。

### ●ネットワーク診断で原因を確かめる

DNSサーバによるエラーが表示された場合、まずは「ネットワーク診断」をしてみましょう。この「ネットワーク診断」は、WindowsやMacでも使用できる機能です。ネットワーク診断の機能を利用することで、インターネット接続における問題をチェックでき問題解決につながります。

- Windowsの場合：スタートメニューから「ネットワーク診断」をクリック
- Macの場合：ワイヤレス診断から「インターネットへの接続状況を診断」をクリック

### ●ルーターの再起動

「DNSサーバは応答していません」のエラーが表示された際に、ルーターを再起動してみると問題が解決することがあります。パソコン本体の不調ではなく、インターネット接続ができなくなっている理由には、ルーターがボトルネックになっていることも考えられます。パソコン側で不調が見つからない場合、一度ルーターを再起動してエラーが解決するか確認してみてください。

### ●ルーターをリセット

DNSサーバを再起動する以外に、ルーターをリセットすることで解決する可能性もあります。DNSエラーの原因が、ルーター側の設定ミスということもあるので、再起動しても直らないようならリセットしてみましょう。ちなみにルーターは、一般的にリセットボタンを長押しすることで設定を初期化できます。この時、接続先の再設定も忘れずに行いましょう。

### ●Windowsを再起動

ルーターを再起動したり、リセットしたりしても直らないようであれば、Windowsの再起動を試してみましょう。なぜなら、Windowsのシステムやネットワークドライバーなどの一時的な問題によって、DNSエラーが発生するケースもあるためです。一度パソコンを再起動してみて、さらにネットワーク診断のトラブルシューティングを試してみてください。

### ●Wi-Fi子機を再接続してみる

「DNSサーバは応答していません」が表示された場合、Wi-Fi子機の再接続で解決する可能性があります。パソコンのUSBのWi-Fi子機を使用している場合、Wi-Fi子機のほうに問題が発生しているケースがあります。一度、別のパソコンにWi-Fi子機を接続し、インターネットへ接続できるかどうかを確認しましょう。また、Wi-Fi子機をUSBハブに接続している場合は直接パソコンのポートに接続してみましょう。

### ●ネットワークドライバーの再インストール

ネットワークドライバーの再インストールで解決する可能性があります。「デバイスマネージャー」から「ネットワークアダプタ」のツリーを展開して、「ネットワークアダプタ名」の右クリックから「デバイスのアンインストール」を選択し、アンインストールを行いましょう。パソコンを再起動することで、ネットワークドライバーが自動でインストールされます。

### ●ISPの障害の確認

「DNSサーバは応答していません」が表示された場合、ISPで障害が起きていることが原因のケースもあります。インターネットのプロバイダー側で障害が発生しており、DNSエラーが発生するケースもあります。そのため、プロバイダー側から障害発生の情報が出ていないかどうか確認してみましょう。障害が原因の場合は、復旧まで待ちましょう。

### ●ルーターを交換する

「DNSサーバは応答していません」が表示された場合、ルーターを交換することで解決する可能性があります。ルーター自体が故障している可能性もあります。特にパソコンだけでなく別の端末でもインターネットに接続できない場合は、ルーターの故障の可能性が考えられます。対処法をすべて試してもDNSエラーが解決しない場合は、ルーターの交換も視野に入れましょう。

### ●配信の最適化機能をオフに設定する

「DNSサーバは応答していません」が表示された場合、配信の最適化機能をオフにすることで解決する可能性があります。Windowsアップデートを高速化する「配信の最適化」が原因でDNSエラーが発生するケースもあります。この場合は、「Windowsマーク」右クリックの「設定」から「更新とセキュリティ」へ進み、「配信の最適化」をクリックして「ほかのPCからダウンロードを許可する」をオフに変更してみましょう。

### ●PCやスマホなどのデバイスを再起動してみる

特定のデバイスのみでエラーが発生している場合、そのデバイスを再起動するだけでエラーが解消される可能性があります。

PCやスマートフォンは内部的なエラーを起こすことも少なくなりません。PCの場合、WindowsやMacのシステムやネットワークドライバーに一時的な障害が発生すると、DNSサーバの応答不良を引き起こすことがあります。再起動を行うことで内部的なエラーを解消できる可能性があるため、ネットワークの不具合が発生した際には、最初にデバイスの再起動を試してみると良いでしょう。

### ●パブリックDNSを設定する

使用中のDNSサーバに問題が発生した場合、パブリックDNSサーバに切り替えることで、DNS関連のエラーを解消できる可能性があります。パブリックDNSサーバとは、無料で利用できる公共のDNSサーバのこと。現在利用しているDNSサーバが応答しない場合や問題が発生した場合は、このパブリックDNSサーバへの切り替えが有効な解決策です。

パブリックDNSサーバに切り替えるには、ユーザーが自身のデバイスを操作し、ネットワーク設定を変更する必要があります。

Googleが提供する無料のパブリックDNSサーバ「Google Public DNS」に切り替える際の設定方法は、以下のとおりです。

**Windows 10：**

1. スタートボタンを右クリックし、「設定」を開く
2. 「ネットワークとインターネット」を選択する
3. 「ネットワークの詳細設定」から「ネットワークアダプタのオプション詳細」をクリックする
4. 使用中のインターネット接続アイコンを右クリックし、「プロパティ」を選ぶ
5. 「インターネットプロトコルv4（TCP/IPv4）」を選択し、「プロパティ」をクリックする
6. 「次のDNSサーバのアドレスを使う」にチェックを入れる
7. 代替DNSサーバに「8.8.4.4」、優先DNSサーバに「8.8.8.8」を入力する
8. 「OK」をクリックして設定を完了する

**Mac：**

1. Appleマークから「システム環境設定」を開き、「ネットワーク」をクリックする
2. 使用中のインターネット接続インターフェースを選び、「詳細」を選択する
3. 「DNS」タブのDNSサーバ欄でプラスボタンを選択する
4. 代替DNSサーバに「8.8.4.4」、優先DNSサーバに「8.8.8.8」を入力する
5. 既存のほかのIPアドレスがあれば、マイナスボタンで削除する
6. 「OK」をクリックして設定を完了する

上記の手順に従って設定を変更することで、パブリックDNSサーバに切り替えられます。

### ●IPv6の機能を無効化してみる

IPv6の機能を無効化することで、エラーが解消される可能性があります。WindowsまたはMacでIPv6を無効にする手順は、以下のとおりです。

**Windows：**

1. スタートボタンを右クリックし、「設定」を選択する
2. 「ネットワークとインターネット」をクリックし、「ネットワークの詳細設定」を選択する
3. 「ネットワークアダプタ オプションの詳細」をクリックする
4. 使用中のインターネット接続のアイコンを右クリックし、「プロパティ」を選択する
5. プロパティ画面で「インターネットプロトコルv6（TCP/IPv6）」のチェックボックスを外し、「OK」をクリックする

**Mac：**

1. 「システム環境設定」から「ネットワーク」に進む
2. 「ネットワーク」をクリックし、「詳細設定」を選択する
3. 「TCP/IP」をクリックする
4. 「IPv6の設定」内のドロップダウンメニューの中から「ローカル専用リンク」を選ぶ
5. 「OK」をクリックしたあとMacを再起動する

IPv6を利用している場合、上記の手順で機能を無効化することでDNSサーバの接続エラーが解消される可能性があります。

### ●不要なネットワークアダプタを無効にしてみる

複数のネットワークアダプタが同時に有効になっている場合、DNSサーバの応答に問題が生じることがあります。このような状況を解決するためには、以下の手順で不要なネットワークアダプタを無効にすることが有効です。

1. スタートメニューを右クリックし、「ネットワーク接続」を選ぶ
2. 「ネットワークとインターネット」を選択する
3. 「ネットワークの詳細設定」に進み、「ネットワークアダプタ オプションの詳細」を開く
4. 使用していないネットワークアダプタを右クリックし、「無効にする」を選択する

ネットワークアダプタを無効にしたあとは、ネットワークの診断を再度実行してみましょう。

## インターネットに欠かせないDNSサーバを理解しよう！

多くの業者が異なるDNSサーバサービスを提供しているため、利用するDNSサーバによって設定方法が変わります。また、エラーが発生した場合、様々な解決策が存在することを理解し、それらを試して問題を解決しましょう。

新たにDNSサーバを導入する際には、自社のドメイン名やIPアドレスをしっかり把握しておくと、DNSサーバの設定がスムーズに進められます。これを機に、DNSサーバの理解を深めてみてください。

インターネットへの接続が突然できなくなった場合、不安になる方も多いと思います。しかし、DNSサーバの仕組みを理解し、対処法を理解していれば、落ち着いて問題に対処できます。ぜひ、この記事を参考にして、万が一インターネットにつながらなくても冷静に対処できるようになりましょう。

## ホームページ制作の最適な発注先をスムーズに見つける方法

ホームページ制作会社選びでお困りではありませんか？  
日本最大級のシステム開発会社ポータルサイト「 [**発注ナビ**](https://hnavi.co.jp/lp/hp3/?argument=bzcUT99h&dmai=a639b5374da89e) 」は、実績豊富なエキスパートが貴社に寄り添った最適な制作会社選びを徹底的にサポートいたします。  
**ご紹介実績：23,000件、登録社数** **：6,300社** **（2025年1月現在）**  
  
外注先探しはビジネスの今後を左右する重要な任務です。しかし、  
  
「なにを基準に探せば良いのか分からない…。」  
「自社にあった外注先ってどこだろう…？」  
「費用感が不安…。」  
  
などなど、疑問や悩みが尽きない事が多いです。  
[**発注ナビ**](https://hnavi.co.jp/lp/hp3/?argument=bzcUT99h&dmai=a639b5374da89e) は、貴社の悩みに寄り添い、最適な外注探し選びのベストパートナーです。  
本記事に掲載するホームページ制作会社以外にも、最適な制作会社がご紹介可能です！  
ご相談からご紹介までは完全無料。  
まずはお気軽に、ご相談ください。　 **→ [詳しくはこちら](https://hnavi.co.jp/lp/hp3/?argument=bzcUT99h&dmai=a639b5374da89e)**

## ピッタリの制作会社が見つかる完全無料で全国6000社以上からご提案

[![即戦力のシステム開発会社を探すなら「発注ナビ」](https://hnavi.co.jp/_next/image/?url=https%3A%2F%2Fwordpress.hnavi.co.jp%2Fwp-content%2Fuploads%2F2022%2F12%2Fcta_hp.jpg&w=640&q=75)](https://hnavi.co.jp/lp/hp3/?argument=bzcUT99h&dmai=a639b5374da89e)

・最短1営業日以内に4～5社ご紹介  
・ITに詳しいコンシェルジュがサポート  
・10年以上、16500件以上の紹介実績  
・ご相談～ご紹介まで完全無料

＼完全無料・たった1日で回答／

[ご相談はコチラ](https://hnavi.co.jp/lp/hp3/?argument=bzcUT99h&dmai=a639b5374da89e)

## ■DNSサーバに関連した記事

[![東京でおすすめのホームページ制作会社32社【最新版】](https://hnavi.co.jp/_next/image/?url=https%3A%2F%2Fwordpress.hnavi.co.jp%2Fwp-content%2Fuploads%2F2023%2F02%2Ftokyo_hp_companies.jpg&w=640&q=75)](https://hnavi.co.jp/knowledge/blog/tokyo_hp_companies/?argument=bzcUT99h&dmai=a61c3cc8dcda68)

[東京でおすすめのホームページ制作会社32社【最新版】](https://hnavi.co.jp/knowledge/blog/tokyo_hp_companies/?argument=bzcUT99h&dmai=a61c3cc8dcda68)

[![大阪でおすすめのホームページ制作会社17社【最新版】](https://hnavi.co.jp/_next/image/?url=https%3A%2F%2Fwordpress.hnavi.co.jp%2Fwp-content%2Fuploads%2F2023%2F06%2Fosaka-hp_companies.jpg&w=640&q=75)](https://hnavi.co.jp/knowledge/blog/osaka_hp_companies/?argument=bzcUT99h&dmai=a61c3cc8dcea09)

[大阪でおすすめのホームページ制作会社17社【最新版】](https://hnavi.co.jp/knowledge/blog/osaka_hp_companies/?argument=bzcUT99h&dmai=a61c3cc8dcea09)

[![即戦力のシステム開発会社を探すなら「発注ナビ」](https://hnavi.co.jp/_next/image/?url=https%3A%2F%2Fwordpress.hnavi.co.jp%2Fwp-content%2Fuploads%2F2024%2F04%2FHN_1200x600a_Orange_6000_2.jpg&w=640&q=75)](https://hnavi.co.jp/lp/hp3/?argument=bzcUT99h&dmai=a639b5374da89e)

著者情報

[![soudan_banner](https://hnavi.co.jp/_next/image/?url=https%3A%2F%2Fwordpress.hnavi.co.jp%2Fwp-content%2Fuploads%2F2023%2F03%2F300x300_49505_170818_3.jpg&w=2048&q=75)

soudan\_banner

](https://hnavi.co.jp/lp/system2b/?argument=bzcUT99h&dmai=a62a80c49928e7)

### 人気記事

- [
	SIer（エスアイヤー）とは？基礎知識から仕事内容まで詳しく紹介
	](https://hnavi.co.jp/knowledge/blog/sier/)
- [
	ICT（情報通信技術）とは？ITとの違いと政府が進めるICTの利活用
	](https://hnavi.co.jp/knowledge/blog/ict/)
- [
	SQLとは？データベース言語の基礎知識をわかりやすく解説！
	](https://hnavi.co.jp/knowledge/blog/sql/)
- [
	Linuxとは？初心者でもわかる基本情報とメリットを紹介
	](https://hnavi.co.jp/knowledge/blog/linux-merit/)
- [
	Accessとは？Excelとの違いや基本操作を紹介！
	](https://hnavi.co.jp/knowledge/blog/access/)

### 関連記事

- [
	システムの運用保守とは？それぞれの役割や外部のサービスを利用する際のポイントを解説！
	](https://hnavi.co.jp/knowledge/blog/operation-maintenance-system/)
- [
	サーバ構築のポイントとは？今さら聞けないサーバに関する基礎知識
	](https://hnavi.co.jp/knowledge/blog/server_construction/)
- [
	システム移行とは？オンプレミスからクラウドなど種類や手順を解説
	](https://hnavi.co.jp/knowledge/blog/system-migration/)
- [
	Linuxとは？初心者でもわかる基本情報とメリットを紹介
	](https://hnavi.co.jp/knowledge/blog/linux-merit/)
- [
	インフラ構築とは？構築方法から外注する際のメリットデメリットまで詳しく紹介
	](https://hnavi.co.jp/knowledge/blog/infrastructure-build/)

### 関連特集

- [
	データセンター活用でおすすめのシステム開発会社13社【2025年版】
	](https://hnavi.co.jp/knowledge/blog/datacenter_system_companies/)
- [
	クラウド構築でおすすめのシステム開発会社20社【2025年版】
	](https://hnavi.co.jp/knowledge/blog/cloud_companies/)
- [
	セキュリティに強いおすすめのシステム開発会社10社【2025年版】
	](https://hnavi.co.jp/knowledge/blog/security_companies/)
- [
	AWS導入・構築でおすすめのシステム開発会社23社【2025年版】
	](https://hnavi.co.jp/knowledge/blog/aws_system_companies/)
- [
	テレワーク環境構築でおすすめのシステム開発会社9社【2025年版】
	](https://hnavi.co.jp/knowledge/blog/telework_companies/)

即戦力のシステム開発会社を探すなら

発注ナビは、システム開発に特化した  
発注先選定支援サービスです。

紹介実績

22000

対応社数

6000

対応  
テクノロジー

319

紹介達成数

92%

システム開発の発注先探しで  
こんなお悩みありませんか？

発注ナビの主な特徴

IT案件に特化

日本最大級6000社以上のシステム開発・WEB制作会社が登録。IT専門だから細かい要望が伝わり、理想的なパートナーが見つかる。

ITへの不安を徹底サポート

専門コンシェルジュがしっかりヒアリングするので、IT知識に不安があっても、まだ要件が固まっていなくても大丈夫。

完全無料・最短翌日紹介

コンシェルジュに発注内容を話すだけで最短翌日に開発会社をご紹介。しかも完全無料・成約手数料も無し。

さらに

東証プライム上場  
「アイティメディア株式会社」  
グループが運営

ご相談内容は一般公開しないため、クローズド案件でも安心。  
ご紹介企業は第三者調査機関にて信用情報・事業継続性を確認済です。

[はじめての方はこちら](https://hnavi.co.jp/guide/)