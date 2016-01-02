+++
date = "2016-01-02T18:56:05+09:00"
draft = false
tags = ["node"]
title = "Node の CLI アプリと Vim などのエディタを連携する"
+++

`git commit` をコマンドラインから実行した時に Vim が自動で開いてコミットコメントを入力できるが、このような挙動を Node のCLIアプリを自作するときにどうやるか調べた。

<!--more-->

ずばりな回答が以下。

[Is there a way to quit Node.js and launch VIM on a file? - Stack Overflow](http://stackoverflow.com/questions/15726004/is-there-a-way-to-quit-node-js-and-launch-vim-on-a-file)

この回答では Vim を固定で指定しているが `process.env.EDITOR` で Vim 以外のエディタにも対応できる。

```javascript
var fs = require('fs');
var file = '~/.foo.txt';
var editor = require('child_process').spawn(process.env.EDITOR, [file], {stdio: 'inherit'});

editor.on('exit', function() {
  var text = fs.readFileSync(file, 'utf-8');
  console.log(text);
});
```

エディタの終了を `exit` イベントで検知してコールバック内でファイルの内容を取得している。

毎回内容をクリアする場合は、`fs.readFile[Sync]` した後に `fs.unlink[Sync]` すればよい。

