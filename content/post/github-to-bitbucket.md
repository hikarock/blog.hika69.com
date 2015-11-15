---
layout: post
title: "Github の非公開リポジトリを Bitbucket に引越"
date: "2014-11-18T11:34:01"
comments: true
tags: 
- github
- bitbucket
---

Githubのプライベートリポジトリの上限がきてしまったので、使用頻度の低いリポジトリをBitbucketの非公開リポジトリに引越した。

<!--more-->

### コードの引越

BitbucketにGithubと同名のリポジトリを作成して、Bitbucketにpushするだけ。
`{username}`、`{reponame}`は適宜読み替えてください。

```bash
$ cd {reponame}
$ git remote add bitbucket https://{username}@bitbucket.org/{username}/{reponame}.git
$ git push bitbucket master
```

### Issues の引越

これを使った。

[sorich87/github-to-bitbucket-issues-migration](https://github.com/sorich87/github-to-bitbucket-issues-migration)

Usage の通りに実行するとzipファイルが生成された。

```bash
$ bundle install
$ bundle exec ruby cli.rb {username}/{reponame} {username} {password} exportfilename.zip
```

BitbucketのウェブUIからzipファイルをインポートできる。URLは以下。

`https://bitbucket.org/{username}/{reponame}/admin/issues/import-export`

### Wiki の引越

コードと同様にGithubのwikiをcloneしてBitbucketにpushする。

以下のエントリを参考にした。

[Export your Issues and Wikis from Github Repo and Import to Bitbucket (Migration)](http://codetheory.in/export-your-issues-and-wikis-from-github-repo-and-import-to-bitbucket-migration/)

wikiをclone。

```bash
$ git clone https://github.com/{username}/{reponame}.wiki.git
$ cd {reponame}.wiki
```

remoteにbitbucketを追加。

```bash
$ git remote add bitbucket https://{username}@bitbucket.org/{username}/{reponame}.git/wiki
$ git remote -v
bitbucket       https://{username}@bitbucket.org/{username}/{reponame}.git/wiki (fetch)
bitbucket       https://{username}@bitbucket.org/{username}/{reponame}.git/wiki (push)
origin  https://github.com/{username}/{reponame}.wiki.git (fetch)
origin  https://github.com/{username}/{reponame}.wiki.git (push)
```

あらかじめデフォルトのWikiが生成されていて、普通にpushするとrejectされてしまったので`-f`オプションをつけた。

```bash
$ git push -f bitbucket master
```

以上で完了。Github Flavored Markdown が一部動いてないけど十分いい感じに移行できた。

