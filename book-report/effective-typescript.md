# Effective Typescript

## chapter 1

- コード生成は方に依存しない
  - 実行時に型をチェックすることはできない
- 型演算は実行時の値に影響しない
  - `val as number`などしても`val`はstring
- any型は言語サポートを受けられない
  - オートコンプリートが行われない
  - シンボルの名前変更を行なっても変更されない
  
## chapter 2

- F2でシンボルをリネームできる
  - 参照する箇所のみ命名変更される
- unknown型の対極がany型
- 型アサーションより型アノテーション
  - 型アノテーションは値がインターフェースに適合するか検証する
  - 型アサーションはエラーを無視する
  - 使い分け：
    - プログラマーがTypeScriptが知っている以上のことを知っているとき
      - DOM要素の型付け

- 型アノテーションサンプル
  ```typescript
    interface Person {name: string};
    const people = ["Alice", "Bob", "jan"].map(
      (name): Person => ({name})
    );
    ```
- 余剰プロパティチェックと型チェックを区別する
  - 余剰プロパティチェック：未知のプロパティを禁止する（フレッシュネス）
  - 型チェック：部分集合であればOK
- typeとinterfaceの使い分け
  - type：unionのtypeがある
  - interface：オーグメンテーション（複数宣言時にマージされる）