+++
date = "2016-05-20T23:29:22+09:00"
tags = ["vim", "php"]
title = "PSR-2 の最終行の空行についての規約を勘違いしていた"

+++

PHP の[PSR-2](http://www.php-fig.org/psr/psr-2/) で「最終行に空行を入れる」という規約がある。

<!--more-->

> All PHP files MUST end with a single blank line.

この規約に従っていたつもりなんだけど、勘違いしていたことがわかった[^1]ので備忘。

[UNIXの行の定義とviの改行コード | Firegoby](https://firegoby.jp/archives/2391) より引用

> POSIXでは「Unix においてテキストファイルとは行の集合であり、行とは改行文字で終わるものと定義される」と定義されている。

> viのような由緒正しきテキストエディターは、この定義を忠実に守っており、行末には必ず改行が入る仕様になっている。

このため Vim で最終行に __改行が見える__ ように記述している場合、最終行に改行が連続(LFLF)して入力されている。

GitHub でも最終行の空行は表示されない。本当はLFが入力されていて PSR-2 の規約を満たしていたのに、Vim でも GitHub でも見えない[^2]ので LFLF を入力してしまっていた。

ということで PSR-2 に従う場合、Vim では最終行に改行が見えない状態であるのが正解のようだ。

[^1]: 発端はプロジェクトの新メンバーのエディタが Atom で、僕が Vim だったため表示が違ったこと
[^2]: GitHub の表示については PHP-FIG のメンバーである [CakePHP](https://github.com/cakephp/cakephp/blob/master/src/Collection/Collection.php#L101) や [Drual](https://github.com/drupal/drupal/blob/8.1.x/core/lib/Drupal.php#L724) の .php ファイルの最終行の表示を確認した。

