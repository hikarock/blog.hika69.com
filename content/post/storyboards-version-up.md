---
title: "ストボのバージョンアップ業"
date: "2013-12-24T20:45:26"
comments: true
tags: 
- stobo
- bootstrap
---

1年程放置ぎみだった[ストーリーボード](http://www.storyboards.jp)のバージョンアップ業を1ヶ月くらいやってました。

<!--more-->

Markdownでプレゼンが作れるあれです。

![](/images/post/storyboards-emoji.png)

### やったこと

- Bootstrap2をBootstrap3に
- Grunt導入
  - lessのコンパイルやBower連携
- Bower導入
  - 手動でいれてたフロント系ライブラリをBowerでインストールするようにした
- Markdownパーサーを[showdown](https://github.com/coreyti/showdown)から[marked](https://github.com/chjj/marked)に変更
- Font Awesome 3.2から4.0に変更
- 絵文字導入([emojify.js](http://hassankhan.github.io/emojify.js/))

こういうのはこまめにやるのがいいですね。
Bootstrap3の`box-sizing: border-box`導入はドラスティックすぎてやばかったです。

