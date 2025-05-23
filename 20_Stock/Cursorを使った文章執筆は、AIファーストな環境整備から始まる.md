---
title: Cursorを使った文章執筆は、AIファーストな環境整備から始まる
source: https://honeshabri.hatenablog.com/entry/cursor_markdown_ecosystem
author:
  - "[[本しゃぶり]]"
published: 2025-03-23
created: 2025-05-15
description: Markdown形式での情報一元管理と音声入力、そしてCursorというAIエディタの組み合わせで、執筆環境が一変した体験を共有。AIが働きやすい環境整備こそが、知的生産性を飛躍的に高める鍵
tags:
  - cursor
  - productivity
  - ai-tools
  - article
  - practical
association:
---
当ブログではアフィリエイト広告を利用しています

なぜCursorを使うと執筆が捗るのか？  
それはAIファーストな環境では、自律的に情報を探索してくれるからだ。

執筆のパラダイムシフトは既に始まっている。

## 文章執筆でAIエディタを活用するには

最近、CursorなどのAIエディタによる文章執筆が注目を集めているが、 **「実際にどう使えば執筆が捗るのか」** というイメージが湧かない人も多いだろう。いくら便利だと言われても、具体的な活用法が見えなければ結局は普通のエディタとの違いが分からない。ではどうしたら執筆に活用できるのか。

俺自身はこの2年間、AIを文章執筆に活かす方法を模索してきた。そしてようやく **3つの要素** が揃ったことで執筆環境が一変したと確信した。

1. EvernoteからObsidianに移行し、すべての情報をMarkdown形式で一元管理
2. 音声入力でアイデアを一気に吐き出し、AIに修正・整理させる手法
3. Cursorの登場により、Markdownで管理された情報とAIを直接連携させる環境

Cursorを使い始めたことで、これまで別々に使っていたツールが一つの環境に統合され、 **シームレスな執筆プロセス** が実現した。AIエディタは単なる「チャットができるテキストエディタ」ではない。知的生産の中心となる **ハブ** なのだ。

ではどのような環境が構築され、どんな体験をもたらすのか。具体的なプロセスを見ていくことで、この新しい執筆スタイルの真価が見えてくるはずだ。

## Markdownを軸とした情報と執筆の流れ

まず、俺の執筆環境の全体像を見てほしい。情報の入力から管理、そして最終的な執筆出力までの流れを図解するとこうなる。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/h/honeshabri/20250323/20250323212638.png)

このマーメイド図もCursorで作った

様々なノードと矢印があって複雑に見えるが、注目すべきは結束点である **Obsidian** と **Cursor** だ。この2つが執筆で重要な役割を果たしているので、それぞれの役割を見ていく。

## 情報収集と一元管理: Obsidianに情報をストックする

執筆を始める前に、まずは情報を効率的に集める仕組みが必要だ。俺のワークフローでは、Obsidianが全ての情報を集約する **ストックヤード（集積地）** の役割を担っている。

日常生活のあらゆる場面で情報を収集し、それをすべてMarkdown形式でObsidianに集約する。具体的には次のような情報だ。

- 音声入力からのメモや文章（SuperWhisperとChatGPTで整形）
- Web記事（Web Clipperで保存）
- Kindle書籍の抜粋（Notebook LMで重要部分を抽出）
- 調査した情報（Deep Researchで収集しMarkdown化）
- PDF文書（Marker PDF Converterで変換）

こうして集めた情報はすべてMarkdown形式に変換され、iCloud Drive経由でデスクトップとモバイルの両方から同じ保管庫にアクセスできる。これにより、場所を選ばず思いついたことをすぐに記録できる環境が実現した。

そしてこれを参照する形で、Cursorの出番となる。

## 執筆環境の統合: Cursorを執筆の中心に置く

Obsidianに集約した情報を基に、実際の執筆はCursorで行う。ここで威力を発揮するのが、Cursorの **マルチルートワークスペース** 機能だ。俺は以下のように設定している。

```
ワークスペース設定例：
- メインフォルダ: ~/GitHub/Articles/hatenablog-Article（Git管理）
- 追加フォルダ: ~/iCloud Drive/Obsidian Vault（非Git管理）
```
![](https://cdn-ak.f.st-hatena.com/images/fotolife/h/honeshabri/20250323/20250323212734.png)

Obsidianの保管庫を丸ごと入れてある

この設定により、以下のような統合された執筆環境が実現できる。

1. **Obsidianの情報への完全アクセス**  
	CursorでObsidianのすべてのノートを自由に探索・参照・編集できる。これまで溜め込んだ情報が有効活用される。
2. **Gitによる変更履歴の管理**  
	執筆過程のすべての変更がGitHubで追跡され、いつでも過去のバージョンに戻れる。原稿の複数のバージョンを維持しながら、安心して大胆な編集を試せる。
3. **iCloudによるモバイル環境との連携**  
	iCloud Drive経由でObsidianをモバイル端末と同期。iPhoneのショートカット機能との連携により、外出先で思いついたアイデアを即座に執筆環境に取り込める。

これだけだと、普通のVSCode + ChatGPTと変わらないと思うかもしれない。もちろんCursorのようなAIエディタの価値は、単なる文章校正や言い回しの改善だけではない。搭載されたAIエージェントの真価は、 **自律的なサポート** を提供してくれることだ。

- **自律的なファイル探索とコンテキスト理解**  
	「先週書いたあのアイデアを探して」という曖昧な指示だけで、AIが自ら適切なファイルを見つけ出す。しかも単に発見するだけでなく、現在の執筆コンテキストに合わせて情報を統合してくれる。
- **複数ファイルを横断した自動処理**  
	「全ての記事のリンクを集めて一覧表を作成して」といった指示で、AIがディレクトリ全体を走査し、関連情報を抽出・整理する。この作業を手動でやるとなると数時間かかるところだ。
- **ファイル構造の自律的な整理**  
	大量の情報やノートをテーマ別に自動分類し、適切なディレクトリ構造を提案・実行してくれる。ファイルの作成、移動、リネームまでAIが一貫して行えるため、情報管理が格段に楽になる。
![](https://cdn-ak.f.st-hatena.com/images/fotolife/h/honeshabri/20250323/20250323215620.png)

Obsidianのインデックスノートを作ってと依頼した

このようにAIエディタを活用することで **「情報収集→構造化→執筆→管理」** というプロセスが一つの環境で完結する。これまでの執筆環境では考えられなかった柔軟性と効率性が実現するのだ。

理論だけではピンとこないかもしれない。では、実際に俺がこのシステムを使って文章を書くとき、どのような体験をしているのだろうか？ 具体的なプロセスを見ていくことで、この新しい執筆スタイルの価値が見えてくるはずだ。

## 実際の執筆プロセス

では、具体的な執筆の流れを時系列で見ていこう。例えば、この記事を書く際には以下のステップを踏んだ。

1. **準備フェーズ（Obsidianで情報収集）**
	- 関連する過去記事をObsidianから検索
	- 執筆テーマについての考えを音声入力し、Markdown化
	- CursorとObsidianの連携について調べた情報を整理
2. **発想・構造化フェーズ（AIの助けを借りる）**
	- 音声入力した雑多なアイデアをAIに整理させる
	- 全体の構成案をAIに提案してもらい、調整する
	- 重要なポイントを箇条書きでまとめる
3. **執筆フェーズ（Cursorで本格的な執筆）**
	- Cursorを起動し、マルチルートワークスペースでObsidianを追加
	- AIの支援を受けながら音声入力を使って文章を一気に書き出す
	- 従来のように頭から順に書くのではなく、全体を一度に作成し、後から調整していく
4. **編集・洗練フェーズ（AIによる推敲）**
	- 文体の一貫性や論理展開をAIにチェック
	- 必要に応じて追加情報をObsidianから検索し反映
	- GitHubにコミットして変更履歴を残す
![](https://cdn-ak.f.st-hatena.com/images/fotolife/h/honeshabri/20250323/20250323214631.png)

この記事も音声入力スタート

具体的なAIの使い方としては、次のような指示を行うことが多い。

- **構成案の生成：** 「以下はまだまとまっていない記事のアイデアで、これを整理し、核となるテーマと構成案を提案して」
- **初稿の作成：** 「記事用に喋ったので、これを誤字脱字直して整理して構造化して、読みやすい形式の記事にしてほしい」
- **文章の洗練：** 「 `writing_style.md` を読み直し、より正確に記事で書くべき文体に近づけて」
- **客観的な意見：** 「 `structure.md` と `target_audience.md` を読み直し、その上でこの原稿を読んで、編集者視点での意見がほしい。どうしたらより良い原稿になるか」

こうした指示により、AIは自分だけの編集者として、執筆の始まりから終わりまで完璧にサポートしてくれる。おかげで **「執筆の質」** が飛躍的に向上した。

これまでは書くことに多くの時間とエネルギーを費やしていたため、完成後の見直しや修正に十分な余力が残っていなかった。しかし、AIのおかげで素早く文章を仕上げることができ、その後の **「ブラッシュアップに注力できる」** ようになった。AIの力を借りることで、質の向上に集中できる環境が整ったのだ。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/h/honeshabri/20250323/20250323213535.png)

Vibe Writingの様子

ただし、ここで紹介した魔法のような執筆プロセスにも、実は **「隠された前提条件」** がある。どんなに優秀な料理人でも、整理された食材と調理器具がなければ腕を振るえないように、AIも適切な環境が整っていなければその能力を発揮できない。

では、AIが最高のパフォーマンスを発揮するために、いったい何が必要なのか。

## AI活用の鍵は「下準備」にある

AIエージェントが自律的に動き回り、複数のファイルを自由に参照・編集・管理するために必要なのも、それは **「下準備」** だ。具体的には以下のような環境整備となる。

- **情報が一元化されていること**
	- すべての情報がObsidianなど一箇所にまとまっていること。情報が散乱するとAIの行動が制限されてしまう。
- **情報がMarkdown形式で統一されていること**
	- AIが最も効率的に扱えるのは、シンプルで構造が明確なMarkdown形式だ。複雑なExcelやPDFと違い、AIが自然に理解し、自律的に作業することが可能になる。

Markdown形式での情報の一元管理という「下準備」が整って初めて、CursorのAIエージェントはその本来の能力を最大限に発揮する。つまり、AI活用の鍵はツールそのものよりも、 **情報管理の方法** にあるのだ。これが **AIファースト** な文章執筆の真髄である。

ゆえに文章執筆でAIエディタを最大限に活用したいなら、以下の3つのステップを実践してほしい。

1. **情報をMarkdown形式で一元管理する環境を整える**  
	ObsidianのようなMarkdownエディタを導入し、あらゆる情報を統一フォーマットで管理しよう。
2. **AIエディタとMarkdownベースの情報を連携させる**  
	マルチルートワークスペースやMCPなどの機能を使い、情報の参照と執筆を一つの環境に統合しよう。
3. **「書いてからAIで整理する」執筆法を試してみる**  
	完璧を求めず、まずはアイデアを音声などで吐き出し、AIの力を借りて整えていく習慣をつけよう。

これでAIエディタは、あなたの執筆活動を最大限サポートしてくれるはずだ。

## おわりに

冒頭でAIエディタについて「実際にどう使えば執筆が捗るのか」という問いを投げかけた。その答えは明確である。 **「AIが働きやすい環境を用意してやること」** だ。IT業界ではMarkdownが当たり前でも、一般的なビジネス現場ではWordやExcelが依然として主流である。しかし、これらのフォーマットはAIとの親和性でMarkdownに大きく劣る。今後、AIとの協業が当たり前になる時代において、この差は明確な **「競争優位性」** へと変わっていくだろう。今日からでもMarkdownを学び始めれば、将来的なAI活用において他者より一歩先を行ける。

このMarkdownという共通言語を介して、人間とAIが共同で知的生産を行う時代はすでに始まっている。この流れに乗るか乗らないかで、今後の知的生産における効率と質に大きな差が生まれるのではないか。俺自身、この新しい執筆スタイルを通じて、 **創造性と生産性の両立** という、かつては相反すると思われていた目標を、今まさに実現しつつある。

振り返ってみると、学生時代に **「使えるとかっこよさそう」** という理由で始めたMarkdownが、1年前のEvernoteからObsidianへの移行を経て、今やAIとの共同作業における共通言語へと進化した。この偶然の出会いが、 **「AIファースト」** な環境を構築できた背景だ。

## 執筆手法・環境の模索の過程

この2年間の変遷を代表的な記事で振り返る。

### 2023年

構成を考えるのにChatGPTを活用した。

  

一気に書き出してからAIに修正してもらう方法を試す。

### 2024年

EvernoteからObsidianへ移行した。

  

Claudeを文章執筆に本格的に活用。

  

2024年10月時点のまとめ。

### 2025年

音声入力に手を出した。

## README

以下、投げ銭のコーナー。お礼の代わりとして、俺がCursorのAIに向けて書いた `README.md` を共有する。プロジェクトルールとして、AIはこれに従って動いている。

[音声入力と生成AIの組み合わせが強すぎて… »](https://honeshabri.hatenablog.com/entry/talk2ai)