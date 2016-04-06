+++
date = "2016-04-06T22:26:17+09:00"
tags = ["node", "api", "javascript"]
title = "REST API のテストツール Frisby.js"

+++

REST API のテストツール Frisby.js を使ってみた。

<!--more-->

### 導入

[REST API Testing—Frisby.js](http://frisbyjs.com/)

npm で Frisby.js を導入する。

jasmine-node は依存パッケージで、config は必須ではないけど本番環境と開発環境の切り替えに使う。

```
$ npm i -D frisby jasmine-node config
```

ファイル構成はこんな感じになった。

```bash
$ tree
 .
 ├── package.json
 ├── node_modules
 ├── config
 │  └── test
 │      └── api
 │          └── v1
 │              ├── development.json
 │              └── production.json
 └── test
     └── api
         └── v1
             ├── bar.spec.js
             └── foo.spec.js
```

設定ファイルには、とりあえずAPIのURLだけ追加。

```
{
  "url": "http://api.example.com/v1",
  "url-auth": "http://test:password@api.example.com/v1"
}
```

`package.json` にテストを実行するタスクを追加する。

```json
  "scripts": {
    "test:api:development": "export NODE_ENV=development && export NODE_CONFIG_DIR=./config/test/api/v1 && jasmine-node ./test/api/v1",
    "test:api:production": "export NODE_ENV=production && export NODE_CONFIG_DIR=./config/test/api/v1 && jasmine-node ./test/api/v1"
  },
```

テストを実行する。

```bash
$ npm run test:api:production
```

### JSON レスポンス 内の型を検証する

例えば以下のようなレスポンスが返るAPIの場合。

```json
{
  "id": 1,
  "name": "hikarock",
  "url": "http://hika69.com"
}
```

以下の `users.spec.js` を用意する。

```js
var frisby = require('frisby');
var config = require('config');

frisby.create('GET:users')
  .get(config.get('url') + '/users/hikarock')
  .expectStatus(200)
  .expectHeaderContains('content-type', 'application/json')
  .expectJSONTypes({
    id: Number,
    name: String,
    url: String
  })
.toss();
```

### 配列の検証

配列内の1レコードの検証を行いたい場合は `expectJSONTypes` の第一引数に `users.0` のような独自の記法で指定する。
複数レコードの検証を行う場合は `users.*` のようにワイルドカードも指定できる。

例えば以下のようなレスポンスが返るAPIの場合。

```json
{
  users: [
    {
      "id": 1,
      "name": "hikarock",
      "url": "http://hika69.com"
    },
    {
      "id": 2,
      "name": "foo",
      "url": "http://example.com"
    }
    ...
  ]
}
```

1レコード目を検証する場合は以下のように記述する。

```js
frisby.create('GET:users')
  .get(config.get('url') + '/users')
  .expectStatus(200)
  .expectHeaderContains('content-type', 'application/json')
  .expectJSONTypes('users.0', {
    id: Number,
    name: String,
    url: String
  })
.toss();
```

### まとめ

導入も楽で、いい感じ。

今回はGETの正常系しか試してないので、実行順を考慮する必要のある更新系なども試してみたい。

