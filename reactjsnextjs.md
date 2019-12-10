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
