---
title: "Chrome 拡張「シンプル翻訳」を更新"
date: "2014-02-24T14:47:27"
comments: true
tags: 
- javascript
- chrome-extension
---

Chrome 拡張機能の「[シンプル翻訳](https://chrome.google.com/webstore/detail/%E3%82%B7%E3%83%B3%E3%83%97%E3%83%AB%E7%BF%BB%E8%A8%B3/pdnmkammncjnifdeclafllianknnoaif)」を2年ぶりに更新しました。

<!--more-->

素敵なバナーやアイコンは [@meganejunkie](https://twitter.com/meganejunkie) 先生に作ってもらいました (ありがとうございます) 。

![](/images/post/simple-honyaku-1.png)

![](/images/post/simple-honyaku-2.png)

利用しているAPIを変更しましたが機能的にはほぼ変わっていません。

- Google翻訳APIから、Bing Translator APIに変更
- オプションから個別にBing Application IDが設定可能に
- 翻訳結果の読み上げ機能(英語のみ)を追加

![](/images/post/simple-honyaku-3.png)

デフォルトだと一つのBing Application IDをみんなで使いまわすので、API制限かかるかもしれません。
なので一応「オプション」画面から個別にBing Application IDを上書きできるようにしています。

インストールはこちらから。

[Chrome ウェブストア - シンプル翻訳](https://chrome.google.com/webstore/detail/%E3%82%B7%E3%83%B3%E3%83%97%E3%83%AB%E7%BF%BB%E8%A8%B3/pdnmkammncjnifdeclafllianknnoaif)

ソースは[GitHub](https://github.com/hikarock/simple_honyaku)に置いてます。

