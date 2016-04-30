---
layout: post
title: "SQL アンチパターン"
date: "2013-05-27T20:51:00"
comments: true
tags: 
- sql
- book
---

3章まで読んだ。Kindle for iPadで読んでると表が見づらい...

<!--more-->

<a href="http://www.amazon.co.jp/SQL%E3%82%A2%E3%83%B3%E3%83%81%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3-Bill-Karwin/dp/4873115892%3FSubscriptionId%3D0AVSM5SVKRWTFMG7ZR82%26tag%3Dhikarock-22%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3D4873115892" target="_blank" title="SQLアンチパターン"><img src="https://images-na.ssl-images-amazon.com/images/I/41qHKrFZi0L.jpg" width="391" height="500" alt="SQLアンチパターン" /></a>

1. ジェイウォーク（信号無視）
1. ナイーブツリー（素朴な木）
1. IDリクワイアド（とりあえずID）

「とりあえずID」を交差テーブルで使ってるようなケースを最近やらかしてて、はっとしてテーブルつくり直した。

外部キーを設定しようとしたら`errno: 150`とか言われて心が折れそうになったけど、このエントリ読んで解決できました。

[MySQLでCan’t create table(errno: 150) ](http://fizsoft.net/?p=294)

ありがとうございます。

引き続き読もう。関係ないけど町田リブロ閉店がショックすぎる。
