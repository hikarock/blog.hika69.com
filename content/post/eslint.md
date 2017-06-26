+++
comments = true
date = "2015-05-05T17:03:27"
tags = ["vim", "javascript"]
title = "eslint を vim-watchdogs から使う"

+++

vim-watchdogs から eslint が使えるように設定したのでメモ。

<!--more-->

[osyo-manga/vim-watchdogs](https://github.com/osyo-manga/vim-watchdogs)

eslint をグローバルにインストールする。

[ESLint - Pluggable JavaScript linter](http://eslint.org/)

```bash
$ npm install -g eslint
```

`.vimrc`でファイルタイプが`javascript`の場合に eslint コマンドが実行されるように設定する。


```vim
let g:watchdogs_check_BufWritePost_enable = 1
let g:watchdogs_check_CursorHold_enable = 1

"
" 以下を追記
"
let g:quickrun_config = {
\   "javascript/watchdogs_checker" : {
\     "type" : "eslint"
\   }
\ }
call watchdogs#setup(g:quickrun_config)
```

これで`:WatchdogsRun`した時に eslint が実行される。

ただ、このままだと ES6/JSX の構文を使っているとエラーとして検出されてしまうので、`.eslintrc`の`ecmaFeatures`に値を指定して機能毎に有効にする。

```javascript
{
  "ecmaFeatures": {
    "templateStrings": true
  }
}
```

`.eslintrc`の設定により以下の構文はエラーとして検出されない。

```javascript
console.log(`Hello, ${name}!`);
```

その他の機能毎の設定方法は以下を参照。

[Documentation - ESLint - Pluggable JavaScript linter](http://eslint.org/docs/user-guide/configuring.html)

