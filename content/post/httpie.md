+++
date = "2017-02-20T22:55:56+09:00"
tags = ["httpie"]
title = "HTTPie の Sessions の使い方"

+++

API のリクエストにめっちゃ長いトークンを含めないといけなかったりすると、コマンドを打つのもコピペするのも面倒なんだけど、[HTTPie](https://httpie.org/) に [Sessions](https://httpie.org/doc#sessions) という便利そうな機能があったので試してみた。

<!--more-->

ここでは例として `foo` という名前の Session を作る。 `Authorization:"~"` が入力がめんどいトークンなど。

```
% http --session=foo api.example.com Authorization:"Bearer {very-very-long-token}"
```

Session `foo` を使うと、上記のヘッダ付きのリクエストになる。

```
% http --session=foo api.example.com/bar
```

Session の情報は以下のパスに保存されていた。

```
% cat ~/.httpie/sessions/api.example.com/foo.json
{
    "__meta__": {
        "about": "HTTPie session file",
        "help": "https://httpie.org/docs#sessions",
        "httpie": "0.9.8"
    },
    "auth": {
        "password": null,
        "type": null,
        "username": null
    },
    "cookies": {},
    "headers": {
        "Authorization": "Bearer {very-very-long-token}"
    }
}
```
