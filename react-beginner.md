# React ビギナーズガイド

- 章毎の要素メモ
- つくったもの  
[https://github.com/hatsumi3/react-beginner](https://github.com/hatsumi3/react-beginner)

## ライフサイクル

- props
  - proptypes
    - 昔はreactのライブラリだったが今は別。
    - proptypesをクラス前に宣言、クラス後に代入 or クラス後に定義、代入
- state
- method
  - componentWillUpdate
  - componentDidUpdate
  - componentWillMount
  - componentDidMount
  - componentWillUnmount
  - shouldComponentUpdate
- mixins
  - es6ではサポートなし。

## Excel js jsx

- 上のgithubにコードあり。
- table,thead,tr,th,tbody,td
- sortの書き方
- イベントハンドラ
- イベントリスナー
  - bindの明記
- edit
- search
  - ボタン押下時に状態を保存
  - その状態に対してfilter
  - 文字をlowerにしてfilterする
  - 複数条件filterはそのたびの状態をstateに保存する
- download
  - json,csv
- babel
  - トランスパイラ
  - jsx to js
  - es6 to es5
  - htmlにreact直書きならscript srcにてライブラリ指定。
- スプレッド演算子　...
  - react非推奨。利用は限定的。
  - class -> className
  - for -> htmlFor
  - styleはオブジェクト指定
  - 閉じタグ必須
  - キャメルケース属性名
  
## 開発環境

- ディレクトリ構成
- ソフトウェア紹介
  - node.js
  - broeserify
    - nodeの文をブラウザ用に変換
    - babelifyで変換後・・
  - タスクランナー　jsをまとめる
    - デプロイなど
    - npm script
    - gulp grunt
    - webpack

## コンポーネント

パーツ毎に呼び出しに変更.  
公式↓（Fluxで実装）
[http://www.whinepad.com/](http://www.whinepad.com/)

## 品質、型チェック、テスト

- package.json
  - babelの設定
    - presets
  - script
- ESLint
- Flow
  - 静的型チェックツール
  - コメントをつける　@flow
- テスト
  - jest
    - describe() テストスイート
      - it()　テスト仕様
        - except　あさーしょん
  - [https://qiita.com/sand/items/af2af0766ca00558457d](https://qiita.com/sand/items/af2af0766ca00558457d)
  - test utilsはes5？

## Flux

- MVC
- 単方向のデータの流れに
  - Action View Store
  - Store 各モジュールから利用、イベント発生
  - Action　Storeのデータ変更
    - 呼び出し元が単一　dispatcher
  - View Action呼び出し　イベント受け取り
- immutable
  - リスト
