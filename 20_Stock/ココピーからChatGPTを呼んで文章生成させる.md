---
title: "ココピーからChatGPTを呼んで文章生成させる"
author: "無駄と文化"
source: "https://blog.mudatobunka.org/entry/2025/04/01/100000"
published: 2025-04-01
created: 2024-05-16
tags:
  - programming
  - api
  - ai
  - chatgpt
  - javascript
  - browser
  - tool
  - article
  - practical
---

# ココピーからChatGPTを呼んで文章生成させる

ココピー (cocopy) というブラウザ拡張機能がある。

[chromewebstore.google.com](https://chromewebstore.google.com/detail/cocopy/ihnfodlbkhgjnbheemjhkjfkfglgbdgc)

Webページを見ていて URL やページタイトルなどをコピー＆ペーストしたくなったとき、ココピーを使うと思い思いの形式でクリップボードへのコピーができて便利だ。

ココピーについてはいろいろな人が紹介記事を書いているのでそれを読んでもらうのがいいと思う。

[blog.pokutuna.com](https://blog.pokutuna.com/entry/introduce-cocopy)

[motemen.hatenablog.com](https://motemen.hatenablog.com/entry/2022/02/cocopy-rich-text)

## ココピーは任意の JavaScript コードを実行できる

ココピーは現在見ているページの URL・ページタイトル・コンテンツを JavaScript コードでいい感じに整形してからクリップボードに突っ込めるツールだ。  
JavaScript コードはユーザーが好き勝手に書くことができる。 `fetch()` を使って外部 API を叩くこともできる。

というわけで ChatGPT (OpenAI API) を呼んでみようと思う。

## ココピーから OpenAI API を呼ぶ

OpenAI API はエンドポイント `https://api.openai.com/v1/chat/completions` に POST リクエストを投げるだけで動いてくれる。なので雑に `fetch()` を呼べば動く。

```javascript
({ content }) => {
  const endpoint = 'https://api.openai.com/v1/chat/completions';
  const apiKey = '{YOUR_OPENAI_API_KEY}';
  const payload = {
    model: 'gpt-4o',
    messages: [
      { role: 'system', content: '100字で要約してください' }, 
      { role: 'user', content },
    ],
  };

  return fetch(endpoint, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': \`Bearer ${apiKey}\`
    },
    body: JSON.stringify(payload)
  }).then(response => response.text());
}
```

ChatGPT にページを要約させた結果がクリップボードに入る。

### 前処理と後処理

実際には前処理と後処理をちゃんとやったほうがいい。HTML 文章をそのまま渡すと HTML タグやインデントの空白文字でトークンがかさんでクレジットを消費しまくってしまう。

```javascript
const parser = new DOMParser().parseFromString(content, 'text/html');
const paragraphs = parser.querySelectorAll('body p');
const article = Array.from(paragraphs)
  .map(p => p.textContent.trim().replace(/\s+/g, ' '))
  .filter(text => text.length > 0)
  .join("\n").slice(0, 10000);
```

`<p>` タグだけ取り出して空白をトリミングした。

次は後処理、API からは JSON 形式で結果が返るのでパースしてから結果の部分だけ取り出す。

```javascript
return fetch(endpoint, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': \`Bearer ${apiKey}\`
  },
  body: JSON.stringify(payload)
})
  .then(response => response.json())
  .then(data => data.choices[0].message.content);
```

ヨシッ！(雑すぎる)

## 作例

実際に遊んでみよう。

### 俳句を詠ませる

まずはページ内容を要約した俳句を詠ませてみる。

適当に洗濯のページを開く

俳句ココピー実行！

結果

```
デリケート　衣類のケアで　賢く洗う
```

素敵な俳句がクリップボードにコピーされる。

### 3行まとめ

ページをシェアするとき3行まとめがあると便利かも。

3行まとめココピー実行！

結果

```
洗濯機の「ドライ」って？「ドライコース」でスーツやニットなどデリケートな衣類を洗おう | Lidea(リディア) by LION
https://lidea.today/articles/003999

【3行まとめ】
・洗濯機のドライコースを解説  
・デリケートな衣類を優しく洗う方法  
・ドライクリーニングとの違いも注意
```

まとめてくれた。

### ブクマを任せる

はてなブックマークのタグとコメントを生成させれば、まったく脳みそを使わずにブクマが完了する。

ブクマココピー実行！

結果

```
[洗濯][家事][ライフスタイル] ドライコースの重要性がよく分かりました。お気に入りの服を守るために、洗濯表示を確認する癖がつきそうです。
```

そのままブクマする

ラクだなー。

## スクリプトとかプロンプトとか

今回使ったスクリプトの全文をここに置いておこうと思う。

- [ココピーからChatGPTを呼んで文章生成させる · GitHub](https://gist.github.com/todays-mitsui/b9bd574f6267f82f64aa51f6b20b48ab)

ココピーで実行するには事前に API キーの発行とクレジットチャージ (課金) が必要です。スクリプトの中の `{YOUR_OPENAI_API_KEY}` のところを各自で発行した API キーで置き換えてください。

## まとめ

遊んだ。楽しかった。 