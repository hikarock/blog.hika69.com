---
title: "IB_DESIGNABLE を使おう"
date: "2015-02-10T19:55:15"
comments: true
tags: 
- xcode
- ios
- objective-c
---

Xcode6 から使える `IB_DESIGNABLE` と `IBInspectable` が非常に便利なんだけど情報が少ない。

ので、基本とかは置いといて、とりあえず知ってることを雑多にまとめてみる。

<!--more-->

### 有用なリンク

とりあえずここらへんを眺めると良いとおもう。

- [Creating a Custom View That Renders in Interface Builder](https://developer.apple.com/library/ios/recipes/xcode_help-IB_objects_media/chapters/CreatingaLiveViewofaCustomObject.html)
- [IBInspectable / IBDesignable - NSHipster](http://nshipster.com/ibinspectable-ibdesignable/)
- [How to make awesome UI components in iOS 8 using Swift and XCode 6 - IBDesignable and IBInspectable](https://www.weheartswift.com/make-awesome-ui-components-ios-8-using-swift-xcode-6/)

### `IB_DESIGNABLE` な View は `UIView` もしくは `NSView` を継承して作る

![](/images/post/ib-designable-1.png)

`UITableViewCell` とか継承して `IB_DESIGNABLE` を設定しても IB 上では動かない。Preview でも同様に反映されなかった。残念。

ただし `IBInspectable` は使える。コードを書かないで値を設定できるだけでも便利なので、使い道はあると思う。
心の目で見るんだ。

### 初期値を設定する

`initWithCoder` で設定する。ただし、ここで設定した初期値が IB 上のフォームで選択状態になってたりはしない。心の目で見るんだ。

```objective-c
- (id)initWithCoder:(NSCoder *)aDecoder {
  self = [super initWithCoder:aDecoder];
  if (self != nil) {
    self.cornerRadius = 8;
    self.borderWidth  = 0.5;
  }
  return self;
}
```

### `IBInspectable` で使える型

`enum` が使えると便利そうだと思ったけどだめだった。

![](/images/post/ib-designable-2.png)

上でも紹介している、[このエントリ](https://www.weheartswift.com/make-awesome-ui-components-ios-8-using-swift-xcode-6/)
にある型を試した。他にも使える型があるのかもしれない。

### 要素の消し忘れに注意

![](/images/post/ib-designable-3.png)

`IBInspectable` で設定した値は `Identity Inspector` 上の `User Defined Runtime Attributes` 上に保存される。
プロパティを設定後に、プロパティ名を変更したり削除したりすると、ここに迷子の要素が残ってしまう。

迷子の要素がある画面を iOS7.1 で開いたら何もログを出さずに落ちたが、要素を削除したら問題なく起動した(iOS8 では問題なかった)。やっかいだ。

### `IBInspectable` な View を継承する

継承した `IBInspectable` な View のプロパティが両方 IB 上に表示される。

![](/images/post/ib-designable-4.png)

2つの View でそれぞれ設定した値がストーリーボード上に反映されている。

(`UIView` を継承している `UITableViewCell` の場合は `IB_DESIGNABLE` が動かなかったので、このへんの仕組みがよくわからない感じ)

