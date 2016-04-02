+++
date = "2016-04-02T12:21:22+09:00"
tags = ["raspberry-pi"]
title = "Raspberry Pi 3 のセットアップ"

+++

使いみちを決めないで Raspberry Pi 3 を買ってしまったけど、とりあえず OS いれて起動するまでセットアップしてみる。

<!--more-->

### 購入したもの

- [Amazon.co.jp： Raspberry Pi3 Model B (Element14): パソコン・周辺機器](http://www.amazon.co.jp/Raspberry-Pi3-Model-B-Element14/dp/B01D1FR2WE%3Fpsc%3D1%26SubscriptionId%3D0AVSM5SVKRWTFMG7ZR82%26tag%3Dhikarock-22%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3DB01D1FR2WE)
- [Amazon.co.jp： 東芝 microSDHC 32GB EXCERIA 48MB/s UHS-I Class10 TOSHIBA 海外向パッケージ品 [並行輸入品]: パソコン・周辺機器](http://www.amazon.co.jp/%E6%9D%B1%E8%8A%9D-microSDHC-EXCERIA-TOSHIBA-%E6%B5%B7%E5%A4%96%E5%90%91%E3%83%91%E3%83%83%E3%82%B1%E3%83%BC%E3%82%B8%E5%93%81/dp/B00ZF75RLK%3FSubscriptionId%3D0AVSM5SVKRWTFMG7ZR82%26tag%3Dhikarock-22%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3DB00ZF75RLK)
- [Amazon.co.jp： Raspberry Pi 3B/2B/B+ 専用ケース（Green）: パソコン・周辺機器](http://www.amazon.co.jp/Raspberry-Pi-3B-2B-%E5%B0%82%E7%94%A8%E3%82%B1%E3%83%BC%E3%82%B9-%EF%BC%88Green%EF%BC%89/dp/B00OH026UQ%3Fpsc%3D1%26SubscriptionId%3D0AVSM5SVKRWTFMG7ZR82%26tag%3Dhikarock-22%26linkCode%3Dxm2%26camp%3D2025%26creative%3D165953%26creativeASIN%3DB00OH026UQ)
- HDMI ケーブル

以下は、すでに持っているもので流用。

- USB キーボード
- USB マウス
- 2.5A の電源推奨らしいがとりあえず余っていたiPhone用の 1A の電源
- 電源接続用の MicroUSB ケーブル

### OS のセットアップ

ここから RASPBIAN JESSIE をダウンロードする。

[Download Raspbian for Raspberry Pi](https://www.raspberrypi.org/downloads/raspbian/)

ダウンロードしたファイルを展開して `2016-03-18-raspbian-jessie.img` を確認。

OSのインストールは以下のドキュメントを見ながら進める。

[Installing operating system images on Mac OS - Raspberry Pi Documentation](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md)

microSD カードを Mac に挿入してから以下のコマンドを実行する。

```bash
$ diskutil list
```

microSD カードが `/dev/dis2` でマウントされていることを確認。

```bash
$ sudo diskutil unmountDisk /dev/disk2
$ sudo dd bs=1M if=/Users/hikarock/Downloads/2016-03-18-raspbian-jessie.img of=/dev/disk2
```

しばし待つ。完了後に Finder で Mac のデバイスとして認識されているのを確認してから microSD カードを取り出す。
取り出した microSD を Raspberry Pi に挿入する。

### Mac から SSH 接続できるようにする

電源の Micro USB ケーブルを差すだけで OS が起動する。

#### Wi-Fi の接続

右上のメニューから GUI で設定できる。

#### SSH の有効化

IP アドレスを確認する。

```bash
pi@raspberrypi:~ $ ip addr
```

`raspi-config` で設定画面が起動するので、ssh を有効化する。

```bash
pi@raspberrypi:~ $ sudo raspi-config
```

Mac から ssh する (初期パスワードは `raspberry`)。

```bash
$ ssh pi@192.168.X.X
```

入れた。ということで何に使おうか。

