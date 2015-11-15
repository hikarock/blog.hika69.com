---
layout: post
title: "editorify というライブラリを作った"
date: "2015-02-13T00:55:57"
comments: true
tags: 
- javascript
- jquery
---

エディタリファイと読むんだろうか。とにかく`fy`ってつけたかっただけな感じで言いづらい jQuery プラグインを作った。

<!--more-->

[hikarock/editorify](https://github.com/hikarock/editorify)

ひとことでいうと `<textarea>` を使って簡単なウィジウィグとか作れるライブラリ。
例えば、markdown のリンクを挿入する場合はこんな感じに書く。

```javascript
$('.link').on('click', function (evt) {
  $('.editor').editorify([
    ['clear'],
    ['insert', '[](http://example.com)'],
    ['start'],
    ['right', 1]
  ]);
});
```

`left` とか `start` とかでカーソル移動できるので `[]` の中にカーソルが移動している状態になる。[DEMO](https://dl.dropboxusercontent.com/u/459142/editorify/index.html)はこちら。

[Stobo](http://www.storyboards.jp/) のエディタ部分を絶賛改造中でモンスターブランチがもうスカイツリーくらいに育っているんだけど、その中の成果が分割されて editorify になった。
