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
    - ```<button onClick={this.関数}>```
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
      - ```<Context Provider value={}> </Context>```の間だけvalueの値を使用する
      - cssテーマの変更などで利用

## redux

- 値の総合管理。状態管理ツール。
- 仕組み
  - ストア　データ（ステート）を補間
  - ステート　状態
  - プロバイダー　ストアを他のコンポーネントに受け渡す
    - コンテキストのProvider
  - レデューサー　ステートを変更する
- 使い方
  - store
    - varient = createStore(reducer);
  - reducer
    - function functionName(state=state,action){}
      - action.typeでswitchする
  - provider
    - ```<Provider store={store}> <App /></Provider>```
    - 中にあるコンポーネントに内容を渡せる
  - connect
    - app=connect(設定内容)(コンポーネント)
      - 正確にはconnect(mapStateToProps,mapDispatchToProps)(ChildComponent)
        - mapStateToProps:storeが持っているstateをpropsに入れて子コンポーネントに渡す
        - mapDispatchToProps:dispatchを呼び出す関数をpropsに入れて子コンポーネントに渡す
    - [参考１](http://yucatio.hatenablog.com/entry/2018/09/21/225716)
    - [参考２](https://qiita.com/MegaBlackLabel/items/df868e734d199071b883)
  - dispatch,action
    - this.props.dispatch({type:'~~~'});
      - dispatchはアクションを送る
    - {type:'~~~'}がアクション部分
    - reducer、必要な処理が呼び出される
- 流れ
  - state用意,reducer用意,store用意
  - dispatch呼び出し,reducer呼び出し,actionで分岐
- memoアプリを作る
  - Store.js
    - reducer
      - switch文でreducerActionを呼び出し
    - reducerAction
      - add,delete.findで行う処理を書く
      - 文字操作関連メモ
        - unshift:配列の先頭に追加
        - indexOf:indexとりだし
        - splice:indexから文字数分削除する
        - slice:配列をつくる
          - setStateで元の配列を渡しても更新が行われない。
        - 参考[http://www.htmq.com/js/array_splice.shtml](http://www.htmq.com/js/array_splice.shtml)
    - ActionCreater
      - dispatchで渡す
      - reducerがswitch判定してreducerAction
  - Form.js
    - form onSubmit=this.doAction
      - actioncreater作ってdispatch
- redux persist
  - 永続化。上記実装だとブラウザ更新で中身が消える。
  - ローカルストレージで管理する
  - persistReducer
    - const varient - persistreducer(config,reducer);
  - persister
    - store = createStore(persistRreducer)
    - varient = persistStore(store)
  - PersistGate
    - ```<PersistGate persister={persister}/>```
    - コンポーネントの表示を待たせるもの
  - sample

    ```js
    const persistConfig = {
    key: 'memo',
    storage:storage,
    blacklist: ['message','mode','fdata'],
    whitelist: ['data']
    };

    const persistedReducer = persistReducer(persistConfig,memoReducer);

    let store = createStore(persistedReducer);
    let pstore = persistStore(store);

    ReactDOM.render(
        <Provider store={store}>
            <PersistGate loading={<p>loading...</p>} persistor={pstore}>
                <App />
            </PersistGate>
        </Provider>,
        document.getElementById('root')
    );
    ```
  
  - storageの中身を確認
    - chrome developer tools -> application -> local storage
    - ストレージにはオブジェクトは保存できないときがある
      - メモアプリだと new Date()　など。
  - blacklist,whitelist
    - 保管するデータを整理
    - persistReducerを作る時のconfigで渡す
    - 停止・再に変更
      - purge:保存データ削除
      - flush:最新状態を反映
      - pause:保存を一時停止
      - persist:保存処理を再開
      - persisterのメソッドとして呼び出し。
        - persister.flush()　など

## next.js

- クライアントサイドレンダリング
  - スクリプトを受け取りレンダリング
  - 検索エンジンに正しく判断してもらえない
- サーバサイドレンダリング
  - サーバ側でページを用意しておき配信
  - Reactはクライアント側で生成するもの
  - サーバサイドでやるためのライブラリnext.js
- pages folder
  - webページを配置しておく場所
  - next.config.js
    - トップページの設定
    - index.jsが読み込まれるようにするなど
- スタイル
  - cssは使えない
  - ビルドインCSS
    - ```<style jsx>{` css `}</style>```
    - css部分にcss表記で記述できる
      - Reactのstyleと混同しないように、、
      - fontSize: '8pt' , <--> font-size:8pt;
- 複数ページ
  - ```<Link href>```
  - なかのa要素にhrefがなくてもOK
  - style.js
    - ビルドインCSSのファイルを分けて書いて読み込む
  - Layout.js
    - body要素
      - header
      - footer
    - head要素も埋め込める
      - ```<Head>```
- reduxの使用
  - redux-thunk　ミドルウェア。非同期実行の関数を提供。
  - redux-store.js
  - 修正Appコンポーネント
    - 説明がなかったため略
    - TODO 別途調べる。
  - store.js
    - exportがcreateStore()ではなく、関数のエクスポートに
    - createStore()の引数にミドルウェアを含める
- 足し算アプリ
  - store.jsの中身を変更
    - 計算部分
      - 数字意外を取り除いて数値をとりだす。
        - replace(/[^0-9]/g,"")
      - 文字列*1で数字として認識させて足し算する。
    - リセット
