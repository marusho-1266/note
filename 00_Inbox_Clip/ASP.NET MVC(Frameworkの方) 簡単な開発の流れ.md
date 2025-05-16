---
title: "ASP.NET MVC(Frameworkの方) 簡単な開発の流れ"
source: "https://qiita.com/masayakomono/items/7d8b4ec793d23a7d74dd"
author:
  - "[[Qiita]]"
published: 2023-10-05
created: 2025-05-16
description: "ASP.NET MVC(Frameworkの方) 簡単な開発手順まとめ前提として、ChatGPT使いながらベースを書いたため誤りがあったら申し訳ございません。開発にはVisual Studio …"
tags:
  - "clippings #inbox"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

この記事は最終更新日から1年以上が経過しています。

## ASP.NET MVC(Frameworkの方) 簡単な開発手順まとめ

前提として、ChatGPT使いながらベースを書いたため誤りがあったら申し訳ございません。  
開発にはVisual Studio 2017を利用しています。言語はC#です。  
画像は無いです。  
サンプルコードはおおよそ問題ないと思いますが、自動生成なので間違っていたら申し訳ございません。

## プロジェクト作成から MVC サイト作成まで

### 1\. プロジェクトの作成

1. **Visual Studio を開く**
	- パソコンに Visual Studio がインストールされていることを確認します。
2. **新規プロジェクトの作成**
	- 「ファイル」メニューから「新規作成」を選択します。
3. **プロジェクトの選択**
	- 「プロジェクト...」をクリックして新しいプロジェクトを作成します。
4. **テンプレートの選択**
	- 左側のメニューから「Visual C#」を選び、その下で「ASP.NET Web アプリケーション(.NET Framework)」を選択します。
5. **ターゲットの.NET Framework の選択**
	- ターゲットの.NET Framework を選択します。
	- 注意点としては自身の開発端末に使いたいバージョンの.Net Frameworkの開発パックがインストールされているか確認してください。
6. **プロジェクトの設定**
	- 任意のプロジェクト名や保存先を設定し、「作成」ボタンをクリックします。
7. **テンプレートの選択**
	- 「テンプレート」で「空」を選択します。
8. **フォルダー及びコア参照の追加**
	- 「フォルダー及びコア参照の追加」を選択します。
9. **MVC を追加する**
	- 画面に表示されるオプションから「MVC」を選択します。
10. **OK を押す**
	- 設定が完了したら、「OK」ボタンをクリックしてプロジェクトの作成を確定します。

### 2\. フォルダの作成

1. **コンテンツ保存用フォルダの作成**
	- プロジェクトができたら、以下のコンテンツ保存用フォルダを作成します。
		- **css**
		- **js**
		- **img** 　などなど
	これに加えて、作成するサイトで必要なフォルダがあれば、追加してください。

### 3\. コントローラーの作成

#### MVC におけるコントローラーについて

コントローラーはユーザーのアクションに応じて呼び出され、モデルからデータを取得し、適切なビューを呼び出す役割を果たします。ユーザーからのリクエストを受け取り、それに対する処理を実行するのが主な役割であり、ビジネスロジックの制御の中心です。

1. **Controllers フォルダにコントローラーを作成する**
	- ソリューションエクスプローラで `Controllers` フォルダを右クリックします。
	- 「追加」を選択します。
	- 「コントローラー」を選択します。
	- 「MVC 5 コントローラー - 空」を選択します。
2. **コントローラーの設定**
	- コントローラー名に「〇〇 Controller」と入力します（〇〇には任意の名前が入ります）。
	- 「追加」をクリックしてコントローラーを作成します。
3. **各画面の処理を記述**
	- コントローラーには各画面で「GET」および「POST」が行われた際の処理を記述します。
	```csharp
	// GET: /〇〇/Index
	public ActionResult Index()
	{
	    // 〇〇画面のGETリクエスト時の処理
	    return View();
	}
	// POST: /〇〇/Index
	[HttpPost]
	public ActionResult Index(FormCollection form)
	{
	    // 〇〇画面のPOSTリクエスト時の処理
	    // formオブジェクトを使用してフォームデータにアクセスする例
	    return View();
	}
	```

### 4\. モデルの作成

#### MVC におけるモデルについて

モデルはアプリケーションのデータやビジネスロジックを扱います。MVC では、モデルはアプリケーションのデータ構造やデータベースの操作を担当し、コントローラーとビューの間でデータのやり取りを行います。これにより、アプリケーションの各部分が役割分担され、柔軟で保守しやすいコードを実現します。

#### Models フォルダに新しいクラスを作成

1. **Models フォルダにクラスを作成する**
	- ソリューションエクスプローラで `Models` フォルダを右クリックします。
	- 「追加」を選択します。
	- 「新しい項目」を選択します。
	- 「Visual C#」 > 「コード」から「クラス」を選び、任意のクラス名を入力して「追加」ボタンをクリックします。
2. **Modelsクラスを実装する**
	- モデルのクラスを実装します。
	```csharp
	using System;
	using System.ComponentModel.DataAnnotations;
	using System.Linq;
	public class User
	{
	    [Key]
	    public int UserId { get; set; }
	    [Required(ErrorMessage = "ユーザー名は必須です。")]
	    [StringLength(50, ErrorMessage = "ユーザー名は50文字以内で入力してください。")]
	    public string UserName { get; set; }
	    [Required(ErrorMessage = "メールアドレスは必須です。")]
	    [EmailAddress(ErrorMessage = "正しいメールアドレスの形式で入力してください。")]
	    public string Email { get; set; }
	    [Required(ErrorMessage = "パスワードは必須です。")]
	    [DataType(DataType.Password)]
	    [StringLength(20, MinimumLength = 6, ErrorMessage = "パスワードは6〜20文字で入力してください。")]
	    [CustomPasswordValidation(ErrorMessage = "パスワードには英大文字、英小文字、数字、特殊文字をそれぞれ1文字以上含めてください。")]
	    public string Password { get; set; }
	}
	public class CustomPasswordValidationAttribute : ValidationAttribute
	{
	    public override bool IsValid(object value)
	    {
	        if (value == null || string.IsNullOrWhiteSpace(value.ToString()))
	        {
	            return false;
	        }
	        // パスワードに英大文字、英小文字、数字、特殊文字をそれぞれ1文字以上含むかどうかを検証
	        var password = value.ToString();
	        return HasUpperCase(password) && HasLowerCase(password) && password.Any(char.IsDigit) && password.Any(IsSpecialCharacter);
	    }
	    private bool HasUpperCase(string str)
	    {
	        return str.Any(char.IsUpper);
	    }
	    private bool HasLowerCase(string str)
	    {
	        return str.Any(char.IsLower);
	    }
	    private bool IsSpecialCharacter(char c)
	    {
	        // 例として、特殊文字が含まれていると判断する条件を設定
	        var specialCharacters = new[] { '!', '@', '#', '$', '%', '^', '&', '*' };
	        return specialCharacters.Contains(c);
	    }
	}
	```

### 5\. ビューの作成手順

#### MVC におけるビューについて

MVC（Model-View-Controller）では、ビューはユーザーに表示される情報を担当します。ビューは HTML や他のマークアップ言語を使用して、データをユーザーに適切な形で表示します。ビューは主にコントローラーから受け取ったモデルのデータを使ってページを構築します。また、ユーザーからの入力を受け付け、コントローラーに送信することもあります。

ビューは通常、特定のアクションや操作に関連付けられており、各ビューは対応するコントローラーアクションと結びついています。ビューの作成手順を理解することで、MVC アーキテクチャ内で柔軟かつ効果的にユーザーインターフェースを構築できます。

1. **フォルダの作成**
	- Views フォルダ内に、各コントローラーごとに対応するフォルダを作成します。例えば、 `Views/Home` 、 `Views/User` など。
2. **ビューの追加**
	- 作成したフォルダを右クリックし、「追加」を選択します。
	- 「ビュー」を選択します。
3. **ビューの設定**
	- ビュー名を入力します。例えば、 `Index` や `Register` など。
	- 「テンプレート」で、モデルを使用する場合は「Empty」、モデルを使用しない場合は「Empty(モデルなし)」を選択します。
	- 「レイアウトページの指定」では、必要に応じてレイアウトページをファイルを選択する形式で指定します。
	- 「追加」ボタンをクリックしてビューを作成します。
4. **ビューを実装する**
	- ビューを実装します。

### 6\. ルート設定手順

1. **App\_Start フォルダーの作成:**
	- プロジェクトのルートに `App_Start` フォルダーが存在しない場合は、新しく作成します。
2. **RouteConfig.cs の追加:**
	- `App_Start` フォルダー内で、右クリックして「新しい項目の追加」を選択します。
	- 「Visual C#」 > 「コード」から「クラス」を選択し、名前を `RouteConfig.cs` として追加します。
3. **RouteConfig.cs の編集:**
	- `RouteConfig.cs` を開いて、 `RegisterRoutes` メソッドを作成し、必要なルート設定を追加します。以下はサンプルです。
4. **グローバルアプリケーションクラスの設定:**
	- Global.asax や Global.asax.cs など、グローバルなアプリケーションクラスで、RouteConfig を有効にします。以下はサンプルです。

### 7\. エラーハンドリングフィルター設定

#### フィルターについて

FilterConfig.cs は ASP.NET MVC プロジェクトにおいて、グローバルなフィルターを設定するためのクラスです。このクラスは通常、アプリケーションの開始時に呼び出される FilterConfig.RegisterGlobalFilters メソッドを使用して、グローバルなフィルターを登録します。

フィルターは、MVC アプリケーションで実行されるアクション メソッドや要求、またはその他のイベントに影響を与えるカスタム ロジックを提供します。例えば、エラーハンドリング、認証、出力キャッシュなどがフィルターを使用して実装されることがあります。

1. **App\_Start フォルダーの作成:**
	- プロジェクトのルートに `App_Start` フォルダーが存在しない場合は、新しく作成します。
2. **FilterConfig.cs の追加:**
	- `App_Start` フォルダー内で、右クリックして「新しい項目の追加」を選択します。
	- 「Visual C#」 > 「コード」から「クラス」を選択し、名前を `FilterConfig.cs` として追加します。
3. **RouteConfig.cs の編集:**
	- `RegisterGlobalFilters` メソッド内に以下の行を追加して、 `HandleErrorAttribute` を登録します。
	- HandleErrorAttribute はエラーが発生した場合、デフォルトで~/Views/Shared/Error.cshtml ビューを表示します。もしカスタムのエラーページを使用したい場合は、Error.cshtml を作成し、ビューを変更してください。
4. **global.asax.cs への追記:**
	- `global.asax.cs` ファイルを開き、Application\_Start メソッド内に以下の行を追加します。

実際はCSSやJSを書いたりまだまだやることはありますが、おおよそこの流れで開発すれば一通りの物が出来上がると思います。

[0](https://qiita.com/masayakomono/items/#comments)

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fmasayakomono%2Fitems%2F7d8b4ec793d23a7d74dd&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fmasayakomono%2Fitems%2F7d8b4ec793d23a7d74dd&realm=qiita)

[7](https://qiita.com/masayakomono/items/7d8b4ec793d23a7d74dd/likers)

6