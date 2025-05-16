---
title: "MVCの歴史と実装パターン分析"
source: "https://qiita.com/tshinsay/items/5b1724baf32b8b5113c2"
author: "Qiita"
created: 2025-05-16
updated: 2025-05-16
tags:
  - "#programming"
  - "#mvc"
  - "#architecture"
  - "#advanced"
  - "#concept"
---

# MVCの歴史と実装パターン分析

## 概要
MVCアーキテクチャの歴史的背景と発展過程を解説し、特にWebアプリケーションにおける「プッシュ型MVC」と「プル型MVC」の違いと特性を分析した記事。MVCの本質的理解を深める内容。

## MVCの発展過程

### 一層構造からの始まり
- 最初は全ての機能（表示・ロジック）を一箇所に集約
- プログラマーとデザイナーの専門性の違いや、頻繁なデザイン変更への対応が困難に
- コード管理と保守の観点から分割の必要性が発生

### 二層構造への移行（MV分割）
- 表示をView、ビジネスロジックをModelに分離
- 問題点：UIの条件分岐をどちらに配置するかで困難が発生
  - Viewに配置すると重複コードが増加
  - Modelに配置するとUIロジックで肥大化

### 三層構造の確立（MVC）
- ControllerがUIイベントを受け取り、意味のある操作に変換
- Controllerの責務：「低レベルUIイベントを意味のある大きなイベントに変換」
- 各要素の役割が明確化：
  - Model：ビジネスロジックの記述
  - View：Modelのデータを表示
  - Controller：Viewの入力を受け取り判断し、Modelを起動

## MVCの実装パターン

### プッシュ型MVC（Push-MVC / MVP / Passive View）
- ControllerがModelとViewの接続を担当
- ControllerがViewにModelの情報をプッシュ
- Viewは受動的な役割（Passive View）
- Webアプリケーションで一般的に使用される形式

### プル型MVC（Pull-MVC）
- ViewがModelを直接参照してデータを取得
- GUIアプリケーションでは一般的だが、Webでは実装が複雑
- HTTPの壁やステートレス性により実装が困難

## Webアプリケーションにおける変化

### HTML5による再定義
- クライアントサイドでのHTML操作が容易になり、View定義が変化
- 従来：(Browser)View -|Network|- (Routing)Controller - (Logic)Model
- HTML5後：(Browser)View - Controller -|Network|- (Logic)Model

### 今後の動向
- SPA（シングルページアプリケーション）ではプル型MVCの利点が増加
- SEOやコンテンツ中心のサイトではプッシュ型MVCも引き続き重要
- ハイブリッドアプローチも増加傾向

## まとめ
- MVCは単なるコード分割パターンではなく、アプリケーションの特性に合わせた設計思想
- Webアプリケーションではプッシュ型が主流だが、クライアントサイドの進化でプル型も増加
- アーキテクチャパターンの理解はフレームワーク選択や設計の指針になる 