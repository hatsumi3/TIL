# node.js react アプリケーション

本：いまどきのjsプログラマーのための node.jsとreactアプリケーション開発テクニック
URL:[https://www.socym.co.jp/book/1114](https://www.socym.co.jp/book/1114)

## ch1

### javascript history

- 1995　誕生
  - webブラウザMosaicの開発
  - Netscape Navigator
    - 2.0にjavascript搭載
    - 当時はlivescriptという名前
- 1996　JScript
  - IE登場
- 1997　標準化
  - ECMA
- 2000年前半
  - flushの普及
- 2005
  - ajax googlemap
- 2008
  - googlechrome V8エンジン
- 2009
  - node.js
- 2010半ば
  - altjs coffeescript typescript
- 2015
  - es2015 babel

### サーバサイド処理

- node.js
  - 要素
    - nodeコアライブラリ(Javascript)
    - Node.jsバインディング
    - V8(Javascriptエンジン)
    - 非同期I/O(C言語)
  - 特徴
    - 実行効率の良い環境
    - サーバ、クライアント同じ言語
    - 便利なライブラリnpm
    - 大量アクセスに強い

### npm

- npm install
  - --saveでdependenciesに追加
  - -gでグローバルインストール
  - packed.jsonに記述される
    - node_modulesディレクトリを
- npm init
  - 初期化
- npm run start
  - script/start に記述したコマンドが実行される
- yarn
  - facebookの開発したnpm互換のパッケージ管理ツール

### コードスタイル

- JS標準スタイル
  - [https://standardjs.com/](https://standardjs.com/)
- ルール
  - インデントはスペース2
  - シングルクォート
  - セミコロンなし
  - 単語、関数後ろにスペース
  - 記号前後にスペース
  - キャメルケース、クラス名はパスカルケース
  - 等価演算子　===
- 整形
  - standard 整形ツール

### webアプリ、非同期処理

- httpヘッダ、レスポンス
- URLで変更

```js
// httpモジュールを読み込む
const http = require('http')
const ctype = { 'Content-Type': 'text/html;charset=utf-8' }

// Webサーバーを実行 --- (※1)
const svr = http.createServer(handler) // サーバーを生成
svr.listen(8081) // ポート8081番で待ち受け開始

// サーバーにアクセスがあった時の処理 --- (※2)
function handler (req, res) {
  // URLの判断
  const url = req.url
  // トップページか？
  if (url === '/' || url === '/index.html') {
    showIndexPage(req, res)
    return
  }
  // サイコロページか？
  if (url.substr(0, 6) === '/dice/') {
    showDicePage(req, res)
    return
  }
  // その他
  res.writeHead(404, ctype)
  res.end('404 not found')
}

// インデックスページにアクセスがあったとき --- (※3)
function showIndexPage (req, res) {
  // HTTPヘッダを出力
  res.writeHead(200, ctype)
  // レスポンスの本体を出力
  const html = '<h1>サイコロページの案内</h1>\n' +
    '<p><a href="/dice/6">6面体サイコロ</a></p>' +
    '<p><a href="/dice/12">12面体サイコロ</a></p>'
  res.end(html)
}

// サイコロページにアクセスがあったとき --- (※4)
function showDicePage (req, res) {
  // HTTPヘッダを出力
  res.writeHead(200, ctype)
  // 何面体のサイコロが必要？
  const a = req.url.split('/')
  const num = parseInt(a[2])
  // 乱数を生成
  const rnd = Math.floor(Math.random() * num) + 1
  // レスポンスの本体を出力
  res.end('<p style="font-size:72px;">' + rnd + '</p>')
}
```

- 非同期処理
  - readfile
    - 引数に読み込み後の処理の関数を記述
  - readfileSyncは同期
  - アロー関数、無名関数
    - () => {}
    - 上記の後()で実行
  - コールバック
    - readfileのあとにreadfileすると関数のネストが深くなっていく
    - Promise
      - .then(...)でつなぐ。
    - generator
      - yield,next
    - async await
      - 連続非同期、並列非同期
    - 以下記事が参考になった。
      - [https://qiita.com/soarflat/items/1a9613e023200bbebcb3#%E9%80%A3%E7%B6%9A%E3%81%97%E3%81%9F%E9%9D%9E%E5%90%8C%E6%9C%9F%E5%87%A6%E7%90%86](https://qiita.com/soarflat/items/1a9613e023200bbebcb3#%E9%80%A3%E7%B6%9A%E3%81%97%E3%81%9F%E9%9D%9E%E5%90%8C%E6%9C%9F%E5%87%A6%E7%90%86)

### babel

- トランスパイラ
- babelコマンド
  - filename.js --presets= -o outfile.js
    - 変換して保存
  - filename.js -w --presets= -o outfile.js
    - 監視をつける。変更があったら自動変換
  - src -d dest
    - ディレクトリ内を全て変換
  - src --compact=true -d dest
    - コメント、スペース削除
  - filename.js -w --presets= -o outfile.js -source-maps
    - 変換するのでデバックがしにくい
    - 変換前後を橋渡しするもの
- モジュール
  - requireで呼ぶ
  - import/export
    - node.jsならbabelで変換して利用できる

## React

### 基本

- htmlからscriptを読み込めば使える
  - react,babel
  - 要素を指定してscriptで描画。

  ```js
  <div id="root"></div>
    <!-- スクリプトの定義 ───(※3) -->
    <script type="text/babel">
      ReactDOM.render(
        <h1>Hello, world!</h1>,
        document.getElementById('root')
      )
    </script>
  ```

### jsx

- {value}で埋め込み
- 閉じタグが必要
- 括弧でくくる必要がある箇所あり。
- 複数DOMは返せない。
- style
  - cssはオブジェクトにあてる
  - ケバブケースではなく、キャメルケース（css通りではない）
- 変数の値はエスケープされる
  - XSSを防げる

### virtualDOM

- 仮想DOM
  - DOMの状態をメモリ上に保存しておいて、更新前と更新後の状態を比較して、必要最小限の部分だけを更新するという機能
  - 差分だけ更新される。画面の更新が速い。
- サンプル

```js
  <script type="text/babel">
  // 定期的に時間を表示
  setInterval(showClock, 1000)
  // 毎秒実行される関数
  function showClock () {
      const d = new Date()
      const [hour, min, sec] = [ // 時分秒を各変数に代入 --- (※1)
        d.getHours(),
        d.getMinutes(),
        d.getSeconds()]
      const z2 = (v) => { // 0で埋めて表示する関数を定義 --- (※2)
        const s = "00" + v
        return s.substr(s.length - 2, 2)
      }
      // 表示するDOMを指定 --- (※3)
      const elem = (<div>
        {z2(hour)}:{z2(min)}:{z2(sec)}
      </div>);
      // DOMを書き換える
      ReactDOM.render(elem,
        document.getElementById("root"))
    }
  </script>
```

- substr(arg1,arg2)
  - arg1:開始位置
  - arg2:文字数
  - 上の例だと後ろの2文字をとるため、先頭に0を付与できる

### コンポーネント

- 部品。組み合わせる、再利用する。
- サンプル

```js
function Greeting(props) {
  return <h1>{props.type}</h1>
}

// Greetingコンポーネントを利用する
const dom = <div>
  <Greeting type="Good morning!" />
  <Greeting type="Hello!" />
  <Greeting type="Good afternoon!" />
</div>

```

- classでもComponentを定義できる

```js
    class CBox extends React.Component {
      // コンストラクタ --- (※2)
      constructor (props) {
        super(props)
        // 状態の初期化
        this.state = {checked: false}
      }
      render () {
        // 未チェックの状態 --- (※3)
        let mark = '□'
        let bstyle = { fontWeight: 'normal' }
        // チェックされているか --- (※4)
        if (this.state.checked) {
          mark = '■'
          bstyle = { fontWeight: 'bold' }
        }
        // クリックした時のイベントを指定 --- (※5)
        const clickHandler = (e) => {
          const newValue = !this.state.checked
          this.setState({checked: newValue})
        }
        // 描画内容を返す --- (※6)
        return (
          <div onClick={clickHandler} style={bstyle}>
            {mark} {this.props.label}
          </div>
        )
      }
    }
    // ReactでDOMを書き換える --- (※7)
    const dom = <div>
      <CBox label="Apple" />
      <CBox label="Banana" />
      <CBox label="Orange" />
      <CBox label="Mango" />
    </div>
    ReactDOM.render(dom,
      document.getElementById('root'))
  </script>


```

- アロー関数でも定義できる
- stateの利用
  - コンポーネントの状態管理
  - setStateで状態を更新する
    - その後renderが呼び出される
- 要素のイベント
  - button等、onClick onChangeなどキャメルケースの既存イベントが用意されている。

### 自動ビルド、webpack

- create-react-appで開発環境を生成できる、
  - npm run build
    - ビルド
  - serve -s build -p 3000
    - サーバー公開
- webpack
  - リソースファイルの最適化
  - 変換して１つのjavascriptに。
  - webpack main.js out/test.js
  - 変換指示書　webpack.config.js
    - webpackの実行で変換できる
    - --configをつけて設定ファイルを指定
    - -p　最適化してビルド
    - -watch　監視モードで差分ビルド
    - 設定ファイル内容
      - test　ファイルパターン
      - loader　どのプラグインを使うか
      - options　プラグインのオプション
      - サンプル

        ```js
          module.exports = {
            entry: './src/main.js',
            output: {
              filename: './out/bundle.js'
            },
            module: {
              rules: [
                {
                  test: /.js$/,
                  loader: 'babel-loader',
                  options: {
                    presets:['es2015', 'react']
                  }
                }
              ]
            }
          };
        ```

- npm init force
  - すでにインストールされているパッケージも含めて、全ての依存パッケージをインストール
