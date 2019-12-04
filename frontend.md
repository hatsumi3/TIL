# はじめてのフロントエンド

## 動向

- MVC
  - Model
    - データとそれにアクセスする機能
  - View
    - 外部にどのように出力するか
  - Controller
    - ルーティング
    - ModelとやりとりしてViewに返す。
- MVP
  - Presenter
    - ViewとModelの間
    - アクション、更新 <= P => 情報更新、更新の通知
- MVVM
  - ViewModel
    - データバインディング
- Flux
  - view actioncenter dispacher store
  - 常に一方方向のやりとり

## javascript

- node,npm
- webpack
  - entry output loader plugins
  - 深堀する。
    - [https://qiita.com/soarflat/items/28bf799f7e0335b68186](https://qiita.com/soarflat/items/28bf799f7e0335b68186)
- 文法
  - 変数宣言 let const
  - テンプレートリテラル　`${user}`
  - 関数　function(){return ...;}
  - アロー関数　=>
  - 継承　extends
  - Promise .then(() => {})
  - モジュール　export class function default function

## typescript

- 変換
  - tsc -t ES5 hello.ts
- 文法
  - 変数名：型名　＝値；
  - 型変換　\<number\> as number
  - 関数引数　任意？
  - interface オブジェクトがどのプロパティをもっているかを書く
    - interface { name: string}
  - クラス　public private protected
  - ジェネリクス　c++でいうテンプレートクラス
    - function identity\<t\>(arg: T): T { return arg;}
  - デコレータ　＠で関数修飾
  - jsx　ok tsx

## firebase

mBaaS

- authentication
- firestore
- storage
- hosting
- functions

本書ではhosting,functionsのみ使用。
node.js＋expressにてwebAPI作成

## React

- 略。

## Angular

- フルスタック
- typescript
- RxJS

## Vue

- 単一ファイルコンポーネント

```js
  <template>
  <script>
  <style>
```
- 分けることも可能

## React Native

- 略

## memo

- slack likeなアプリ作成
  - react,typescriptでフロントエンド
  - node.js express firebase でバックエンド
