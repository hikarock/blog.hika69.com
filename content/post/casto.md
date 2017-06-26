---
title: "ライブコーディングアプリ「Casto」ができるまで"
date: "2014-04-02T00:42:15"
comments: true
published: true
tags: 
- casto
- rendr
---

「[Casto](http://ca.storyboards.jp)」(きゃすと)をリリースしました。

<!--more-->

アプリの使い方などは以下の紹介記事が詳しいです。

- [超簡単なライブコーディング支援サービス「Casto」- ソフトアンテナブログ](http://www.softantenna.com/wp/webservice/casto/)
- [Castoというサービスを作りました - Live coding in browse, using text editor. - テノニッキ (@hideack &#39;s diary)](http://hideack.hatenablog.com/entry/2014/03/30/163413)

![](/images/post/casto-01.png)

ひとことで言うと「リアルタイムで更新できるgist」(by [@hideack](https://twitter.com/hideack))です。

### きっかけ

41日前の会話。

![](/images/post/casto-02.png)

結局「ペアプロ」は[課題が多く](https://github.com/hikarock/casto/issues/38#issuecomment-38985343)、「ライブコーディング」のアプリとして発表しました。

### ブラウザからファイルの更新を検知できるのか

肝になるのは「ローカルのエディタ（VimとかXcode）からの変更を、ブラウザで検知できるのか」という所だけど、ちょっと調べるとFile APIでできそうだということが分かりました。

[javascript - Check if file has changed using HTML5 File API - Stack Overflow](http://stackoverflow.com/questions/14284124/check-if-file-has-changed-using-html5-file-api)

この回答のサンプルをそのままMeteorにアップロードしてみたのが[こちら](http://casto.meteor.com/)。
ファイルを保存したタイミングで画面が更新できています。

![](/images/post/casto-03.png)

### ファイルの変更箇所を検知できるか

これはFile APIを単純に使うだけでは実現できなそうでした。

ファイルのどの部分が更新されたのかわからないと、編集中の位置を閲覧者が探さないといけない。数行のスクリプトならともかく数百行あるファイルを共有するときには致命的に使いにくくなりそう。

最初は「変更箇所をマークする記法をあらかじめ決めておいて、エディタでそれを入力してもらって目印にする」という方式を考えていたのですが、社内の開発者に相談したところ、[@hikalin8686](https://twitter.com/hikalin8686)が、「変更前のテキストを保存しておいてDiffをとればいいのでは」というアイディアを出してくれて、その方法で実装しました。

![](/images/post/casto-04.png)

変更した行がハイライトされています。

Diffは以下のライブラリで実現できました。

[kpdecker/jsdiff](https://github.com/kpdecker/jsdiff)

### ブラウザ内でエディタをどう実装するか

[Ace](http://ace.c9.io/#nav=about)を使いました。すごいですねAce。超便利。

Castoではシンタックスの判定にファイルの拡張子を使いますが、拡張子からAce内の`Mode`に変換するプログラムもAceのものを流用できました。

[ace/lib/ace/ext/modelist.js at master · ajaxorg/ace](https://github.com/ajaxorg/ace/blob/master/lib/ace/ext/modelist.js)

### サーバーサイド - UI層

今回は[Rendr](https://github.com/rendrjs/rendr)を使ってみました。

Rendrについては、

[Rendr入門(1): Node.js + Backbone.jsでサーバ & クライアントを構築する"Rendr"の紹介](http://qiita.com/mshk/items/5912dcd4d9fa52ff6371)

この記事で知って使いたいなあと思っていたけど、正直よく理解してなかったので、がんばってMongoDB / MySQLなどのバックエンド接続も含めてRendrで実装しようとしていました。

[Rendr徒然 - 日記](http://d.hatena.ne.jp/koichik/20131207/1386421200)

そんなときにこの記事を読んで「Rendrらしい」アーキテクチャへの理解が進みました。

一般的なSPA (「Reder徒然」からの引用)

```
client             server
==========         ==============

+---------+  HTTP  +------------+
| Browser |<======>| API server |
+---------+        +------------+
```

一般的なSPAや、Web APIを使ったiOS/Androidアプリもこの形に近くなると思います（[REMP](http://www.remp.jp/hello)もこの作りです）。

Rendrを使ったSPA (「Reder徒然」からの引用)

```
client             server
==========         ======================================

UI層                                       サービス層
===================================        ==============

+---------+  HTTP  +--------------+  HTTP  +------------+
| Browser |<======>| Rendr server |<======>| API server |
+---------+        +--------------+        +------------+
```

ビューに関係する「UI層」内はサーバー・クライアント問わずJavaScript / Backbone.jsで統一されるため、テンプレートやバリデートの使い回しがしやすくなります。

またSPAでは、バルクAPI（複数のAPIをまとめたAPI）がない場合、初回表示で複数のAPIを叩くことになって表示が重くなりがちです。
Rendrでは初回表示はUI層内のサーバーでテンプレートを組み立てて描画するのでこの問題はありません（少なくともクライアント側では）。

Rendrについては基本的な動作以外はまだ理解できてない部分が多いので、バージョンアップ含めて追いかけていこうと思います。

### サーバーサイド - サービス層

Padrino製の[Storyboards API](http://www.storyboards.jp)にあいのりする形で、@hideackがCasto用のAPIを構築しました。

### デプロイ

こちらを参照。

[Rendrで作られたnode.jsアプリをminaでデプロイする - テノニッキ (@hideack &#39;s diary)](http://hideack.hatenablog.com/entry/2014/02/22/191700)

### これから

[イシュー](https://github.com/hikarock/casto/issues?state=open)を淡々とこなします。~~Firefox対応がわりと重めですね。~~ 対応しました。

- [Casto :: Live coding in browse, using text editor.](http://ca.storyboards.jp/)
- [Twitter](https://twitter.com/__casto__)
- [GitHub](https://github.com/hikarock/casto)

