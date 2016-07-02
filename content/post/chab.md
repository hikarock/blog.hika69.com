+++
date = "2016-07-01T14:18:19+09:00"
tags = ["javascript"]
title = "Web SQL Database を使って <table> タグを SQL ライクにセレクトするライブラリ chab を再公開した"

+++

chab という6年前に作ったライブラリなんだけど、しばらく放置してたら動かなくなっていたので、改修して GitHub で再公開した。

<!--more-->

### リポジトリ

詳細は [README.md](https://github.com/hikarock/chab) にて。

### デモ

[https://hikarock.github.io/chab/](https://hikarock.github.io/chab/)

### 今回の修正内容

[Web SQL Database](https://www.w3.org/TR/webdatabase/) の openDatabase の引数を2つしか指定していなかったのが原因で動作していなかったようなので修正した。

第3引数は `displayName` だけど Chrome Developer Tools を見る限りだと第1引数に指定した値が表示されている。どこで使われるのかはわからなかった。

![](https://dl.dropboxusercontent.com/u/459142/img/blog/chab-01.png)

第4引数は `estimatedSize` はDBのサイズを byte で指定する。

```js
this.db = openDatabase("chabdb", "1.0.1", "chab", 1024 * 1024);
```
