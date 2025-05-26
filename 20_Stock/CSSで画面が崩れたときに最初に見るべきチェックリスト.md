---
title: "CSSで画面が崩れたときに最初に見るべきチェックリスト"
source: "https://qiita.com/SugarMilk23/items/83ebf8009359e94149b5"
author:
  - "[[SugarMilk23]]"
published: 2025-05-22
created: 2025-05-26
description: "こんにちは！今回は、CSSをいじっていて突然画面が崩れてしまったときの対処法について書いていきます。私も日々CSS書いていて「あれ？さっきまで正常だったのに...」ということがよくあります。そんなと…"
tags:
  - "clippings"
  - "programming"
  - "css"
  - "practical"
  - "reference"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

[@SugarMilk23](https://qiita.com/SugarMilk23)

投稿日

こんにちは！今回は、CSSをいじっていて突然画面が崩れてしまったときの対処法について書いていきます。私も日々CSS書いていて「あれ？さっきまで正常だったのに...」ということがよくあります。そんなときに慌てずに済むよう、まず最初にチェックすべきポイントをまとめました！

## はじめに：なぜ画面が崩れるのか？

CSSで画面が崩れる原因は本当に様々ですが、実は **基本的なミス** が原因であることが大半です。複雑なバグを疑う前に、まずは基本的な部分から確認していくのが効率的です。

この記事では、私の経験上よく遭遇する問題を優先順位の高い順に並べてチェックリストにしました。上から順番に確認していけば、ほとんどの問題は解決できるはずです！

## 【緊急度★★★】まず最初にチェックすべき項目

### 1\. 構文エラーがないか確認

**最重要ポイント！** CSSの構文エラーは画面崩れの最大の原因です。

#### チェック項目

- **セミコロン（;）の漏れ**
	```css
	/* ❌ NG：セミコロンがない */
	.container {
	  width: 100px
	  height: 50px;
	}
	/* ✅ OK */
	.container {
	  width: 100px;
	  height: 50px;
	}
	```
- **波括弧（{}）の対応**
- **コロン（:）の漏れ**
	```css
	/* ❌ NG：コロンがない */
	.box {
	  width 100px;
	}
	/* ✅ OK */
	.box {
	  width: 100px;
	}
	```

#### 確認方法

- ブラウザの開発者ツールでConsoleタブをチェック
- CSSファイルの最後に書いたスタイルから逆順に確認
- VSCodeなどのエディタの構文ハイライトを確認

### 2\. セレクタの書き間違い

クラス名やID名の書き間違いは頻発するミスです。

#### よくあるパターン

```css
/* HTML */
<div class="main-container">...</div>

/* ❌ NG：ハイフンとアンダースコアの間違い */
.main_container {
  width: 100%;
}

/* ❌ NG：スペルミス */
.main-containr {
  width: 100%;
}

/* ✅ OK */
.main-container {
  width: 100%;
}
```

#### 確認方法

- HTMLとCSSでクラス名・ID名が一致しているか
- 大文字小文字が正確か
- ハイフンとアンダースコアが正しいか

### 3\. CSSファイルが正しく読み込まれているか（CSSファイル読み込みの場合）

意外に見落としがちですが、そもそもCSSファイルが読み込まれていない場合があります。

#### チェック項目

```html
<!-- ✅ linkタグが正しく記述されているか -->
<link rel="stylesheet" href="style.css">

<!-- ❌ よくある間違い -->
<!-- hrefのパスが間違っている -->
<link rel="stylesheet" href="css/style.css">
<!-- relが間違っている -->
<link rel="stylesheets" href="style.css">
```

#### 確認方法

- ブラウザの開発者ツールのNetworkタブで404エラーがないか確認
- CSSファイルのパスが正しいか確認
- ファイル名にスペルミスがないか確認

## 【緊急度★★】次にチェックすべき項目

### 4\. CSSの優先順位（詳細度）の問題

複数のCSSルールが競合している場合、意図しないスタイルが適用されることがあります。

#### よくあるパターン

```css
/* 詳細度が低い */
.button {
  background-color: blue;
}

/* 詳細度が高い（後から書いても青が適用される） */
div .button {
  background-color: red;
}
```

#### 確認方法

- 開発者ツールのElementsタブで、適用されているスタイルを確認
- 取り消し線が引かれているプロパティがないかチェック
- より具体的なセレクタで上書きされていないか確認

### 5\. ボックスモデルの理解不足

`width` や `height` の計算方法を間違えて理解していることがあります。

#### チェック項目

```css
/* デフォルトのbox-sizing: content-box */
.box {
  width: 200px;
  padding: 20px;
  border: 1px solid black;
  /* 実際の幅 = 200px + 40px(padding) + 2px(border) = 242px */
}

/* box-sizing: border-boxを使う場合 */
.box {
  box-sizing: border-box;
  width: 200px;
  padding: 20px;
  border: 1px solid black;
  /* 実際の幅 = 200px（padding, borderを含む） */
}
```

#### 確認方法

- 開発者ツールでボックスモデルを視覚的に確認
- `box-sizing` プロパティの値を確認
- 親要素からはみ出していないか確認

### 6\. Flexboxの設定ミス

Flexboxを使用している場合の設定ミスも多いです。

#### よくある問題

```css
/* ❌ NG：親にdisplay: flexがない */
.item {
  flex: 1; /* 効果なし */
}

/* ✅ OK */
.container {
  display: flex;
}
.item {
  flex: 1;
}
```

#### チェック項目

- 親要素に `display: flex` が設定されているか
- `flex-direction` の方向が意図通りか
- `justify-content` と `align-items` の設定が正しいか

## 【緊急度★】その他のチェック項目

### 7\. z-indexの重なり順序

要素が意図しない順序で重なっている場合があります。

#### チェック項目

```css
/* position: staticの要素にz-indexは効かない */
.element1 {
  z-index: 999; /* 効果なし */
}

/* positionを指定する必要がある */
.element2 {
  position: relative;
  z-index: 999; /* これなら効く */
}
```

### 8\. レスポンシブ対応の問題

メディアクエリの設定ミスや、ビューポートの設定忘れ。

#### チェック項目

```html
<!-- head内にviewportの設定があるか -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

```css
/* メディアクエリの書き方 */
@media screen and (max-width: 768px) {
  /* スマホ向けのスタイル */
}
```

### 9\. フロート関連の問題

`float` を使用している場合のクリアフィックス忘れ。

#### チェック項目

```css
/* フロートをクリアする */
.clearfix::after {
  content: "";
  display: table;
  clear: both;
}
```

### 10\. 単位の間違い

`px` 、 `%` 、 `em` 、 `rem` などの単位を間違えている場合があります。

#### よくある間違い

```css
/* ❌ 単位を忘れる */
.box {
  width: 100; /* 効果なし */
}

/* ✅ 正しい */
.box {
  width: 100px;
}
```

## 効率的なデバッグ方法

### 開発者ツールを活用する

1. **要素の検証** ：右クリック → 「検証」で該当要素を選択
2. **スタイルの確認** ：Stylesパネルで適用されているCSSを確認
3. **リアルタイム編集** ：開発者ツール上で直接CSSを編集して確認
4. **ボックスモデルの確認** ：Computedタブでmargin、padding、borderを視覚的に確認

### コメントアウト法

問題の範囲を特定するために、CSSを部分的にコメントアウトして原因を絞り込みます。

```css
/* 問題がありそうな部分をコメントアウト */
/*
.problematic-style {
  position: absolute;
  top: 0;
  left: 0;
}
*/
```

### 段階的な確認

1. 最後に編集した部分から確認
2. 全体的な崩れなら、全体のレイアウト（グリッド、フレックスボックス）を確認
3. 部分的な崩れなら、該当箇所のスタイルを確認

## 予防策：崩れにくいCSSの書き方

### 1\. CSS設計を意識する

- BEMやFLOCSSなどの設計手法を取り入れる
- クラス名に意味を持たせる
- ネストを深くしすぎない

### 2\. リセットCSSを使用

ブラウザ間の表示差異を最小限にするため、リセットCSSやNormalize.cssを使用する。

### 3\. ベンダープレフィックス

必要に応じてベンダープレフィックスを追加する（または自動化ツールを使用）。

### 4\. 定期的な確認

- 複数のブラウザでの表示確認
- 異なるデバイスサイズでの確認
- 定期的なコードレビュー

## まとめ

CSSで画面が崩れたときは、慌てずに以下の順序で確認しましょう：

1. **構文エラー** （セミコロン、括弧、コロンの確認）
2. **セレクタの書き間違い**
3. **CSSファイルの読み込み確認**
4. **CSS優先順位の問題**
5. **ボックスモデルの理解**
6. **Flexboxの設定**

ほとんどの場合、これらの基本的な確認で問題は解決できます。複雑なバグを疑う前に、まずは基本から確認する習慣をつけることが大切です。

開発者ツールを使いこなして、効率的にデバッグできるようになりましょう！皆さんのCSS開発がスムーズになることを願っています。

何か質問や「こんなケースもあるよ！」というコメントがあれば、ぜひ教えてください！

[0](https://qiita.com/SugarMilk23/items/#comments)

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2FSugarMilk23%2Fitems%2F83ebf8009359e94149b5&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2FSugarMilk23%2Fitems%2F83ebf8009359e94149b5&realm=qiita)

[5](https://qiita.com/SugarMilk23/items/83ebf8009359e94149b5/likers)

6