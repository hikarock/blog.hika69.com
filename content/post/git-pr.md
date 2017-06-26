---
title: "hub コマンドで GitHub と GHE に PullRequest する"
date: "2015-02-09T19:36:38"
comments: true
tags: 
- git
- github
---

GitHubでは以下のalias (リンク参照) を設定して`git pr master`のように使っていたけれど、GHEで動かなかったので`.gitconfig`を編集した。

<!--more-->

- [GitHub へ簡単に Pull Request を送る - Qiita](http://qiita.com/banyan@github/items/3c82fa357fe355c16f32#comment-cf6dbbe34ce80d0ac3a7)

```
[alias]
  pr = !REP_OWNER=$(git remote -v | grep -e '(push)' | awk '{print $2}' | awk -F '[:/]' '{print $2}') && hub pull-request $2 -h $REP_OWNER:`git rev-parse --abbrev-ref HEAD` -b $REP_OWNER:$1 && shift "$#" 1>/dev/null 2>/dev/null
```

快適。

