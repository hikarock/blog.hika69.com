+++
date = "2016-11-07T23:14:10+09:00"
tags = ["polymer", "web-components", "javascript"]
title = "HyperTalk の play コマンド風のカスタム要素 <x-play> をつくった"

+++

20 年ほど前に買った [HyperCardの本](http://booklog.jp/item/1/479529609X) をパラパラめくっていたら、`play` というコマンドが紹介されていた。音色とメロディーを指定して、音楽を再生することができる便利なコマンドだ。

<!--more-->

[HyperTalk Commands \- play](http://www.kreativekorp.com/docs/openxion/1.4/manual/hyp/cm/play.html)

```
play "harpsichord" "ch d e f g a b c5w"
```


Web で似たようなことができると面白そうだな、と思いたって Polymer のカスタム要素で同じようなインターフェースを実装してみた。

- [hikarock/x\-play](https://github.com/hikarock/x-play)
- [DEMO (音が出ます)](https://hikarock.github.io/x-play/)

記述方法はこんな感じ。

```html
<x-play sound="piano" tempo="80" repeat="1" melody="d2 e2 c2 c1 g1"></x-play>
```

タグを並べて、同時に再生もできるようです (たくさん並べたら音がずれそう...)

```html
<x-play sound="snare" tempo="100" repeat="2" melody="-  c4  - c4  - c4  - c4"></x-play>
<x-play sound="kick"  tempo="100" repeat="2" melody="c4 c4 c4 c4 c4 c4 c4 c4"></x-play>
```

### 動作環境

最新版の Mac Chrome / Safari / Firefox あたりで動作を確認した。iOS の Safari だと音がでないが、まだ原因は調べてない。

### 音源について

[魔王魂](http://maoudamashii.jokersounds.com/) さんの音源を利用させていただきました。

[楽器の無料・フリー効果音素材/魔王魂](http://maoudamashii.jokersounds.com/list/se12.html)

> まず最初に断っておきますが、「このページにある素材の使い道は作者の私すら分かりかねます。」

とありますが、まさにぴったりの素材でとても助かりました。

### 余談

@hideack が HyperTalk の play コマンドよりも歴史の古い MML という言語を教えてくれた。

以下のページによると初期の MML は 1978 年にシャープ社のコンピュータに搭載されたらしい。

[Music Macro Language \- Wikipedia](https://en.wikipedia.org/wiki/Music_Macro_Language)
