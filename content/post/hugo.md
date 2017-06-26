+++
date = "2015-11-15T13:07:37+09:00"
tags = ["hugo"]
title = "ブログを Hugo に変えた"

+++

[前回のエントリ](/blog/2015/07/01/roots/)で、Roots について調べていたけどなぜか Hugo でブログをリニューアルしていた。
Octopress にくらべて生成速度が速くてとても快適だ。

<!--more-->

[Hugo :: A fast and modern static website engine](https://gohugo.io/)

Hugo でのブログ構築についてはいろいろと情報が出回っているので、使用している Hugo 用のテーマ Casper のカスタマイズ方法などをまとめておく。

[vjeantet/hugo-theme-casper](https://github.com/vjeantet/hugo-theme-casper)

### Casper のカスタマイズ

`config.toml` に Casper 独自の設定を加える。

```
[params]
  customHeaderPartial = "customHeader.html"
  customFooterPartial = "customFooter.html"
```

`customHeaderPartial` `customFooterPartial` にファイルを指定して `layouts/partials` 以下に配置すると、それぞれ `</head>` と `</body>`の末尾で読み込んでくれる。

### シンタックスハイライト

`customHeaderPartial` の指定。

シンタックスハイライト用に `highlight.js` 用の CSS を読み込む。その他に、テーマの上書き用のCSSも読み込んでおく。

```
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/8.9.1/styles/paraiso.dark.min.css">
<link rel="stylesheet" href="/css/custom.css">
```

`customFooterPartial` で `highlight.js` の本体と実行処理を記述している。

```
<script src="https://yandex.st/highlightjs/8.0/highlight.min.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

### 意図しない箇所で組文字に変換される現象の回避

Casper テーマのデフォルトのスタイルでは見出しなどで「ページ」や「株式会社」などの文字列が単一の文字に変換される現象が発生していた。

[組文字](https://ja.wikipedia.org/wiki/%E7%B5%84%E6%96%87%E5%AD%97) というものらしいが `font-feature-settings` のスタイルを上記の上書き用 CSS でリセットすることで、回避できた。

```
* {
  -webkit-font-feature-settings: normal !important;
  -moz-font-feature-settings: normal !important;
  -o-font-feature-settings: normal !important;
}
```

詳細は以下。

- [フォントの機能を使えるCSS3のFont feature settingsとは](http://www.riaxdnp.jp/?p=5094)
- [font-feature-settings - CSS | MDN](https://developer.mozilla.org/ja/docs/Web/CSS/font-feature-settings)

### カバー画像の設定

カバー画像はパブリック・ドメインの NASA の写真を公開している Flickr アカウントから探した。

[NASA on The Commons | Flickr - Photo Sharing!](https://www.flickr.com/photos/nasacommons/)

`config.toml` に以下の記述を追加して `static` 以下に画像を配置する。

```
[params]
  cover = "images/cover.jpg"
```

