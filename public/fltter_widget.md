---
title: fltter_widget
tags:
  - ''
private: true
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
Flutterは、モバイルアプリ開発のための人気のあるフレームワークであり、その基本的な構成要素は「Widget」です。Widgetは、画面上のあらゆるものを表現するための要素として機能します。ここでは、FlutterのWidgetについて初心者向けに解説します。

# Widgetとは何ですか？
FlutterのWidgetは、ユーザーインターフェースの構成要素を表すオブジェクトです。Widgetは、ボタン、テキスト、画像、レイアウト、アニメーションなど、さまざまな要素を表現できます。

## 親子関係
FlutterにおけるWidgetは、親子関係を持つツリー構造を形成します。これは、UIの階層構造を表現するために使用されます。

```
WidgetTree
│
└─ Container
   │
   ├─ Text
   │
   └─ Row
      │
      ├─ Icon
      │
      └─ Text
```
この例では、WidgetTree が最上位の親Widgetであり、Container Widgetがその子であり、更にその子に Text と Row Widgetがあります。さらに、Row Widgetは Icon と Text Widgetを持つ子を持っています。

このようなツリー構造により、Widgetは階層的に配置され、親Widgetによって子Widgetが配置されます

## ScaffoldWidgetの解説
ScaffoldWidgetは、アプリの基本的なマテリアルデザインレイアウトを提供し、標準的なアプリケーションの構造を定義します。一般的に、アプリのトップレベルに配置され、AppBar、Drawer、SnackBar、FloatingActionButtonなどのさまざまなコンポーネントを含むことができます。

主な要素は以下の通りです：

appBar: アプリの上部に表示されるアプリバー（タイトルバー）を定義します。
body: アプリのメインコンテンツを表示するためのWidgetを含みます。
drawer: サイドメニューやナビゲーションメニューを表示するためのドロワーWidgetを定義します。
floatingActionButton: フローティングアクションボタンを配置します。

### Scaffole の例

```
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('My App'),
      ),
      body: Center(
        child: Text('Welcome to My App!'),
      ),
      drawer: Drawer(
        child: ListView(
          padding: EdgeInsets.zero,
          children: <Widget>[
            DrawerHeader(
              child: Text('Drawer Header'),
              decoration: BoxDecoration(
                color: Colors.blue,
              ),
            ),
            ListTile(
              title: Text('Item 1'),
              onTap: () {
                // メニュー項目をタップした時の処理
              },
            ),
            ListTile(
              title: Text('Item 2'),
              onTap: () {
                // メニュー項目をタップした時の処理
              },
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // フローティングアクションボタンをタップした時の処理
        },
        child: Icon(Icons.add),
      ),
    );
  }
}
```


# StatelessWidgetとStatefulWidget
FlutterのWidgetには、主に2つのタイプがあります：StatelessWidgetとStatefulWidget。

## StatelessWidget
StatelessWidgetは、一度作成されたら変更できない状態を持つWidgetです。これは、静的なコンテンツを表示するために使用されます。たとえば、テキストやアイコンなどの固定されたコンテンツを表示する場合に使用されます。

```dart
import 'package:flutter/material.dart';

class MyStaticWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text('Hello, World!'),
    );
  }
}
```

## StatefulWidget
StatefulWidgetは、内部状態を持つWidgetです。これにより、ユーザーの操作や外部の状態変化に応じて、Widgetの外観や動作を動的に変更できます。たとえば、ボタンが押されたときにカウンターを増やすWidgetなどに使用されます。

```dart
import 'package:flutter/material.dart';

class MyDynamicWidget extends StatefulWidget {
  @override
  _MyDynamicWidgetState createState() => _MyDynamicWidgetState();
}

class _MyDynamicWidgetState extends State<MyDynamicWidget> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          Text('Counter: $_counter'),
          ElevatedButton(
            onPressed: _incrementCounter,
            child: Text('Increment'),
          ),
        ],
      ),
    );
  }
}

```
