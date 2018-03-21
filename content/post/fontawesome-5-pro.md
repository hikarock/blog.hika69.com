---
title: "Font Awesome Pro 導入メモ"
date: 2018-03-21T15:18:33+09:00
tags = ["fontawesome", "font", "vue", "node"]
---

Font Awesome Pro を購入してみた。

<!--more-->

無料版との機能の比較はこちら。

[Go Pro! | Font Awesome](https://fontawesome.com/pro)

購入するときに email の登録が必要で、同時に Font Awesome のアカウントが作成される。このアカウントで以下からログインする。

[Sign In | Font Awesome](https://fontawesome.com/sessions/sign-in)

### script タグで導入する

前ステップでログイン済みであれば、ヘッダーのリンクからフォント一式をダウンロードできる。

`<head>` 内に以下を記述すれば全てのアイコンが利用できる。

```html
<script defer src="/static/fontawesome/fontawesome-all.js"></script>
```

ref: [Get Started | Font Awesome](https://fontawesome.com/get-started)

### Node から導入する

Font Awesome Pro を Node から利用するにはちょっと設定が必要。

[How to Use | Font Awesome](https://fontawesome.com/how-to-use/use-with-node-js#pro)

前ステップでログイン済みであれば `{TOKEN}` に自分のトークンが表示されている。

```
% npm config set @fortawesome:registry https://npm.fontawesome.com/{TOKEN}
```

このコマンドを実行すればプロジェクトを問わず Font Awesome Pro が `npm install` できるようになる。

特定のプロジェクトで有効にしたい場合や、リポジトリを共有しているチームでも利用したい場合などは、プロジェクト直下に `.npmrc` を作成して `@fortawesome:registry=https://npm.fontawesome.com/{TOKEN}` を保存する。

### Vue から利用する

コンポーネントとして `FortAwesome/vue-fontawesome` を利用する。

[FortAwesome/vue-fontawesome: Font Awesome 5 Vue component](https://github.com/FortAwesome/vue-fontawesome)

```
% npm i --save @fortawesome/vue-fontawesome
```

`@fortawesome/fontawesome` にはアイコンは含まれないので、利用したいスタイル別のアイコンを追加でインストールする。

```
% npm i --save @fortawesome/fontawesome
% npm i --save @fortawesome/fontawesome-pro-light
% npm i --save @fortawesome/fontawesome-pro-regular
% npm i --save @fortawesome/fontawesome-pro-solid
```

ルートのコンポーネントで利用したいスタイルを import して追加する。

```js
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
import fontawesome from '@fortawesome/fontawesome'

import proSolid from '@fortawesome/fontawesome-pro-solid'
import proRegular from '@fortawesome/fontawesome-pro-regular'
import proLight from '@fortawesome/fontawesome-pro-light'

fontawesome.library.add(proSolid, proRegular, proLight)

export default {
  name: 'App',
  components: {
    FontAwesomeIcon
  }
}
```

アイコンを利用する子コンポーネントの例。

```html
<template>
  <div>
    <font-awesome-icon :icon="['fal', 'camera-retro']" />
  </div>
</template>

<script>
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
export default {
  name: 'User',
  components: {
    FontAwesomeIcon
  }
}
</script>
```

`:icon` に指定している配列の1要素目にスタイルを指定できる。指定しなかった場合はデフォルトの `fas` が使われる。

- `fas` (default): Solid
- `fal`: Light
- `far`: Regular

