+++
date = "2016-07-24T14:51:49+09:00"
tags = ["stylus", "css"]
title = "SPAのよくあるレイアウトの雛形を作った"

+++

SPA を書き始める時に、大枠のレイアウトをどう組むか迷う。

<!--more-->

タイトルの **よくあるレイアウト** は、固定のヘッダ・フッタがあり、中央のメイン部が2カラムでスクロール可能なもの。要するに Mac の Finder のようなレイアウト。

![](https://cloud.githubusercontent.com/assets/236607/17078886/a8b40728-513a-11e6-9e17-2f24f8604f52.png)

Grid 系のフレームワークを検討はするものの Bootstrap のような重量級のフレームワークはおおげさに感じる。

また、Electron のような単一の実行環境の場合は、クロスブラウザを意識したフレームワークが無駄に感じられるので使いたくない、というのもある。

それで `position: absolute` を使った数十行の CSS (Stylus) をいつも書いている気がするので、以下のリポジトリにまとめた。

[hikarock/spa\-layout\-boilerplate](https://github.com/hikarock/spa-layout-boilerplate)

それぞれのプロジェクトから `@require 'layout'` して使うことを想定している。

セレクタがタグなのが行儀が悪いけれど、`header` とか `main` を同一アプリで繰り返し使わない、と割り切る。`nav` は繰り返し使いたくなりそうだけど、まあその時はクラスセレクタに変更すればよいだろう。
