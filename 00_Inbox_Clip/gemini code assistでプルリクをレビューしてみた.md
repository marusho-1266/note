---
title: "【個人でも無料で使える】gemini code assistでプルリクをレビューしてみた"
source: "https://zenn.dev/yamazaking/articles/gemini-code-assistant-tutorial"
author:
  - "[[Zenn]]"
published: 2025-02-26
created: 2025-05-16
description:
tags:
  - "clippings #inbox"
---
38

11[tech](https://zenn.dev/tech-or-idea)

## はじめに

本日GoogleのAIツールであるGemini code assistが個人向けに無料で開放されました。

この記事では、そのうちのコードレビュー機能を試したものとなります。  
なお、出力結果は2025年2月26日のものであり、差異が生じる可能性があることに留意ください。

## gemini-code-assist導入

こちらのページからgemini-code-assistをGitHubに統合します。

![](https://res.cloudinary.com/zenn/image/fetch/s--nbeeBS0L--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/16e6a6d846776089ad212bf2.png%3Fsha%3Db7c1ca7677bd44cff5116fd19f838bc2a5ae1ce7)

完了したらこのようなページが最後に表示されます。  
![](https://res.cloudinary.com/zenn/image/fetch/s--L-Ghs8rs--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/c9f158c7dee65553a663be4b.png%3Fsha%3D429e86886e17b7230406f5e46f08802e91e0d7be)

GitHubに統合したかどうかはGitHubの `Settings>integrations Applications` の、 `Installed GitHub Apps` で確認できます。  
![](https://res.cloudinary.com/zenn/image/fetch/s--okzvqk9l--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/a63f2a7cbc0467544cb5dfa7.png%3Fsha%3D9505d27a499c73ea19af83576df68eeb5cb3fb0a)

それでは、実際にCode Reviewを試してみます。

## Code Review

自分はFlutterエンジニアであり、Flutterの場合は最初に簡単なカウンターテンプレートを作ることができます。  
プロジェクト作成した場合、Flutter開発者にはよく見慣れたカウンターアプリの画面がでてきます。タイトルを変更して、このような感じになります。  
右下のボタンを押すと、数字が１ずつ足される簡単なアプリケーションです。

![](https://res.cloudinary.com/zenn/image/fetch/s--zrL019Au--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/30abe8a6ea51028976957099.png%3Fsha%3D9f8ed8b049ccea2158c71ebb70c197028dd42949)

### Pull Request

それでは、簡単にPull Requestを出していきます。  
数字が３の倍数のときに赤くなるようにします。

```dart
Text(
    '$_counter',
    style: Theme.of(context).textTheme.headlineMedium!.copyWith(
        color: _counter % 3 == 0 ? Colors.red : Colors.black),
),
```

commitメッセージは次のようにしました。（英語としては完全ではないです。）  
`number color changes red when _counter is 3 times`

この内容で、Pull Requestを出します。タイトルと内容も、このcommitメッセージと同じものとしました。

### gemini code assistantのレビュー

こんな感じのレビューをもらいました。  
![](https://res.cloudinary.com/zenn/image/fetch/s--OZ-i7K3W--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/aaa80b92a957787e13a4d417.png%3Fsha%3Da6946ff96c912a31e2247f64e4209cbcb9818808)

まずは、プルリクの全体的な総評がなされてるようです。

```md
Summary of Changes
This pull request, titled "number color changes red when _counter is 3 times", modifies the lib/main.dart file to change the color of the counter text to red when the _counter variable is a multiple of 3. The change involves using the copyWith method on the headlineMedium text style to conditionally set the color to red or black based on the _counter value.
```

トリビアもありました。クイズ？（他のレビューではなかったりするので、確率的に発生？）

```
Trivia time!

What is the purpose of the copyWith method in Flutter's TextStyle class?
Click here for the answer

The \`copyWith\` method in Flutter's TextStyle class is used to create a new TextStyle object with specified properties changed, while keeping the other properties the same as the original TextStyle.
```

プルリクに豆知識を入れ込むことは稀だと思うのですが、なかなかいいかもしれません。

次のコメントには、このようにしっかりとレビューしてくれてるようです。

![](https://res.cloudinary.com/zenn/image/fetch/s--Iaxv00Xa--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/8885fe935de45de841663ff7.png%3Fsha%3D04d9eff32864861364d9b7aef4e76adabfbb44a9)

提案内容は、カラーロジックを分けたほうが読みやすいとのこと。的を得たレビューだと思います。優先度のラベルもあり、このレビュー内容がどれくらい推奨されるものかを示していそうです。

ここにあるようにコメントのところで、geminiコマンドを使えるそうです。

| Feature | Command | Description |
| --- | --- | --- |
| Code Review | /gemini review | Performs a code review for the current pull request in its current state. |
| PR Summary | /gemini summary | Provides a summary of the current pull request in its current state. |
| Comment | @gemini-code-assist | Responds in comments when explicitly tagged, both in issue comments and review comments. |
| Help | /gemini help | Displays a list of available commands. |

対話的に聞くこともできます。  
心配なところがあるか、聞いてみました。

![](https://res.cloudinary.com/zenn/image/fetch/s--Fym-cUKD--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/527b4f6103ca55483935d870.png%3Fsha%3D6dde4004138215f675cd4f8c0800a25dec69217b)

ちゃんと答えてくれてすごい。コミニケーション苦手なエンジニア淘汰されるかも。

### Code Review (wrong pull request)

それでは、次に間違った内容のPull Requestをやってみたいと思います。

次のようにコードを修正します。  
内容としては、4の倍数で青色になるようにしました。

```dart
Text(
    '$_counter',
    style: Theme.of(context).textTheme.headlineMedium!.copyWith(
        color: _counter % 4 == 0 ? Colors.blue : Colors.black),
    ),
```

しかし、コミットメッセージとプルリクの内容は「数字が５の倍数で黄色になる」とします。

レビュー内容はこのようになりました。

![](https://res.cloudinary.com/zenn/image/fetch/s--g5RGSy8H--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/2c99ba7810259679dfe02a76.png%3Fsha%3D57b15efba17654067f9b04f1227c1cb302517f76)

パット見わかりずらいかもしれませんが、SummaryとHighlightの部分で、「PRタイトルと内容では5の倍数と書いてるけど、4の剰余としている」とあります。  
ただ、コードレビューでは4をマジックナンバーとしていることを指摘しています。  
ここらへんはまだ発展途上かもしれません。それでも、この指摘も的を得ています。

### 日本語でレビューを行う

リポジトリディレクトリに`.gemini/styleguide.md` を作成します。ここでgemini code assistの動作をカスタマイズできます。 [参照](https://developers.google.com/gemini-code-assist/docs/customize-gemini-behavior-github?hl=ja)  
ここで、日本語で回答するようにします。

```
日本語で回答してください
```

こんな感じで日本語で回答してくれます。  
多国籍チームでの開発でも使えそう✨️

![](https://res.cloudinary.com/zenn/image/fetch/s--mmspuMiD--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/2339310eeee7c6835a732bfd.png%3Fsha%3Da10bf30ff6803cc847ea2c3a11fbcfb3e98e2d21)

## おわりに

本日発表されたgemini code assistantのレビュー機能をざっと使ってみました。  
驚いたのは、これが無料で使えるということです。  
今回は簡単な修正だったのもあるかもしれませんが、指摘内容が的確で2,3秒で結果が出てきたのが驚きました。  
また、コメントのところで `@gemini-code-assist` とすることで、対話的に聞くこともできて気になる箇所とかを集中的に聞けたりするため、とても効果的だなと思います。  
人間が相手だとどうしても時間を取ったりすることだったり、質問内容を気遣いますが、AIであれば簡単に聞くことも容易です。  
日々進化していくAI技術に対して、使いこなして効率性を高めていくことが求められそうです。

## 参考記事

[GitHubで編集を提案](https://github.com/zakiyamaaaaa/zenn/blob/main/articles/gemini-code-assistant-tutorial.md)

38

11