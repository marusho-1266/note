
Githubのアカウントで仕事用と個人用を同一PCで取り扱う必要が出てきた。
あまり、特に気にせずどちらものアカウントからリポジトリをクローンして作業をしていたのだが、いざプッシュをする時にエラーが発生した。
原因を調査すると、どうやら同一PC内で複数のGitHubアカウントを取り扱う場合は、諸々設定が必要のよう。

今回は、その諸々で四苦八苦したので覚書。

## なぜプッシュできない？

そもそもなぜプッシュ出来なかったのか。

Gitは同一PCで複数アカウントを扱うことを考慮されていないようで、基本的にはユーザー情報を1つしか持てないよう。
なので、アカウントBとしてクローンしたリポジトリをアカウントAとしてプッシュしようとして、競合を起こす、といった事が発生していたようだ。

という事は、やらないといけない事としては、
 1. 特定のアカウントとしてクローンを作りますよ、と明示する
 2. 特定のアカウントとしてプッシュしますよ、と明示する

 上記のどちらかが必要となってくるという事になる。

 結論としては1.で対応をするようになる。
 これ以降はその手順の説明。

## 設定方法：SSHキーとconfigファイルを使う

最も一般的な方法が、SSHキーと `~/.ssh/config` ファイルを使う方法だ。
というか私はこれでしか出来なかった。

### 1. SSHキーの生成

各GitHubアカウント用に異なるSSHキーペアを作成する。
すでにキーがある場合は、既存のものとは別の名前で生成する必要がある。

```bash
$ ssh-keygen -t rsa -f ~/.ssh/id_rsa_personal # 個人用キー
$ ssh-keygen -t rsa -f ~/.ssh/id_rsa_work # 仕事用キー
```

生成時にパスフレーズを設定することも可能だが、空のままでも利用できる。

### 2. `~/.ssh/config` の設定

`~/.ssh/config` ファイルに、アカウントごとの接続設定を記述する。
ファイルがない場合は新規作成する。

```text
# 個人アカウント設定
Host github-personal # 任意のホスト名
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_personal # 個人用秘密鍵のパス
  User git
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes

# 会社アカウント設定
Host github-work # 任意のホスト名
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_work # 会社用秘密鍵のパス
  User git
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes
```

`Host` には、後ほどGitコマンドで使う任意の名前を設定する。
`IdentityFile` には、先ほど生成したアカウントごとの秘密鍵のパスを指定する。

### 3. GitHubに公開鍵を登録

生成したSSHキーペアの公開鍵 (`.pub` ファイル) の内容を、各GitHubアカウントのSSH設定に登録する。
GitHubのSettings > SSH and GPG keys から "New SSH key" で追加できる。

### 4. Gitコマンドでの使い分け

クローンやリモート設定時に、`~/.ssh/config` で設定した`Host`名を指定することで、使用するアカウントを切り替える。

**リポジトリをクローンする場合:**

```bash
$ git clone git@github-personal:yourname/your-personal-repo.git # 個人用アカウントでクローン
$ git clone git@github-work:yourorg/your-work-repo.git # 仕事用アカウントでクローン
```

**既存のリポジトリにリモートを追加する場合:**

```bash
$ git remote add origin git@github-personal:yourname/your-personal-repo.git # 個人用アカウントのリモートを追加
$ git remote add origin git@github-work:yourorg/your-work-repo.git # 仕事用アカウントのリモートを追加
```

## 設定方法：Git Configを使う

Gitの設定 (`user.name` と `user.email`) を切り替える方法もあるらしい。私はこちらを試していないが、興味がある方はぜひ。上手くいったら教えてください。

### 1. ローカルリポジトリごとの設定

プロジェクトディレクトリ内で以下のコマンドを実行すると、そのリポジトリだけに設定が適用される。

```bash
$ git config --local user.name "Your Name"
$ git config --local user.email "your.email@example.com"
```

複数アカウントを使い分ける場合は、この `--local` オプションを使うのが推奨される。

### 2. ターミナルで簡単に切り替え（zshの例）

`~/.zshrc` などに切り替え用の関数を定義しておくと便利とのこと。

```bash
# 個人用アカウントに切り替え
function git_personal() {
  git config --global user.name "[個人用アカウント名]"
  git config --global user.email "[個人用アカウントのメールアドレス]"
  echo "Switched to Personal Account"
}

# 仕事用アカウントに切り替え
function git_work() {
  git config --global user.name "[仕事用アカウント名]"
  git config --global user.email "[仕事用アカウントのメールアドレス]"
  echo "Switched to Work Account"
}
```

関数を実行するだけで、グローバル設定を切り替えられるようになる。
変更を反映するには `source ~/.zshrc` を実行する必要がある。

### 3. ディレクトリごとに自動切り替え

`~/.gitconfig` に `[includeIf]` ディレクティブを使うと、特定のディレクトリにいる場合に別の設定ファイルを読み込むことができる。

まず、各アカウント用の設定ファイルを作成する (例: `~/.gitconfig_personal`, `~/.gitconfig_work`)。

`~/.gitconfig_personal` の内容:
```ini
[user]
  name = [個人用アカウント名]
  email = [個人用アカウントのメールアドレス]
```

`~/.gitconfig_work` の内容:
```ini
[user]
  name = [仕事用アカウント名]
  email = [仕事用アカウントのメールアドレス]
```

次に、`~/.gitconfig` ファイルに以下の設定を追加する。

```ini
[user]
  name = [デフォルトのアカウント名]
  email = [デフォルトのアカウントのメールアドレス]

[includeIf "gitdir:~/path/to/your-personal-projects/"] # 個人用プロジェクトのディレクトリパスを指定
  path = ~/.gitconfig_personal

[includeIf "gitdir:~/path/to/your-work-projects/"] # 仕事用プロジェクトのディレクトリパスを指定
  path = ~/.gitconfig_work
```

これで、指定したディレクトリに移動するだけでGitの設定が自動的に切り替わるようになる。

## まとめ

GitHubの複数アカウントを使い分けるには、主にSSHキーとconfigファイルを活用する方法と、`git config`でユーザー情報を管理する方法がある。
用途に応じてこれらの方法を組み合わせることで、スムーズなアカウント切り替えが可能になる。

これらの設定を行うことで、より効率的にGitHubを利用できるだろう。 