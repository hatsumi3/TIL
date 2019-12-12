# React.js Next.js 入門

章ごとに内容をメモ。

## reactの準備

- 仮想DOM
- node.js
  - npx create-react-app プロジェクト名
  - npm init react-app プロジェクト名
  - 実行
    - yarn start npm start
    - yarn build npm run build
    - yarn test npm test
    - yarn eject npm run eject
  - yarnはfacebook社

## dom

- タグ、エレメント、ノード
  - ノードは改行文字、空白文字
- createElement
  - createElementの下に要素を重ねることもできる
  
  ```js
  React.createElement('div',{}[
    React.createElement('h2',{},"react"),
  ])
  ```

  - renderで描画できるのは１つだけ
  - jsx
    - babel 古い仕様にコンパイル
    - style
      - styleタグは使えない
      - CSSのfont-size -> fontSizeなど
    - 条件分岐
      - {真偽地　&& JSX}
        - trueならjsx実行
        - falseならjsx実行内
      - map
        - 配列.map((value)=> 項目・・);
      - setInterval(,1000)
        - 1秒後に実行
      - 変数代入時にコンパイルされる（エレメントの確定）
    - クリックすると実行する関数の中に本体を作れる
      - アロー関数でイベントをつくる、その中のjsxのonClickイベントでアロー関数呼び出し

## Component

- 要素指定方法
  - document.querySelector()
    - CSSセレクタで最初にマッチした要素
  - document.getElementById()
    - 指定されたIDを持つ要素を指定
    - グローバルのdocumentオブジェクトのみ利用可能
  - 参考は[ここ](https://qiita.com/amamamaou/items/25e8b4e1b41c8d3211f4)
- 関数コンポーネント
  - コンポーネント名は大文字で始まる必要がある。Component
  - 関数コンポーネントの場合ReactDOM.render(arg1,arg2)で描画。
    - arg1:描画内容
    - arg2:要素
  - クラス
    - constructor(props)
      - 継承している場合はsuper(props)する
      - React.Componentをextendsするのでほぼ必須？
    - render(){} で描画
    - props 属性
      - <Component名 arg1=arg1 arg2=arg2 />でコンポーネントに属性を渡せる
- プロジェクト
  - npm init react-app プロジェクト名 で作ったやつの説明
    - index.js
      - index.htmlに記載なく読み込まれる
      - importでreact系モジュールの読み込み。スタイルクラス、コンポーネント等
      - import React,{Component} from 'react';とかけばComponentのextendsのときReact.を省略可能
      - コンポーネントの別ファイル化
        - スタイル、コンポーネント等はファイル分けできる
- state 状態
  - constructorで初期化
    - this.state={値}
  - 更新
    - this.setState({value})
    - this.setState((state)=>({value}))
      - stateの値を利用して何かするときはこちら。前の値に追加するときなど。。
  - イベントのバインド
    - <button onClick={this.関数}>
    - this.関数=this.関数.bind(this);でイベントから関数が実行可能に
    - stateを切り替えて表示内容変更など（count+=1）
      - 参考演算子で場合分けして表示内容変更
      - data.push({});this.setState({list:this.data})でリストをstateに
        - 直接操作は出来ないので上記のような形式になる
  - this.props.childrenでコンポーネント内のテキストを取り出し
  - map処理
    - 配列.map((value,key)=>(~~~))で配列の要素1つづつ取り出して処理
  - form
    - event.preventDefault()
      - イベントを消費する
      - 本書だと機能をはたしていないのでは・・？
  - input
    - validation
      - required  必須要素
      - pattern   正規表現。[A-Za-z _,.]+など
      - max,min
      - maxlength,minlength
    - alert
      - ブラウザ上に表示
        - 表示側と内容処理側でわける
        - 例えば内容処理側で文字数判断しておき、アラートがあればthis.props.onCheck(e)で表示側の処理
  - コンテキスト
    - コンポーネントの共有
    - const 変数 = React.Context(value);で宣言
    - static aaa=変数;でクラス内で利用
    - Providerでコンテキストを変更
      - <Context Provider value={}> </Context>の間だけvalueの値を使用する
      - cssテーマの変更などで利用
