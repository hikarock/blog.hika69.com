---
title: "Underscore.js の _.debounce が便利そう"
date: "2013-05-28T21:53:00"
comments: true
tags: 
- javascript
- backbone
- underscore
- book
---


「[Backbone.jsガイドブック](http://booklog.jp/item/1/4899773501)」の読書会2回目。

<!--more-->

<a href="http://www.amazon.co.jp/Backbone-js%E3%82%AC%E3%82%A4%E3%83%89%E3%83%96%E3%83%83%E3%82%AF-%E9%AB%98%E6%A9%8B-%E4%BE%91%E4%B9%85/dp/4899773501%3FSubscriptionId%3D0AVSM5SVKRWTFMG7ZR82%26tag%3Dhikarock-22%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3D4899773501" target="_blank" title="Backbone.jsガイドブック"><img src="https://images-na.ssl-images-amazon.com/images/I/31tI0WaZukL.jpg" width="394" height="500" alt="Backbone.jsガイドブック" /></a>

1章の後半から始めた。underscore.jsのタイマー系の関数が超絶便利そうだった。

- [debounce](http://underscorejs.org/#debounce)
- [throttle](http://underscorejs.org/#throttle)
- [delay](http://underscorejs.org/#delay)
- [defer](http://underscorejs.org/#defer)

この辺。

重いレンダリング処理とか保存処理のイベント多発を`setTimeout`とか`clearTimeout`でがんばってブロックしてたところは`_.debounce`の方がきれいに書けそう。

[REMP](http://www.remp.jp/hello)のライブラリ保存処理がちょうどこんな感じで、

<script type="text/javascript" src="https://jsdo.it/blogparts/4prZ/js?width=100%&height=540&view=javascript"></script>

`_.debounce`だとこんな感じになる。

<script type="text/javascript" src="https://jsdo.it/blogparts/sUcm/js?width=100%&height=320&view=javascript"></script>

[ストーリーボード](http://www.storyboards.jp)のMarkdownのレンダリングのとこもこれ使ったらいいな、と思ってたらドキュメントにもまんまなこと書いてあった。

> For example: rendering a preview of a Markdown comment, recalculating a layout after the window has stopped being resized, and so on.

かきなおそ。
