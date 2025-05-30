---
title: "Google、WebUIの自動生成ツール「Stitch」をベータ公開。自然言語で指示するとHTML/CSSを生成"
source: "https://www.publickey1.jp/blog/25/googlewebuistitchhtmlcss.html"
author:
published:
created: 2025-05-29
description: "Googleは日本時間5月21日未明に開幕した年次イベント「Google I/O 2025」で、自然言語で指示を入力すると自動的にWeb UIを生成するツール「Stitch」のベータ公開を発表しました。 StitchはもともとGalileo..."
tags:
  - clippings
  - ai
  - webdev
  - news
  - tool
---
2025年5月22日

  

Googleは日本時間5月21日未明に開幕した年次イベント「Google I/O 2025」で、自然言語で指示を入力すると自動的にWeb UIを生成するツール「 [Stitch](https://stitch.withgoogle.com/) 」のベータ公開を [発表しました](https://developers.googleblog.com/en/stitch-a-new-way-to-design-uis/) 。

![fig](https://www.publickey1.jp/2025/stitch-la01.png)

StitchはもともとGalileo AI社が提供する同名の「Galileo AI」と呼ばれるサービスでした。 [これをGoogleが買収](https://www.usegalileo.ai/explore) し、Geminiをベースにしたサービスにしたものと言えます。

## Stitchのデモ画面

下記がStitchの画面です。左側に例（Examples）が並び、右側にはプロンプトの入力欄があるというシンプルなものです。

![fig](https://www.publickey1.jp/2025/stitch-la02.png)

これから作成するUIがデスクトップ版かモバイル版かを選択可能。また、生成AIとしてGemini 2.5 Flashを使う「Standard Mode」、より忠実なデザインかつ画像見本からもUIを生成できる「Experimental Mode」が選択できます。

以下はStandard Modeのデモ画面です。プロンプトに作りたいWeb UIの内容を自然言語で入力。

![fig](https://www.publickey1.jp/2025/stitch-la03.png)

「Generate designs」ボタンをクリックすると、複数のデザイン案が表示されます。

![fig](https://www.publickey1.jp/2025/stitch-la04.png)

ライトモードとダークモード、基本カラー、角の丸め具合、フォントなどのテーマを設定します。

![fig](https://www.publickey1.jp/2025/stitch-la05.png)

デザイン案にテーマが設定されます。

![fig](https://www.publickey1.jp/2025/stitch-la06.png)

プロンプトで追加の内容を指示します。

![fig](https://www.publickey1.jp/2025/stitch-la07.png)

指示されたサーチ機能も追加されたWeb UIが生成され、完成です。

![fig](https://www.publickey1.jp/2025/stitch-la08.png)

生成されたWeb UIは、HTML/CSSのコードが表示されコピー＆ペーストが可能なほか、Figmaへのエクスポートも可能と説明されています。

#### あわせて読みたい

- [生成AIに疑似コードで指示すると自然言語よりも効率的にプログラムが生成できるというアイデアから生まれた、生成AI用の疑似言語「SudoLang」](https://www.publickey1.jp/blog/24/aiaisudolang.html)
- [「Migrate for Anthos」、Googleがベータ公開。物理サーバやVMware/AWS/Azureの仮想マシンからコンテナ化、Google Kubernetes Engineへの移行ツール](https://www.publickey1.jp/blog/19/migrate_for_anthosgooglevmwareawsazuregoogle_kubernetes_engine.html)
- [Google Cloud、AIによるアプリの自動生成ツール「Firebase Studio」公開。プロンプトで作りたいアプリを説明するだけ、無料で利用可能](https://www.publickey1.jp/blog/25/google_cloudaifirebase_studio.html)
- [［速報］GitHub、自然言語による指示だけでアプリケーションを生成する「GitHub Spark」テクニカルプレビュー公開](https://www.publickey1.jp/blog/24/githubgithub_spark.html)

[![fbシェア](https://www.publickey1.jp/2024/fbshare_btn.png)](http://www.facebook.com/share.php?u=https%3A%2F%2Fwww.publickey1.jp%2Fblog%2F25%2Fgooglewebuistitchhtmlcss.html)

[![Xポスト](https://www.publickey1.jp/2024/xpost_btn.png)](https://twitter.com/intent/tweet?original_referer=https%3A%2F%2Fwww.publickey1.jp%2F&text=Google%E3%80%81WebUI%E3%81%AE%E8%87%AA%E5%8B%95%E7%94%9F%E6%88%90%E3%83%84%E3%83%BC%E3%83%AB%E3%80%8CStitch%E3%80%8D%E3%82%92%E3%83%99%E3%83%BC%E3%82%BF%E5%85%AC%E9%96%8B%E3%80%82%E8%87%AA%E7%84%B6%E8%A8%80%E8%AA%9E%E3%81%A7%E6%8C%87%E7%A4%BA%E3%81%99%E3%82%8B%E3%81%A8HTML%2FCSS%E3%82%92%E7%94%9F%E6%88%90%20%EF%BC%8D%20Publickey&url=https%3A%2F%2Fwww.publickey1.jp%2Fblog%2F25%2Fgooglewebuistitchhtmlcss.html)

[![Feedly](https://www.publickey1.jp/2024/feedly_btn.png)](https://feedly.com/i/subscription/feed%2Fhttps%3A%2F%2Fwww.publickey1.jp%2Fatom.xml)

  

*≫次の記事*  
[国産インメモリDB「劔（Tsurugi）」がMCP対応、オープンソースで公開。自然言語で操作可能に](https://www.publickey1.jp/blog/25/dbtsurugimcp.html)  
  
*≪前の記事*  
[Google、自律型コーディングエージェント「Jules」をベータ公開](https://www.publickey1.jp/blog/25/googlejules.html)

  
  

#### タグクラウド

[クラウド](https://www.publickey1.jp/cloud/)  
[AWS](https://www.publickey1.jp/cloud/aws/) / [Azure](https://www.publickey1.jp/cloud/microsoft-azure/) / [Google Cloud](https://www.publickey1.jp/cloud/google-cloud/)  
[クラウドネイティブ](https://www.publickey1.jp/cloud/cloud-native/) / [サーバレス](https://www.publickey1.jp/cloud/serverless/)  
[クラウドのシェア](https://www.publickey1.jp/cloud/cloud-share/) / [クラウドの障害](https://www.publickey1.jp/cloud/cloud-failure/)  

[コンテナ型仮想化](https://www.publickey1.jp/container-vm/)

[プログラミング言語](https://www.publickey1.jp/programming-lang/)  
[JavaScript](https://www.publickey1.jp/programming-lang/javascript/) / [Java](https://www.publickey1.jp/programming-lang/java/) / [.NET](https://www.publickey1.jp/programming-lang/net/)  
[WebAssembly](https://www.publickey1.jp/programming-lang/webassembly/) / [Web標準](https://www.publickey1.jp/programming-lang/web-standards/)  
[開発ツール](https://www.publickey1.jp/devtools/) / [テスト・品質](https://www.publickey1.jp/devtools/software-test/)

[アジャイル開発](https://www.publickey1.jp/devops/agile/) / [スクラム](https://www.publickey1.jp/devops/scrum/) / [DevOps](https://www.publickey1.jp/devops/)

[データベース](https://www.publickey1.jp/database/) / [機械学習・AI](https://www.publickey1.jp/database/machine-learning-ai)  
[RDB](https://www.publickey1.jp/database/rdb/) / [NoSQL](https://www.publickey1.jp/database/nosql/)  

[ネットワーク](https://www.publickey1.jp/network/) / [セキュリティ](https://www.publickey1.jp/network/security)  
[HTTP](https://www.publickey1.jp/network/http/) / [QUIC](https://www.publickey1.jp/network/quic/)

[OS](https://www.publickey1.jp/os) / [Windows](https://www.publickey1.jp/os/windows) / [Linux](https://www.publickey1.jp/os/linux) / [仮想化](https://www.publickey1.jp/os/vm)  
[サーバ](https://www.publickey1.jp/hardware/server/) / [ストレージ](https://www.publickey1.jp/hardware/storage/) / [ハードウェア](https://www.publickey1.jp/hardware/)

[ITエンジニアの給与・年収](https://www.publickey1.jp/trends/payment/) / [働き方](https://www.publickey1.jp/trends/workstyle/)

[殿堂入り](https://www.publickey1.jp/after-words/recommend/) / [おもしろ](https://www.publickey1.jp/after-words/funny) / [編集後記](https://www.publickey1.jp/after-words/)

[全てのタグを見る](https://www.publickey1.jp/tags.html)

#### Blogger in Chief

![photo of jniino](https://www.publickey1.jp/images/profile.jpg)

Junichi Niino（jniino）  
IT系の雑誌編集者、オンラインメディア発行人を経て独立。2009年にPublickeyを開始しました。  
（ [詳しいプロフィール](https://www.publickey1.jp/about-us.html) ）

Publickeyの新着情報をチェックしませんか？  
Twitterで ： [@Publickey](https://twitter.com/publickey/)  
Facebookで ： [Publickeyのページ](https://www.facebook.com/publickey/)  
RSSリーダーで ： [Feed](https://www.publickey1.jp/atom.xml)  

#### 最新記事10本

- [マイクロソフト、GitHub CopilotでJavaと.NETのコードを自動的にモダナイズする機能をパブリックプレビュー公開](https://www.publickey1.jp/blog/25/github_copilotjavanet.html)
- [Docker、CVE脆弱性がほぼゼロのセキュリティ強化型コンテナイメージ「Docker Hardened Images」提供開始](https://www.publickey1.jp/blog/25/dockercvedocker_hardend_images.html)
- [金融庁、金融機関にパスワードつきZipファイルの電子メール送付の慣行を改めるよう要求との報道](https://www.publickey1.jp/blog/25/zip.html)
- [Visual Studio Codeが本体にAI関連機能を組み込みへ、「オープンソースのAIエディタ」になると表明](https://www.publickey1.jp/blog/25/visual_studio_codeaiai.html)
- [国産インメモリDB「劔（Tsurugi）」がMCP対応、オープンソースで公開。自然言語で操作可能に](https://www.publickey1.jp/blog/25/dbtsurugimcp.html)
- [Google、WebUIの自動生成ツール「Stitch」をベータ公開。自然言語で指示するとHTML/CSSを生成](https://www.publickey1.jp/blog/25/googlewebuistitchhtmlcss.html)
- [Google、自律型コーディングエージェント「Jules」をベータ公開](https://www.publickey1.jp/blog/25/googlejules.html)
- [［速報］「GitHub Copilot Coding Agent」パブリックプレビュー。AIにIssueをアサインすると、解決に向け自律的にプログラミング](https://www.publickey1.jp/blog/25/github_copilot_coding_agentaiissue.html)
- [［速報］マイクロソフト、Copilotに対してユーザーが追加学習を行える「Microsoft 365 Copilot Tuning」発表](https://www.publickey1.jp/blog/25/copilotmicrosoft_365_copilot_tuning.html)
- [［速報］マイクロソフト、WindowsがMCPをサポートすると発表。AIエージェントでWindowsやアプリとの連携が可能に](https://www.publickey1.jp/blog/25/windowsmcpaiwindows.html)