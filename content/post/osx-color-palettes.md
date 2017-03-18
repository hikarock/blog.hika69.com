---
layout: post
title: "Xcode でカラーパレットを保存して共有したい"
date: "2014-12-21T00:31:08"
comments: true
tags: 
- mac
- xcode
---

と思ったので調べた。Xcode というより Mac OS X の機能ぽい (OS X Yosemite & Xcode 6.1.1 の場合。他のバージョンで使えるかはわからない)。

<!--more-->

Xcode でカラーパレットを開いて、中央のタブ (Color Palettes) を選択する。

![](/images/post/osx-color-palettes-1.png)

左端の歯車のアイコンをクリックして`New`を選択するとカラーパレットを作成することができる。

保存したカラーパレットは`~/Library/Colors/`以下に`Foo.clr`のようなファイル名で保存されているので、このファイルをプロジェクトメンバー間で共有すると便利そう。

