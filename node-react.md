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
