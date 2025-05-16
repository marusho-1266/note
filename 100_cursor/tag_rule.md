# Obsidian タグ付けルール

以下のルールに従ってCursorがObsidianのノートに自動的にタグ付けを行います。

## 1. タグの基本設計

### 1.1 命名規則
- **小文字統一**: タグ名はすべて小文字で記述する
  - 例: #meeting、#project-alpha
- **スペース禁止**: タグ名にスペースは使用せず、単語間の区切りにはハイフン（-）やアンダースコア（_）を使用する
  - 例: #to-do、#research_notes
- **日本語タグ**: 日本語のタグも許可するが、英語タグを優先する
  - 例: #ai（○）、#人工知能（△）

### 1.2 タグの種類の明確化
- **内容タグのみ使用**: ノートの主題やトピックを表すタグのみを使用し、以下は禁止
  - 状態タグ（#未整理、#要修正）
  - 時間タグ（#2024、#q1）
  - 場所タグ（#東京、#オフィス）

### 1.3 形式の統一
- **単数形使用**: タグ名は基本的に単数形を使用する
  - 例: #note（○）、#notes（×）

### 1.4 特殊文字の使用制限
- **禁止文字**: スペース、特殊記号、絵文字
- **許可文字**: ハイフン（-）、アンダースコア（_）、スラッシュ（/）

## 2. タグ管理プラクティス

### 2.1 タグ数の制限
- 1つのノートに付与するタグは最大5つまでとする

### 2.2 階層タグの使用
- 関連性のあるタグは階層構造で表現する
  - 例: #tech/programming/python、#ec/btob/market

### 2.3 禁止タグ
- 以下のテンプレート関連タグは使用しない
  - #todo、#routine、#journal、#study、#exercise

## 3. 主要タグカテゴリ

### 3.1 情報タイプ
- #article - 記事
- #book - 書籍
- #paper - 論文
- #news - ニュース
- #tutorial - チュートリアル
- #reference - リファレンス
- #clippings - Webクリップ

### 3.2 技術・IT関連
- #programming - プログラミング全般
- #ai - 人工知能
- #ml - 機械学習
- #cloud - クラウド
- #infra - インフラ
- #security - セキュリティ
- #dns - DNS関連
- #network - ネットワーク
- #database - データベース

### 3.3 ビジネス・EC関連
- #business - ビジネス全般
- #ec - Eコマース全般
- #ec/btob - BtoB EC
- #ec/btoc - BtoC EC
- #marketing - マーケティング
- #sales - セールス
- #strategy - 戦略
- #market - 市場分析

### 3.4 生産性・ツール関連
- #productivity - 生産性
- #tool - ツール全般
- #obsidian - Obsidian関連
- #cursor - Cursor関連
- #ai-tools - AI関連ツール
- #note-taking - ノート取り

### 3.5 内容の深さ
- #basic - 基本知識
- #intermediate - 中級知識
- #advanced - 高度な知識
- #concept - 概念説明
- #practical - 実践的内容
- #case-study - 事例研究

## 4. タグの自動付与プロセス

タグ付けを実行する際の指示例:
```
@00_Inbox_Clip フォルダ内のファイルを分析し、以下の観点でタグ付けを行ってください:
1. 主要トピック（#dns、#ec、#obsidian、#ai など）
2. 情報の種類（#article、#reference、#news など）
3. 情報の深さ（#basic、#intermediate、#advanced など）
4. 必要に応じて階層タグを使用（#ec/btob/case-study など）

このタグ付けルールに則り、各ファイルの内容を分析して最適なタグを提案してください。ただしタグは1ファイルにつき最大5つまでとします。
