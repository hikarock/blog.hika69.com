---
layout: post
title: "GitHub の非公開リポジトリを Bitbucket に引越"
date: "2014-11-18T11:34:01"
comments: true
tags: 
- github
- bitbucket
- git
---

GitHub のプライベートリポジトリの上限がきてしまったので、使用頻度の低いリポジトリを Bitbucket の非公開リポジトリに引越した。

<!--more-->

### コードの引越

Bitbucket に GitHub と同名のリポジトリを作成して、Bitbucket に push するだけ。
`{username}`、`{reponame}`は適宜読み替えてください。

```bash
$ cd {reponame}
$ git remote add bitbucket https://{username}@bitbucket.org/{username}/{reponame}.git
$ git push bitbucket master
```

### Issues の引越

これを使った。

[sorich87/github-to-bitbucket-issues-migration](https://github.com/sorich87/github-to-bitbucket-issues-migration)

Usage の通りに実行すると zip ファイルが生成された。

```bash
$ bundle install
$ bundle exec ruby cli.rb {username}/{reponame} {username} {password} exportfilename.zip
```

Bitbucket のウェブUIから zip ファイルをインポートできる。URL は以下。

`https://bitbucket.org/{username}/{reponame}/admin/issues/import-export`

### Wiki の引越

コードと同様に GitHub の wiki を clone して Bitbucket に push する。

以下のエントリを参考にした。

[Export your Issues and Wikis from Github Repo and Import to Bitbucket (Migration)](http://codetheory.in/export-your-issues-and-wikis-from-github-repo-and-import-to-bitbucket-migration/)

wiki を clone 。

```bash
$ git clone https://github.com/{username}/{reponame}.wiki.git
$ cd {reponame}.wiki
```

remote に bitbucket を追加。

```bash
$ git remote add bitbucket https://{username}@bitbucket.org/{username}/{reponame}.git/wiki
$ git remote -v
bitbucket       https://{username}@bitbucket.org/{username}/{reponame}.git/wiki (fetch)
bitbucket       https://{username}@bitbucket.org/{username}/{reponame}.git/wiki (push)
origin  https://github.com/{username}/{reponame}.wiki.git (fetch)
origin  https://github.com/{username}/{reponame}.wiki.git (push)
```

あらかじめデフォルトの wiki が生成されていて、普通に push すると reject されてしまったので `-f` オプションをつけた。

```bash
$ git push -f bitbucket master
```

以上で完了。GitHub Flavored Markdown が一部動いてないけど十分いい感じに移行できた。

