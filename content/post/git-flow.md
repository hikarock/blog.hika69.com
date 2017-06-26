---
title: "iOSアプリ開発のGit運用"
date: "2014-12-16T09:37:50"
comments: true
tags: 
- git
- git-flow
- ios
---

iOSアプリはリリースサイクルが長くなりがちなので、[GitHub Flow](https://gist.github.com/Gab-km/3705015)ではなく、[git-flow](http://danielkummer.github.io/git-flow-cheatsheet/index.ja_JP.html)を参考にしたフローを使っている。

<!--more-->

仕事もプライベートもこのフローで今のところ問題はない感じ (複数人開発含む)。

### 各ブランチの役割

基本的に普通の `git-flow` と変わらないけれど、AppStore がからむとこを中心に説明します。

- master
  - 現在AppStoreで配布中の最新バージョンと常にイコール
- release
  - リリース準備ができたら`develop`から分岐する。AppStoreでリリース申請通過後に__リリース完了した時点__で`master`と`develop`にマージする。リリース前に`release`上で変更があった場合は適宜`develop` `feature`にマージする。
  - リリース後にタグを打つのを忘れずに。`git tag -a v1.0.1 -m 'my version 1.0.1'`
- develop
  - 開発ブランチ。次期バージョンの要件を満たしたら`release`ブランチを作成する。
- feature
  - `develop`ブランチから作成して、作業完了したら`develop`にマージする。
- hotfix
  - AppStoreで配布中のバージョンで障害が発生した場合に`master`から作成する。AppStoreでリリース申請通過後に__リリース完了した時点__で`master`と`develop`に(必要であれば`release` `feature`にも)マージする。

### git-flow コマンドについて

`git-flow`コマンドは上記のようなフローでの開発を補助するgitのサブコマンドです。
各ブランチの作成やマージが便利になるコマンドが用意されているけど、プルリクエストやGithub上でのマージは考慮されていない。

上記フローを実践するために必ずしも`git-flow`コマンドを使う必要はない。僕は普通のgitコマンドでブランチ切ったり、Github上のプルリクエストからマージしたりしてる。

### 何がうれしいか

- iTunes Connectで申請中に、次のバージョンの機能を`develop`にさくさくマージできる
  - GitHub Flow だと`master`とトピックブランチだけなのでシンプルで良いんだけど、iOSアプリはリリース申請期間が長いし、その間も開発はしたいので無理が出てくる
  - 日次で`develop`と`release`ブランチをそれぞれ DeployGate 等に配布する仕組みがあれば検証する人も便利
- リリース版のアプリで不具合があった時に、常に`master`がリリースされたものと同一だと安心感がある。`hotfix`で障害対応が可能
- `release`ブランチをプルリクエストしておくと`What's New in this Version`をまとめるときに便利

