---
title: DNSとは？インターネットを支える仕組み - ルートサーバーとキャッシュサーバーについても解説
source: https://www.rworks.jp/system/system-column/sys-entry/16303/
author:
  - "[[クラウド導入・システム運用ならアールワークスへ]]"
published: 
created: 2025-05-02
description: 目次はじめにDNS（Domain Name System）とはDNSの機能DNSルートサーバーとキャッシュサーバーDNSを使用したアクセスの仕組みおわりに はじめに DNSは、インターネットをスムーズに活用するために必要 […]
tags:
  - dns
  - network
  - basic
  - article
  - concept
association:
---
## Managed Service Column ＜システム運用コラム＞DNSとは？インターネットを支える仕組み – ルートサーバーとキャッシュサーバーについても解説

- [HOME](https://www.rworks.jp/ "クラウド導入・システム運用ならアールワークスへへ移動")
\>- [システム運用監視サービス](https://www.rworks.jp/system/ "システム運用監視サービスへ移動")
\>- [システム運用コラム](https://www.rworks.jp/system/system-column/ "システム運用コラムへ移動")
\>- [入門編](https://www.rworks.jp/cat-system-column/sys-entry/ "入門編へ移動")
\>- DNSとは？インターネットを支える仕組み – ルートサーバーとキャッシュサーバーについても解説

![DNS(DomainNameSystem)](https://www.rworks.jp/wp-content/uploads/2020/07/dns-1.png)

Category： [入門編](https://www.rworks.jp/cat-system-column/sys-entry/)

## はじめに

DNSは、インターネットをスムーズに活用するために必要不可欠なシステムです。しかし、普段DNSの存在を意識することはほとんどないので、どのような仕組みなのかよくわからないという方もいらっしゃるのではないでしょうか。

DNSは、私たちが何気なく利用しているインターネットのさまざまな場面で活躍しています。例えば、ブラウザからWebページを閲覧するときや、メールを送信するときにも必ず利用する機能です。

そこで今回は、DNSの機能や仕組み、ルートサーバーとキャッシュサーバーの役割などについて解説します。

## DNS（Domain Name System）とは

DNSは「Domain Name System」の略称で、ドメイン名とIP（Internet Protocol）を紐付けるシステムです。ドメイン名はインターネットの住所にあたり、通信の宛先を特定するための重要な手がかりとなります。

インターネットを利用するすべての端末には、必ずIPアドレスが与えられます。そのため、ある端末のブラウザからWebページを参照したいと思ったときは、相手のWebページを管理している端末のIPアドレスに接続する必要があります。

しかし、Webページへアクセスする際に瞬時にそのIPアドレスを知ることは難しいでしょう。そこで、 **人間が見てわかりやすい「ドメイン名」をインターネット上の通信で使われるIPアドレスに変換してくれる** のがDNSです。

例えば「192.0.2.1」などの数字で与えられたIPアドレスと、「https://www.rworks.jp/」というような名前の対応付けを双方向(IPアドレスから名前、名前からIPアドレス)で行う役割を持っています。この変換の仕組みは「 **名前解決** 」と呼ばれることもあります。

ここで変換される名前について説明しておきましょう。

IPアドレスに対応したWebページの名前を「https://www.rworks.jp/」とすると、ドメイン名は「rworks.jp」の部分です。世界に1つだけの名前で、他と重複することはありません。「www」はホスト名といって、ネットワーク上のある端末を表します。こちらはドメイン名と違い、ネットワークが異なっていると重複する可能性があります。

ドメイン名とホスト名を合わせた部分の名前を「FQDN（Fully Qualified Domain Name）」と呼び、ここでは「www.rworks.jp」の部分にあたります。この場合、「rworks.jpというネットワーク上に存在する、wwwという名前の端末」という意味になります。

![ホスト名 ドメイン名 FQDN](https://www.rworks.jp/wp-content/uploads/2020/07/fqdn.png)

通常、ドメイン名をIPアドレスに変換する役割を持つDNSサーバーと呼ばれるサーバーをプロバイダーが所有していて、Webページを閲覧する際に自動的にIPアドレスを変換してくれます。そのため、通常はあまりDNSを意識してインターネットを利用することは多くありません。

しかし、なかにはパブリックDNSと呼ばれるDNSもあり、それらを利用するように端末に手動で設定することもできます。 **パブリックDNSはインターネット上に公開されている誰でも自由に使えるDNS** で、種類によって、安全性が高い、応答速度が非常に速いなどの特徴があります。

有名なパブリックDNSには、Cloudflareの「1.1.1.1」、Google Public DNSの「8.8.8.8」「8.8.4.4」などがあります。「1.1.1.1」はIPアドレスなどのアクセスログを残さないセキュリティの高さが特徴です。一方の8.8.8.8や8.8.4.4は安全性も高く、Googleの膨大なデータ量を駆使した応答速度の速さが魅力です。

DNSには膨大なIPアドレスとドメインの情報が蓄積されていて、アクセスのリクエストがあるたびにDNSサーバーからIPアドレスを検索します。世界中のあらゆるDNSが、インターネットのアクセスを日夜にわたって支えているのです。

## DNSの機能

端末からドメイン名に紐付いたIPアドレスが何であるか問い合わせがあったときに、そのオリジナル情報を端末へ返すサーバーを「権威DNSサーバー」といいます。権威DNSサーバーはコンテンツサーバーとも呼ばれていて、「ゾーン」という決められたドメイン内のIPアドレスとの紐付け情報を蓄積しています。このドメインのIPアドレスとの紐付け情報をまとめたファイルを「ゾーンファイル」と呼びます。

![Domain Name Space](https://www.rworks.jp/wp-content/uploads/2020/07/dns-2.png)

(出典： [Wikipedia "Domain Name System"](https://en.wikipedia.org/wiki/Domain_Name_System))

世界には膨大な数のドメイン名が存在しているので、権威DNSサーバーは1台だけで運用しているわけではありません。そのため、問い合わせたDNSサーバーが目的のドメインの権威DNSサーバではない場合も考えられます。

その場合、ツリー構造でつながれているネットワーク上に配置されたほかのDNSサーバーに対して、問い合わせを受けたDNSサーバーが問い合わせをします。この問い合わせをするプログラムのことを「フルサービスリゾルバ」といいます。

ほかにも「スタブリゾルバ」というプログラムもありますが、スタブリゾルバは利用者の端末からDNSサーバーへIPアドレスの問い合わせを行なうプログラムのことです。単にリゾルバといったときは「スタブリゾルバ」を指していることが多いです。

フルサービスリゾルバは該当のデータが見つかるまでほかの権威DNSサーバーへ問い合わせを繰り返し、引き当たったIPアドレスをスタブリゾルバに返します。それをスタブリゾルバが端末に返すことで、リクエストを送信した端末がアクセス先のIPアドレスが何であるかを把握し、Webページにアクセスできるという仕組みです。

![DNS resolver](https://www.rworks.jp/wp-content/uploads/2020/07/dns-4-5.png)

## DNSルートサーバーとキャッシュサーバー

DNSルートサーバーはドメイン名が保存されているDNSサーバーです。登録済みのサーバーは世界に13あり、A～Mの略称が付けられています。

DNSルートサーバーは全部で12の組織が運用しており、そのほとんどは米国ですが、日本にも1つだけDNSルートサーバーがあります。1997年8月から複数の大学が共同でインターネットに関する研究・運用する「WIDEプロジェクト」によって管理されてきましたが、2005年12月以降はJPRS（株式会社日本レジストリサービス）との共同運用になりました。

一方、キャッシュサーバーはドメイン名がどのIPアドレスに対応しているかという情報を一定期間保存しておくためのサーバーです。同じ端末からアクセスがあると、DNSルートサーバーへ問い合わせを行なわずにIPアドレスを返すことができます。キャッシュサーバーに必要な情報がなかった場合は、DNSルートサーバーからIPアドレスを取得します。

世界に13という限られた数の **DNSルートサーバーですべてのリクエストを処理できるのは、膨大なキャッシュサーバーが仲介しているからに他なりません。キャッシュサーバーはDNSルートサーバーへの問い合わせを削減してIPアドレスの検索を効率化し、DNSサーバーに過剰な負担をかけないようにする役割** を果たしています。

## DNSを使用したアクセスの仕組み

端末の利用者がブラウザからWebページを参照するまでには、いくつかの手順があります。順を追って見ていきましょう。

まず、URLにブラウザでアクセスすると、利用者の端末はキャッシュサーバーにIPアドレスの問い合わせをします。キャッシュサーバーに必要な情報が保存されていればそのままIPアドレスを返しますが、ない場合はDNSルートサーバーに「どのDNSが必要な情報を持っているか」を問い合わせます。

次に、問い合わせを受けたDNSルートサーバーが情報を持っているほかのDNSサーバーを教え、キャッシュサーバーは情報を持っているDNSサーバーに改めてアクセスします。そこに必要な情報があればIPアドレスを取得しますが、問い合わせたサーバーにも適切な情報が見つからない場合は、見つかるまで同じ動作を繰り返します。

最終的に、キャッシュサーバーは取得したIPアドレスを端末に返します。このような流れを経て、利用者はIPアドレスを使ってブラウザからWebページへのアクセスが可能になります。

## おわりに

DNSの機能や仕組み、各サーバーの役割などについて解説してきました。

DNSは今日のインターネットの発展を支えている大切な仕組みです。 世界中に張り巡らされた広大なネットワークの一つひとつに名前を付けてくれるDNSがあるからこそ、当たり前のようにWebページを閲覧することができる のです。

![Free](https://www.rworks.jp/wp-content/themes/rworks-2020/img/service/detail/mark02.png)

### 資料ダウンロード

課題解決に役立つ詳しいサービス資料はこちら

![資料ダウンロード](https://www.rworks.jp/wp-content/uploads/2021/10/system-op-cat-d.jpg)
- システム運用代行サービスカタログ
	システム運用代行サービスのメニューと料金をご確認いただけます。
	[ダウンロード](https://www.rworks.jp/download/op-service-catalog/)

[![システム運用個別相談会（無料）](https://www.rworks.jp/wp-content/uploads/2022/01/58f15ebfca5c557fb692bde169400b10.png)](https://www.rworks.jp/inquiry3/)

Tag： [DNS](https://www.rworks.jp/tag-system-column/dns/)

- [
	（Apache/Nginx/IIS）Webサーバー機能とは？よく使われるサーバーごとの違いについても解説
	](https://www.rworks.jp/system/system-column/sys-entry/16296/)
- [
	ロードバランサー（LB）とは？仕組みやDNSラウンドロビンとの違いについて解説
	](https://www.rworks.jp/system/system-column/sys-entry/16305/)

## 関連記事

- [![Webサイトが表示されるまでの仕組みを用語の解説を交えて紹介](https://www.rworks.jp/wp-content/uploads/2020/09/web-view-1.png)
	Webサイトが表示されるまでの仕組みを用語の解説を交えて紹介
	](https://www.rworks.jp/system/system-column/sys-entry/21249/)
- [![ロードバランサー（LB）とは？仕組みやDNSラウンドロビンとの違いについて解説](https://www.rworks.jp/wp-content/uploads/2020/09/Load-Blancer.png)
	ロードバランサー（LB）とは？仕組みやDNSラウンドロビンとの違いについて解説
	](https://www.rworks.jp/system/system-column/sys-entry/16305/)

single.php