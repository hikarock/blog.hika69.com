---
layout: post
title: "拡大・縮小自在な LESS の mixin の作り方"
date: "2014-01-02T22:40:46"
comments: true
tags: 
- less
- stobo
---

ストーリーボードではスライドをHTMLとCSSで描画するため、同じレイアウト・見た目で縮尺だけ変えたいことがある。

<!--more-->

#### エディタページ

![](https://dl.dropboxusercontent.com/u/459142/img/blog/less-mixin-1.png)

#### スライドページ(PC用)

![](https://dl.dropboxusercontent.com/u/459142/img/blog/less-mixin-2.png)

#### スライドページ(スマートフォン用)

![](https://dl.dropboxusercontent.com/u/459142/img/blog/less-mixin-3.png)

#### ブログパーツ

![](https://dl.dropboxusercontent.com/u/459142/img/blog/less-mixin-4.png)

こんな感じにいろんなサイズで描画したい。

でもサイズごとにCSSを書くのは大変なので、倍率を渡すとよろしく描画してくれるmixinを定義しています。

できるだけ`em`のように相対単位を指定して、サイズ系の値にはすべて`@multiplier`を乗じています。

#### mixin (一部抜粋)

```css
.stbd-slide(@multiplier) {

  padding: 25px * @multiplier;
  color: gray;
  font-family: @maru-go;
  word-wrap: break-word;
  font-size: @font-size-s * @multiplier;

  img {
    margin-bottom: @font-size-s;
    width: 85%;
  }

  h1, h2, h3, h4, h5, h6 {
    color: gray;
    font-family: @maru-go;
    line-height: 1.25em;
    margin-top: 0;
    margin-bottom: @font-size-s * @multiplier;
  }

  h1 {
    font-size: @font-size-s * 3 * @multiplier;
  }
```

#### 呼び出し側

こんな感じで呼び出し側でサイズを指定します。

```css
.stbd-slide(0.8);
```

レスポンシブ対応もこれだけ。
(以下はBootstrap 3を併用している場合)

```css
.stbd-slide(1);

@media (min-width: @screen-sm-min) {
  .stbd-slide(1.5);
}
@media (min-width: @screen-md-min) {
  .stbd-slide(2.5);
}
@media (min-width: @screen-lg-min) {
  .stbd-slide(3);
}
```

シンプルですね。

