---
title: "大規模コードベースをAIの業務知識に！作ってわかったMCPサーバー使いこなし術"
author: "MonotaRO Tech Blog"
source: "https://tech-blog.monotaro.com/entry/2025/05/20/090000"
published: 2025-05-20
created: 2025-05-20
tags:
  - ai
  - programming
  - practical
  - advanced
  - article
---

# 大規模コードベースをAIの業務知識に！作ってわかったMCPサーバー使いこなし術

## TL;DR

社内の開発情報にアクセスするMCPサーバーを作成して、AI開発ツールが業務知識を活用できるようにしてみた。

具体的なツール事例（DBスキーマ参照、コード検索など）と、AIに活用させるための命名、レスポンス、権限などの考え方とコツを紹介。

## はじめに

MonotaROでは、[ドメインモデリングでリアーキテクチャに挑む](https://levtech.jp/media/article/interview/detail_545/)と同時に、[モノタロウのAI駆動開発の全貌をご紹介します](https://tech-blog.monotaro.com/entry/2025/04/24/091000)のようにAIツールを活用しています。また、[マイクロサービステンプレート](https://tech-blog.monotaro.com/entry/2024/06/25/090000)を活用するため開発ツールを社内に提供して、それを支援しています。

今回、社内の開発ツールにMCPサーバー機能を追加してAI開発ツールから利用できるようにしたところ、社内の業務知識を活用してAI開発ができるようになりました。

なお、本記事は[モノタロウのAI駆動開発の全貌をご紹介します](https://tech-blog.monotaro.com/entry/2025/04/24/091000)の社内のMCPツールに関する内容となっています。

## MCPとは

Model Context Protocolの略で、prompt, resource, toolを通じてAIエージェントに機能を提供することができます。  
Anthropicがオープンソースで[公式ドキュメント](https://modelcontextprotocol.io/)を提供しており、現在ではVSCode Copilot、Cline、CursorといったAI開発ツールで利用可能です。プロトコルはJSON-RPCで、一般にトランスポートにはstdioまたはSSEが利用されます。

MCPクライアントであるAI開発ツールは、まずMCPサーバーからツールの一覧を取得してメタデータを読み込みます。  
AI開発ツールのエージェントがそのツールを使うべきだと判断した場合に、ツールが呼ばれ、結果がプロンプトとしてエージェントの会話に反映されます。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/y/y_fujimoto/20250516/20250516123246.png)

たとえば、以下のような流れでツールが使われます：

1. ユーザー：「商品の在庫データの構造を教えて」
2. AIエージェント：(内部で判断)「このクエリには業務知識が必要だ。search-local-filesツールを使おう」
3. AIエージェント： `search-local-files(query="在庫", path="/path/to/src")` を呼び出す
4. MCPサーバー：検索を実行し、関連するコードと内容のリストを返す
5. AIエージェント：ツールから受け取った内容を分析し、ユーザーに回答する

このようにして元々AIが知り得ない情報を扱うことができる反面、ユーザーが直接利用するものではないので、いくつか考慮すべきポイントがあります。それらを社内での事例を通じて説明します。

## MonotaROでのMCP

社内の開発ツールがGoで作成されており、整備されたインストール手順と活用実績があったので、そこにMCPサーバー機能を追加しました。フレームワークには[mcp-go](https://github.com/mark3labs/mcp-go)を使いました。実装例については記事の最後に紹介しているのでそちらをご参照ください。

## DBスキーマを使ったコード生成

社内のDBチームがGitHubにDBスキーマを同期しているので、それをAIエージェントから参照できるようにするため`lookup-database-information`というツールをMCPサーバーに組み込みました。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/y/y_fujimoto/20250516/20250516123233.png)

[GitHubの公式MCPサーバー](https://github.com/github/github-mcp-server)を使うことでも可能でしたが、GitHubリポジトリの構成を都度プロンプトで説明する必要がなくなり、DBスキーマ情報を元にモデルやテストといったコードを生成することが容易になりました。

## 既存ソースコードからの業務知識抽出

MonotaROのレガシーなシステムにはソースコードがUTF-8ではないものがあり、GitHub検索にヒットしないため分析が難しい問題がありました。古典的なnkfやgrepの組み合わせで検索させることもできますが、結局catして「文字化けしていて読めないようです」のようなレスポンスになってしまうことがしばしばありました。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/y/y_fujimoto/20250516/20250516123242.png)

そこで、`search-local-files`と`cat-local-file`というツールを作成し、例えば"在庫"のようなキーワードを検索して関係するファイルから網羅的に業務知識を抽出できるようにしました。やや強引ではありますが上の画像のようにツール名を指定することでAIが利用するようになり、関連する概念を拾ってくれることがわかります。

MonotaROのシステムでは"商品"や"在庫"といってもドメインによって関心ごとが異なるのですが、コメントの周辺のコードから違いを見つけてくれるのでちょっとしたDeepResearchのような仕様調査ができるようになりました。

## 共通モジュールの使用例の検索

GitHub検索にパラメータを追加して、絞り込んだ結果を返す`search-code-in-github`を作りました。社内共通モジュールの使用例をAIエージェントが調べて取り入れることで活用が進みます。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/y/y_fujimoto/20250516/20250516123237.png)

こちらも公式MCPサーバーでも可能ではありますが、`org:`や`language:`のようなGitHub検索固有の絞り込みをあらかじめツールのパラメータとしておくことで、検索範囲の絞り込みとトークン数の節約ができます。ツールのレスポンスが大きすぎるとコンテキストウインドウを圧迫するので、適度な制約の下でAIエージェントが試行錯誤して正しい結論を導き出す、という面もあります。

また、Copilotの場合は組織の設定で[パブリックコードに一致する候補の無効化](https://docs.github.com/ja/copilot/managing-copilot/managing-copilot-as-an-individual-subscriber/managing-your-copilot-plan/managing-copilot-policies-as-an-individual-subscriber#enabling-or-disabling-suggestions-matching-public-code)をしている場合もあると思います。そのような場合にもあらかじめ制約を入れておくことで不必要な参照を回避することができます。

## MCPツール実装のコツ

典型的なReActエージェントではAIがツールの利用を判断し、ツールの出力がプロンプトに追加されます。MCPツールもAI開発ツールが「使うべきだ」と判断しないと利用されないですし、ツールが返した情報がプロンプトへの回答に有用でないと参考にしてくれないこともあります。

いくつか、試行錯誤していく中でわかってきたコツを紹介したいと思います。

## プロセスの状態に依存せずツールを利用できるようにする

MCPサーバーのプロセスはstdioでは標準入出力を通じてクライアントとやりとりします。AI開発ツールはIDEに組み込まれていてワークスペース内のファイルを参照できますが、MCPサーバーのプロセスの状態までは分からないので、なるべくAIから受け取ったパラメータで動作することが望ましいと言えます。

MCPの仕様にある[Roots](https://modelcontextprotocol.io/docs/concepts/roots)を使うか、パラメータで対象の絶対パスを渡すようにするなどすると良いでしょう。

## できるだけ具体的なツール名をつけてパラメータを修飾する

AIにツールを使うべき状況を判断させるために、動詞と目的語を組み合わせて、使うべき場面を明確に指示することが重要です。ツール名を指定して使わせることもできるのですが、その場合でもツールのパラメータと目的の関連性が明確である方が、より活用されやすくなります。

以下は、実際に社内のMCPツールで使っているサンプルです。

例:

```
- ツール名: search-local-files
- 説明: Search text in the local files recursively regardless text encoding.
- パラメータ: 
  - query: Search query. Full-keyword, case-sensitive, exact match only.
  - path: Absolute path to search in
  - extension: File extension to search in. Default is all files
```

こうすることで、「ローカルファイルを検索する」という目的が明確になり、AIエージェントが適切なタイミングでツールを選択できます。また、パラメータの説明も具体的であることが重要です。例えば「完全一致のみ」という制約を示すことで、AIが不適切な検索クエリを送信するリスクを減らせます。

同様に、データベース情報を参照するツールであれば、

```
- ツール名: lookup-database-information
- 説明: Lookup database information. If table name is not specified, returns a list of tables and comments. Otherwise, returns the DDL of the specified table.
- パラメータ:
  - database_schema: Database schema name (enum: "order", "products", "web", "customer")
  - table_name: Table name (Optional)
```

このように、アノテーション等でツールの機能とパラメータが明確に定義されていれば、AIエージェントは適切なタイミングでツールを選択し、必要なパラメータを正しく設定することができます。

## レスポンスとしてinformativeなフィードバックを返す

AIはツールを使うべきか判断しますが、結果をきちんとレスポンスに含めないとプロンプトを通じて察知できません。ReActエージェントではツールのレスポンスから次のアクションを決定するので、成功/失敗によらず有益な情報を返すことでツールがより活用されるようになります。

例えば、検索ツールのレスポンスを考えてみましょう。

悪い例:

```
Not found.
```

良い例:

```
Query XXX: No match found in YYY files.
```

この場合、後者の方が「クエリの結果が見つからない」という事実をより明確に伝えているので、AIがクエリを変えてリトライする可能性が高くなります。

## (なるべく)実行ユーザーの権限を使う

MCPサーバーは通常のプロセスなので、使える権限は一般的なコマンドラインツールと同じく、

- コマンドライン引数
- プロセスの環境変数
- home以下のconfig等

が主な取得元になります。

一般的に、配布するバイナリやイメージに個人のTokenを埋め込むのは好ましくないですし、システムの環境変数にするのも避けた方が良いでしょう。

たとえば、GitHubのPATやService Accountのクレデンシャルを起動時の環境変数で渡すのは一般的なプラクティスです。あるいは、`gh auth token`のようなコマンドで取得する方法もあります。

MCPツールを考える上で、「手元のコマンドラインで実行できるか？」を目安に考えると現実的だと思います。

## まとめ

MCPサーバーを作ってみたことで、AI開発における業務知識の活用に向けた道筋が見えてきました。これらのコツを押さえつつ、試しながらMCPツール向きかどうかを判断することで「MCPなら何でもできるんじゃないか？」といった過度な期待にならずに実践的なAIエージェントの活用を進めることができます。

より詳細な実装に興味のある方は、個人的に開発している[go-dev-mcp](https://github.com/fpt/go-dev-mcp)にここで説明した内容の一部が含まれているので、参考になれば幸いです。

モノタロウでは随時エンジニアの採用募集をしております。この記事に興味を持っていただけた方や、モノタロウのエンジニアと話してみたい！という方は[カジュアル面談 登録フォーム](https://docs.google.com/forms/d/e/1FAIpQLSe2j2qezz_R-SeKGv9cB7YmEnHNhm6RF3tmnnBirRY0H22XEw/viewform)からご応募お待ちしております。

---

追記: 悪意あるMCPサーバーを利用してしまうと情報漏洩のリスクがあります。 