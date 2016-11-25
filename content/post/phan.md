+++
date = "2016-11-25T17:49:12+09:00"
tags = ["php"]
title = "開発環境に Phan を導入した"

+++

開発環境 (Mac) で PHP の静的解析がしたくて Phan を導入した。

さくっといけるかと思いきやいろいろつまづいたので備忘。

<!--more-->

### 参考

- [etsy/phan: Static analyzer for PHP](https://github.com/etsy/phan)
- [Phan静的解析がもたらす大PHP型検査時代 \- pixiv inside](http://inside.pixiv.net/entry/2016/11/11/202656)

### PHP7 を準備

phan が要求する PHP7 を導入する。今回は PHP5.6 も同居させたいので phpbrew を使う。

以下のエントリが参考になった。

[MacでのPHP開発はphpbrewが非常に良い \| karakaram\-blog](http://www.karakaram.com/mac-install-phpbrew)

#### phpbrew をインストール

以下のドキュメントを見つつ進める。

[phpbrew/phpbrew#install](https://github.com/phpbrew/phpbrew#install)

phpbrew をダウンロードしてパスの通ったところに配置する。

```
% curl -L -O https://github.com/phpbrew/phpbrew/raw/master/phpbrew
% chmod +x phpbrew
% mv phpbrew ~/bin/
```

初期設定のコマンドを実行。

```
% phpbrew init
```

`.zshrc` に以下の記述を追記。

```sh
if [ -d ${HOME}/.phpbrew ]; then
  source $HOME/.phpbrew/bashrc
fi
```

利用可能なPHPのバージョンを調べる。

```
% phpbrew known
```

7系で最新のバージョンをインストールする。

```
% phpbrew install 7.0.13 +default
```

ちなみに 5.3.x のような古いバージョンを入れたい時は `--old` オプションで表示される。

```
% phpbrew known --old
```

インストールが完了したらバージョンを切り替える。

```
% phpbrew switch php-7.0.13
```

### phan をインストール

[Getting Started · etsy/phan Wiki](https://github.com/etsy/phan/wiki/Getting-Started#from-phanphar)

"From Phan.phar" の手順で進める。

```
% curl -L https://github.com/etsy/phan/releases/download/0.6/phan.phar -o phan.phar;
% mv phan.phar ~/bin
```

[pixiv さんのエントリ](http://inside.pixiv.net/entry/2016/11/11/202656) を参考に、以下のスクリプトを、パスの通ったところに設置する。

```sh
#!/bin/sh

/Users/hikarock/.phpbrew/php/php-7.0.13/bin/php ${PHAN_BIN:-/Users/hikarock/bin/phan.phar} "$@"
```

### php-ast をインストール

phan が依存している php-ast をインストールする。

[nikic/php\-ast: Extension exposing PHP 7 abstract syntax tree](https://github.com/nikic/php-ast#installation)

```sh
% git clone git@github.com:nikic/php-ast.git
% cd php-ast
% phpize
% ./configure
% make
% sudo make install
```

`php -i | grep extension_dir` などで extension のインストール先を確認して、該当ディレクトリに `ast.so` が配置されていることを確認。

ちなみに以下に配置されていた。

```
% pwd
/Users/hikarock/.phpbrew/php/php-7.0.13/lib/php/extensions/no-debug-non-zts-20151012
```

php.ini の `[php]` 配下に以下を追記して extension を読み込む。php.ini の場所は `php --ini` で確認する。

```
extension = "ast.so"
```

### 足りてなかった extension を追加

phan が依存している `sqlite3` が足りていなかったので phpbrew で追加する

```
% phpbrew ext install sqlite3
```

ここでようやく phan コマンドが正常動作した。

```
phan -h
```

### .phan/config.php の設置

静的解析を行いたいプロジェクトのディレクトリ直下に `.phan/config.php` を作成する。

```php
<?php
return [
    // 解析対象のディレクトリ
    'directory_list' => [
        'lib/'
    ],
    // 解析対象外のディレクトリ
    'exclude_analysis_directory_list' => [
        'vendor/'
    ],
];
```

実行する。

```
% cd /path/to/project
% phan --progress-bar -ophan.log
```

大量の解析結果が `phan.log` に出力された。めでたしめでたし。
