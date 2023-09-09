---
title: ngModelで配列参照した時の悩み
tags:
  - Angular
private: false
updated_at: '2023-09-09T11:37:11+09:00'
id: 9bd1cef78bfc110f0ba0
organization_url_name: null
slide: false
---
Angular Advent Calendar 2017 9日目の記事です。

私が働いている[株式会社カケハシ](https://kakehashi.life/)では、Musubiというプロダクトを開発しており、そこにはAngularが使われています。

ちょっとあまり有用な情報ではないかもしれませんが、個人的にすごく悩んだ部分であるので、まとめて見ます。

## 悩み

例えば、以下のようなコードを書きます。

```html:テンプレート側
<button (click)="addText()">add</button>
<div *ngFor="let text of texts;let i = index">
    <input [(ngModel)]="texts[i]">
</div>
```

```typescript:typescript側
 private texts: string[] = ['test1', 'test2'];

  addText() {
    this.texts.push('');
  }
```

この時の出力は、

<img width="224" alt="スクリーンショット 2017-12-03 12.16.23.png" src="https://qiita-image-store.s3.amazonaws.com/0/4044/9ee6f495-a90e-89b3-0e55-2af3e7bf770d.png">

のようになります。

ここで、inputに文字入力したいと思います。まずフォーカスを当てます。

<img width="195" alt="スクリーンショット 2017-12-03 12.17.20.png" src="https://qiita-image-store.s3.amazonaws.com/0/4044/94493977-2175-ee53-abee-6ac60589a09f.png">

そして文字を入力します。すると…・

<img width="306" alt="スクリーンショット 2017-12-03 12.17.53.png" src="https://qiita-image-store.s3.amazonaws.com/0/4044/f3c483bd-62a7-b5f8-bd7d-7b2fbe40e358.png">

フォーカスが外れてしまうのです。こいつは困った。


## 解決策1:配列を直接参照しないようにする

```typescript:改善後
interface StringBox {
  text1: string;
}

@Component({
  selector: 'app-hello',
  templateUrl: './hello.component.html',
  styleUrls: ['./hello.component.css']
})
export class HelloComponent  {
  private texts: StringBox[] = [{text1: 'test1'}, {text1: 'test2'}];

  addText() {
    this.texts.push({text1: ''});
  }

}
```

```html:テンプレート側
<button (click)="addText()">add</button>
<div *ngFor="let text of texts;let i = index">
    <input [(ngModel)]="texts[i].text1">
</div>
```

配列を直接参照しなければいいので、配列のメンバーを参照するとフォーカスが外れて連続で文字が入力できなくなる問題が解決します。
まあでもこの方法はいけてないですね。

## 解決策2:trackByを使う

```typescript:改善後
@Component({
  selector: 'app-hello',
  templateUrl: './hello.component.html',
  styleUrls: ['./hello.component.css']
})
export class HelloComponent {
  private texts: string[] = ['test1', 'test2'];

  addText() {
    this.texts.push('');
  }

  myTrackBy(index: number, obj: any): any {
    return index;
  }

}
```

```html:テンプレート側
<button (click)="addText()">add</button>
<div *ngFor="let text of texts;let i = index;trackBy: myTrackBy">
    <input [(ngModel)]="texts[i]">
</div>
```

これで解決します。

trackBy関数とは、ngForによるオブジェクトの追跡のためのキーを決める式です。
どうも文字を入力すると、Angularは今どの配列を参照しているかわからなくなり、フォーカスが外れてしまうのですが、
trackByでキーを教えてあげると、配列のどの位置を入力されているのがわかるようになり、そのまま入力できるようです。

これで解決できました。


