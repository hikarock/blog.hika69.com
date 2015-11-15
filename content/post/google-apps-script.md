---
layout: post
title: "Twitter 検索の結果を Google Drive のスプレッドシートに出力する"
date: "2014-01-28T17:07:27"
comments: true
tags: 
- twitter-api
- gas
- javascript
---

以前Google Apps Scriptで作成したプログラムがTwitter API Ver.1の廃止で動かなくなってたので直しました。

<!--more-->

ついでにライブラリとしてだれでも使えるようにしました。~~ソースコードは[ここ](#)に置いてます。~~

使い方まとめるよ。

**2015/10/7 追記: Google Apps Scriptで`OAuthConfig`が廃止されたため、このスクリプトは動きません。**

### 1) Twitterアプリの作成

[Twitter Developers](https://dev.twitter.com/)でGoogle Apps Script用のアプリを作成してください。

以下は入力例です。`Callback URL`は以下で指定するURLを設定してください。
他の項目は自分がわかりやすいもので。

- Name: `GoogleAppsScript`
- Description: `GoogleAppsScriptから利用します`
- Website: `http://example.com/`
- Callback URL: `https://spreadsheets.google.com/macros`

作成が成功すると`Consumer key`と`Consumer secret`が発行されます。これはあとで使います。

### 2) スプレッドシートの作成

Google Driveでスプレッドシート作成します。

メニューから`ツール > スクリプトエディタ`を選択します。

![](https://dl.dropboxusercontent.com/u/459142/img/blog/google-apps-script-01.png)

「無題のプロジェクト」が開きますので、プロジェクトに適当に名前をつけてください。

### 3) ライブラリの読込

`リソース > ライブラリ`を選択すると以下のウィンドウが表示されます。

![](https://dl.dropboxusercontent.com/u/459142/img/blog/google-apps-script-02.png)

「ライブラリを検索」に `MHGYgT-unu1fC7zXOkNfBUVSHJARy_IP7` を入力して選択ボタンを押します。

![](https://dl.dropboxusercontent.com/u/459142/img/blog/google-apps-script-03.png)

最新のバージョンを選択します。この例では「1」を選択して保存。

### 4) プロジェクトのプロパティの設定

次に`ファイル > プロジェクトのプロパティ`を選択します。
ユーザーのプロパティタブを開いて以下の名前のプロパティを設定します。

- `TWITTER_CONSUMER_SECRET`
- `TWITTER_CONSUMER_KEY`

それぞれに先ほどTwitter Developersで取得した「Consumer key」と「Consumer secret」の値を設定してください。

![](https://dl.dropboxusercontent.com/u/459142/img/blog/google-apps-script-04.png)

### 5) トリガースクリプトの作成

コード.gsに以下のようなスクリプトを記述してください。
この例は検索キーワード「都知事」で最初(0番目)のシートに検索結果を出力する、という意味です。

```js
function myFunction() {
  Twitter.search("都知事", 0);
}
```

実行するとスプレッドシートに検索結果が出力されます。
初回実行時はTwitterとGoogleの承認画面が表示されると思います。

![](https://dl.dropboxusercontent.com/u/459142/img/blog/google-apps-script-06.png)

こんな感じで検索結果が出力されてたら成功。

### 6) 定期的に検索を実行する

メニューから`リソース > 現在のスクリプトのトリガー`を選択します。

![](https://dl.dropboxusercontent.com/u/459142/img/blog/google-apps-script-05.png)

この設定だと1時間おきに検索が実行されて、検索結果がスプレッドシートに貯まっていきます。

