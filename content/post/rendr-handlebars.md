---
title: "rendr-handlebars で部分テンプレートを使う"
date: "2014-04-23T00:10:08"
comments: true
tags: 
- javascript
- rendr
- handlebars
- casto
---

[rendr-handlebars](https://github.com/rendrjs/rendr-handlebars)でpartial (部分テンプレート) が使えることにさっき気づいた。

<!--more-->

引数に`app/templates`以下のパスを指定する。

app/templates/home/index.hbs

```
{{partial "home/include"}}
```

上のようにパスだけ指定した場合は読み込み元と同じコンテキストで変数にアクセスすることができる。

app/templates/home/include.hbs

```
{{partial "home/include" name="taro"}}
```

こんな感じでコンテキストを渡すこともできる。

便利ですね。

