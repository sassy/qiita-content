---
title: とりあえずInferを使ってみた。
tags:
  - infer
private: false
updated_at: '2015-06-23T16:42:42+09:00'
id: 3d5f580feebfcf9f2c61
organization_url_name: null
slide: false
---

# 何？

* 静的コード解析ツール
* C / Java / Objective-C のコードを解析可能 
* C++は解析できないっぽい


# インストール

バイナリ落としてくるのが楽。Macだと　 https://github.com/facebook/infer/releases/download/v0.1.1/infer-osx-v0.1.1.tar.xz からv0.1.1を落としてこれる


落としてきて解答したら
``infer/infer/bin`` にPATHを通しましょう。


# 解析

iOSということで、ずごく昔に作った
https://github.com/sassy/iOSFirstTwitterApp
を解析かけてみた。


```bash:
$ git clone git@github.com:sassy/iOSFirstTwitterApp.git
$ cd mac:iOSFirstTwitterApp
$ infer -- xcodebuild -target TwitterViewer  -configuration Debug -sdk iphonesimulator
```

そしたら、

```shell-session:
Starting analysis (Infer version v0.1.1)
Analysis done

5 files analyzed


No issues found

```

何も見つからなかった。
 ```infer-out```　というディレクトリができていて、
その中のbugs.txtに結果が記録されている。

実際は
http://fbinfer.com/docs/hello-world.html#hello-world-ios
にあるみたいにいろいろ指摘がかかれるっぽい。


# 詳しくは

http://fbinfer.com/docs/

を見ましょう。

何かあったら追記します。


