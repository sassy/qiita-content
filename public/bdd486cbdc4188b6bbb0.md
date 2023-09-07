---
title: canvasで画像の２値化
tags:
  - JavaScript
  - HTML5
  - 画像処理
  - canvas
private: false
updated_at: '2014-07-16T00:55:22+09:00'
id: bdd486cbdc4188b6bbb0
organization_url_name: null
slide: false
---


# 画像の２値化
画像の画素を白か黒かの２階超に変換する処理を２値化といいます。

# 閾値
画像の2値化の白か黒かを判別するための閾値をきめなくてはいけません。
閾値の設定はヒストグラムを作成し、その山が二つある場合その山の間の谷に閾値を設定すると、うまく２値化できるようです。
以前つくった輝度のヒストグラムを使ってみます。


![binary.png](https://qiita-image-store.s3.amazonaws.com/0/4044/a2722c08-7554-fd3b-9d2b-de71941aa28c.png "binary.png")

だいたい65を境に色を変えてみたらいい感じになったので、
65を閾値にしてみます。

# 2値化のプログラム


````binarization.html
<!DOCTYPE html>
<html>
<head>
<title>画像処理</title>
<script type="text/javascript">
const THRESHOLD = 65;

window.onload = function() {
    var canvas = document.getElementById("c1");
    var ctx = canvas.getContext("2d");
    var img = new Image();
    img.src = "test.jpg";
    img.onload = function() {
        ctx.drawImage(img, 0, 0);
        var src = ctx.getImageData(0, 0, canvas.width, canvas.height);
        var dst = ctx.createImageData(canvas.width, canvas.height);

        for (var i = 0; i < src.data.length; i=i+4) {
            var y = ~~(0.299 * src.data[i] + 0.587 * src.data[i + 1] + 0.114 * src.data[i + 2]);
            var ret = (y > THRESHOLD) ? 255 : 0;
            dst.data[i] = dst.data[i+1] = dst.data[i+2] = ret;
            dst.data[i+3] = src.data[i+3];
        }

        ctx.putImageData(dst, 0, 0);
    };
};
</script>
</head>
<body>
  <canvas id="c1" width="300" height="300">
</body>
</html>

````

結果は以下のようになります。
ちょっといい感じにできましたかね？

![binary2.png](https://qiita-image-store.s3.amazonaws.com/0/4044/103ad253-7194-6a12-337f-be9e5bd649e3.png "binary2.png")



