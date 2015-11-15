---
layout: post
title: "npm init 時のデフォルト設定をカスタマイズする"
date: "2015-02-21T19:17:39"
comments: true
tags: 
- npm
- node
---

`npm init`した時にlicenceの種類やauthorの情報を自動的にいれる設定。

<!--more-->

```
$ npm config set init.author.name "hikarock"
$ npm config set init.author.email i@hika69.com
$ npm config set init.author.url http://blog.hika69.com
$ npm config set init.license MIT
```

`~/.npmrc`というファイルが作られて以下のように設定されている。

```
init.license=MIT
init.author.name=hikarock
init.author.email=i@hika69.com
init.author.url=http://blog.hika69.com
```

また`npm config list`でも現在の設定を確認できる。

[config | npm Documentation](https://docs.npmjs.com/cli/config)

