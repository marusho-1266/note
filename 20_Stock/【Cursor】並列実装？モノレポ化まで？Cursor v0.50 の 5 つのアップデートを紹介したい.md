---
title: "【Cursor】並列実装？モノレポ化まで？Cursor v0.50 の 5 つのアップデートを紹介したい"
author: "tetsuro_b"
source: "https://zenn.dev/kikagaku/articles/26c6d5e37ffccb"
published: 2025-05-11
created: 2025-05-15
tags:
  - cursor
  - ai-tools
  - article
  - practical
  - programming
---

# 【Cursor】並列実装？モノレポ化まで？Cursor v0.50 の 5 つのアップデートを紹介したい

こんにちは。  
株式会社キカガクの [@tetsuro\_b](https://x.com/tetsuro_b) です。

2025 年 5 月 10 日、 Cursor `v0.50` がリリースされました。

直近の Cursor のアップデートは細かい調整や、他の AI エディタにもある機能の輸入のような内容が多かったですが `v0.50`...、 **実に驚きのアップデート内容でした・・・！**

アップデート内容の紹介に入る前に、最近の AI エディタ界隈を振り返ると...

- **Copilot Agent がついに GA** 。王者 Micorsoft が AI エディタ界隈に満を持して参入
	- [https://code.visualstudio.com/updates/v1\_99](https://code.visualstudio.com/updates/v1_99)
- OpenAI が **Windsurf を 4,300 億円買収合意**
	- [https://www.nikkei.com/article/DGXZQOGN082HU0Y5A500C2000000/](https://www.nikkei.com/article/DGXZQOGN082HU0Y5A500C2000000/)

などなど、Cursor はこれからどう戦っていくんだ！？と不安な部分もありましたが、 `v0.50` で Cursor がまた一歩先に出る進化を遂げてくれました。

一部のアップデート内容は段階的なリリースであるため、 **まだ私も触れていない機能もありますが実際に触ったものに関しては検証＆感想も執筆** しておりますので、ぜひ最後までご覧ください！

## 1️⃣ Cursor が並列実装可能に！バックグラウンドエージェント登場！

今回のアップデートの大目玉はなんといってもこれでしょう！  
**Cursor 上から複数のタスクを並列してエージェントに実装を任せられます。**  
※ イメージ的には Devin が近い！

公式の [Changelog](https://www.cursor.com/ja/changelog) を日本語訳したものをそのまま貼り付けます。  
※ [Changelog](https://www.cursor.com/ja/changelog) にデモ動画がありますのでそちらでイメージ感をよりつかめると思います。

> 早期プレビューで段階的に展開中: カーソル エージェントがバックグラウンドで実行できるようになりました。  
> これにより、複数のエージェントを並行して実行し、より大きなタスクに取り組むことができます。エージェントはそれぞれ独自のリモート環境で実行されます。いつでもステータスを確認したり、フォローアップを送信したり、引き継いだりすることができます。

Cursor を活用する際のこれまでのセオリーは以下が主流だったかと思いますが、

- ✅ タスクは小さく分解
- ✅ そのうえで、タスクを順次実行

複数のエージェントを並列で実行できるようになるようなので、Cursor 活用のセオリーも今後大きく変わりそうですね！

### 実装から PR 作成まで自動でやれてしまうかも！？

公式の動画をよく見ると以下の情報がわかります。

![](https://storage.googleapis.com/zenn-user-upload/cc76e03b5392-20250510.png)

タスク依頼　→ タスクの並列実行、さらに PR の作成まで Cursor で実現できる？ようです！

とにかく早く触ってみたい...。  
てもとの Cursor に１日でも早く機能がリリースされるのが待ち遠しいですね！

## 2️⃣ モノレポ化はもう不要。Multi-root Workspaces に対応。

😩「 **AI をフル活用するためには AI が参照しやすいように関連するリポジトリは全部ひとつのリポジトリにまとめてモノレポ化したい...。でもその手間が...。** 」

弊社でもちょうど最近メンバー間で話し合っていた部分でした。  
モノレポ化はむずかしいので GitHub MCP や Claude Code MCP なんかを活用したらやりたいことはできそうなんじゃないかと...。

ところが、今回のアップデートで Cursor が **Multi-root Workspaces に正式対応したことによりこの悩みが解消されます！**

**もう Cursor 利用の前提があれば AI のためにわざわざモノレポ化を検討する必要はありません！**  
※ わたしは Devin についてはまだ活用できていないので Devin はモノレポがいいのかも？（わかりません。）

実際にわたしが開発を担当しているプロジェクトで Multi-root Workspaces の設定を行ったところ、 **ワークスペースに設定された異なるリポジトリ間をまるで人間のように縦横無尽に駆け巡り実装を行ってくれました。**  
※ これまでは Frontend, Backend のリポジトリが別れていたので FE → BE のロジックを一貫して AI が確認することが難しかった。

### Multi-root Workspaces とは？

わたしは今日まで知らなかったのですが VSCode には **関連するフォルダをひとつのワークスペースにまとめて作業ができる機能** があったようで、 `v0.50` から Cursor でもこの機能が使えるようになりました。  
※ Changelog には「インデックス化され、Cursorで利用できるようになります」とのニュアンスの文章が記載されているので、従来までも利用はできていたが思うようなパフォーマンスが発揮できていなかったということかもしれません。

### 設定方法

1. 「command + shift + N」 で Cursor で新規ウィンドウを立ち上げる
2. 「フォルダーをワークスペースに追加」を選択し、任意のフォルダを複数選択し追加  
	![](https://storage.googleapis.com/zenn-user-upload/6ae937798343-20250510.png)
3. 「名前をつけてワークスペースを保存」  
	![](https://storage.googleapis.com/zenn-user-upload/86a4c2b1051b-20250510.png)

上記の手順でワークスペースを作成することができます。

次回以降、同じワークスペースを開くには、3. で保存された `xxxxx.code-workspace` を Cursor にドラッグ＆ドロップすることで開くことができます。

### Cursor Rules は Multi-root Workspaces でどういった挙動になるのか

> .cursor/rules are supported in all folders added  
> [https://www.cursor.com/ja/changelog](https://www.cursor.com/ja/changelog)

んー、Changelog の説明ではあまりイメージがつかないので検証してみました。

ざっくり検証結果をまとめると以下のとおりです。

| RuleType | 適用されるケース |
| --- | --- |
| Always | 常に全てのリポジトリのルールが適用 |
| Auto Attached | 関連するリポジトリ内のファイルがコンテキストに追加された場合のみ適用 |
| Agent Requested | 会話内容に適したルールは常に全て適用（ただし不安定） |

以下詳細です。

#### RuleType: Always → ワークスペース内の Rules が常に全て適用される

- 例）以下のように Always の Rules を設定した場合、両方の Rules が適用される
- **リポジトリ A** ：語尾に「ワン」とつけてください
- **リポジトリ B** ：語尾に「パオーン」とつけてください
- ![](https://storage.googleapis.com/zenn-user-upload/2c28b339f6e4-20250510.png)

#### RuleType: Auto Attached → Cursor Rules が設定されているリポジトリ内のファイルに対して有効

- 例）リポジトリA に `*/spec.ts` の Cursor Rules が設定されている場合
- **リポジトリ A** の `*/spec.ts` をコンテキストに追加すると **Rules が適用される**
- **リポジトリ B** の `*/spec.ts` をコンテキストに追加すると **リポジトリ A のRules は適用されない**

#### RuleType: Agent Requested → Agent との会話に関連があれば作業するリポジトリは関係なく呼び出される

- 例）以下のように Agent Requested のルールを設定した場合、両方の Rules が適用される
- **リポジトリ A** ：💪 の絵文字をつけて会話する
- **リポジトリ B** ：🙆♂️ の絵文字をつけて会話する
- ![](https://storage.googleapis.com/zenn-user-upload/a91ef7b02481-20250511.png)
- ※ SQL うんぬんは関係ない部分なので無視してください。
- **⇒ （注意）ただ、リポジトリ A のルールだけが適用される場合も多かった！！！**

※ **RuleType: Manual** → 手動で Attach するものなので検証から除外

## 3️⃣ Tab キーがついにファイル境界を超えてサジェスト可能に！

Cursor といえば Tab キーを活用したコードサジェストが有力ですが、 **従来まではファイル内の編集候補に対してのみ** サジェストがされていました。  
それが `v0.50` でなんと **ファイルをまたいで関連する箇所についてもサジェストをしてくれるようになりました！**

[https://www.cursor.com/ja/changelog#:~:text=With this we've also added syntax highlighting to the completion suggestions](https://www.cursor.com/ja/changelog#:~:text=With%20this%20we%27ve%20also%20added%20syntax%20highlighting%20to%20the%20completion%20suggestions).

実際に以下のケースで試してみました。

何回か試したところ理想通りの挙動はなかなかしてくれないかも？という感触ではあったのですが、日常的に使っていくことで活躍してくれる気配はするので、実際に業務で使っていくのが楽しみです。  
※ その他、あまりいい検証方法が思い浮かばずでした🙇♂️

## 4️⃣ @codebase が復活。リポジトリ全部をチャットコンテキストに追加可能に。

`@codebase` の後継とも言える機能が追加されました。  
Cursor のチャット上で `@{ローカルリポジトリのフォルダ名}` と入力することでコンテキストに追加可能です。

![](https://storage.googleapis.com/zenn-user-upload/d2cea2894dde-20250511.png)

この機能を有効化するには「Cursor Settings > Features > Full folder contents」にチェックを入れる必要があります。

![](https://storage.googleapis.com/zenn-user-upload/082995574dbd-20250511.png)

### どれくらい Cursor が賢くなるのか？

実際の業務で使っているリポジトリでコンテキストへの追加有無で性能を比較してみました。

- **特定のロジックについて詳しく教えてもらう系の質問**
	- `@{ローカルリポジトリのフォルダ名}` をつけてもつけなくても関連する実装ファイルにはたどり着くので、回答の質に違いはない
- **プロジェクトのディレクトリ構成を出力するように依頼**
	- 自分が試した限りでは `@{ローカルリポジトリのフォルダ名}` をつけたほうがより詳細にディレクトリ構造が出力された

### 【注意】 サイズが大きすぎるリポジトリでは使えません

ファイル数？フォルダ数？もしくは Codebase Indexing の file Sync 数なのか何が上限の目安になのかはわかりません。

※ 少なくとも手元のプロジェクトでは Codebase Indexing が 1,400 前後のリポジトリでは NG で、1,800 前後のリポジトリでは OK だったので Codebase Indexing とは別の基準かと思われます。

サイズが大きすぎる場合は、以下のように ⚠️ マークがつくのでそこで判断が可能です。

![](https://storage.googleapis.com/zenn-user-upload/f8da64cbc6ee-20250511.png)

## 5️⃣ command + K がアップデート。ファイル全体も編集可能に。

従来の機能が「Edit Selection」で、「 `command` + `.`」で「Edit Full File」に切替えることで使用可能です。

![](https://storage.googleapis.com/zenn-user-upload/48c4e28d63ad-20250511.png)

### 単一ファイルの編集であれば Agent モードよりも倍早い

検証として 6,000 行前後のとあるファイル内の特定の変数名の書き換えを指示しました。

| 実行方法 | 所要時間 |
| --- | --- |
| Agent | 125秒 |
| command + K | 71秒 |

Agent モードでは行数が多すぎてそもそもファイル全体を読み込むのに手間取ったり、変更箇所も 30 箇所以上あったので「最初はひとつずつ編集 → 効率が悪いのでコマンドで編集 → 上手くいかなくて修正して再実行...」と 125 秒かかりました。

対して `command + K` では丁寧に **１箇所ずつ編集を行ったように見えたものの約半分の時間である 71 秒で終わりました！！**

変更範囲が単一ファイルに閉じる修正や実装であれば、今後は `command + K` が大活躍すること間違いなしです！ 