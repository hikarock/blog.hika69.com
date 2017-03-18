---
layout: post
title: "Xcode のストーリーボードで背後の要素を選択する"
date: "2015-01-08T10:54:17"
comments: true
tags: 
- xcode
---

Xcode のストーリーボード上で背後の要素をあれこれしたかったけど、前面の要素がじゃまして選択できなくて困ってたが、コマンドで簡単に選択できることがわかった。

<!--more-->

選択したい要素にポインタを移動して、`Command + Shift` を押しながらクリックすると、同じ座標にある View の一覧が表示される。

![](/images/post/xcode-select-bg-elements.png)

ここから View を選択すればOK。

