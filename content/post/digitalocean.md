+++
date = "2016-09-03T16:41:41+09:00"
tags = ["digitalocean", "vagrant"]
title = "DigitalOcean 上に Vagrant でサーバーを立ち上げる"

+++

[DigitalOcean](https://m.do.co/c/a129e4a597f1) 上に Vagtant で仮想サーバー (DigitalOcean では `droplet` と言うらしい) が立ち上がるまでのログ。

<!--more-->

DigitalOcean アカウントは作成済みの前提。

### tugboat の導入

DigitalOcean を便利に操作できるコマンドラインツール tugboat を導入する。

[pearkes/tugboat](https://github.com/pearkes/tugboat)

あらかじめ以下のページで tugboat 用のトークンを発行しておく。

[DigitalOcean \- API Tokens](https://cloud.digitalocean.com/settings/api/tokens)

```sh
% gem install tugboat
```

以下のコマンドで 上記で発行したトークンを設定する。

```sh
% tugboat authorize
```

CentOS の images を一覧する例。

```sh
% tugboat images | grep centos
6.7 x32 (slug: centos-6-5-x32, id: 14782899, distro: CentOS)
6.7 x64 (slug: centos-6-5-x64, id: 14782952, distro: CentOS)
5.11 x32 (slug: centos-5-x32, id: 18292035, distro: CentOS)
5.11 x64 (slug: centos-5-x64, id: 18292131, distro: CentOS)
7.2 x64 (slug: centos-7-0-x64, id: 18835303, distro: CentOS)
7.2 x64 (slug: centos-7-x64, id: 19389325, distro: CentOS)
6.8 x64 (slug: centos-6-x64, id: 19389823, distro: CentOS)
6.8 x32 (slug: centos-6-x32, id: 19389931, distro: CentOS)
```

`7.2 x64` がなぜか2つある。slug `centos-7-x64` を今回は利用してみる。

### Vagrant の準備

Vagrant のバージョンを確認。

```sh
% vagrant -v
Vagrant 1.8.5
```

Vagrant のプラグイン `vagrant-digitalocean` を導入する。

[devopsgroup\-io/vagrant\-digitalocean](https://github.com/devopsgroup-io/vagrant-digitalocean#configure)

```sh
% vagrant plugin install vagrant-digitalocean
```

README.md にあるように `Vagrantfile` を作成する。`provider.xxx` のあたりは tugboat を使って調べつつ適宜書き換える。

```ruby
Vagrant.configure('2') do |config|
  config.vm.hostname = 'foo.hika69.com'

  config.vm.provider :digital_ocean do |provider, override|
    override.ssh.private_key_path = '~/.ssh/id_rsa'
    override.vm.box = 'digital_ocean'
    override.vm.box_url = 'https://github.com/devopsgroup-io/vagrant-digitalocean/raw/master/box/digital_ocean.box'
    provider.token = 'xxxxxxxxxxxxxxxxxxxxxxxxx'
    provider.image = 'centos-7-x64'
    provider.region = 'sgp1'
    provider.size = '512mb'
  end
end
```

`provider.region` のバリエーションを調べる。

```sh
% tugboat regions
```

`provider.size` の場合。

```sh
% tugboat sizes
```

tugboat を使わないで以下のように vagrant-digitalocean の機能を使っても同じことができる (ただトークンをつけるのが少し面倒)。

```sh
% vagrant digitalocean-list regions {TOKEN}
```

### Vagrant up する

仮想サーバーを起動する。

```sh
% vagrant up --provider=digital_ocean
```

### サーバーに SSH 接続する

接続は `vagrant ssh` するだけ。

```sh
% vagrant ssh

Last login: Sun Sep  4 07:55:24 2016 from xxx
[root@foo ~]# cat /etc/redhat-release
CentOS Linux release 7.2.1511 (Core)
```

