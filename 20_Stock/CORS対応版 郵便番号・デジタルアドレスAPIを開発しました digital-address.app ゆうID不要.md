---
title: "CORS対応版 郵便番号・デジタルアドレスAPIを開発しました digital-address.app ゆうID不要"
source: "https://qiita.com/relu/items/9b8085e89c01bcf6d35a?utm_source=dlvr.it&utm_medium=twitter"
author:
  - "[[relu]]"
published: 2025-05-26
created: 2025-05-28
description: "ソースコードを公開しました！ご自身で開発したい方向けhttps://github.com/GitHub30/digital-address.php本記事では、日本郵便公式の「郵便番号・デジタル…"
tags:
  - clippings
  - api
  - webdev
  - tutorial
  - news
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

ソースコードを公開しました！ご自身で開発したい方向け  
[https://github.com/GitHub30/digital-address.php](https://github.com/GitHub30/digital-address.php)

本記事では、日本郵便公式の「郵便番号・デジタルアドレスAPI」をブラウザ上で直接利用できるようにした **CORS対応版クライアントAPI** をご紹介します。公式APIは2025年5月26日より無料で提供開始され、常に最新の郵便番号データと連携するため、従来の `ken_all.csv` よりも信頼性が高いのが特長です。デモは [digital-address.app](https://digital-address.app/) で動作確認でき、APIドキュメントは [guide-biz.da.pf.japanpost.jp/api/](https://guide-biz.da.pf.japanpost.jp/api/) にて公開されています。

- **公式API提供開始** ：2025年5月26日より無料提供開始 ([郵便局 | 日本郵便株式会社](https://www.post.japanpost.jp/notification/pressrelease/2025/00_honsha/0526_02_01.pdf?utm_source=chatgpt.com "[PDF] 日本郵便公式「郵便番号・デジタルアドレス API」の提供を開始 ..."))
- **デモサイト** ： [digital-address.app](https://digital-address.app/) で即時テスト可能
- **ブラウザ対応** ：CORS設定済みなのでReactなどのフロントエンドから `fetch` で直接呼び出し可
- **高い信頼性** ：日本郵便の最新データを常に参照 ([郵便局 | 日本郵便株式会社](https://www.post.japanpost.jp/notification/pressrelease/2025/00_honsha/0526_02_01.pdf?utm_source=chatgpt.com "[PDF] 日本郵便公式「郵便番号・デジタルアドレス API」の提供を開始 ..."))
- **完全無料** ：企業・個人問わず追加コストなしで利用可 ([郵便局 | 日本郵便株式会社](https://www.post.japanpost.jp/notification/pressrelease/2025/00_honsha/0526_02.html?utm_source=chatgpt.com "日本郵便公式「郵便番号・デジタルアドレスAPI」の提供を開始 ..."))

---

## はじめに

日本の住所データはかつてCSVで配布されることが一般的でしたが、データ更新や運用コストが課題でした。  
2025年5月26日に日本郵便が「郵便番号・デジタルアドレスAPI」を公式提供開始し、住所データをAPIで安定的に取得できるようになりました ([郵便局 | 日本郵便株式会社](https://www.post.japanpost.jp/notification/pressrelease/2025/00_honsha/0526_02_01.pdf?utm_source=chatgpt.com "[PDF] 日本郵便公式「郵便番号・デジタルアドレス API」の提供を開始 ..."))。  
さらに、Webブラウザから直接呼び出せるようCORS対応を施したクライアントAPIを開発しましたので、そのポイントを解説します。

---

## 特徴

### 1\. ブラウザから fetch で直接呼び出し可能

通常、ブラウザから別ドメインのAPIを呼ぶとCORSエラーが発生しますが、本APIでは日本郵便公式APIへのすべてのリクエストに対してCORSヘッダーを付与しています。

- デモサイトで動作確認可能： [digital-address.app](https://digital-address.app/)
- APIドキュメント：公式ドキュメントに従うだけでOK ([郵便番号・デジタルアドレス for Biz](https://guide-biz.da.pf.japanpost.jp/api/?utm_source=chatgpt.com "郵便番号・デジタルアドレスAPI"))

### 2\. 日本郵便公式の最新データを利用

- CSV配布の `ken_all.csv` と異なり、常に最新の郵便番号データと連携 ([郵便局 | 日本郵便株式会社](https://www.post.japanpost.jp/notification/pressrelease/2025/00_honsha/0526_02_01.pdf?utm_source=chatgpt.com "[PDF] 日本郵便公式「郵便番号・デジタルアドレス API」の提供を開始 ..."))
- 漢字・カナ・ローマ字対応、フリーワード検索も可能 ([郵便局 | 日本郵便株式会社](https://www.post.japanpost.jp/notification/pressrelease/2025/00_honsha/0526_02_01.pdf?utm_source=chatgpt.com "[PDF] 日本郵便公式「郵便番号・デジタルアドレス API」の提供を開始 ..."))

### 3\. デジタルアドレス対応

- 7桁の英数字（デジタルアドレス）から住所を逆引き ([GIGAZINE](https://gigazine.net/news/20250526-japanpost-digital-address/?utm_source=chatgpt.com "日本郵便が住所を英数字7桁で表して送り状作成を簡略 ... - GIGAZINE"))
- デジタル庁のレジストリ構想とも親和性が高く、今後の拡張も期待 ([ケータイ Watch](https://k-tai.watch.impress.co.jp/docs/news/2017171.html?utm_source=chatgpt.com "英数字7桁で伝えられる「デジタルアドレス」の提供を開始 公式API ..."))

### 4\. 完全無料で商用利用OK

- 企業向けにも無償で開放 ([郵便局 | 日本郵便株式会社](https://www.post.japanpost.jp/notification/pressrelease/2025/00_honsha/0526_02.html?utm_source=chatgpt.com "日本郵便公式「郵便番号・デジタルアドレスAPI」の提供を開始 ..."))
- APIキー取得や利用制限の心配なく導入できる

---

## 使い方

```bash
curl https://digital-address.app/<7桁の郵便番号>
curl https://digital-address.app/<7桁のデジタルアドレス>
```

例

```bash
curl https://digital-address.app/1000001
curl https://digital-address.app/CV8LFCG
```

JavaScript

```js
const res = await fetch('https://digital-address.app/1000001')
if (res.ok) {
  const data = await res.json()
  const addr = data.addresses[0]
  alert(addr.pref_name + addr.city_name + addr.town_name)
} else {
  alert('住所が見つかりません')
}
```

---

## おわりに

本APIを使えば、フロントエンドからシンプルに郵便番号・デジタルアドレスの住所検索が可能になります。事業者のシステム開発やウェブフォームへの組み込みが一気に楽になるはずです。  
今後のアップデートでは、もっと細かい検索オプションの追加やサーバーレス対応サンプルも公開予定です。  
ぜひお試しください！

---

## PS

本番環境は固定IP必須なのつらすぎ～。ip range0.0.0.0/0は使えないし、固定IPは10個まででIPv6も使えない～  
サーバー環境の選択肢がなくなるよ～。GCPの無料e2-microに静的IP割り当てるしかないのか..きびしいって

> ※本記事で紹介した情報は2025年5月26日時点のものです。最新の情報は公式サイトやAPIドキュメントをご確認ください。

[0](https://qiita.com/relu/items/?utm_source=dlvr.it&utm_medium=twitter#comments)

コメント一覧へ移動

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Frelu%2Fitems%2F9b8085e89c01bcf6d35a%3Futm_source%3Ddlvr.it%26utm_medium%3Dtwitter&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Frelu%2Fitems%2F9b8085e89c01bcf6d35a%3Futm_source%3Ddlvr.it%26utm_medium%3Dtwitter&realm=qiita)

[16](https://qiita.com/relu/items/9b8085e89c01bcf6d35a/likers)

いいねしたユーザー一覧へ移動

17