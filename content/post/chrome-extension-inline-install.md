+++
date = "2016-05-27T00:20:53+09:00"
tags = ["javascript", "chrome-extension"]
title = "Chrome 拡張の Inline Installation に対応する"

+++

Chrome 拡張を Chrome ウェブストア に遷移せずにインストールできる Inline Installation に対応した時の覚書。

<!--more-->

### オフィシャルのドキュメント

[Using Inline Installation - Google Chrome](https://developer.chrome.com/webstore/inline_installation)

### 手順

あらかじめ Chrome ウェブストアのデベロッパー ダッシュボードと Google Search Console で、Inline Installation を導入したいウェブサイトを設定する。

Inline Installation を設置したいサイトの head 内に link タグを挿入する。`{itemId}` に該当の拡張機能のURLに含まれるIDを指定する。

```html
<link rel="chrome-webstore-item" href="https://chrome.google.com/webstore/detail/{itemId}">
```

リンクやボタンなどを押下したタイミングで Inline Installation を実行する。

```javascript
$(document).on('click', '.install-button', function(evt) {
    evt.preventDefault();

    if (!('chrome' in window)) {
        console.log('拡張機能をインストールするには、Google Chrome をご利用ください');
        return;
    }

    chrome.webstore.install('', function() {
        // successCallback
    }, function(errMessage, code) {
        // failureCallback
        console.log(errMessage, code);
    });
});
```

### インストール済みか判定して表示を変更する

`chrome.app.isInstalled` の値を判定すればよいだけかと思っていたけど、Chrome 拡張機能の場合はこの値は常に false になる。

[chrome.app.isInstalled always returns false for Google Chrome extensions? - Stack Overflow](http://stackoverflow.com/questions/18275960/chrome-app-isinstalled-always-returns-false-for-google-chrome-extensions)

この回答によると `isInstalled` が使えるのは Hosted apps の場合のみで、拡張機能の場合は [Content Script](https://developer.chrome.com/extensions/content_scripts) で以下のように body に判定用の DOM を挿入しないといけないようだ。

```javascript
$('body').append($('<div>').attr('id', 'extension-is-installed'));
```

```javascript
if ($('#extension-is-installed').length > 0) {
  $('#install-button').css('display', 'none');
}
```

このようにインストール済みか判定をしないとどうなるかというと、インストール済みでボタンを押下しても初回と同様のダイアログが表示されて、再インストールが実行されるだけ。

インストール済みの場合に拡張機能のチュートリアルを表示したい、とかでない限りはこの対応はいらない気がする。

