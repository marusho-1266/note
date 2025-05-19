---
title: "Google Apps Script用のテストライブラリ「GASUnit」の紹介"
author: "munieru_jp"
source: "https://qiita.com/munieru_jp/items/101ee00c6906847df750"
created: 2025-05-16
published: 2019-08-08
tags: #programming #javascript #testing #article #practical
---

# Google Apps Script用のテストライブラリ「GASUnit」の紹介

Google Apps Scriptを使うと、手軽にサーバーサイドプログラムを作ることができて便利ですよね。  
かくいう僕も、Google Apps Scriptで作ったBOTを社内のSlackで運用しています。  
今回は、僕が作成したGoogle Apps Script用のテストライブラリ「**[GASUnit](https://github.com/gasunit/GASUnit)**」をご紹介します。

## Google Apps Scriptの開発手法

Google Apps Scriptには、規模や好みに応じて様々な開発手法が存在します。

- オンラインエディタで書く
- オンラインエディタで書き、[Google Apps Script GitHub アシスタント](https://chrome.google.com/webstore/detail/google-apps-script-github/lfjcgcmkmjjlieihflfhjopckgpelofo)でGitHubと同期
- ローカルで書き、[clasp](https://github.com/google/clasp)でGoogleドライブと同期

一時的に使うような小さなプログラムであればオンラインエディタのみで充分ですが、BOTのように継続的に運用していくものとなると、少なくともソースコード管理はしておきたいところです。  
それにくわえて、品質を担保するためのテストコードもあれば最高ですよね？

## Google Apps Scriptにおけるテスト

Google Apps Scriptには、標準でテストを実行するための仕組みが存在しません。  
ローカルで開発する場合は通常のJavaScriptと同様に[Mocha](https://mochajs.org/)や[Jasmine](https://jasmine.github.io/)などのテストフレームワークを使えばいいのですが、オンラインエディタ上で開発する場合はそれらのフレームワークは使えません。  
[QUnit for Google Apps Script](https://github.com/simula-innovation/qunit/tree/gas/gas)というGoogle Apps Script用のテストライブラリもあるにはありますが、[QUnit](https://qunitjs.com/)が元となっているため癖があります（テストを実行するために`doGet`関数を用意してウェブアプリケーションとして公開する必要がある）。  
そこで、癖のないGoogle Apps Script用のテストライブラリとして[GASUnit](https://github.com/gasunit/GASUnit)を作りました。

## GASUnitの特徴

GASUnitでは、テスト結果をGoogle Apps Scriptのログに出力するか、Slackに投稿することができます。  
テストの記述スタイルはいくつかある中から選べる……予定なのですが、現時点ではExportsスタイルのみをサポートしています。

## 使用方法

実際にGASUnitを使用してテストを実行する方法をご紹介します。

### ライブラリを追加

`リソース` -> `ライブラリ`からライブラリのダイアログを開き、以下のプロジェクトキーを入力して追加します。

`MSnMmw8hLWgjUG6uKSTQBEzVZgzu5bsVr`

複数のバージョンがありますが、基本的には最新版を選べばOKです（バージョン表記は[セマンティックバージョニング](https://semver.org/lang/ja/)に準拠しています）。

### テストコードを書く

例として、`Array#indexOf`のテストコードをExportsスタイルで書いてみましょう。

#### 使用する関数を変数に代入

ファイルの先頭で、テストに使用する`exports`関数と`assert`関数を変数に入れます。  
変数に入れずに毎回`GASUnit.exports`や`GASUnit.assert`のように書いてもいいですが、こうすることでテストコードが簡潔になります。

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

あるいは以下のように書くことで、スクリプトのプロパティに`WEBHOOK_URL`が設定されていればSlackに投稿し、設定されていなければログに出力するようにもできます。

```js
var WEBHOOK_URL = PropertiesService.getScriptProperties().getProperty('WEBHOOK_URL')
var exports = WEBHOOK_URL ? GASUnit.slack(WEBHOOK_URL).exports : GASUnit.exports
var assert = GASUnit.assert
```

#### テストを書く

Google Apps Scriptでは関数単位で実行するので、関数の中にテスト処理を記述していきます。  
関数名はなんでもいいのですが、Google Apps Scriptの仕様上末尾に`_`をつけるとプライベート扱いになって実行できないので、末尾に`_`をつけないようにしてください。

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
`assert`関数は引数がtruthyなら成功、falsyなら失敗となる単純な実装です。

あるいは、公式アサーションライブラリの[AssertGAS](https://github.com/gasunit/AssertGAS)を使って、以下のように書くこともできます。

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

#### テスト結果をSlackに投稿する場合

関数を実行すると、Slackにテスト結果が投稿されます。

## おわりに

あなたもぜひGASUnitを使って、Google Apps Scriptのテストコードを書いてみてください。  
また、GASUnitは[オープンソース](https://github.com/gasunit/GASUnit)なので、[プルリクエスト](https://github.com/gasunit/GASUnit/pulls)や[イシュー](https://github.com/gasunit/GASUnit/issues)などの[コントリビュート](https://github.com/orgs/gasunit/projects/1)を歓迎しています！

## 使用例

- [gasunit/example](https://github.com/gasunit/example)