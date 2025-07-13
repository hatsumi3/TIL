# 講義スライド: 明日から使える！ TypeScript型設計はじめの一歩

---

## 1. 導入 (2分): なぜ「型」を設計するのか？

### JavaScriptのよくある悩み

コードを書いている時、こんな経験はありませんか？

- `response.data.userName` のつもりが `response.data.username` とtypoしてエラー
- `calculate(10, 5)` を期待していたのに `calculate("10", "5")` と呼ばれて `"105"` になる
- この関数、一体どんなオブジェクトを渡せばいいんだっけ…？ (エディタの補完が効かない！)

**↓**

### TypeScriptが解決します！

TypeScriptの「型」は、コードがやりとりする**データの形を定義**する設計図です。

- **typoをコンパイル時に発見** → `Property 'username' does not exist on type ...`
- **意図しない型での呼び出しを禁止** → `Argument of type 'string' is not assignable to parameter of type 'number'.`
- **エディタが賢く補完** → `user.` と打つだけでプロパティ候補が表示される

**型は、バグを未然に防ぎ、開発体験を向上させるための強力な武器になります。**

---

## 2. 本編1: `any`は卒業！ `unknown`で安全に (3分)

### `any`は何でもできてしまう、危険な型

`any`型を使うと、どんなプロパティにもアクセスでき、関数として呼び出すこともできてしまいます。
これはコンパイル時のチェックをすり抜けて、実行時エラーの原因になります。

```typescript
// YAMLをパースする関数 (戻り値がany)
function parseYAML(yaml: string): any {
  // ...実装は省略
}

const book = parseYAML(`
  name: Jane Eyre
  author: Charlotte Brontë
`);

// コンパイルエラーが出ない！
console.log(book.title);  // 存在しないプロパティ -> undefined
book('read');             // 関数ではないのに呼び出せる -> 実行時エラー！
```

### `unknown`で安全なコードに

`any`の代わりに`unknown`型を使うと、TypeScriptは「型が不明である」ことを認識します。
そのため、プロパティにアクセスしたり、呼び出したりする前に、「型をチェックすること」を強制できます。

```typescript
function safeParseYAML(yaml: string): unknown {
  return parseYAML(yaml);
}

const book = safeParseYAML(`...`);

// `book`は`unknown`型なので、そのままでは操作できない
console.log(book.title); // エラー: 'book' is of type 'unknown'.

// 型ガードを使って安全に操作する
if (typeof book === 'object' && book !== null && 'name' in book) {
  // このブロックの中では book は { name: unknown } 型として扱える
  console.log(book.name); // OK!
}
```

**教訓: 外部APIのレスポンスなど、型が不明な値にはまず`unknown`を使い、型ガードで安全に扱いましょう。**

---

## 2. 本編2: 「文字列」卒業！ Union型で意図を伝える (4分)

### `string`型は、あまりにも曖昧

`string`型はどんな文字列でも受け入れてしまうため、意図しない値が入り込む可能性があります。

```typescript
interface Album {
  artist: string;
  title: string;
  recordingType: string; // "live" か "studio" のはずが…
}

const myAlbum: Album = {
  artist: "Miles Davis",
  title: "Kind of Blue",
  recordingType: "Studio" // 大文字始まり。typoだがエラーにならない
};
```

### Union型で、表現力と安全性を高める

特定のリテラル（具体的な値）を `|` で繋ぐことで、その型の取りうる値を正確に表現できます。
これにより、TypeScriptコンパイラが間違いを検知してくれます。

```typescript
type RecordingType = 'studio' | 'live'; // この2種類しか受け付けない

interface Album {
  artist: string;
  title: string;
  recordingType: RecordingType;
}

const myAlbum: Album = {
  artist: "Miles Davis",
  title: "Kind of Blue",
  recordingType: "Studio" // エラー: Type '"Studio"' is not assignable to type 'RecordingType'.
};
```

### 応用: 「状態」をUnion型で表現する

APIリクエストのような非同期処理の状態を、複数の`boolean`で管理すると、「読み込み中かつエラー」のような**ありえない状態**が生まれてしまいます。

```typescript
// アンチパターン: isLoadingとerrorが同時にtrueになりうる
interface State {
  isLoading: boolean;
  error?: string;
  pageText: string;
}
```

これを、`state`という共通のプロパティを持つUnion型で表現することで、状態遷移をより安全かつ明確に管理できます。

```typescript
// おすすめのパターン
type RequestState =
  | { state: 'pending' }
  | { state: 'error'; error: string }
  | { state: 'ok'; pageText: string };

// stateの値によって、他のプロパティの有無が決まる
// これで「ありえない状態」を型レベルで防げる！
```

**教訓: 取りうる値の候補が限られている場合は、`string`ではなくUnion型を使いましょう。特に「状態」の管理に絶大な効果を発揮します。**

---

## 2. 本編3: `type`と`interface`、どう使い分ける？ (3分)

### ほとんどの場面で、できることは同じ

`type`と`interface`は、オブジェクトの型を定義するという点では非常に似ています。

```typescript
// typeで定義
type UserType = {
  name: string;
  age: number;
};

// interfaceで定義
interface UserInterface {
  name: string;
  age: number;
}
```

### 違いと使い分けの指針

| | `interface` | `type` |
|:---|:---|:---|
| **得意なこと** | オブジェクトの「形」を定義する | 型に「名前」を付ける（Union型、複雑な型など） |
| **拡張** | `extends`で継承できる | `&`（交差型）で合成できる |
| **特徴** | 同じ名前で再度定義すると「宣言がマージ」される | 同じ名前で再定義はできない |


**初学者向けのシンプルな使い分け指針:**

1.  **`interface` を第一候補に:**
    APIのレスポンスや、関数に渡すオブジェクトの形など、**オブジェクトの構造を定義する場合**は、まず`interface`を使いましょう。拡張性があり、エラーメッセージも分かりやすい傾向があります。

2.  **`type` を使う場合:**
    Union型 (`string | number`) や、タプル型 (`[string, number]`) など、**`interface`で表現できない複雑な型を定義したい場合**に`type`を使いましょう。

```typescript
// オブジェクトの形なので interface
interface Point {
  x: number;
  y: number;
}

// Union型なので type
type Status = 'success' | 'error';
```

**教訓: まずは`interface`での定義を試し、それで表現できない場合に`type`を検討するのがおすすめです。**

---

## 3. まとめ: 型は未来の自分へのメッセージ (3分)

### 本日のまとめ

1.  **`any`ではなく`unknown`を使う**
    -   安全でない`any`を避け、型ガードと組み合わせて`unknown`を使いこなそう。

2.  **`string`ではなくUnion型を使う**
    -   取りうる値が限られるなら、リテラルUnion型で型の表現力を高め、typoを防ごう。
    -   特に「状態管理」で効果絶大！

3.  **`type`と`interface`を使い分ける**
    -   オブジェクトの形にはまず`interface`を検討しよう。

### 型は未来の自分やチームメイトへのメッセージ

今日学んだテクニックは、単にバグを減らすだけではありません。

-   **コードの可読性が上がる:** 型定義を見るだけで、そのデータがどんな構造で、どんな値を取りうるのかが分かる。
-   **変更が容易になる:** 型定義を変更すれば、影響範囲をコンパイラが教えてくれる。
-   **開発体験が向上する:** エディタの補完が最大限に機能し、ストレスなく開発が進められる。

**良い型設計は、未来の自分やチームメイトを助けるための最高のドキュメントです。**

ぜひ、Effective TypeScriptの他のサンプルも覗いて、さらなる知見を深めてみてください。

**ご清聴ありがとうございました！**
