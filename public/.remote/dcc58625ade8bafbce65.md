---
title: RubyMotionでAndroidアプリを試してみる
tags:
  - Android
  - RubyMotion
private: false
updated_at: '2023-09-09T11:37:11+09:00'
id: dcc58625ade8bafbce65
organization_url_name: null
slide: false
---

rubymotionがpublic betaでAndroidに対応しました。

せっかくなので、さっそく試してみました。

基本的にdeveloper centerのドキュメント通りにやっていればできますが、ちょっと素直につまづいたところもあるので念のため

# RubyMotionでAndroidをつかえるようにする

まだpublic betaなので、以下のコマンドで使ってAndroid beta版をインストールします

```shell-session:
$ sudo motion update --pre
```

# SDKとNDKの設定

.bash_profile等に以下を書いて、SDKとNDKの設定をします。

```bash:
export RUBYMOTION_ANDROID_SDK=~/android-rubymotion/sdk
export RUBYMOTION_ANDROID_NDK=~/android-rubymotion/ndk
```

これは、使っているAndroidのSDK/NDKの場所を指定してあげればいいと思いますが、
developer centerにも書いてあるとおり、Mac OS X 64bit NDKの32bitターゲットを使ってください。64bitターゲット版は対応してないので、使わないでください。
僕はNDKを入れ直しました。

# プロジェクト作成とビルド

```shell-session:
$ motion create --template=android Hello
```

とすると、Helloというプロジェクトを作成します。
そして、

```shell-session:
$ cd Hello
$ rake
```

でビルドができます。

ただ、自分の環境だと、SDKのバージョンがandroid-Lになってしまうのですが、ndkの32ビットターゲットはandroid-Lに対応してないので、

```shell-session:
    ERROR! It looks like your version of the NDK does not support API level L. Switch to a lower API level or install a more recent NDK.
```

と出てしまいました。
そこで、使用するSDK versionを19に変更して対応しました。
Rakefileにapi_versionを書いてあげればよいです。

```ruby:Rakefile
Motion::Project::App.setup do |app|
  # Use `rake config' to see complete project settings.
  app.name = 'Hello'
  app.api_version = '19'
end
```

これで、お使いの端末をつなげる、もしくはエミュレータを起動してRakeコマンドを叩けばアプリを動かせます。

# サンプル

サンプルは下記にあるので、参考にするといいでしょう。
https://github.com/HipByte/RubyMotionSamples/tree/master/android

見てみると、RubyでAndroidのアプリを書く感じがわかります。
Android開発をしている人は、SDKの使い方のノウハウがそのまま生かせそうです。
