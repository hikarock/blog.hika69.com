+++
comments = true
date = "2013-06-01T22:28:00"
tags = ["javascript", "less", "grunt"]
title = "Grunt を導入"

+++

Gruntを[REMP](http://www.remp.jp/hello)に導入するためのメモ。

<!--more-->

### node.jsのインストール

node.jsを最新版にする。[node.js](http://nodejs.org/)のinstallボタンで落ちてくるパッケージから再インストールするだけ。

```
node -v
v0.10.8
npm -v
1.2.23
```

### grunt-cliのインストール

グローバルにgrunt-cliをインストール。

```
npm install -g grunt-cli
```

### プロジェクト毎にgruntを導入する

プロジェクトのルートでpackage.jsonを生成する。

```
npm init
```

プロジェクトのルートでgruntをインストール。

```
npm install grunt --save-dev
```

`--save-dev`をつけるとpackage.jsonにインストールしたタスクの依存関係が記録される。
package.jsonに依存関係が記録されていれば`npm install`だけで一括導入ができるので流用しやすそう。
なので`--save-dev`をつけることにする。

### タスクのインストール

1. grunt-contrib-uglify
    - JavaScriptを圧縮する
1. grunt-contrib-watch
    - Lessを監視して変更した時に別のタスクを呼び出す
1. grunt-contrib-less
    - Lessをcssにコンパイルしたり圧縮したりする

```
npm install grunt-contrib-uglify grunt-contrib-watch grunt-contrib-less --save-dev
```

以下だと便利タスク一式が一発で入るらしいけど、一個ずつ選びたいので使わない。

```
npm install grunt-contrib
```

### いまの設定

<script src="https://gist.github.com/hikarock/5690366.js"></script>

<blockquote class="twitter-tweet" lang="ja"><p>lessコンパイルで対象ファイル一個ずつ書かなくていい方法。ありがとうstackoverflow - <a href="http://t.co/GRA670cij9" title="http://stackoverflow.com/questions/15344584/grunt-0-4-less-task-how-to-not-concatenate-destination-files">stackoverflow.com/questions/1534…</a></p>&mdash; hika69さん (@hika69) <a href="https://twitter.com/hika69/status/340436916019286017">2013年5月31日</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

lessファイルとコンパイル先のcssを両方ワイルドカード指定する方法がわからなくてはまったけど解決。
このリンクを参考に設定した。

### 実行

デフォルトのタスクを実行する。

```
grunt
```

タスクを指定して実行する。

```
grunt less:dist
```

### 参考

- [Sumple Gruntfile](http://gruntjs.com/sample-gruntfile)
- [Grunt 日本語リファレンス](http://js.studio-kingdom.com/grunt)

