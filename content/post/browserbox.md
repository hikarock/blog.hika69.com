---
layout: post
title: "JavaScript の IMAP クライアント browserbox で Gmail に接続する"
date: "2015-02-24T12:47:45"
comments: true
tags: 
- javascript
---

iOS 標準のメモアプリのように、IMAP で Gmail と同期できるクライアントを JS で作れないかな、と思って[whiteout-io/browserbox](https://github.com/whiteout-io/browserbox)を試してみた。

<!--more-->

もろもろフリーダムな環境にしたかったので[nwjs/nw.js](https://github.com/nwjs/nw.js)を使って環境を作る。

```javascript
{
  "name": "imap",
  "main": "index.html",
  "dependencies": {
    "browserbox": "^0.8.1"
  }
}
```

```html
<!DOCTYPE html>
<html>
  <head>
    <title>mail</title>
    <script src="mail.js"></script>
  </head>
  <body>
  </body>
</html>
```

```javascript
var BrowserBox = require('browserbox'),

var host     = 'imap.gmail.com',
    port     = 993,
    email    = 'example@gmail.com',
    password = 'xxxxxxxxxxxxxxxx',
    mailbox  = 'INBOX';

var client = new BrowserBox(host, port, {
  auth: {
    user: email,
    pass: password
  }
});

client.connect();
client.onauth = function() {
  client.listMailboxes(function(err, mailboxes) {
    console.log(err || mailboxes);
    client.selectMailbox(mailbox, function(err, mailbox) {
      console.log(err || 'Mailbox was successfully changed to ' + mailbox);
    });
  });
};
```

実行。

```
$ /Applications/nwjs.app/Contents/MacOS/nwjs .
```

Gmail で2段階認証を使っている場合はこのコードで接続できない。
さらに、以下の設定で「安全性の低いアプリ」を許可しないと接続できなかった。

- [安全性の低いアプリ - アカウント設定](https://www.google.com/settings/security/lesssecureapps)

許可しない場合は以下のエラーが出る。

```
[98549:0224/125046:INFO:CONSOLE(41)] ""[2015-02-24T03:50:46.248Z][browserbox IMAP]" "[1] S: W3 NO [ALERT] Please log in via your web browser: http://support.google.com/mail/accounts/bin/answer.py?answer=78754 (Failure)"", source: /Users/projects/mail/node_modules/browserbox/node_modules/axe-logger/axe.js (41)
```

このままでは実用的ではない感じ。


