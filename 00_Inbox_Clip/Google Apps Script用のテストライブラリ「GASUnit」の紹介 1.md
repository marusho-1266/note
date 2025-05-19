---
title: "Google Apps Script用のテストライブラリ「GASUnit」の紹介"
source: "https://qiita.com/munieru_jp/items/101ee00c6906847df750"
author:
  - "[[Qiita]]"
published: 2019-08-08
created: 2025-05-16
description: "Google Apps Scriptを使うと、手軽にサーバーサイドプログラムを作ることができて便利ですよね。かくいう僕も、Google Apps Scriptで作ったBOTを社内のSlackで運用…"
tags:
  - "clippings #inbox"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

この記事は最終更新日から5年以上が経過しています。

Google Apps Scriptを使うと、手軽にサーバーサイドプログラムを作ることができて便利ですよね。  
かくいう僕も、Google Apps Scriptで作ったBOTを社内のSlackで運用しています。 [^1]  
今回は、僕が作成したGoogle Apps Script用のテストライブラリ「 **[GASUnit](https://github.com/gasunit/GASUnit)** 」をご紹介します。

## Google Apps Scriptの開発手法

Google Apps Scriptには、規模や好みに応じて様々な開発手法が存在します。

- オンラインエディタで書く
- オンラインエディタで書き、 [Google Apps Script GitHub アシスタント](https://chrome.google.com/webstore/detail/google-apps-script-github/lfjcgcmkmjjlieihflfhjopckgpelofo) でGitHubと同期
- ローカルで書き、 [clasp](https://github.com/google/clasp) でGoogleドライブと同期

一時的に使うような小さなプログラムであればオンラインエディタのみで充分ですが、BOTのように継続的に運用していくものとなると、少なくともソースコード管理はしておきたいところです。  
それにくわえて、品質を担保するためのテストコードもあれば最高ですよね？

## Google Apps Scriptにおけるテスト

Google Apps Scriptには、標準でテストを実行するための仕組みが存在しません。  
ローカルで開発する場合は通常のJavaScriptと同様に [Mocha](https://mochajs.org/) や [Jasmine](https://jasmine.github.io/) などのテストフレームワークを使えばいいのですが、オンラインエディタ上で開発する場合はそれらのフレームワークは使えません。  
[QUnit for Google Apps Script](https://github.com/simula-innovation/qunit/tree/gas/gas) というGoogle Apps Script用のテストライブラリもあるにはありますが、 [QUnit](https://qunitjs.com/) が元となっているため癖があります（テストを実行するために `doGet` 関数を用意してウェブアプリケーションとして公開する必要がある）。  
そこで、癖のないGoogle Apps Script用のテストライブラリとして [GASUnit](https://github.com/gasunit/GASUnit) を作りました。

## GASUnitの特徴

GASUnitでは、テスト結果をGoogle Apps Scriptのログに出力するか、Slackに投稿することができます。  
テストの記述スタイルはいくつかある中から選べる……予定なのですが、現時点ではExportsスタイルのみをサポートしています。

## 使用方法

実際にGASUnitを使用してテストを実行する方法をご紹介します。

### ライブラリを追加

`リソース` -> `ライブラリ` からライブラリのダイアログを開き、以下のプロジェクトキーを入力して追加します。

`MSnMmw8hLWgjUG6uKSTQBEzVZgzu5bsVr`

[![eecbb2f7-3cdb-a8d6-3d50-36ed07336859.png](https://qiita-image-store.s3.amazonaws.com/0/132608/eecbb2f7-3cdb-a8d6-3d50-36ed07336859.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F132608%2Feecbb2f7-3cdb-a8d6-3d50-36ed07336859.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=56d0126162359345996c1a3394808fa0)

複数のバージョンがありますが、基本的には最新版を選べばOKです（バージョン表記は [セマンティックバージョニング](https://semver.org/lang/ja/) に準拠しています）。

[![c03dcdb8-b0ec-8339-b47b-feccb077b2bf.png](https://qiita-image-store.s3.amazonaws.com/0/132608/c03dcdb8-b0ec-8339-b47b-feccb077b2bf.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F132608%2Fc03dcdb8-b0ec-8339-b47b-feccb077b2bf.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8407d7e06262b26f5d65c1c11011285a)

### テストコードを書く

例として、 `Array#indexOf` のテストコードをExportsスタイルで書いてみましょう。

#### 使用する関数を変数に代入

ファイルの先頭で、テストに使用する `exports` 関数と `assert` 関数を変数に入れます。  
変数に入れずに毎回 `GASUnit.exports` や `GASUnit.assert` のように書いてもいいですが、こうすることでテストコードが簡潔になります。

テスト結果をログに出力する場合：

```js
var exports = GASUnit.exports
var assert = GASUnit.assert
```

テスト結果をSlackに投稿する場合：

```js
var WEBHOOK_URL = 'https://...'
var exports = GASUnit.slack(WEBHOOK_URL).exports
var assert = GASUnit.assert
```

ソースコードを公開する場合はWebhook URLをリテラルとして書くべきではないので、スクリプトのプロパティを使用してください。

```js
var WEBHOOK_URL = PropertiesService.getScriptProperties().getProperty('WEBHOOK_URL')
var exports = GASUnit.slack(WEBHOOK_URL).exports
var assert = GASUnit.assert
```

あるいは以下のように書くことで、スクリプトのプロパティに `WEBHOOK_URL` が設定されていればSlackに投稿し、設定されていなければログに出力するようにもできます。

```js
var WEBHOOK_URL = PropertiesService.getScriptProperties().getProperty('WEBHOOK_URL')
var exports = WEBHOOK_URL ? GASUnit.slack(WEBHOOK_URL).exports : GASUnit.exports
var assert = GASUnit.assert
```

#### テストを書く

Google Apps Scriptでは関数単位で実行するので、関数の中にテスト処理を記述していきます。  
関数名はなんでもいいのですが、Google Apps Scriptの仕様上末尾に `_` をつけるとプライベート扱いになって実行できないので、末尾に `_` をつけないようにしてください。

```js
function test_array () {
  exports({
    'Array': {
      '#indexOf()': {
        'should return -1 when not present': function () {
          assert([1, 2, 3].indexOf(4) === -1)
        },
        'should return the index when present': function () {
          assert([1, 2, 3].indexOf(3) === 2)
        }
      }
    }
  })
}
```

Exportsスタイルでは、引数にオブジェクトを渡して階層構造で記述していきます。  
`assert` 関数は引数がtruthyなら成功、falsyなら失敗となる単純な実装です。

あるいは、公式アサーションライブラリの [AssertGAS](https://github.com/gasunit/AssertGAS) を使って、以下のように書くこともできます。

```js
function test_array () {
  exports({
    'Array': {
      '#indexOf()': {
        'should return -1 when not present': function () {
          var index = [1, 2, 3].indexOf(4)
          assertThat(index).is(-1)
        },
        'should return the index when present': function () {
          var index = [1, 2, 3].indexOf(3)
          assertThat(index).is(2)
        }
      }
    }
  })
}
```

### テストを実行

テストが書けたら、Google Apps Scriptの通常の関数と同様に実行するのみです。

#### テスト結果をログに出力する場合

関数を実行すると、テスト結果がログに出力されます。

成功パターン：  
[![8813f106-094d-0f96-4d8a-6ddd652f48cd.png](https://qiita-image-store.s3.amazonaws.com/0/132608/8813f106-094d-0f96-4d8a-6ddd652f48cd.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F132608%2F8813f106-094d-0f96-4d8a-6ddd652f48cd.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=a2e384437eee74860d61a1ed993e04ef)

失敗パターン：  
[![207c784b-afad-4a46-0851-74f7bb636705.png](https://qiita-image-store.s3.amazonaws.com/0/132608/207c784b-afad-4a46-0851-74f7bb636705.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F132608%2F207c784b-afad-4a46-0851-74f7bb636705.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=83d8027a0851f239fdbc8ba2ce12d5f4)

#### テスト結果をSlackに投稿する場合

関数を実行すると、Slackにテスト結果が投稿されます。

成功パターン：  
[![00918694-fb80-3117-9b06-d8ee857c9ec5.png](https://qiita-image-store.s3.amazonaws.com/0/132608/00918694-fb80-3117-9b06-d8ee857c9ec5.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F132608%2F00918694-fb80-3117-9b06-d8ee857c9ec5.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3d78ea829c742e3b9f709c1ceb317e34)

失敗パターン：  
[![03b15dd8-3ef3-3222-13e5-d46dbb13a61b.png](https://qiita-image-store.s3.amazonaws.com/0/132608/03b15dd8-3ef3-3222-13e5-d46dbb13a61b.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F132608%2F03b15dd8-3ef3-3222-13e5-d46dbb13a61b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7acb35a40a54a38452c17d5f08868796)

## おわりに

あなたもぜひGASUnitを使って、Google Apps Scriptのテストコードを書いてみてください。  
また、GASUnitは [オープンソース](https://github.com/gasunit/GASUnit) なので、 [プルリクエスト](https://github.com/gasunit/GASUnit/pulls) や [イシュー](https://github.com/gasunit/GASUnit/issues) などの [コントリビュート](https://github.com/orgs/gasunit/projects/1) を歓迎しています！

## 使用例

- [gasunit/example](https://github.com/gasunit/example)

[0](https://qiita.com/munieru_jp/items/#comments)

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fmunieru_jp%2Fitems%2F101ee00c6906847df750&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fmunieru_jp%2Fitems%2F101ee00c6906847df750&realm=qiita)

[56](https://qiita.com/munieru_jp/items/101ee00c6906847df750/likers)

46

[^1]: [Google Apps ScriptでSlackの新規(ユーザー|チャンネル|絵文字)を通知するBOTを作った - Qiita](https://qiita.com/munieru_jp/items/4634eb353ebf6efe347d)