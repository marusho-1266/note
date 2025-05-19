---
title: "AIお任せでプログラム開発。ChatGPTの新機能｢Codex｣を試してみた"
source: "https://www.gizmodo.jp/2025/05/chatgpt_codex_handson.html"
author:
  - "[[mediagene Inc.]]"
published: 2025-05-18
created: 2025-05-19
description: "人生効率が変わる機能。2025年5月17日、ChatGPTに新機能｢Codex｣が追加されました。Codexは“ソフトウェア・エンジニアリング・エージェント”です。プログラムを開発するにあたって必要になるバグ修正・コードレビューといったタスクを自動で行なってくれます。実際に触ってみたのですが、かなりすごい。人間がしっかり開発の舵をとりつつも、｢そこは自分でやんなくてもいいか｣という作業を全部やっ"
tags:
  - "clippings #inbox"
---
- [グローバルナビゲーションへジャンプ](https://www.gizmodo.jp/2025/05/#globalNav)
- [フッターへジャンプ](https://www.gizmodo.jp/2025/05/#footer)

- 17,897
- かみやまたくみ
![AIお任せでプログラム開発。ChatGPTの新機能｢Codex｣を試してみた](https://media.loom-app.com/gizmodo/dist/images/2025/05/18/top.jpg?w=1280)

Image: かみやまたくみ

人生効率が変わる機能。

2025年5月17日、 [ChatGPTに新機能｢Codex｣が追加](https://www.gizmodo.jp/2025/05/openai_codex.html) されました。Codexは“ソフトウェア・エンジニアリング・エージェント”です。 **プログラムを開発するにあたって必要になるバグ修正・コードレビューといったタスクを自動で行なってくれます。**

実際に触ってみたのですが、かなりすごい。人間がしっかり開発の舵をとりつつも、｢そこは自分でやんなくてもいいか｣という作業を全部やってもらえます。とにかく手がかからない。

以下、どんな感じかご紹介します。 [マニュアル](https://platform.openai.com/docs/codex) が公開されているので、実際に使ってみたいという方はそちらも併せてご覧ください。

## 指示を出す。実作業はAI。あとはチェックするだけ

Codexは **コード共有・管理プラットフォーム｢GitHub｣との連携が前提** になっています。GitHubにアップしているデータとの接続が必須なので、｢GitHubなにそれ？｣という方はまずはそこから知識を入れる必要あり。

その代わり、ちょっとでも使った経験があるのであれば、試した瞬間に｢もう戻れない｣となる感じになっています。

![codex_main](https://media.loom-app.com/gizmodo/dist/images/2025/05/18/codex_main.jpg?w=640)

codex\_main

こちらが [メイン画面](https://chatgpt.com/codex/) 、ChatGPT（Web版）左カラムから飛べます。｢Codexに何をさせるか｣を入力するチャット欄と、現在進行中・処理済みのタスクが表示されます。

- [![時間を忘れさせるほどのサウンドを生み出すヘッドホン](https://cdn-content-production.cxpublic.com/fff739b346e80d3b3b9638747d2afd81f1327fba423102c54b3de5eb2cd80067.jpg)
	時間を忘れさせるほどのサウンドを生み出すヘッドホン
	時間を忘れさせるほどのサウンドを生み出すヘッドホン Sponsored by Bowers & Wilkins
	](https://www.gizmodo.jp/2025/04/bowerswilkins_px7s3.html?utm_source=voluntary&utm_medium=cxense-ad&utm_campaign=000000017b536594)

![suggested_tasks](https://media.loom-app.com/gizmodo/dist/images/2025/05/18/suggested_tasks.jpg?w=640)

suggested\_tasks

できることの1つ目が｢ **やるべきことの提案** ｣です。リポジトリ（GitHubに保存されているプログラムのソースコード群）を確認し、ミスや不整合がないかをチェック、修正すべき内容をまとめてくれます。

![explain_codebase](https://media.loom-app.com/gizmodo/dist/images/2025/05/18/explain_codebase.jpg?w=640)

explain\_codebase

**コードベース（プログラムのソースコード全体）を読んで、どういうプログラムなのか、どういう機能があるのか、改善できるところはどこなのか、といった要点をまとめる** こともできます。

チームでの開発を前提とした機能で、新しいチームメンバーに｢こういうの作ってるんだよ｣をさっと共有するためのものですが、個人開発でも現況全体を俯瞰できるのはとても便利です。

![auto_fixing](https://media.loom-app.com/gizmodo/dist/images/2025/05/18/auto_fixing.jpg?w=640)

auto\_fixing

目玉はやはり、 **プログラムの修正・機能追加などを自動でやってもらえること** です。

どこをどう修正するかは提案してもらえるうえ、 **提案された修正タスクを実行すると、具体的なプロンプトが入力された状態でチャットが始まります。** ほとんど｢ボタンをクリックしただけで修正が済む｣って感じですね。

![auto_add_function](https://media.loom-app.com/gizmodo/dist/images/2025/05/18/auto_add_function.jpg?w=640)

auto\_add\_function

｢あ、この機能ほしいな｣と思いついたら、ざくっと打ち込むだけで｢実装｣してもらえます。 **雑な指示でも** [**o3**](https://www.gizmodo.jp/2025/04/o3_4o_mini.html) **カスタムの高性能モデル｢codex-1｣がいい感じに解釈、広範な知識を生かしてエラーなく動作するようにコードを書き換えてくれます。**

思いつきはすぐに形になり、すぐに試せます。

![create_pull_request](https://media.loom-app.com/gizmodo/dist/images/2025/05/18/create_pull_request.jpg?w=640)

create\_pull\_request

修正や新機能は、 **右上の｢プッシュ通知｣＞｢PR（Pull Request）を作成する｣で、リポジトリに反映** できます。

- [![大阪・関西万博の目玉と確信。スイスパビリオンはテックな暮らしのヒントがあります](https://cdn-content-production.cxpublic.com/09ca302e6328260100d7fe97297ef58c199aa4be2845191358977b7ae16f1d9d.jpg)
	大阪・関西万博の目玉と確信。スイスパビリオンはテックな暮らしのヒントがあります
	大阪・関西万博の目玉と確信。スイスパビリオンはテックな暮らしのヒントがあります Sponsored by FDFA, Presence Switzerland
	](https://www.gizmodo.jp/2025/04/expo2025-switzerland.html?utm_source=voluntary&utm_medium=cxense-ad&utm_campaign=000000017b50578a)

![live_activity](https://media.loom-app.com/gizmodo/dist/images/2025/05/18/live_activity.jpg?w=640)

Codexに依頼した作業の進捗をiPhoneでチェックできる

内容やコードの長さにもよりますが、処理にはそれなりに時間がかかります。 **短いものでも1-2分、長いものだと10分** とか。

Advertisement

iPhoneにChatGPTアプリを入れている場合、ライブアクティビティで進捗を表示させられるのですが、かかる時間を考えると神対応だと思いました。間に別の作業ができるので、通知がきたら確認・対応とすることで時間を有効活用しやすいです。

## “ポチポチ片手間開発”は世界を変えそう

感覚的には｢ポチポチしながらたまにチャット入力するだけで開発が進む｣です。人間がやるのは大枠を決める・AIの提案/生成を承認する、だけ。誇張なしにそういう感じです。

リポジトリ作成済みを前提に作られている＝ノー知識・ゼロから開発用ではないのですが、 **ひな形を作ってファーストコミットしちゃえるなら（o3に相談しながらやるだけですけど…）、あとは片手間気味に開発を進められる** でしょう。

![codex_cursor](https://media.loom-app.com/gizmodo/dist/images/2025/05/18/codex_cursor.jpg?w=640)

codex\_cursor

**実際、この記事を書いている隣で、個人的に作ってるプログラムの開発が進んでいたりします。** 自分はちょっとしたコードを書く程度の人間なのであれですが、仕事で開発されている方だとめちゃくちゃ生かせる｢キラー機能｣な気がしますが、どうでしょうか？

Advertisement

Codexは今のところ、ChatGPT Pro/Team/Enterpriseプラン向けの機能ですが、追々Plus/Eduでも使えるようにするのを検討しているようです。強力すぎる｢o3 でDeep Research｣もあることを踏まえると、高価な上位プランに明確な価値が生まれてきているのではないかと思います。

- [![ソニーの新型ヘッドホン、従来チップから7倍以上の処理速度を実現。迫力が違う](https://cdn-content-production.cxpublic.com/f7fe56162c52c4897e5cf2831faac47518d26a0764a5473a7422bac2c61efcdc.jpg)
	ソニーの新型ヘッドホン、従来チップから7倍以上の処理速度を実現。迫力が違う
	ソニーの新型ヘッドホン、従来チップから7倍以上の処理速度を実現。迫力が違う Sponsored by ソニーマーケティング株式会社
	](https://www.gizmodo.jp/2025/05/sony-wh-1000xm6.html?utm_source=voluntary&utm_medium=cxense-ad&utm_campaign=000000017ba6d7e2)

Source: [OpenAI](https://platform.openai.com/docs/codex)

  

[“やる余裕がない”を解消。ChatGPTに次世代AIコーディングパートナー｢Codex｣が追加](https://www.gizmodo.jp/2025/05/openai_codex.html)

[いずれ、他の分野でもこういうツールが出てくるんでしょうか。2025年5月17日、OpenAIがAIエンジニアリングエージェント｢Codex｣を発表しました。C...](https://www.gizmodo.jp/2025/05/openai_codex.html)

[https://www.gizmodo.jp/2025/05/openai\_codex.html](https://www.gizmodo.jp/2025/05/openai_codex.html)

[![](https://media.loom-app.com/gizmodo/dist/images/2025/05/17/codex.jpg?w=240)](https://www.gizmodo.jp/2025/05/openai_codex.html)

[スクショからプログラムをまるごと生成。OpenAI純正ツール｢Codex CLI｣がおもしろそうなんだけど](https://www.gizmodo.jp/2025/04/openai_codex_cli.html)

[2025年4月17日、OpenAIがChatGPTのアップデートを発表しました。ChatGPTで新しくできるようになったことや、新AIモデルの紹介が主でしたが...](https://www.gizmodo.jp/2025/04/openai_codex_cli.html)

[https://www.gizmodo.jp/2025/04/openai\_codex\_cli.html](https://www.gizmodo.jp/2025/04/openai_codex_cli.html)

[![](https://media.loom-app.com/gizmodo/dist/images/2025/04/17/codex_1.jpg?w=240)](https://www.gizmodo.jp/2025/04/openai_codex_cli.html)

[OpenAIの新モデル｢o3｣｢o4-mini｣。賢くなりつつ｢エージェント化｣ってどういうこと？](https://www.gizmodo.jp/2025/04/o3_4o_mini.html)

[2025年4月17日、OpenAIが新たな高性能推論モデル｢o3｣とハイバランスモデル｢o4-mini｣を発表、ChatGPTに実装しました。それぞれどんなモ...](https://www.gizmodo.jp/2025/04/o3_4o_mini.html)

[https://www.gizmodo.jp/2025/04/o3\_4o\_mini.html](https://www.gizmodo.jp/2025/04/o3_4o_mini.html)

[![](https://media.loom-app.com/gizmodo/dist/images/2025/04/17/o3_o4_mini-1.jpg?w=240)](https://www.gizmodo.jp/2025/04/o3_4o_mini.html)[![](https://media.loom-app.com/gizmodo/dist/images/2025/05/13/IFDC12.jpeg?w=160&h=120)](https://www.gizmodo.jp/2025/05/intel-foundry-direct-connect.html)

[

### 王の帰還は近い? AI時代にむけ技術を研ぎ澄ましてきたインテル

](https://www.gizmodo.jp/2025/05/intel-foundry-direct-connect.html)[![](https://media.loom-app.com/gizmodo/dist/images/2025/05/16/HP4_05.jpg?w=160&h=120)](https://www.gizmodo.jp/2025/05/harry-potter-props.html)

[

### 元々は金属製だった? 「炎のゴブレット」をガチで作った人に話を聞いてきた

](https://www.gizmodo.jp/2025/05/harry-potter-props.html)[![](https://media.loom-app.com/gizmodo/dist/images/2025/02/20/tij_cordcat_main.jpg?w=160&h=120)](https://www.gizmodo.jp/2025/02/alpha_theta_cord_cat.html)

[

### “鍵盤”恐怖症でも作曲OK。『Chordcat』でコード進行の闇から解放されたよ

](https://www.gizmodo.jp/2025/02/alpha_theta_cord_cat.html)

## LATEST NEWS

- 	[![](https://media.loom-app.com/gizmodo/dist/images/2025/05/15/IMG_7346-1.jpg?w=640)](https://www.gizmodo.jp/2025/05/pixel-ai-feature-five.html)
	[
	### Pixelにしてよかった。目立たないけどありがたい4つの機能
	](https://www.gizmodo.jp/2025/05/pixel-ai-feature-five.html)
- 	[![](https://media.loom-app.com/gizmodo/dist/images/2025/05/16/marshmallow-test_main.jpg?w=640)](https://www.gizmodo.jp/2025/05/new_twist_on_the_marshmallow_test.html)
	[
	### 約束を守るカギは｢バディ｣。仲間の存在が、子どもの我慢を強くする？
	](https://www.gizmodo.jp/2025/05/new_twist_on_the_marshmallow_test.html)
- 	[![](https://media.loom-app.com/gizmodo/dist/images/2025/05/16/250516fluidbattery.jpg?w=640)](https://www.gizmodo.jp/2025/05/researchers-develop-a-soft-battery-that-has-the-consistency-of-toothpaste.html)
	[
	### これ、歯磨き粉じゃなくて｢バッテリー｣（開発中）。可能性、無限大っぽそう
	](https://www.gizmodo.jp/2025/05/researchers-develop-a-soft-battery-that-has-the-consistency-of-toothpaste.html)
- 	[![](https://media.loom-app.com/gizmodo/dist/images/2025/05/16/20250516-weleda-scalp-cleansing-01.jpg?w=640)](https://www.gizmodo.jp/2025/05/weleda-scalp-cleansing.html)
	[33 WANT](https://www.gizmodo.jp/2025/05/weleda-scalp-cleansing.html)
	[
	### 頭皮の｢もわっ｣は、これ1つでケア。爽快感が最高な頭皮クレンジング
	](https://www.gizmodo.jp/2025/05/weleda-scalp-cleansing.html)[![](https://media.loom-app.com/gizmodo/dist/images/2025/05/13/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%882025-05-1311.25.47.jpeg?w=160&h=120)](https://www.gizmodo.jp/2025/05/g-shock-gd-b500.html)

[

### 縦長コンパクト。G-SHOCKの新作はスマホ接続で健康の管理も

](https://www.gizmodo.jp/2025/05/g-shock-gd-b500.html)[![](https://media.loom-app.com/gizmodo/dist/images/2024/05/21/240521typhonmillet_01-1.jpg?w=160&h=120)](https://www.gizmodo.jp/2025/05/millet-typhon-50000-st-trek-pant-review-1.html)

[

### ハイスペ防水パンツを買って約1年半。普段使いもできるし、いろんな場面でマジ便利

](https://www.gizmodo.jp/2025/05/millet-typhon-50000-st-trek-pant-review-1.html)[![](https://media.loom-app.com/gizmodo/dist/images/2025/05/14/DSC09833_R-1.jpg?w=160&h=120)](https://www.gizmodo.jp/2025/05/xreal-eye.html)

[

### ARメガネ「XREAL One」にカメラがつきます。パーフェクト6DoFメガネになります

](https://www.gizmodo.jp/2025/05/xreal-eye.html)[![](https://media.loom-app.com/gizmodo/dist/images/2025/05/16/20250516-weleda-scalp-cleansing-01.jpg?w=160&h=120)](https://www.gizmodo.jp/2025/05/weleda-scalp-cleansing.html)

[

### 頭皮の「もわっ」は、これ1つでケア。爽快感が最高な頭皮クレンジング

](https://www.gizmodo.jp/2025/05/weleda-scalp-cleansing.html)[![](https://media.loom-app.com/gizmodo/dist/images/2025/05/15/switchbot.jpg?w=160&h=120)](https://www.gizmodo.jp/2025/05/switchbot-k20-plus-pro.html)

[

### お掃除だけじゃない。SwitchBotの家庭用ロボはおやつも運んでくれる

](https://www.gizmodo.jp/2025/05/switchbot-k20-plus-pro.html)

[MORE](https://www.gizmodo.jp/sale/)

![GIZMODO TV](https://www.gizmodo.jp/assets/pc/img/logo_gz-video.svg) ![](https://www.youtube.com/watch?v=ZwtE9caD-7M)![GIZMODO VIDEO](https://i.ytimg.com/vi/ZwtE9caD-7M/mqdefault.jpg) ![GIZMODO VIDEO](https://i.ytimg.com/vi/QyUW6_zka5Y/mqdefault.jpg) ![GIZMODO VIDEO](https://i.ytimg.com/vi/Q_7hhVrtELc/mqdefault.jpg) ![GIZMODO VIDEO](https://i.ytimg.com/vi/6-ryY8CY7nU/mqdefault.jpg)