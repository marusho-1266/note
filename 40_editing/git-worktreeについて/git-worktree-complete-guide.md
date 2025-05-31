最近ネットを見てると「Git Worktreeが便利」という内容のものを見かけたので、調べた結果を覚書。



## Git Worktreeとは？

Git Worktreeは、一つのGitリポジトリで複数の作業ツリー（ワーキングディレクトリ）を管理できる機能だ。従来のGitでは一つのリポジトリにつき一つの作業ディレクトリしか使えなかったが、Git Worktreeを使うことで、同じリポジトリの異なるブランチを複数のディレクトリで同時に作業できる。


## どういう時に使う？

- 機能開発ブランチで作業中に緊急バグ修正の依頼が来た
- プルリクエストのレビューをしたいが、作業中のコードをコミットしたくない
- 複数のブランチを並行して開発・検証したい

Gitは同一ローカルリポジトリで作業を行う関係上、このような場面では**git stash**や**git commit**を使って現状を保持してからブランチを切り替える工程が必要になる。

こういった時に、**Git Worktree**を使えば、複数のブランチを同時に、かつ安全に作業できるようになる。


### 従来の問題点

上記にもある通り、通常のGitワークフローでは、以下のような問題があった：

```bash
# 作業中にブランチを切り替える際の典型的な手順
git add .
git stash save "一時保存"
git checkout hotfix-branch
# バグ修正作業
git add .
git commit -m "Fix urgent bug"
git checkout feature-branch
git stash pop
```

この方法の問題点：
- 作業の中断が必要
- stashの管理が煩雑
- コンテキストスイッチが頻繁
- 一時的なコミットによるコミット履歴の汚染

### Git Worktreeによる解決

Git Worktreeを使えば、これらの問題を解決できる：

```bash
# hotfix用のワークツリーを別ディレクトリに作成
git worktree add ../myproject-hotfix hotfix-branch

# hotfixディレクトリで作業
cd ../myproject-hotfix
# バグ修正、コミット、プッシュ

# 元の作業ディレクトリに戻る
cd ../myproject
# 作業中の状態がそのまま残っている
```

## Git Worktreeの仕組み

Git Worktreeは、リポジトリの`.git`フォルダを共有する形で新しい作業ディレクトリを作成する。

```
myproject/
├── .git/
│   ├── worktrees/
│   │   ├── myproject-hotfix/
│   │   │   ├── HEAD
│   │   │   ├── index
│   │   │   └── gitdir
│   │   └── myproject-feature/
│   └── ...
├── src/
└── README.md

myproject-hotfix/
├── .git  (ファイル：../myproject/.git/worktrees/myproject-hotfix を指す)
├── src/
└── README.md
```

各ワークツリーは独立しており、以下の特徴がある：
- 同じリポジトリ情報を共有
- それぞれ独立したHEADとindex
- ブランチの重複チェックアウト不可
- ディスク容量の効率的な使用

## 基本的な使い方

なにはともあれ、実際にやってみないと分からないので、作ってみる。
個人で個人的に作成した日報ツールがあるのでそれで実験。

### 0. 事前準備
ブランチを作成し、未コミット状態のファイルを作っておく。
この状態でmainブランチに切り替えると。

画像１貼り付け

このようにエラーが発生する。
本来なら、ここから**git stash**や**git commit**を行う。

### 1. ワークツリーの作成

ワークツリーは以下のコマンドで作成できる

```bash
# 既存ブランチをワークツリーに追加
git worktree add <パス> <ブランチ名>

```

実際に作成してみる。

画像２貼り付け

出来たっぽい。


### 2. ワークツリーの確認

```bash
git worktree list
```

出力例：
```
/Users/user/myproject              a1b2c3d [main]
/Users/user/myproject-develop      e4f5g6h [develop]
/Users/user/auth-feature          i7j8k9l [feature-user-auth]
```

実行前のローカルリポジトリ
画像３貼り付け

実行後のローカルリポジトリ
画像４貼り付け

新しくフォルダが作成された。


作成されたフォルダの中を見ると、確かにmainブランチの内容になっている。

画像５貼り付け

今回はローカルリポジトリ内に作ってしまったので変なフォルダ構成になってしまったが、実際に使う時は作業中のリポジトリと異なる場所に作成すれば問題無いと思われる。


ちなみにこのWorktreeだが、Cursorだと便利に切り替えれる機能が存在している。

Cursorの拡張機能から「Git Worktree Manager」と検索すると、以下のようなツールが表示されるので、これをインストールする。

画像６貼り付け


これを利用すると、このようにワークツリーが一覧形式で表示される。

画像７貼り付け

例えばこのmainを選択すると、新しいウィンドウでワークツリーが開かれるので、スムーズに別作業に取り掛かれる。

画像８貼り付け


### 3. ワークツリーの削除

作業が終わったワークツリーについては、以下のコマンドで削除が可能となっている。

```bash
# ディレクトリごと削除
rm -rf ../myproject-develop

# Git側の管理情報をクリーンアップ
git worktree prune
```

または、より安全な方法：

```bash
# Git経由で削除（Git 2.17以降）
git worktree remove ../myproject-develop
```

## 実際の活用シーン

### シーン1：緊急バグ修正対応

機能開発中に緊急のバグ修正依頼が来た場合：

```bash
# 現在の作業：feature-aブランチで新機能開発中
# 多数のファイルを変更済み、まだコミットしたくない状態

# 緊急修正用のワークツリーを作成
git worktree add ../urgent-hotfix main

# 修正作業ディレクトリに移動
cd ../urgent-hotfix

# バグ修正
vim src/buggy_file.js
git add .
git commit -m "Fix critical bug in payment system"
git push origin main

# 元の作業に戻る
cd ../myproject
# 作業中の変更がそのまま残っている
```

### シーン2：プルリクエストレビュー

作業中にプルリクエストのレビュー依頼が来た場合：

```bash
# PRブランチを取得してワークツリーに展開
git fetch origin pull/123/head:pr-123
git worktree add ../review-pr-123 pr-123

# レビュー作業
cd ../review-pr-123
# コードレビュー、動作確認

# レビュー完了後、クリーンアップ
cd ../myproject
git worktree remove ../review-pr-123
git branch -D pr-123
```

### シーン3：マルチブランチ並行開発

複数の機能を同時開発する場合：

```bash
# 各機能用のワークツリーを作成
git worktree add -b feature-auth ../auth-feature main
git worktree add -b feature-payment ../payment-feature main
git worktree add -b feature-analytics ../analytics-feature main

# ディレクトリ構造
# myproject/          (mainブランチ)
# auth-feature/       (feature-authブランチ)
# payment-feature/    (feature-paymentブランチ)
# analytics-feature/  (feature-analyticsブランチ)
```


最近はAIエージェントで並列開発を行う事が多いらしく、その中でGit worktreeが注目されているとの事。
参考↓  
[AIエージェントで並列実装なら必須技術！ Git Worktree を理解する](https://zenn.dev/siu_issiki/articles/git_worktree)


### プルリクレビューの効率化フロー

```bash
# 1. PRを取得してワークツリーに展開
git fetch origin pull/238/head:pr-238
git worktree add pr-238 pr-238

# 2. レビュー作業
cd pr-238
# 動作確認、コードレビュー

# 3. レビュー完了後のクリーンアップ
cd ../
rm -rf pr-238
git worktree prune
git branch -D pr-238
```

## 注意点とベストプラクティス

### 1. 同一ブランチの重複チェックアウト不可

```bash
# エラーが発生する例
git checkout develop
# fatal: 'develop' is already checked out at '/path/to/myproject-develop'
```

この場合は：
```bash
git worktree prune  # 不要なワークツリー情報をクリーンアップ
git checkout develop  # 再度試行
```

### 2. ワークツリーの適切な管理

- **命名規則の統一**：ブランチ名と同じ、または関連性のある名前を使用
- **定期的なクリーンアップ**：不要なワークツリーは速やかに削除
- **チーム内でのルール策定**：ワークツリーの使用方針を共有

### 3. ESLintなどの設定ファイル競合への対処

親ディレクトリに設定ファイルがある場合、ワークツリーで問題が発生することがある：

```bash
# 一時的に設定ファイルを退避
mv .eslintrc.js .eslintrc.js.bak
mv node_modules node_modules.bak

# ワークツリーで作業
cd feature-worktree
yarn lint  # 正常に動作

# 元ディレクトリに戻って設定ファイルを復元
cd ..
mv .eslintrc.js.bak .eslintrc.js
mv node_modules.bak node_modules
```

### 4. .gitignoreされたファイルの取り扱い

ワークツリーには`.env`ファイルなどの無視されたファイルは含まれないため、必要に応じて手動でコピーする：

```bash
# .envファイルをワークツリーにコピー
cp .env ../feature-worktree/.env
```


## クローンとの違い

## まとめ

Git Worktreeは、現代の複雑な開発ワークフローにおいて非常に強力なツールだ。以下のようなメリットがある：

### 主なメリット
- **作業効率の向上**：ブランチ切り替えによる作業中断がなくなる
- **ストレージ効率**：リポジトリの複製よりもディスク使用量を節約
- **安全性の向上**：作業中の変更を誤って失うリスクが減る
- **並行作業の実現**：複数のタスクを同時進行できる

### 適用シーン
- 緊急バグ修正と機能開発の並行作業
- プルリクエストのレビュー作業
- 複数機能の同時開発
- リリースブランチとの比較検証

### 注意点
- ワークツリーの管理を適切に行う
- チーム内でのルール策定
- 設定ファイルの競合への対処

Git Worktreeを活用することで、より効率的で安全な開発環境を構築できる。特に複数のタスクを並行して進める必要がある現代の開発現場において、その価値は計り知れない。

ぜひ一度試してみて、開発効率の向上を実感してもらいたい。

---

*この記事が役に立った場合は、ぜひスターやブックマークをお願いします！* 