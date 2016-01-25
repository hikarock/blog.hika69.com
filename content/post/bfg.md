+++
date = "2016-01-25T20:04:28+09:00"
draft = false
tags = ["git", "github"]
title = "BFG で巨大なファイルを削除して GitHub に push するまで"

+++

あるリポジトリを GitHub に push した時に、100MB 以上のファイルが含まれていたため拒否されてしまった。

<!--more-->

```
% git push origin master
...
remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
remote: error: Trace: xxx
remote: error: See http://git.io/iEPt8g for more information.
remote: error: File big.exe is 160.21 MB; this exceeds GitHub's file size limit of 100.00 MB
To git@github.com:hikarock/some-big-repo.git
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'git@github.com:hikarock/some-big-repo.git'
```

[Git Large File Storage](https://git-lfs.github.com/) も使ってみたかったけど課金してないので、素直に `more information` のリンクを開く。

[Removing files from a repository's history - User Documentation](https://help.github.com/articles/removing-files-from-a-repository-s-history/)

2パターンのファイル削除方法が掲載されていて、今回はだいぶ昔にコミットされたファイルを消したかったので `Removing a file added in an older commit` で紹介されている BFG を使う。

[BFG Repo-Cleaner by rtyley](https://rtyley.github.io/bfg-repo-cleaner/)

BFG のサイトから jar ファイルをダウンロードして、`.zshrc` にエイリアスを設定した。

```
alias bfg='java -jar ~/bin/bfg-1.12.8.jar'
```

リポジトリに移動して、エラーの原因になっている巨大ファイルを `git rm` する。

```
% cd some-big-repo
% git rm big.exe
rm 'big.exe'
% git ci -m '巨大なファイルを削除'
[master xxx] 巨大なファイルを削除
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100755 big.exe
```

対象リポジトリ内の100M以上のファイルを対象に `bfg` を実行。

```
% cd ..
% bfg --strip-blobs-bigger-than 100M some-big-repo
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.

Using repo : some-big-repo/.git

...

Deleted files
-------------

        Filename                       Git id
        --------------------------------------------------
        big.exe                      | xxx (160.2 MB)

In total, 7 object ids were changed. Full details are logged here:

        /Users/hikarock/some-big-repo.bfg-report/2016-01-25/20-00-01

BFG run is complete! When ready, run: git reflog expire --expire=now --all && git gc --prune=now --aggressive

Has the BFG saved you time?  Support the BFG on BountySource:  https://j.mp/fund-bfg
```

メッセージの最後に出ている通りに `git reflog` を実行する。

```
% git reflog expire --expire=now --all && git gc --prune=now --aggressive
```

もういちど `git push` を実行。

```
% git push origin master
...
To git@github.com:hikarock/some-big-repo
 * [new branch]      master -> master
```

push できた。

