+++
comments = true
date = "2014-07-31T23:25:25"
tags = ["meteor", "triph"]
title = "Meteor でアプリのランディングページを作った"

+++

[triph](http://triph.jp)のランディングページ(というかアプリ全体...)をHerokuに乗せていたんだけど、ちょっとおおげさな感じがしてきたので[Meteor](https://meteor.com)に移した。

<!--more-->

- [hikarock/triph-web](https://github.com/hikarock/triph-web)

### Meteorにデプロイ

アカウントが無い場合はあらかじめ[Meteor](https://meteor.com)でサインアップして`$ meteor login`しておく。

Meteorのサブドメインでサイトを運用する場合は、以下のようにコマンドを叩くだけでデプロイできる。簡単すぎる。

```sh
$ meteor deploy hikarock.meteor.com
```

今回は独自ドメインの`triph.jp`で運用したかったので、以下のようにデプロイする。

```sh
$ meteor deploy triph.jp
```

登録されているサイト一覧を確認。

```sh
$ meteor list-sites
hikarock.meteor.com
triph.jp
```

### DNSの設定

この状態だと`triph.jp`を参照できないので、DNSにAレコードを追加する。
`www.triph.jp`のようにサブドメありでよければCNAMEレコードの値に`origin.meteor.com`を追加する。

DNSの管理画面で以下のように設定。

![](/images/post/meteor-1.png)

`origin.meteor.com`のIPは`dig`で調べた。

```sh
$ dig origin.meteor.com
```

#### 参考にした記事

- [javascript - Meteor.js deploy to &quot;example.com&quot; or &quot;www.example.com&quot;? - Stack Overflow](http://stackoverflow.com/a/15706411)

### デプロイ可能なユーザーを追加する

triphを一緒に作っている [@hypermkt](https://twitter.com/hypermkt) もデプロイ可能する。

```sh
$ meteor authorized triph.jp --add hypermkt
```

triphのiPhoneアプリはもうちょっとで申請できそう。作り始めていろいろ回り道してたら1年たってしまった。

[triph | あなたの散歩を旅行に変えます](http://triph.jp)


