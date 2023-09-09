---
title: font-familyのgeneric font familyがiOSやAndroidで日本語フォントに効かない
tags:
  - CSS
  - Android
  - iOS
private: false
updated_at: '2023-09-09T11:37:11+09:00'
id: b1b1d00fecc38d3d0cf3
organization_url_name: null
slide: false
---
こんなコンテンツ

```html
<head>
  <style>
  .sansserif {
    font-family: sans-serif;
  }
  .serif {
    font-family: serif;
  }
  .cursive {
    font-family: cursive;
  }
  .fantasy {
    font-family: fantasy;
  }
  .monospace {
    font-family: monospace;
  }
  </style>
</head>
<body>
  <p><span class="sansserif">sans-serif<span/> <span class="sansserif">ゴシック体</span></p>
  <p><span class="serif">serif<span/> <span class="serif">明朝体</span></p>
  <p><span class="cursive">cursive<span/> <span class="cursive">筆記体</span></p>
  <p><span class="fantasy">fantasy<span/> <span class="fantasy">装飾体</span></p>
  <p><span class="monospace">monospace<span/>  <span class="monospace">等幅フォント</span></p>
</body>
```

で描画結果

<img width="642" alt="font.png" src="https://qiita-image-store.s3.amazonaws.com/0/4044/3d47d6d1-f68f-a831-5035-3b468fa5b38f.png">

左がPCのGoogle Chrome(version48) 右が iOS9.1(シミュレータ)のWebView

iOSのWebViewは日本語が等幅フォント以外はすべてゴシック体のフォントになってしまっている。
AndroidのWebView(Android5.0/Nexus9)も同様。

generic font familyが効かないと
明朝体(serif)で描画してほしいのに、ゴシック体などの意図しないフォントで描画されてしまう。。。

解決方法はないだろうか。
