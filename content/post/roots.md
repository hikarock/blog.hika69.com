---
title: "静的サイトジェネレータ Roots の覚書"
date: "2015-07-01T22:01:34"
comments: true
tags: 
- roots
- node
---

定期的にブログをリニューアルしたくなる病が発症する。

<!--more-->

以下を条件に探してて roots がよさそうだったので試した。

- node製
- stylus
- ファイル構成がシンプル
- 拡張しやすい
- デフォルトのテーマがかっこいい

[Roots: Enlightened Static Sites](http://roots.cx/)

とりあえずインストールする。

```
$ npm install -g roots
```

ブログ用のテンプレートを追加する必要があるようなので、動画を見つつ進める。

[Roots Templates - YouTube](https://www.youtube.com/watch?v=RitO5dyPH44)

以下のテンプレートを追加。

```
$ roots tpl add blog https://github.com/carrot/roots-template-blog.git
```

`-t`でテンプレートを指定してプロジェクトを作成する。

```
$ roots new hikarock-blog -t blog
```

以下のようにデフォルトのテンプレートを指定しておけば、次から`-t`は不要になる模様。

```
$ roots tpl default blog
```

生成された。

```
.
├── app.coffee
├── app.production.coffee
├── assets
│   ├── css
│   │   ├── _settings.styl
│   │   └── master.styl
│   ├── img
│   └── js
│       └── main.coffee
├── node_modules
├── package.json
├── posts
│   ├── first_post.jade
│   └── second_post.jade
├── public
│   ├── css
│   │   ├── master.css
│   │   └── master.css.map
│   ├── img
│   ├── index.html
│   ├── js
│   │   ├── main.js
│   │   └── main.js.map
│   └── posts
│       ├── first_post.html
│       └── second_post.html
├── readme.md
└── views
    ├── _single.jade
    ├── index.jade
    └── layout.jade
```

ファイル構成はシンプルでよさそう。

サイトを立ち上げる。

```
$ roots watch
```

- デフォルトのテーマはないに等しいくらいに無印スタイル
- jade内に markdown を書くのがちょっと慣れない。インデントがだるい
- ~~GitHub味のmarkdown が解釈されていない。パーサーを marked とかに変えたらよいのだろうか~~
  - もともと marked だった。一部解釈できない記法が含んでいたので崩れていた
- デプロイは試してない

他はいい感じなので気が向いたら引っ越そう。

