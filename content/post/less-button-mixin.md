---
title: "Less でボタンの mixin"
date: "2013-12-05T00:02:29"
comments: true
tags: 
- less
- stobo
---

以下の画像ボタンを再現するLessのmixin書いた。

<!--more-->

![](/images/post/less-button-mixin-1.png)

上が画像、下がLess版。まあまあ再現できた。

色は引数で変えられるようにした。

![](/images/post/less-button-mixin-2.png)

```css
.stbd-btn(@color) {
  margin: auto;
  padding: 4px;
  #gradient > .vertical(fade(darken(greyscale(@color), 90%), 30%),
                        fade(lighten(greyscale(@color), 10%), 10%));
  border-radius: 8px;

  a {
    color: #fff;
    font-family: @kaku-go;
    letter-spacing: 0.1em;
    font-size: 1.3em;
    display: block;
    padding: 17px;
    text-align: center;
    text-shadow: 2px 2px 1px darken(@color, 5%);
    border-radius: 6px;
    border: 1px solid lighten(@color, 30%);
    #gradient > .vertical(lighten(@color, 15%), @color);

    &:hover {
      #gradient > .vertical(lighten(@color, 25%), lighten(@color, 10%));
      text-decoration: none;
    }
  }
}
```

`darken`とか`lighten`とか便利すわ。


