---
title: "Xcode のキー配置をカスタマイズする"
date: "2014-12-28T13:01:51"
comments: true
tags: 
- xcode
- karabiner
- mac
---

[XVim](https://github.com/XVimProject/XVim) を使いはじめたら、Xcodeのキー配置が、カスタマイズ済みのターミナル (およびVim) と微妙に違うのが気になってきた (ターミナルはControlキーをよく使うので、CommandキーとControlキーを入れ替えている) 。

<!--more-->

[Karabiner](https://pqrs.org/osx/karabiner/index.html.ja) を使ってXcodeのCommandキーとControlキーを入れ替えた。

private.xmlに以下を追記した。

```
<?xml version="1.0"?>
<root>
  <list>
    <item>
      <name>Enable at only Xcode</name>
      <appdef>
        <appname>XCODE</appname>
        <equal>com.apple.dt.Xcode</equal>
      </appdef>
      <item>
        <name>Control_L to Command_L (XCODE ONLY)</name>
        <only>XCODE</only>
        <identifier>remap.app_xcode_contolL2commandL2</identifier>
        <autogen>--KeyToKey-- KeyCode::CONTROL_L, KeyCode::COMMAND_L</autogen>
      </item>
      <item>
        <name>Command_L to Control_L (XCODE ONLY)</name>
        <only>XCODE</only>
        <identifier>remap.app_xcode_commandL2controlL2</identifier>
        <autogen>--KeyToKey-- KeyCode::COMMAND_L, KeyCode::CONTROL_L</autogen>
      </item>
    </item>
  </list>
</root>
```

`Reload XML`したら設定が現れるので、両方にチェックをいれて完了。

![](/images/post/karabiner.png)

XVim 入れたらこういうことになるだろうなあ、と思ってたけど案の定 Xcode Way から遠ざかりつつある...

