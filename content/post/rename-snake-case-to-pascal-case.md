+++
date = "2016-01-07T21:46:22+09:00"
draft = true
tags = ["php", "regex"]
title = "スネークケースのファイル名・クラス名をパスカルケースにリネームする"

+++

あるプロジェクトで [PSR-0](http://www.php-fig.org/psr/psr-0/) に対応する作業に関連して、スネークケースのファイル名とクラス名をパスカルケースに変換する必要があった。

<!--more-->

### ファイル名の変換

Mac に rename コマンドをインストールする。

```bash
$ brew install rename
```

変換前

```bash
$ ls -l
-rw-r--r-- 1 hikarock staff 0  1  7 21:47 foo_bar_buzz.php
-rw-r--r-- 1 hikarock staff 0  1  7 21:47 hoge_fuga.php
```

rename コマンドを叩く。

```bash
$ rename -f 's/(^|_)(.)/\U$2\E/g' *.php
```

変換後

```bash
$ ls -l
total 0
-rw-r--r-- 1 hikarock staff 0  1  7 21:47 FooBarBuzz.php
-rw-r--r-- 1 hikarock staff 0  1  7 21:47 HogeFuga.php
```

よさそう。

### クラス名の変換

Mac に標準で入っている sed は使わずに GNU sed をインストールする。

参考: [Homebrew を使って OSX に GNU sed を入れる - おともだちティータイム](http://shunirr.hatenablog.jp/entry/2012/12/19/160544)

```bash
$ brew install gnu-sed
```

変換前。対象はクラス名だけにしたいので、他のスネークケースの変数名や関数名を含んだ状態で、余計な箇所が変更されていないか確認しながら検証する。

```
<?php

class foo_bar_buzz_controller extends bar_buzz_controller {
  private $hoge_fuga;
  public function set_hoge_fuga() {
    //
  }
}
```

gsed コマンドで変換する。`/^class.*\{$/` で対象行でクラス宣言の行だけに絞り込んでいる ( `{` は改行した次の行に書く場合の方が多いかもしれないけど、このプロジェクトでは `class` と同一行に書いている)。


```bash
$ gsed -i -r '/^class.*\{$/ s/_(.)/\U\1\E/g' *.php
```

変換後

```
<?php

class fooBarBuzzController extends barBuzzController {
  private $hoge_fuga;
  public function set_hoge_fuga() {
    //
  }
}
```

クラス名の先頭が小文字のままだった。

今回はたまたま対象クラス名の末尾に必ず `Controller` が含まれていたので、以下のようにしてお茶を濁した。

```
$ perl -pi -e 's/ (\w+?)Controller/ \u\1Controller/g' *.php
```

perl コマンドを使ったのは sed で最短一致ができなかったため。

```
<?php

class FooBarBuzzController extends BarBuzzController {
  private $hoge_fuga;
  public function set_hoge_fuga() {
    //
  }
}
```

完了。

